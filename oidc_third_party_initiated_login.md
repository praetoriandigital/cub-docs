# OpenID connect third party initiated login. 
Status: DRAFT

## LID, connected sites

LID is a  solution for users' auth and management. You can connect any application to LID, and it supports different types of identity providers.

Connected sites - are sites that use LID for auth and user management - for example, the LMS websites.

## OpenID Connect third party initiated login. 

The flow is described here https://openid.net/specs/openid-connect-core-1_0.html#ThirdPartyInitiatedLogin

How to configure it? 

1) Setup generic OpenID auth on the site you need. 
2) Ask for your LID Identity Provider ID. For example oap_dAxR12fJw7X6ObcK
3) The endpoint URL will be https://id.lexipol.com/sso/oidc/XXXX/ where XXXX is the LID Identity Provider ID


##Endpoint params:
1) iss, required. 
2) login_hint, optional. 
3) target_link_uri, optional. But could be required by specific LID configuration for security reasons. 
If target_link_uri passed, LID would check that it related to the site where OpenID Connection was configured. 


## Example of constructed URL
https://id.lexipol.com/sso/oidc/oap_fCzY06fJw7X6ObcK/?iss=https%3A%2F%2Fissuer.com&target_link_uri=https%3A%2F%2Folt.policeoneacademy.com%2Fcourses%2F

More details could be found in the specifications https://openid.net/specs/openid-connect-core-1_0.html#ThirdPartyInitiatedLogin
