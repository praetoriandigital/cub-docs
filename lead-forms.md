## Lead Forms

Code sample:
```html
<body>
  <script>
     // Load Cub widget asynchronously. Put this as higher as possible so
     // widget will load faster.
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

  <div id="lead-form1">
    <!-- Lead Form will be rendered inside this element,
         replacing it's previous contents.
         HINT: you can place a loader icon or text here, like this: -->
    Loading...
  </div>

  <!--
  Cub initialization script needs to be placed on the page only once, no matter
  how many forms do you have. You should place it below last form element which
  needs to be handled by Cub. Configure your forms in 'forms' parameter below:
  -->
  <script>
    var cubAsyncInit = function(cub) {
      // Configuration parameters
      cub.start({
        apiKey: '<your-public-API-key>',

        // 'forms' parameter is an object with CSS selectors as keys and form
        // options as values. CSS selectors identify parent containers for
        // forms to be rendered:
        forms: {
          '#lead-form1': {
            load: '<lead-form-ID>'  // Get this ID in Cub admin
          }
        }

      });
    };
  </script>
</body>
```

## "Server side" lead forms.

Server side lead form are constructed in cub admin. Use created lead form uid to
create lead form as in the example below.

### Providing initial data

If on your site you're using Cub widget for registration, widget will
automatically pre-populate known form fields for logged-in users. Or, you can
provide initial values yourself by passing them in form configuration options,
like this:

```js
    ...
    forms: {
      '#lead-form1': {
        load: '<lead-form-ID>',
        // Initial field values:
        first_name: 'John',
        last_name: 'Doe',
        email: 'john.doe@example.com',
        ...
      }
    }
    ...
```

## "Client side" lead forms.
### Configuring

Cub generic form engine could be used for lead forms creation.

```js
  ...
  forms: {
    '#lead-form1': {
      action: 'leads',
      form: 'lfm_rMvQ1iRVRgpZ6Asw',  //for required for notification rules and processing rules
      // The value when a multi-column form layout transforms into a single column (in px)
      responsiveBreakpoint: 700,
      fieldsets: [{
        name: 'responsive-column',
        // The width for a fieldset layout column (in percents)
        columnWidth: 100,
        className: "extraClassNameForFieldSet", // optional, this class will be added to fieldset div container
        fields: [
          {
            name: 'first_name',
            label: 'First Name',
            type: 'text'
          },
          // Last Name - optional
          {
            name: 'last_name',
            label: 'Last Name',
            type: 'text'
          },
          // Email - required
          {
            name: 'email',
            label: 'Email',
            type: 'text',
            required: true
          },
        ]
      }],
      // The width for a Submit button layout column (in percents)
      submitColumnWidth: 100,
      // Title for Submit button
      submit: 'Subscribe',
    }
  }
  ...
```

See detailed fields description in [Fields docs](./fields.md)

### 'Register me' feature

Cub generic form can be configured to automatically register a new user and redirect to 'experience' page to continue registration process (in case of an existent user redirect will be to 'login' page).

If you defined [onSuccess callback](./form-events.md#supported-callbacks) that has async code you MUST return Deferred/Promise object from onSuccess callback and resolve this Deferred/Promise object when your async code is done. **If you did not return Deferred/Promise object from onSuccess callback CUB-widget will register user and redirect to next page('experience' or 'login') without waiting for your async code.** You can use helpers `cub.helpers.Deferred` or `cub.helpers.Promise`. See examples bellow.

**Field 'email' is required for successful registration.** Fields 'first_name', 'last_name', 'middle_name', 'organization' and 'position', if present, will be used to prefill user profile.

```js
  ...
  forms: {
    '#my-form': {
      action: 'dummy-api',
      fieldsets: [{
        {
          name: 'email',
          label: 'Email',
          type: 'text',
          required: true
        },
        // ... 
        // config for other fields
        // ... 
      }],
      // ... 
      // other configs for generic form
      // ... 

      // Register Me checkbox.
      // If checked and form has valid email it will 
      //   (if there is no user with this email in DB) 
      //   create new user and redirect to 'experience' page
      //     or
      //   (if there is user with this email in DB) 
      //   show alert 'You already registered' and redirect to 'login' page
      //     or
      //   (if registration API returned error) 
      //   redirect to 'registration' page
      registerMe: {
        checked: true,
        label: 'Register Me',
        hideForLogged: true, // When true, hides checkbox for logged user. Default value true.
        maxTimeout: 5000 // default: 2000 
                         // max timeout before registering user 
                         // and redirecting to 'experience' page 
                         // (or 'login' page for existent users)
      }
      onSuccess: function(formData, fromElement) {
        var dfd = new cub.helpers.Deferred() // Deferred helper
        console.log('i am synchronous, no problem')

        // simplest async code
        setTimeout(function() {
          console.log('wait for me!') 
          dfd.resolve() // resolve when async code done
                        // in case of Goolge Analitics you should
                        // resolve Deferred in 'hitCallback'
        }, 3000)

        return dfd.promise() // convert Deferred to Promise for safety
      }
    }
  }
  ...
```

## Email notifications

Email notification recipients can be configured in Cub admin in Lead Form
properties. Also is possible that some extra email notifications will be send
during lead data processing.

## Form events

Lead Forms support the same events and custom handlers as Newsletter
Subscription Form, see [form events](form-events.md) for details.
