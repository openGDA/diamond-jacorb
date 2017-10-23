EventChannelImpl.patch
    - Fixes a bug where events could be lost if an event consumer disappears.
    - Similar to http://osdir.com/ml/corba.jacorb.devel/2003-02/msg00040.html

namemanager.patch
    - Causes all entries to be shown in the NameManager, instead of just the
      first 40. (See also https://www.jacorb.org/bugzilla/show_bug.cgi?id=647)
    - Causes entries in the NameManager to be shown in alphabetical order
      (ignoring case).
