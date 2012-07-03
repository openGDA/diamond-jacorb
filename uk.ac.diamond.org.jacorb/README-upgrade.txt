Currently, when we specify jacorb.config.dir, it points to the directory that
contains an 'etc' folder. JacORB adds '/etc' and looks for jacorb.properties
in there.

A bug was reported against JacORB on 2005-04-26 that resulted in a change in
behaviour:

  http://www.jacorb.org/cgi-bin/bugzilla/show_bug.cgi?id=595

Now '/etc' is *not* appended to jacorb.config.dir.

As of February 2011, the latest version of JacORB (2.3.1, released 2009-05-27)
doesn't include this change, as it was applied on 2009-10-27:

  http://www.jacorb.org/cgi-bin/cvsweb/JacORB/src/org/jacorb/config/JacORBConfiguration.java.diff?r1=1.33;r2=1.34;f=h

If a newer version of JacORB is released before we upgrade, we may need to
consider this change in behaviour by either:

 1. appending '/etc' to jacorb.config.dir ourselves (e.g. in the gda launcher)

or:

 2. moving jacorb.properties files 'up a level' (and removing the empty 'etc'
    directories).
