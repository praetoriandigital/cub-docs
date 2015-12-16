# Cub Widget: Users Login and Registration

### Installation

Add the following HTML code to your site:

```html
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

  // Load Cub widget asynchronously
  (function(){
    if (document.getElementById("cub-widget-script")) {return;}
    var firstScript = document.getElementsByTagName("script")[0];
    var cubJs = document.createElement("script");
    cubJs.id = "cub-widget-script";
    // See notes about widget versioning
    cubJs.src = "//cub-praetorian.netdna-ssl.com/cub-widget.0.9.x.js";
    firstScript.parentNode.insertBefore(cubJs, firstScript);
  }());
</script>
```

### URL configuration

Contents of cub-widget-app tag depends on URL. By default, widget uses
hash URLs, like #login, #registration, etc. Transitions between such URLs
don't trigger page reload, so the rest of the page around widget menu and
widget app elements will remain unchanged. If you'd like to place widget
forms on different pages of your site, please login to Cub Admin and
configure URLs for your site in Widget Configuration section.

### Custom styling

Widget adds some very basic CSS styles for its elements, but in general it
leaves styling to you. Please feel free to define or override any styles for
any elements rendered by widget, just like you do it for any other elements
on the page.
