# Applications
Status: DRAFT

## Applications and Secret Keys
When some other system or site wants to integrate with Lexipol ID and 
get access to API, we will create new Application in Lexipol ID. 
Every Application has a Public Key and Secret Key. 
A Secret Key is something that must be known only by the Lexipol ID server and 
3rd party server because Secret Key gives access to all data/models stored in
Lexipol ID connected to Application. So, if someone knows the Secret Key 
of Application, he can access and modify all data/models connected 
to this Application. 

Secret Keys used not only for API access, but they also used for jwt-token 
signatures. When Lexipol ID issues jwt-token for the User 
(that will be set as Cookie), it will be signed using Secret Key of 
Application where user logins/registers. You can verify jwt-token using 
the Secret Key created for your Application.

## API access and permissions
TL;DR: Permissions of the Application are defined by combined permissions 
of all the Users connected to this Application.

To use LID API/Widget, you need one of:
* a valid user token (for LID API access on behalf of this User)
* a secret key (for LID API access on behalf of Application)
* a public key (for LID Widget)

You have to create a new entry in the Application container in LID 
admin to generate the keys.
A user token can be acquired by Application on user login 
(through LID Widget or LID API) or using a reissue-token API call 
(if an Application got user token issued for another Application during SSO)

If User is connected to Application, this Application will have read/write 
access to User's data and the same set of permissions as this User. 
For example, if User is org-admin, then Application will have permissions 
to edit Organization data and manage members of this Organization.

If a user token is used instead of a secret key for API access, 
you will have the same set of permissions as this User.  

## Data connected to the Application
Application connection to certain model instance determines if Application 
will have read/write access to this instance and whether it will receive
webhooks for this instance 
([more about when and where webhooks are sent](./webhooks.md#where-and-when-webhooks-are-sent))


### User
User can be connected to the Application:
- if a user was **registered through API with Application's secret key**
- if User **logs in on the Site connected to the Application**
- during **UserImport through LID Admin**

### Organization
The Organization is connected to Application **if at least one of the Users 
connected to the Application is an active member of this Organization**.
If the Organization is connected to the Application, it **doesn't mean that all
Members of this Organization are connected to this Application**.

### Member
The Member is connected to the Application **if related User is connected** 
to this Application.

### MemberPosition
The MemberPosition is connected to the Application **if related Member 
is connected** to this Application.

### Group
The Group is connected to the Application **if related Organization 
is connected** to this Application.

### GroupMember
The GroupMember is connected to the Application **if related Group 
is connected** to this Application.

### OrganizationTrust
The OrganizationTrust is connected to the Application **if trusted Organization
or trusting Organization is connected** to this Application.

### Lead
The Lead is connected to the Application **if it was submitted from the Site
connected to this Application** or **the related LeadForm has this Application
in "Extra Connected Apps"**.

### Product
The Product is connected to all active Applications.

### Plan
The Plan is connected to all active Applications.

### ServiceSubscription
The ServiceSubscription is connected to the Application **if related Customer 
(User or Organization) is connected** to this Application.

### Order
The Order is connected to the Application **if related Customer (User
or Organization) is connected** to this Application.

### OrderItem
The OrderItem is connected to the Application **if related Order is connected** 
to this Application.

### SKU
The SKU is connected to the Application **if related Product is connected** 
to this Application.

### SitePlan
The SitePlan is connected to the Application **if related Plan is connected** 
to this Application.

### Charge
The Charge is connected to the Application **if related Customer (User
or Organization) is connected** to this Application.

### Subscription (to MailingList)
The Subscription is connected to the Application **if related User 
is connected** to this Application.

### MailingList
The MailingList is connected to the Application **if one fo the related Sites 
is connected** to this Application.
