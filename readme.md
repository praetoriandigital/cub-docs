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

* [Users Login and Registration](./registration.md)
* [Newsletter Subscription Form](./newsletter-form.md)
* [Lead Forms](./lead-forms.md)
* [Fields docs](./fields.md)
* [Form events](./form-events.md)

## Debugging

If you want to know what Cub-widget is doing - open browser console
and enter `cub.debug()`. Cub-widget will start logging actions,
api requests/responses and other usefull stuff. Be aware that it
can affect perfomance because internally it uses deepcopy of logged items.
After `cub.debug()` call, debug logging will be active only for browser
window where it was called. To stop debug logging use `cub.debug(false)`.

## Browser compatibility

Tested vs. IE10+, Chrome, Firefox, Safari, Mobile Safari and Android Browser.

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

   <https://cub-praetorian.netdna-ssl.com/cub-widget.0.26.x.js>

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

* 0.26.x Sep 21, 2019
  - Added support for create organization members without email. 
  - CSS fix for buttons. 
  - Support for multiple trusting organization management in the members form. 
  - Fixed the bug with double events from LabeledCombobox.
  - Show discount value on the payment page. 
 
* 0.25.x Sep 12, 2019
  - Introduced 2 step login form with saml sp initiated flow. 
  - Password reset functionality was added for the org admins. 

**Outdated versions:**

* 0.24.x, May 29, 2019
  - **ATTENTION!**
    - cub.policeone.com is now deprecated
    - default apiUrl was changed to 'https://id.lexipol.com/v1'
    - if you explicitly pass apiUrl in cub.start you should change it from
      'https://cub.policeone.com/v1' to 'https://id.lexipol.com/v1'
      (or cross-site SSO will not work)

* 0.23.x, Dec 20, 2018
  - hid Newsletters step from Registration flow if no Mailing Lists to show
  - [field 'checkboxgroup'](./fields.md#user-content-checkboxgroup):
    - 'Check/Uncheck all' button
    - columns layout for checkboxes
  - fixed error when redirect.to function and respectRedirect were used at the same time
  - added the optional zip code field to `OrganizationSubform`
  - added the option to skip the subscribe the site mailing lists when the "RegisterMe" feature is used

* 0.22.x, Dec 13, 2018
  - [field 'position'](./fields.md#user-content-position):
    - label changed to "Title/Position"
  - redirect.to of ['Redirect' feature](./lead-forms.md#redirect-feature) now can be a function

* 0.21.x, Nov 9, 2018
  - [field 'organization'](./fields.md#user-content-organization):
    - collect size and state info for existent orgs (if don't have size and state info)
  - members are now autoactivated on invite
  - improved third-party payments
  - modified the code of the Billing page to accomodate Stripe API changes
  - modified the GenericForm component to require Org Size and Org State fields
    if they are missing in Cub

* 0.20.x, Nov 2, 2018
  - implemented new SearchGrid component for server-side search
  - updated other UIs related to members (groups, members count tabs) to use
    data provided from the server instead of calculated on the client
  - updated caching mechanism with the ability to skip some data models that
    will be marked with a special flag

* 0.19.x, Oct 23, 2018
  - changed registration wokflow (show benefits page before newsletters)

* 0.18.x, December 06, 2017
  - updated react-widgets package to v4.1.1
  - CSS classes namespace changed from '.CubWidgetMyComponent' to '.cub-MyComponent' according SUIT
  - group of CSS classes related to '.cub-UserPhoto' was changed to '.cub-Image'
  - the User Image layout updates:
    - the Upload and Remove Buttons have hints instead of texts
    - the Buttons are moved to the top right corner above the User Image now
  - New LabeledOrganitzationLogo Control for GenericForm
  - New field 'organization_search'
  - [Cross site SingleSO feature](./registration.md#singleso-feature)
  - Google Analytics command queue, e.g. `cub.analytics.ga('send', 'event', 'User Login')`
  - [Generic Forms: new multi-column layout for fieldsets: new 'width' parameter for a fields](./lead-forms.md#client-side-lead-forms)
  - [Generic Forms: new parameter 'legend' for fieldsets](./lead-forms.md#client-side-lead-forms)

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
  - Field 'organization': ability to set initial value
  - ['Register me' feature](./lead-forms.md#register-me-feature)
  - [Generic 'success' message](./lead-forms.md#custom-success-message)
  - ['Redirect' feature](./lead-forms.md#redirect-feature)
  - ['cub.debug()' feature](./readme.md#debugging)
  - New LabeledMultiselect Control for GenericForm based on react-widgets

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

## Troubleshooting

Cub Widget makes its best to be developer-friendly and reports its errors and
warnings into your browser console. If something goes wrong, please check
messages in browser console first. If that doesn't help, or you found a bug,
or you'd like to suggest a feature - please feel free to open an
[Issue on Github](https://github.com/praetoriandigital/cub/issues).
