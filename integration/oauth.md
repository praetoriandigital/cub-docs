# Access to the API
Status: DRAFT

## How to get access to the data related to the user
Your application will be able to get access only to the data of users that are
connected to your application.
([more about what means "user connected to application"](/api/applications.md#api-access-and-permissions))

One way to connect the user to your application is to use OAuth and request access
to the desired data using "scopes".

Here is what you should do and how it works:

### 1) Register your application in LID
Contact Lexipol Staff rswank@lexipol.com and ask them to create an Application.  Your contact will then provide you with the Secret Key and UID of your Application and also a list of available scopes for Step #2.

### 2) Start OAuth flow
Redirect the user's browser to this URL:
```
https://id.lexipol.com/oauth/authorize?client_id={app_uid}&redirect_uri={uri}&scope={scopes}&state={state}
```
Where:
- `client_id` - UID of your Application in LID (provided in Step #1).
- `redirect_uri` - where the browser will be redirected with `authorization_code` (see Step #3).
- `scope` - list of scopes (data that you are interested in)

To play with this OAuth flow initiation, you could use https://oauthdebugger.com/

Here is a concrete example:
```
https://id.lexipol.com/oauth/authorize?client_id=app_RsHaqufAay6MsfIQ&redirect_uri=https%3A%2F%2Foauthdebugger.com%2Fdebug&scope=lid.user%3Aread.profile%20kms.policy:read&state=test
```

### 3) Exchange `authorization_code` for token
After being granted access to data for your application, the browser will be
redirected to your `redirect_uri` with `code`. You should exchange this `code`
for a token.

```
$ curl -X POST -d 'grant_type=authorization_code&code={code}&client_id={app_uid}&client_secret={app_secret}&redirect_uri={your_redirect_uri}' https://id.lexipol.com/oauth/access_token
{
"access_token": "{token}",
"token_type": "bearer"
}
```
Here is what a decoded token may look like:
```
{'user': 'usr_vjpyLg7ildvmlOGF',
 'app': 'app_RsHaqufAay6MsfIQ',
 'exp': 1584769011,
 'scope': 'lid.user:read.profile kms.policy:read'}
```

**You may use this token only to access LID API.** For example, to get data about
the user.

### 4) Get user info from LID API
Next, use the LID API to retrieve data for your desired user. For example...
```
$ curl -X GET -H 'Authorization: Bearer {token}' https://id.lexipol.com/v1/users/usr_vjpyLg7ildvmlOGF
{
  "birth_date": "1990-01-04",
  "date_joined": "2017-06-19T10:20:28Z",
  "email": "johnsnow@example.com",
  "first_name": "John",
  "gender": "male",
  "id": "usr_vjpyLg7ildvmlOGF",
  "last_name": "Snow",
  "middle_name": "",
  "object": "user",
  "original_username": "johnsnow@example.com",
  "photo_large": null,
  "photo_small": null,
  "retired": false,
  "username": "johnsnow"
}
```

### 5) Issue new token to access LID API
If the previously issued token has expired, you can request a fresh token by using the following example:
```
$ curl -X POST -d 'grant_type=https%3A%2F%2Fid.lexipol.com%2Foauth%2Fgrant-type%2Ftoken-issue&client_id={app_uid}&client_secret={app_secret}&user={user_uid}' https://id.lexipol.com/oauth/access_token
{
"access_token": "{token}",
"token_type": "bearer"
}
```

### 6) Issue token to access another application API
To access the API of another application (not LID), you must issue a delegated token.
You will be able to do this only if you know UID of this application, and
during OAuth, you have requested scopes that are related to the application that you want to access.

```
$ curl -X POST -d 'grant_type=https%3A%2F%2Fid.lexipol.com%2Foauth%2Fgrant-type%2Ftoken-issue&client_id={app_uid}&client_secret={app_secret}&app={app_uid_of_application_you_want_to_access}&user={user_uid_on_behalf_of_which_you_want_to_access_application}' https://id.lexipol.com/oauth/access_token
{
"access_token": "{token}",
"token_type": "bearer"
}
```
Here is what the decoded token could look like:
```
{'user': 'usr_vjpyLg7ildvmlOGF',
 'app': 'app_ufPJLafQ66W6yX1s',
 'exp': 1584770564,
 'scope': 'lid.user:read.profile kms.policy:read',
 'act': {'app': 'app_RsHaqufAay6MsfIQ'}}
```

