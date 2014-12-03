If you find this:
```
2014-12-03 14:59:42 ERROR Zope.SiteErrorLog 1417615182.70.607391088835 http://localhost:8080/Plone/++contextportlets++plone.rightcolumn/+/plonesocial.microblog.portlet
Traceback (innermost last):
  Module ZPublisher.Publish, line 138, in publish
  Module ZPublisher.mapply, line 77, in mapply
  Module ZPublisher.Publish, line 48, in call_object
  Module plone.app.portlets.browser.formhelper, line 59, in __call__
  Module z3c.form.form, line 218, in __call__
  Module plone.z3cform.fieldsets.extensible, line 58, in update
  Module plone.autoform.form, line 33, in updateFields
  Module plone.autoform.base, line 54, in updateFieldsFromSchemata
  Module plone.autoform.form, line 22, in schema
NotImplementedError: The class deriving from AutoExtensibleForm must have a 'schema' property
```

do this:

```diff
diff --git a/plonesocial/activitystream/browser/activitystream_portal.py b/plonesocial/activitystream/browser/activitystream_portal.py
index dad0f8a..548476a 100644
--- a/plonesocial/activitystream/browser/activitystream_portal.py
+++ b/plonesocial/activitystream/browser/activitystream_portal.py
@@ -1,18 +1,21 @@
-from zope.interface import implements
-from zope.component import getMultiAdapter
-from zope.formlib import form
+# -*- encoding: utf8 -*-
+from .interfaces import IActivitystreamPortlet
 from Acquisition import aq_inner
-
-from plone.app.portlets.portlets import base
-from zope.viewlet.interfaces import IViewlet
+from Products.CMFPlone.utils import getFSVersionTuple
 from Products.Five import BrowserView
 from Products.Five.browser.pagetemplatefile import ViewPageTemplateFile
+from plone.app.portlets.portlets import base
+from zope.component import getMultiAdapter
+from zope.formlib import form
+from zope.i18nmessageid import MessageFactory
+from zope.interface import implements
+from zope.viewlet.interfaces import IViewlet
 
-from .interfaces import IActivitystreamPortlet
 
-from zope.i18nmessageid import MessageFactory
 _ = MessageFactory('plonesocial.activitystream')
 
+PLONE4 = getFSVersionTuple()[0] <= 4
+
 
 class PortalView(BrowserView):
     """Home page view containing activity stream viewlets."""
@@ -108,11 +111,17 @@ class Renderer(base.Renderer):
 
 
 class AddForm(base.AddForm):
-    form_fields = form.Fields(IActivitystreamPortlet)
+    if PLONE4:
+        form_fields = form.Fields(IActivitystreamPortlet)
+    else:
+        schema = IActivitystreamPortlet
 
     def create(self, data):
         return Assignment(**data)
 
 
 class EditForm(base.EditForm):
-    form_fields = form.Fields(IActivitystreamPortlet)
+    if PLONE4:
+        form_fields = form.Fields(IActivitystreamPortlet)
+    else:
+        schema = IActivitystreamPortlet
```
