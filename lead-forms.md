## Lead Forms

Code sample:
```html
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

## Providing initial data

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

## Email notifications

Email notification recipients can be configured either in Cub admin in Lead Form
properties, or extracted from FrontContent containers, or both. Cub 
automatically takes care of avoiding duplicated notifications, so even if the 
same recipient email is mentioned twice for a lead (for example, if the same 
person is listed both in Cub and in FC container) - that's fine, Cub will 
understand that this is the same recipient, and only one notification email will 
be sent. 

If you'd like to pull notifications for a lead from a FC container, please 
provide ``notifications`` parameter in form initialization data in the following 
format:

```
notifications: '<fc-site>:<container>:<content-ID>'
```
 
... where

* ``<fc-site>`` - can be one of ``p1``, ``c1``, ``fr1``, ``ems1``, ``h1`` - 
  in which FC Site Cub should look for a container;
* ``<container>`` - the only supported value for now is ``campaign``;
* ``<content-ID>`` - primary key (content_id) of a record in that container.
  
Example:

```js
    ...
    forms: {
      '#lead-form1': {
        load: '<lead-form-ID>',
        // Pull notifications from Campaign #42 in P1:
        notifications: 'p1:campaign:42',
        ...
      }
    }
    ...
```

## Form events

Lead Forms support the same events and custom handlers as Newsletter 
Subscription Form, see [form events](form-events.md) for details.

