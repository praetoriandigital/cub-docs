## Lead Webhooks
*Status: DRAFT*

On-demand you can receive webhooks for new leads (contact Praetorian sales team
if you need this).

You should implement https endpoint-URL(**it must be https**) where we will send POST
requests with JSON data. 

To prevent malicious submits from third-parties on your endpoint we can 
add **'Authorization' header with a secret key** provided by you or you can use 
**a random secret string in endpoint-URL**
(for example, https://your-domain.com/lead/webhook/{random-string-here}/).
This is optional, and not required by us.

```bash
# curl-example of a webhook, that you will receive on your endpoint
curl -X POST 'https://your-domain.com/lead/webhook' \
    -H 'Content-Type: application/json' \
    -H 'Authorization: {YOUR_SECRET_KEY_HERE}' \
    -d '[{
  "id": "led_slp3mt0qxvIXYUIK",
  "url": "https://some-site.com/page-with-lead-form/",
  "created": "2018-07-12T16:29:42Z",
  "data": {
    "first_name": "John",
    "middle_name": "",
    "last_name": "Doe",
    "phone": "+123123123123123",
    "position": "Agent",
    "email": "sergunich@gmail.com",
    "state": "Georgia",
    "organization_name": "Org",
    "organization_size": "101-250",
    "organization_country": "United States",
    "organization_state": "Alabama",
    "organization_city": "Montgomery",
    "interested_in": "I would like information for my department"
  }
}]'
```

Webhook payload contains **an array of dicts in JSON format**.
Each dict represent one lead. Usually, **the payload will contain one or more leads, 
but sometimes we can send empty array (zero leads) for debugging purposes**, 
to check that your endpoint is accessible. The default type for values 
in dicts - string, unless otherwise specified. In future to those dicts 
can be added new keys with values, your endpoint should be able 
to handle them (for example, just ignore them).

```js
[{
  "id": "led_slp3mt0qxvIXYUIK",       // unique id of lead
  "url": "https://some-site.com/page-with-lead-form/", // where was submitted
                                                       // this lead
  "created": "2018-07-12T16:29:42Z",  // UTC timestamp, when lead was submitted
  "data": {
    "first_name": "John",           // user name
    "middle_name": "",
    "last_name": "Doe",
    "phone": "+123123123123123",    // user phone
    "position": "Agent",            // user position
    "email": "sergunich@gmail.com", // user email
    "state": "Georgia",             // state provided by user
    "organization_name": "Org",               // organization name
    "organization_size": "101-250",           // organization size
    "organization_country": "United States",  // organization country
    "organization_state": "Alabama",          // organization state
    "organization_city": "Montgomery",        // organization city
    // ... 
    // other fields, let us know what are you interested in
    // ... 
  }
},{
  // ... one payload can contain one or more leads
}]
```

In case of failed requests (non-200 status responses), we will make 
new attempts to send failed webhooks with increasing timeouts. 
After ~24 hours of unsuccessful attempts, webhook will be dropped.


## Checklist
* Your endpoint must use https
* Your endpoint can handle one or more leads(dicts) in payload
* Your endpoint can handle zero leads(dicts) in payload
* Your endpoint can handle new keys and values in leads(dicts) in payload
* Your endpoint can handle retried webhooks that we will send in case of
  non-200 responses
