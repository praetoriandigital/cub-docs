# SSO between PD sites and other systems
Status: DRAFT

## OneSource system and client-side widget
Praetorian Digital (PD, for short) has a wide network of sites including dozens
of LMS sites and media news sites. To avoid users accounts duplication and allow users
to access PD sites with the same account we have **OneSource** system and **OneSource
Widget**. OneSource is our system for users auth and management.
Common users of sites do not access OneSource directly and all communication
with OneSource server is handled with [OneSource Widget](./#cub-widget)
(sometimes called cub-widget, which is a legacy name).

So, when we talk about SSO between PD sites and other systems, we imply that
OneSource would be an essential part of this integration.

## Available SSO options
There can be two separate cases for SSO:
1) When the user wants to **register/login into another system using his PD account**
2) When the user wants to **register/login into PD sites using an account from another system**

For the case 1 (**logging-in into another system using PD account**) we have implemented **OAuth 2.0** in OneSource that other systems can use to get access to users data and authorize the user.  Currently, it is used only to auth PD staff into internal systems but can be adapted for external systems too.

For the case 2 (**logging-in into PD sites using an account from another system**)
we have two options: **SAML 2.0** and **OAuth 2.0**.  Both SAML 2.0
and OAuth 2.0 are implemented in OneSource.


### SAML 2.0
Here is a good overview of SAML 2.0: https://www.okta.com/integrate/documentation/saml/

The two main components of SAML 2.0 are **Identity Provider (IdP)** and **Service Provider(SP)**.
**IdP** allows **SP** to authorize and authenticate a user. The OneSource is an **SP**, and to sign-in users from another system into the PD site, we would need an **IdP** (for example, OKTA or Active Directory).
For SAML 2.0 in OneSource, we have implemented **Just-in-Time Provisioning**, which means that users will be created in OneSource upon login.

For now, we support only **IdP-initiated Login Flow** (when users are redirected from IdP to OneSource).  But we can implement **SP-initiated Login Flow** if needed.

To setup SAML 2.0 SSO we would provide you "SSO service URL" ('Single sign-on URL' in terms of OKTA, or "Relying party SAML 2.0 SSO service URL" in terms of Active Directory) and expect from you to provide us link to your SAML 2.0 IdP 'Metadata configuration' and configure those 'Attribute Statements': 'Email', 'LastName' and 'FirstName'.


### OAuth 2.0
Currently, **OAuth 2.0** is used for login/registration on the PD sites with Facebook or Google accounts.
Using OAuth 2.0 code authorization flow, we acquire the `access_token` and then using Graph API (for Facebook) or OpenID Connect `id_token` (for Google) we pull data about the user.
If you wanted to use OAuth 2.0 for SSO, you would need to implement **OAuth 2.0 Code Authorization flow** (https://oauth.net/2/grant-types/authorization-code/) and **provide us some API to pull from your system the data required for user registration/login**.

Bare minimum data that we need to register new user are `email`(or `username`), `first name` and
`last name`. In some cases it may be required to pull some additional data (for
example, organization membership), so, we would need API from you to pull this
data.


## SCIM for users sync
Using SAML 2.0 and OAuth 2.0 user accounts will be created in OneSource only
when users login/access PD site for the first time. In most cases this is ok.
But sometimes it may be required to have users accounts created in OneSource
before they access PD site (for example, to allow Organization Admin to
assign some courses to users in LMS).
For such cases, we support **SCIM** protocol (http://www.simplecloud.info/#Overview).
Using SCIM user accounts from your system can be synced to OneSource.

Currently, we support sync for this user data: `email`, `username`, `first name`, `last name`, `organization membership`, `is member active?`, `is member admin of the organization?`.


## Use cases and examples
- If you wanted to have links to LMS courses in your system and expect that users will be automatically logged in after they click on the link, then we would need to implement either **SP-initiated Flow for SAML 2.0 with RelayState support** or **OAuth 2.0 integration with your system**.
- If there would be a special link to login/register in LMS somewhere in your system, then we could use already implemented **IdP-initiated Flow for SAML 2.0**.

