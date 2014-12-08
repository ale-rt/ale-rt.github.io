**Plone 5** portlets are expected to use **z3c.form**, while in **Plone<=4** portlets have been using **formlib**.

Given this change you may find error like this:

```log
2014-12-03 14:59:42 ERROR Zope.SiteErrorLog 1417615182.70.607391088835 http://localhost:8080/Plone/++contextportlets++plone.rightcolumn/+/example.portlet
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

The reason is that the add and edit form classes for the portlets are expected to have an attribute called `schema` that should be equal to the interface defining the portlet fields. In Plone<=4 that interfaced was used to fill an attribute called `form_fields`.

<!-- TEASER_END -->

The easiest way to support **both** Plone 5 and Plone<=4, is just to add a `schema` class attribute equal to the interface you are using:

```diff
 class AddForm(base.AddForm):
+    schema = IExamplePortlet
     form_fields = form.Fields(IExamplePortlet)

     def create(self, data):
         return Assignment(**data)


 class EditForm(base.EditForm):
+    schema = IExamplePortlet
     form_fields = form.Fields(IExamplePortlet)
```

In many cases this is all you have to do. If you run in other troubles please leave a comment and will try to update this post.

If you want your code to behave differently according to the Plone version you are using, you may want to use the utility function `getFSVersionTuple` to check for the running version of Plone. You can start from this example

```diff
+from Products.CMFPlone.utils import getFSVersionTuple
+
+PLONE4 = getFSVersionTuple()[0] <= 4
+

 class AddForm(base.AddForm):
-    form_fields = form.Fields(IExamplePortlet)
+    if PLONE4:
+        form_fields = form.Fields(IExamplePortlet)
+    else:
+        schema = IExamplePortlet

     def create(self, data):
         return Assignment(**data)


 class EditForm(base.EditForm):
-    form_fields = form.Fields(IExamplePortlet)
+    if PLONE4:
+        form_fields = form.Fields(IExamplePortlet)
+    else:
+        schema = IExamplePortlet
```
