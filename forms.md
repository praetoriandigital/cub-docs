# Cub Widget: Lead Forms

### Code sample

You can tell Cub Widget to connect to any ``<form>`` tag(s) on your web page
and send submitted form data to central Leads database:

```html

<form id="my-lead-form1">
  <!-- This is a Lead Form. It can contain any number of HTML fields and any
       extra HTML markup. The following field types are supported:
       - text input;
       - select;
       - checkbox;
       - radio buttons;
       - textarea;
       - hidden input. -->

  <!-- "processing_rule" field is required for all forms.
       See notes below about Processing Rules. -->
  <input type="hidden" name="processing_rule" value="{processing_rule_ID}">

  <!-- Add a submit button. It can be <input type="submit">, or <button>, or
       <input type="image">. -->
  <button>Submit</button>
</form>

<form id="my-lead-form2">
  <!-- This is another Lead Form
       ... -->
</form>

<script>
  var cubAsyncInit = function(cub) {
    // Configuration parameters
    cub.setup({
      apiKey: '<your-public-API-key>',

      // forms parameter is an object with CSS selectors as keys
      // and form options (optional) as values. Each CSS selector must
      // identify a <form> tag:
      forms: {
        '#my-lead-form1': {},  // no extra options for 1st form

        '#my-lead-form2': {    // add custom onSuccess handler for 2nd form
          onSuccess: function(formData, formElement) {
            // Replace standard "Thank you.." alert with a
            // redirect to our custom page:
            window.location.href = '/success.html';
          }
        }
      }

    });
    cub.start();
  };

  // Load Cub widget asynchronously
  (function(){
    if (document.getElementById("cub-widget-script")) {return;}
    var firstScript = document.getElementsByTagName("script")[0];
    var cubJs = document.createElement("script");
    cubJs.id = "cub-widget-script";
    // See notes about widget versioning
    cubJs.src = "//cub-praetorian.netdna-ssl.com/cub-widget.js";
    firstScript.parentNode.insertBefore(cubJs, firstScript);
  }());
</script>
```

### Processing Rules

Before creating your form, please define a Processing Rule for it in Cub Admin
first. Processing Rules tell Cub how incoming data should be handled. You can
define required fields there, add email notifications or add data export
targets.

Each form must have a field named "processing_rule" containing ID of Processing
Rule you created for this form. Typically you'll want it to be a hidden field,
unless you do something very special.

### Field names

The widget sends to Cub all HTML fields which you provide in the ``<form>``
tag. This is pretty much like standard ``<form>`` submit behavior, but the
data is sent to Cub. So to add an extra field to the form you just add it to
form HTML markup and all done - it will be sent to Cub. This gives you great
flexibility in adding or modifying form fields.

Although widget is very flexible in accepting arbitrary form data, we strongly
encourage you to follow these naming conventions for pre-defined fields:

**Personal User Data**
- first_name
- last_name
- email
- phone
- address
- city
- state
- country
- zip

**Organization Data**
- organization_name
- organization_size
- organization_phone
- organization_address
- organization_city
- organization_state
- organization_country
- organization_zip

**Organization Membership Data**
- member_position
 
**Miscellaneous**
- comment
- source

### Form events and custom handlers

Cub Widget adds an event listener for "submit" event of your forms and replaces
standard form submit behavior. When a form is submitted, widget does the
following:

  1. Sends form data to Cub;
  2. If you provided a custom onSubmit callback for this form in options,
     widget will call your callback with two parameters, first is an object
     with submitted form data, and second is form DOM element:
     
     ```js
     onSubmit(formData, formElement);
     ```
     
     If you didn't provided a custom callback, widget will execute a standard
     onSubmit handler, which adds "processing" class to submitted ``<form>``.
     You can use it to add a visual indication that form is being processed.
  3. When response from Cub server is received, it can be either success or error.
     Before calling actions for success or error, widget will remove "processing"
     class from a ``<form>`` tag (unless you've replaced onSubmit handler with
     your own). Next action depends on server response:
      * **Success:** widget checks if you provided a custom onSuccess callback,
        and if you did, your callback will be called with formData and formElement
        parameters:

        ```js
        onSuccess(formData, formElement);
        ```
        
        If custom ``onSuccess`` handler wasn't provided, widget shows a "Thank you"
        message and clears the form.
      * **Error:** if you provided a custom onError callback it will be called
        with the following parameters:
        
        ```js
        onError(topError, fieldErrors, formData, formElement);
        ```
        
        ``topError`` is a string containing top-level error message (optional). 
        Top-level error is an error which is not related to a particular field.
        For example, this could be a server connection error, or an error which
        is related to validation of multiple fields. ``fieldErrors`` is an 
        object with field names as keys and error messages as values. ``formData``
        and ``formElement`` mean the same as for other callbacks, these are
        submitted form data and form DOM element respectively.
  
        If you didn't provide custom ``onError`` handler, widget will try to
        locate pre-defined placeholders for form errors and paste error messages
        into them. For top-level form error it will look for
        ``<span class="top-error">`` inside ``<form>`` tag. For field-level errors
        it looks for ``<span class="error" data-field="{field_name}">`` for each
        field.
