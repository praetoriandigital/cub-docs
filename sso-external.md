# SSO between PD sites and other systems
Status: DRAFT

## OneSource system and client-side widget
Praetorian Digital (PD, for short) has a wide network of sites, including dozens
of LMS sites and media news sites. To avoid user account duplications and to allow users
to access PD sites with the same account, we use the **OneSource** system and the **OneSource
Widget**. OneSource is our system for users auth and management.
Common users of sites do not access OneSource directly and all communication
with the OneSource server is handled with [OneSource Widget](./#cub-widget)
(sometimes called by its legacy name, cub-widget).

So, when we talk about SSO between PD sites and other systems, we imply that
OneSource will be an essential part of this integration.

## Available SSO options
There are two main cases for which a user would want to use SSO:
1) For when the user wants to **register/log in to another system using his PD account**
2) For when the user wants to **register/log in to PD sites using an account from another system**

For  case 1 (**logging in to another system using PD account**), we have implemented **OAuth 2.0** in OneSource so that other systems can use it to get access to user data and authorize users.  Currently, it is used only to auth PD staff into internal systems, but it can be adapted for external systems too.

For case 2 (**logging in to PD sites using an account from another system**)
we have two options: **SAML 2.0** and **OAuth 2.0**.  Both SAML 2.0
and OAuth 2.0 are implemented in OneSource.


### SAML 2.0
Here is a good overview of SAML 2.0: https://www.okta.com/integrate/documentation/saml/

The two main components of SAML 2.0 are the **Identity Provider (IdP)** and the **Service Provider(SP)**.
An **IdP** allows an **SP** to authorize and authenticate a user.  OneSource is an **SP**, and to be able to sign-in users from another system into the PD site, we would need an **IdP** (for example, OKTA or Active Directory).
For SAML 2.0 in OneSource, we have implemented **Just-in-Time Provisioning**, which means that users will be created in OneSource upon login.

For now, we support only **IdP-initiated Login Flow** (where users are redirected from IdP to OneSource).  But we can implement **SP-initiated Login Flow** if needed.

To setup SAML 2.0 SSO, we will provide you with an "SSO service URL" ('Single sign-on URL' in terms of OKTA, or "Relying party SAML 2.0 SSO service URL" in terms of Active Directory) and expect that you will  provide us with a link to your SAML 2.0 IdP 'Metadata configuration' and configure these 'Attribute Statements': 'Email', 'LastName' and 'FirstName'.


### OAuth 2.0
Currently, **OAuth 2.0** is used for login/registration on PD sites with Facebook or Google accounts.
Using OAuth 2.0 code authorization flow, we acquire the `access_token` and then we pull data about the user using Graph API (for Facebook) or OpenID Connect `id_token` (for Google).
If you want to use OAuth 2.0 for SSO, you will need need to implement **OAuth 2.0 Code Authorization flow** (https://oauth.net/2/grant-types/authorization-code/) and **provide us with some API to pull the data from your system for user registration/login**.

The bare minimum data that we need to register a new user is `email`(or `username`), `first name` and
`last name`. In some cases it may be necessary to pull some additional data (for
example, organization membership), so we would need an API from you to pull this
data.


## SCIM for users sync
Using SAML 2.0 and OAuth 2.0, user accounts will be created in OneSource only
when users log in to or access the PD site for the first time. In most cases, this is ok.
But a user account needs to be created in OneSource before they access the PD site (for example, to allow an Organization Admin to
assign some courses to users in LMS).
For such cases, we support **SCIM** protocol (http://www.simplecloud.info/#Overview).
Using SCIM, user accounts from your system can be synced to OneSource.

Currently, we support sync for the following user data: `email`, `username`, `first name`, `last name`, `organization membership`, `is member active?`, and `is member admin of the organization?`.


## Use cases and examples
- If you want your system to have links to LMS courses and expect that users will be automatically logged in after they click on a link, then we will need to implement either **SP-initiated Flow for SAML 2.0 with RelayState support** or **OAuth 2.0 integration with your system**.
- If there is a special link to login/register in LMS somewhere in your system, then we could use the already implemented **IdP-initiated Flow for SAML 2.0**.

