# Webhooks
Status: DRAFT

## Synchronization
The API allows two systems to work together. But it doesn’t solve the problem 
of keeping data in sync. To make the synchronization process smoother, 
we have webhooks: 
* All core models support webhooks that are called for
  any object creation/update/deletion. 
* Webhooks are delivered to the endpoints specified
  in the Application settings. 
* Webhooks contain a POST body with json data in the same format 
  as an API response for this type of object.
* Lexipol ID takes care of delivery. It will keep sending webhooks until 
  a response 200 is recieved from the endpoint. It gives up only 
  after 29 retries, with more time between each retry. The overall execution time 
  will not exceed 25 hours.

## Webhooks vs. API
You have Lexipol ID API to pull info about the User when you need it. 
But Lexipol ID Widget has a Profile UI where users can edit their info. What if the user changes his email in profile? 
How will you know about such updates?

When something changes in the User model (or any other Model in which 
you are interested), Lexipol ID sends the latest data for the model 
that was updated to the endpoint that you provided us (and implemented 
on your side). But using only webhooks is not reliable. They should be used 
together with the API, because:
* there can be a race condition between webhooks when a webhook may contain 
  outdated data(for example, on retries after failed requests);
* some webhooks can be lost, and even the most reliable systems sometimes fail;
* webhooks can be sent unintentionally from stage environments (because 
  of misconfigurations)

So, webhooks should be considered as signals to pull the latest data 
through API. Using this most recent data pulled through API, 
you can update or create your local data model.


## Where and when webhooks are sent
### Where
Webhooks for a particular model instance are sent to all **enabled
WebhookEndpoints** of all Applications **connected to this model instance**.
[Here is a description of rules that determine if something is connected 
to the Application.](./applications.md#data-connected-to-the-application)


### When 
A webhook for a model instance is sent when this instance is **created, deleted, 
or updated**. A webhook after an update is sent **only if this update 
introduces changes to the model instance**. The model instance can be changed 
through API or LID Admin.

A webhook for a model instance is sent only **if this concrete model instance 
has changed, not related data**. For example, if User is added 
to the Organization (new Member created), then a webhook will be sent 
only for this new Member, but not for the User or Organization.
