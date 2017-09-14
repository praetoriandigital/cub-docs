<img src="cub-logo.png">

# Cub™ Widget

Cub Widget is a lightweight JavaScript application which can be installed
on any site. Key features:

* Users Login and Registration for your site, connected to 1.8m users
  database of Praetorian Digital sites network;
* Newsletter Subscription Forms for dozens of newsletters for first
  responders;
* Flexible Lead Forms with centralized processing and data export features.

The widget is implemented in pure JavaScript, and talks to a central database
directly from browser via Cub API. Basic widget integration requires no server
programming, so you can add it literaly on any site with minimal efforts.
If you need more sophisticated integration options - we have them, too.

## How it works

Widget is loaded on a page with a piece of JavaScript, like this (example):

```html
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
    cubJs.src = "//cub-praetorian.netdna-ssl.com/cub-widget.js";
    firstScript.parentNode.insertBefore(cubJs, firstScript);
  }());
</script>
```

When loaded, widget adds an interaction to the web page by rendering its
components in target DOM elements which you define. Widget takes configuration
parameters via ``cub.start()`` method. Required parameter for all actions is
``apiKey``. Before you can use the widget, you have to register your
application in Cub Admin and get an API key.

Please follow these links for detailed usage instructions:

* [Users Login and Registration](https://github.com/praetoriandigital/cub-docs/blob/master/registration.md)
* [Newsletter Subscription Form](https://github.com/praetoriandigital/cub-docs/blob/master/newsletter-form.md)
* [Lead Forms](https://github.com/praetoriandigital/cub-docs/blob/master/lead-forms.md)
* [Fields docs](./fields.md)

## Browser compatibility

Tested vs. IE9+, Chrome, Firefox, Safari, Mobile Safari and Android Browser.

## Widget versioning

Widget versions follow [Semantic Versioning](http://semver.org). You can
check current version in browser console as ``cub.version``. Changes in public
widget API or documented behavior result in major or minor version change,
and bugfixes or internal widget improvements come as patch versions.

Widget script is distributed with MaxCDN using ``cub-praetorian.netdna-ssl.com``
domain. It supports both HTTP and HTTPS. It is strongly recommended that you
always use this distribution when including widget on your site, since it comes
with automatic updates and bugfixes. Two options of widget CDN distribution are
available:

1. Always latest version - recommended if you use Lead Forms feature only:

   <https://cub-praetorian.netdna-ssl.com/cub-widget.js>

2. Fixed major and minor version number, always latest patch version -
   recommended if you use Users Login and Registration and applied custom
   styling to it:

   <https://cub-praetorian.netdna-ssl.com/cub-widget.0.17.x.js>

Second option allows you to freeze any updates to widget HTML markup, its API
and documented behaviour, however you will still receive bugfixes as patch
versions until major or minor version changes. Please note that we ship
bugfixes for latest widget version only, so if you're using fixed widget
version you should update it yourself as new versions become available.

## Please use latest widget version

We strongly encourage you to use latest widget version and upgrade
to newer version as soon as it becomes available. Technical  support and
bugfixes are provided for latest version only. Versions marked below as
'outdated' are not guaranteed to work at all.

## Changes

* 0.17.x, July 19, 2017
  - Menu upgrade: added User Photo into User Menu.
  - Prevent double submit for cub generic forms. 
  - hideForLogged changed to true by default for "register_me" checkbox. 
  - Reload user data after lead form submission. 
  - New field 'organization': for finding or creacting new organization
  - New config option 'siteRaven': pass here your Raven instace to collect
    additional breadcrumbs and extra context related to the Cub widget
  - GenericForm 'action' option: if set to absolute url with same domain as
    current - xhr(instead xdm) transport will be used. It allows to submit data
    by GenericForm to your domain (xdm has crossdomain submit problems)
  - Field 'state': new option 'country' - states of which country to suggest
  - Field 'checkboxgroup': added 'required' validation
    

* 0.16.x, June 24, 2017
  - 'Register me' checkbox added to generic forms.  
  - 'Add member' from organization management now points to /members/ endpoint and
    creates user with member if it doesn't exist (instead of creating Invitation).
  - 'Resend' and 'Revoke' buttons now both work for newly created users and for old-style
    invitations.
  - 'Invited members' tab shows both old invitations and newly created users.
  - Fixed: postal_code was not saving on organization creation on Profile page
    (My Expirience section)

* 0.15.x, January 14, 2017
  - introducing 'Keep me signed in' checkbox on Login form
  - introducing one-click payments for Orders with credit cards which we
    already have on file
  - ACH is now supported as a payment method for Orders
  - new cross-domain API transport: instead of JSONP we now use iframe +
    postMessage. Benefits - it supports large POST requests, better
    connection errors and timeouts handling.
  - Fixed missed required validation for State and Position fields.
  - prevented re-submission for user forms
  - added 'combobox' field type for custom lead forms
  - initial support for user authentication with social network accounts

* 0.14.x, September 27, 2016
  - introducing Notifications for My Profile page
  - backwards-incompatible changes in Verification Requests API
  - introducing Paid Plans for Sites: Sign up for a Plan workflow,
    Billing page, support for Plans for Verified Users
  - introducing Order Payment page (for one-time purchases)
  - 'Register' menu item on Invitation page now points to the same page
  - Cub API wrapper exposed to public - you can now use ``cub.api`` in
    browser
  - updated ToS (now includes Paid Plan Subscriptions)
  - updated Verification Requests - added Notes
  - widget Forms now support absolute API URLs in 'action' parameter,
    so they can be used with external APIs, not only Cub API

**Outdated versions:**

* 0.13.x, August 5, 2016
  - major HTML markup and styles refactoring
  - Forms rendering moved to client side, new Forms API introduced
  - Verification Requests introduced
  - dropped support for IE8
* 0.12.x, April 24, 2016
  - added Purchasing Roles to My Profile
  - introducing Newsletter Subscription forms
  - upgrades to registration step 2: renamed tabs, added optional Vendor tab
* 0.11.x, April 8, 2016
  - Organizations search major refactoring and UI improvements
  - added Employees dropdown to Organization Add Form
  - added Retired option to 2nd Registration step and My Profile
* 0.10.x, March 2, 2016
  - 3-steps registration
  - registration no longer requires to choose username (it is autogenerated)
  - mailing list subscriptions removed from 1st registration step
  - mailing list descriptions added onto Newsletters page
  - improved Organizations search, now with filter by tags
  - Experience block refactored: organizations and positions ordered by date, Delete button for position now appears in Edit mode only
  - Forgot Password page now has option for users who forgot their emails
  - minor text and layout changes and bugfixes
* 0.9.x, November 2015
  - My Profile upgrade: added users photo upload, added Experience
    section;
  - change in registration flow: after registration users are now by default
    redirected to My Profile
  - newsletter subscriptions during registration are now implicit
  - Lead Forms feature added
* 0.8.x - introducing Mailing List Subscriptions
* 0.7.x - load site configuration from API
* 0.6.x - introducing localStorage cache

## Troubleshoouting

Cub Widget makes its best to be developer-friendly and reports its errors and
warnings into your browser console. If something goes wrong, please check
messages in browser console first. If that doesn't help, or you found a bug,
or you'd like to suggest a feature - please feel free to open an
[Issue on Github](https://github.com/praetoriandigital/cub/issues).
