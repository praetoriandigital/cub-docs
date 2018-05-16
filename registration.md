## Users Login and Registration

Add the following HTML code to your site:

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
  <!-- User menu will be rendered inside this element. You can place it anywhere
       on the page. This element should be present on all site pages. -->
  <div id="cub-widget-menu"></div>

  <!-- Login, Registration, Reset Password, My Profile,  My Subscriptions,
       and other widget forms will be rendered inside this element. You can
       place it anywhere on the page. It must be present on those pages where
       you expect a widget form to appear. -->
  <div id="cub-widget-app"></div>
    
    <!-- Cub widget initialization script.
         Important: place it below cub-widget-menu and cub-widget-app tags. -->
  <script>
    var cubAsyncInit = function(cub) {
      // Configuration parameters
      cub.start({
        apiKey: '<your-public-API-key>'
      });
    };
  </script>
</body>
```

## URL configuration

Contents of cub-widget-app tag depends on URL. By default, widget uses
hash URLs, like #login, #registration, etc. Transitions between such URLs
don't trigger page reload, so the rest of the page around widget menu and
widget app elements will remain unchanged. If you'd like to place widget
forms on different pages of your site, please login to Cub Admin and
configure URLs for your site in Widget Configuration section.

## Custom styling

Widget adds some very basic CSS styles for its elements, but in general it
leaves styling to you. Please feel free to define or override any styles for
any elements rendered by widget, just like you do it for any other elements
on the page.

## 'Register me' feature

User can be automatically registered during form submission using ['Register me' feature](./lead-forms.md#register-me-feature).

## 'SingleSO' feature

If SingleSO enabled for a site, a user would be automatically logged in on
this site if he is already logged in on one of the sites with enabled
SingleSO. For SingleSO to work between two sites, SingleSO must be enabled
for both sites. For the particular user, SingleSO will start to work after
he accesses the site with enabled SingleSO (already being logged in) or after
he logs in to the site with enabled SingleSO.
To work, **SingleSO must be enabled for the site in CubAdmin**.

If SingleSO enabled and you want to log out a user, the user should be logged out only with standard
"#logout" url(or other url configured as logout page),
**do not try just to delete cub cookies or user will be automatically logged back again**.

**After user log out** on a site with enabled SingleSO **further automatic
logins(with SingleSO) on this site** will be prevented **during current browser session**.
