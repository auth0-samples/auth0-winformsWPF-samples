# # Auth0 WinformsWPF  
<img src="https://img.shields.io/badge/community-driven-brightgreen.svg"/> <br>

To start playing with our WinformsWPF sample make use of this repo.

This repo is supported and maintained by Community Developers, not Auth0. For more information about different support levels check https://auth0.com/docs/support/matrix .

## Getting started

### Installation

#### Install NuGet

~~~ps
  Install-Package Auth0.WinformsOrWPF
  ~~~

#### Instantiate Auth0Client

~~~cs
  var auth0 = new Auth0Client(
     "{YOUR_AUTH0_DOMAIN}",
     "{YOUR_CLIENT_ID}");
  ~~~

## Usage

### Trigger login (with Widget)

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

  ![](http://puu.sh/3YxF9.png)

Or you can use the connection as a parameter (e.g. here we login with a Windows Azure AD account):

~~~cs
auth0.LoginAsync(this, "auth0waadtests.onmicrosoft.com").ContinueWith(t => .. );
~~~

Or a database connection:

~~~cs
auth0.LoginAsync("my-db-connection", "username", "password").ContinueWith(t => .. );
~~~

### Scope

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

## Contribute

Feel like contributing to this repo? We're glad to hear that! Before you start contributing please visit our [Contributing Guideline](https://github.com/auth0-community/getting-started/blob/master/CONTRIBUTION.md).

Here you can also find the [PR template](https://github.com/auth0-community/auth0-winformsWPF-samples/blob/master/PULL_REQUEST_TEMPLATE.md) to fill once creating a PR. It will automatically appear once you open a pull request.

## Issues Reporting

Spotted a bug or any other kind of issue? We're just humans and we're always waiting for constructive feedback! Check our section on how to [report issues](https://github.com/auth0-community/getting-started/blob/master/CONTRIBUTION.md#issues)!

Here you can also find the [Issue template](https://github.com/auth0-community/auth0-winformsWPF-samples/blob/master/ISSUE_TEMPLATE.md) to fill once opening a new issue. It will automatically appear once you create an issue.

## Repo Community

Feel like PRs and issues are not enough? Want to dive into further discussion about the tool? We created topics for each Auth0 Community repo so that you can join discussion on stack available on our repos. Here it is for this one: [auth0-winformsWPF-samples](https://community.auth0.com/t/auth0-community-oss-auth0-winformswpf-samples/15999)

<a href="https://community.auth0.com/">
<img src="/Assets/join_auth0_community_badge.png"/>
</a>

## License

This project is licensed under the MIT license. See the [LICENSE](https://github.com/auth0-community/auth0-winformsWPF-samples/blob/master/LICENSE) file for more info.

## What is Auth0?

Auth0 helps you to:

* Add authentication with [multiple authentication sources](https://docs.auth0.com/identityproviders), either social like
  * Google
  * Facebook
  * Microsoft
  * Linkedin
  * GitHub
  * Twitter
  * Box
  * Salesforce
  * etc.

  **or** enterprise identity systems like:
  * Windows Azure AD
  * Google Apps
  * Active Directory
  * ADFS
  * Any SAML Identity Provider

* Add authentication through more traditional [username/password databases](https://docs.auth0.com/mysql-connection-tutorial)
* Add support for [linking different user accounts](https://docs.auth0.com/link-accounts) with the same user
* Support for generating signed [JSON Web Tokens](https://docs.auth0.com/jwt) to call your APIs and create user identity flow securely
* Analytics of how, when and where users are logging in
* Pull data from other sources and add it to user profile, through [JavaScript rules](https://docs.auth0.com/rules)

## Create a free Auth0 account

* Go to [Auth0 website](https://auth0.com/signup)
* Hit the **SIGN UP** button in the upper-right corner
