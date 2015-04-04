# Plone 5.0b1 is out

**Caveat**: This is **work in progress**.

The First beta of Plone 5 is in the wild and is performing really good.

It is time to start migrating our products.

On this page I am going to link all the resources I am finding
about porting code from Plone 4 to Plone 5.


### Changed imports
I will try to keep this list up to date.

#### Import zope IIntIds from zope.intid (was zope.app.intid)
```diff
-from zope.app.intid.interfaces import IIntIds
+from zope.intid.interfaces import IIntIds
```

### Products you will not need anymore

There a re some products that have been included/superseeded
by Plone5 default packages.

Here a list of those and some instructions on how to remove them.

#### wildcard.foldercontents
Many of my sites where using **wildcard.foldercontents**.

This is enough in most cases (not all):

 - remove `wildcard.foldercontents` your buildouts
 - remove `wildcard.foldercontents` in your setup.py files
 - remove `<dependency>profile-wildcard.foldercontents:default</dependency>`
   in your metadata.xml files

In some other case you may have to search deeper in your code
(python imports, template customizations, ...)

### Theme migration

If your Plone 4 theme was based on "Sunburst Theme"
remember to modify this: it should be based on "Plone Default".

You should do something similar:
```diff
- <skin-path name="My Site Theme" based-on="Sunburst Theme">
+ <skin-path name="My Site Theme" based-on="Plone Default">
```

### Portlets compatibility

Do you want to port your portlets to Plone5?
You have to port the to the new z3c.form machinery.
Luckily [this is really easy](http://ale-rt.github.io/posts/how-to-port-plone4-portlets-to-plone5.html)
