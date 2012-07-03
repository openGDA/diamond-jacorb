JacORB includes an implementation of the org.omg.* packages. Ideally we should
be using these. However it doesn't appear to be possible to get JacORB working
in an OSGi environment unless the org.jacorb.* classes are used together with
the JRE's implementation of the org.omg.* packages.

Ideally this plugin would have the JacORB JARs on its classpath, and would
export the JacORB and OMG packages in jacorb.jar. Other plugins could depend on
this one, and all JacORB/OMG classes would be provided by it.

This doesn't work. On startup of the RCP client, a VerifyError is thrown:

    java.lang.VerifyError: (class: org/jacorb/orb/Delegate, method: getReference
        signature: (Lorg/jacorb/poa/POA;)Lorg/omg/CORBA/portable/ObjectImpl;)
        Incompatible object argument for function call

An example of this problem (which may in fact be the only instance of it) can be
seen if org.jacorb.orb.Delegate is loaded. As JacORB classes are loaded, so too
are some of the OMG classes (and these are loaded from jacorb.jar). Eventually,
org.jacorb.orb.Reference is loaded. This is a subclass of javax.rmi.CORBA.Stub,
so this is also loaded; but it is only present in the JRE. The Stub class refers
to other CORBA classes that have already been loaded from jacorb.jar. As
javax.rmi.CORBA.Stub is part of the JRE, it can only link to classes from the
JRE (not from a bundle). Therefore some OMG classes are being loaded twice,
leading to the VerifyError.

Some more information about this problem can be found in this thread on the
Apache Felix Users mailing list:

    Bundelizing Jacorb -- getting screwed
    http://www.mail-archive.com/users@felix.apache.org/msg05402.html

Also see this thread on the jacorb-developer list:

    OSGi bundle for JacORB
    http://comments.gmane.org/gmane.comp.corba.jacorb.devel/5164

The solution for now seems to be to make this plugin depend on the OSGi system
bundle. Then, while loading JacORB classes, any OMG classes will be loaded from
the system bundle (i.e. the JRE platform) and not from jacorb.jar.

This means that (at least in the RCP client) we are using the JacORB classes
in conjunction with org.omg.* classes from the JRE, rather than JacORB's own
implementation of them. Fortunately this doesn't seem to have caused any
problems (yet).

(Details of the algorithm used by Eclipse to load classes can be seen in the
findClassInternal method in the org.eclipse.osgi.internal.loader.BundleLoader
class.)
