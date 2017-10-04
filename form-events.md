## Form events and custom handlers

For both [Lead Forms](lead-forms.md) and 
[Newsletter Subscription Form](newsletter-form.md) Cub widget provides the same
simple workflow:

1. Submit data to Cub API;
2. If errors were found, render errors on the form;
3. If no errors found, tell user that form was submitted successfully.

The widget provides a way for you to inject your JavaScript callbacks into this 
workflow, so you can either customize form behavior or integrate it with other
parts of your application. You can define your callbacks like this:
 
```js
   ...
   forms: {
     'my-form1': {
       ...
       onSuccess: function(formData, formElement) {
         window.location.href = '/form-success.html';
       }
     }
   }
   ...
```
Sample code above shows how you can substitute standard success notification
with a redirect to your custom page using ``onSuccess`` callback. 

## Supported callbacks

* **onSubmit(formData, formElement)** - called when form was submitted. 
  Parameters:
    - formData - object with submitted form fields;
    - formElement - DOM element of ``<form>`` being submitted.
    

* **onError(topError, fieldErrors, formData, formElement)** - called when form
  submission resulted in error(s). Parameters:
    - topError - form-level error, i.e. not related to a particular field, but
      to the whole form;
    - fieldErrors - field-level errors, object with field names and errors;
    - formData - object with submitted form fields;
    - formElement - DOM element of ``<form>`` being submitted.

* **onSuccess(formData, formElement)** - called when form was submitted 
   successfully. If you provide this callback, it substitutes standard success 
   behavior. Parameters:
    - formData - object with submitted form fields;
    - formElement - DOM element of ``<form>`` being submitted.

  **IMPORTANT**: if you have async code in onSuccess callback and active ['Register me' feature](./lead-forms.md#register-me-feature) you should return `Deferred` or `Promise` from onSuccess callback and resolve it when async code is done. See example [here](./lead-forms.md#register-me-feature).
