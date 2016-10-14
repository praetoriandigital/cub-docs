## Newsletter Subscription Form

Code sample:
```html
<div id="newsletter-form1">
  <!-- Newsletter Form will be rendered inside this element, 
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
        '#newsletter-form1': {
          action: 'subscribers',
          fieldsets: [
            {
              name: 'responsive-column',
              fields: [
                // First Name - optional
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
                // Newsletter subscription options - required. 
                // You can find mailing list IDs and names in Cub admin.
                // As an alternative to checkboxes you can use hidden field 
                // for this, see example below.
                {
                  name: 'subscribe',
                  type: 'checkboxgroup',
                  options: [
                    ['mlt_uc4ca4238a0b9238', 'PoliceOne Member Newsletter'],
                    ['mlt_u8f14e45fceea167', 'P1 Breaking News Alerts']
                  ]
                }
              ],
              // Title for Submit button
              submit: 'Subscribe'
            }
          ]
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
 
If on your site you're using Cub widget for registration, First Name and Last 
Name (if present) and Email fields will be pre-populated for logged-in users. Or, you
can provide initial values for known users yourself via 'value' parameter, like 
this:

```js
    ...
    {
      name: 'first_name',
      label: 'First Name',
      type: 'text',
      value: 'John'  // initial value
    },
    ...
```

If you'd like newsletter checkboxes to be checked on by default, provide value 
for 'subscribe' field as an array:
 
```js
    ...
    {
      name: 'subscribe',
      type: 'checkboxgroup',
      options: [
        ['mlt_uc4ca4238a0b9238', 'PoliceOne Member Newsletter'],
        ['mlt_u8f14e45fceea167', 'P1 Breaking News Alerts']
      ],
      value: ['mlt_uc4ca4238a0b9238', 'mlt_u8f14e45fceea167']
    }
    ...
```

## Hiding newsletter checkboxes

Sometimes you may want to hide newsletter checkboxes. This can be done by 
replacing checkboxes with hidden field, like this:
  
```js
    ...
    {
      name: 'subscribe',
      type: 'hidden',
      value: 'mlt_uc4ca4238a0b9238,mlt_u8f14e45fceea167'
    },
    ...
```
  

## Form events

Newsletter Subscription Form supports the same events and custom handlers as 
Lead Forms, see [form events](form-events.md) for details.
