## Newsletter Subscription Form

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
            // The value when a multi-column form layout transforms into a single column (in px)
            responsiveBreakpoint: 700,
            fieldsets: [
              {
                name: 'responsive-column',
                // The width for a fieldset layout column (in percents)
                columnWidth: 100,
                className: "extraClassNameForFieldSet", // optional, this class will be added to fieldset div container
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
                // The width for a Submit button layout column (in percents)
                submitColumnWidth: 100,
                // Title for Submit button
                submit: 'Subscribe'
              }
            ]
          }
        }

      });
    };
  </script>
</body>
```

## Providing initial data

If your site uses Cub widget for registration, the First Name, Last
Name (if present), and Email fields will be pre-populated for logged-in users. Or, you
can provide initial values for known users yourself via the 'value' parameter, like
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

To check newsletter checkboxes by default, use an array for the "subscribe" field value:

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
replacing checkboxes with a hidden field, like this:

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
