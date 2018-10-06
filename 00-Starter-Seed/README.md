## Usage

1. Install NuGet

  ~~~ps
  Install-Package Auth0.WinformsOrWPF
  ~~~

2. Instantiate Auth0Client

  ~~~cs
  var auth0 = new Auth0Client(
     "{YOUR_AUTH0_DOMAIN}",
     "{YOUR_CLIENT_ID}");
  ~~~

3. Trigger login (with Widget) 

  ~~~cs
  auth0.LoginAsync(this).ContinueWith(t =>
  {
  /* Use t.Result to do wonderful things, e.g.: 
    - get user email => t.Result.Profile["email"].ToString()
    - get facebook/google/twitter/etc access token => t.Result.Profile["identities"][0]["access_token"]
    - get Windows Azure AD groups => t.Result.Profile["groups"]
    - etc. */ 
  },
  TaskScheduler.FromCurrentSynchronizationContext());
  ~~~

  ![](https://i.imgur.com/N4BaEjy.png)

Or you can use the connection as a parameter (e.g. here we login with a Windows Azure AD account):

~~~cs
auth0.LoginAsync(this, "auth0waadtests.onmicrosoft.com").ContinueWith(t => .. );
~~~

Or a database connection:

~~~cs
auth0.LoginAsync("my-db-connection", "username", "password").ContinueWith(t => .. );
~~~

### Scope

Optionally you can specify the `scope` parameter:

* __scope: "openid"__ _(default)_ - It will return, not only the `access_token`, but also an `id_token` which is a Json Web Token (JWT). The JWT will only contain the user id.
* __scope: "openid email nickname favorite_food"__ - will return claims for `openid` in addition to the `email`, `nickname` and `favorite_food` fields if they are available.
* __scope: "openid profile"__ - If you want the entire user profile to be part of the `id_token` (not recommended). This can cause problems when sending or receiving tokens in URLs (e.g. when using response_type=token) and will likely create an unnecessarily large token. Keep in mind that JWTs are sent on every API request, so it is desirable to keep them as small as possible.

### Delegation Token Request

You can obtain a delegation token specifying the ID of the target client (`targetClientId`) and, optionally, an `IDictionary<string, string>` object (`options`) in order to include custom parameters like scope or id_token:

~~~cs
var targetClientId = "{TARGET_CLIENT_ID}";
var options = new Dictionary<string, string>
{
    { "scope", "openid name email picture" },		// default: openid
    { "id_token", "USER_ID_TOKEN" }		// default: id_token of the authenticated user (auth0.CurrentUser.IdToken)
};

auth0.GetDelegationToken(targetClientId, options)
     .ContinueWith(t =>
        {
            // Call your API using t.Result["id_token"]
        });
~~~

## What is Auth0?

Auth0 helps you to:

* Add authentication with [multiple authentication sources](https://docs.auth0.com/identityproviders), either social like **Google, Facebook, Microsoft Account, LinkedIn, GitHub, Twitter, Box, Salesforce, amont others**, or enterprise identity systems like **Windows Azure AD, Google Apps, Active Directory, ADFS or any SAML Identity Provider**.
* Add authentication through more traditional **[username/password databases](https://docs.auth0.com/mysql-connection-tutorial)**.
* Add support for **[linking different user accounts](https://docs.auth0.com/link-accounts)** with the same user.
* Support for generating signed [Json Web Tokens](https://docs.auth0.com/jwt) to call your APIs and **flow the user identity** securely.
* Analytics of how, when and where users are logging in.
* Pull data from other sources and add it to the user profile, through [JavaScript rules](https://docs.auth0.com/rules).

## Create a free Auth0 Account

1. Go to [Auth0](https://auth0.com) and click Sign Up.
2. Use Google, GitHub or Microsoft Account to login.

## Issue Reporting

If you have found a bug or if you have a feature request, please report them at this repository issues section. Please do not report security vulnerabilities on the public GitHub issue tracker. The [Responsible Disclosure Program](https://auth0.com/whitehat) details the procedure for disclosing security issues.

## Author

[Auth0](auth0.com)

## License

This project is licensed under the MIT license. See the [LICENSE](LICENSE) file for more info.
