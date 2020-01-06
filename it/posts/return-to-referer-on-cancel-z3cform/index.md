# Return to referer when pressing cancel on z3c.form

The default cancel action on plone.app.z3cform
is to just clear the form.
This is hardly what you need in many cases.

Probably you will want to direct the user on
something similar to a thank you page,
or maybe just close a containing pop up.

In this post I will show you how to override
the default behavior in order to redirect the user to the referer.

Generally speaking you can get the HTTP referer from your zope request.

The first thing to do is to store the referer url in to a form field.
This can be done adding a *URI field* on your form.
```
from z3c.form.field import Fields
from zope.schema import URI

class MyForm(Form):

    def updateFields(self):
        ''' Adds a referer that can be used when a cancel button is pressed
        '''
        super(AddForm, self).updateFields()
        referer = URI(
            __name__='referer',
            required=False,
            default=self.request.get('HTTP_REFERER', ''),
        )
        self.fields += Fields(referer)
```

In addition we want to have that field hidden.

To set up the referer widget, we override the `updateWidgets` method:
```
    def updateWidgets(self):
        ''' Adds a referer to be used when a cancel button is pressed
        '''
        super(AddForm, self).updateWidgets()
        self.widgets['referer'].mode = 'hidden'
```

The default behavior when you submit your form is to traverse to your
`next_url`.

```
    def nextURL(self):
        ''' The next URL
        '''
        if self.immediate_view is not None:
            return self.immediate_view
        referer = self.request.get('form.widgets.referer', '')  # custom code
        if referer:                                             # custom code
            return referer                                      # custom code
        return self.context.absolute_url()
```

The three lines check if we have an `immediate_view` instance property.
If we have it, it means a content has been created.

Note: it would be better to push the referer to that instance property.
