---
weight: 30
title: Single sign-on
layout: redirect
---

{{< product-c8y-iot >}} provides single sign-on functionality, that allows a user to login with a single 3rd-party authorization server using the OAuth2 protocol, for example Azure Active Directory. Currently authorization code grant is supported only with access tokens in form of JWT.

> **Info:** This feature is built on top of cookies technology. To be able to use it, you must have cookies enabled in the settings of your browser.

### Prerequisistes

Before switching to the single sign-on option it is mandatory that:

* The authorization server you use supports OAuth2 authorization code grant.
* The access token is issued as JWT and you know what goes into the token content.
* The JWT must consist of a unique user identifier, "iss" (issuer), "aud" (audience) and "exp" (expiration time) fields.
* The {{< product-c8y-iot >}} platform is in version 10.4.6 but preferably higher.
* All microservices are build with Microservice Java SDK 10.4.6 but preferably higher. For custom-built microservices, refer to [General aspects > Security](/microservice-sdk/concept/#security) in the *Microservice SDK guide*.
* For on-premise installations the domain-based tenant resolution is configured properly.

>**Info:** In order to use the single sign-on feature for {{< enterprise-tenant >}}s, the enterprise domain must be set up as redirect URI in the basic configurations. If single sign-on providers have a list of allowed domains, the enterprise domain should be added to that list.


### Configuration settings

To enable the feature, the administrator has to configure a connection with the authorization server. This is done in the Administration application.

Click **Single sign-on** in the **Settings** menu in the navigator.

At the top left, you can choose a template. The chosen option has an effect on the look of the panel. The default template is "Custom" which allows for a very detailed configuration with virtually any authorization server using OAuth2 authorization code grant. Other templates provide simplified views for well known and supported authorization servers. In the next steps there will first be a definition of how to use the "Custom" template followed by a view dedicated to Azure Active directory.

#### Custom template

![Request configuration](/images/users-guide/Administration/admin-sso-1.png)

As the OAuth protocol is based on the execution of HTTP requests and redirects, a generic request configuration is provided.

##### Authorization request and token request

The first part of the **Single sign-on** page consists of the request configuration. Here you can configure the HTTP request address, request parameters, headers and body in case of token and refresh requests. The authorize method is executed as a GET, token and refresh method by POST requests.

>**Info:** Be aware that the body field of each request, after filling placeholders with values, is sent in the request 'as is'. This means it is not encoded by {{< product-c8y-iot >}}. Many authorization servers require values inside the body to be URL-encoded (x-form-urlencoded). This can be achieved by entering already encoded values in a body field.

##### Logout request

Specifying a logout request is optional. It performs [front-channel single logout](https://openid.net/specs/openid-connect-frontchannel-1_0.html). If configured, the user is redirected to the defined authorization server logout URL after logging out from {{< product-c8y-iot >}}.

![OAuth configuration](/images/users-guide/Administration/admin-sso-logout-custom.png)

##### Basic

The **Basic** section of the **Single sign-on** page consists of the following configuration settings:

|Field|Description
|:---|:---|
|Redirect URI|Redirect parameter. Can be used in request definitions as a ${redirectUri} placeholder
|Client ID|OAuth connection client ID. Can be used in request definitions as a ${clientId} placeholder
|Button name|Name displayed on the button on the **Login** page
|Token issuer|OAuth token issuer
|Provider name|Name of the provider
|Audience|Expected aud parameter of JWT
|Visible on Login page|Indicates whether the login option is enabled or not.

##### Access mapping

Each time a user logs in, the content of the access token is verified and is a base for user access to the {{< product-c8y-iot >}} platform. The following section provides the mapping between JWT claims and access to the platform.

 ![OAuth configuration](/images/users-guide/Administration/admin-sso-7.png)

 In the example above, if a user tries to login a decoded JWT claim look like:

```json
{
...
"user": "john.wick",
...
}
```

The user will be granted access to the global role "business" and the default application "cockpit".

If no access mapping matches the user access token, the user will get an "access denied" message when trying to log in. This will also happen if there is no access mapping defined causing all users to be unable to log in using SSO.

New rules can be added by clicking **Add access mapping** at the bottom. An access mapping statement can consist of multiple checks like in the image below. You can add a rule to an existing statement by clicking **and**. Click the Minus button to remove a rule.

New roles are added to the user from every matching access mapping. If one access mapping statement assigns the role "admin" and a second one assigns the role "business" and both meet the defined conditions, then the user will be granted access to the global roles "business" and "admin"."

When using "=" as operator you may use wildcards in the **Value** field. The supported wildcard is asterisk (\*) and it matches zero or more characters. For example, if you enter "cur\*" this matches "cur", "curiosity", "cursor" and anything that starts with "cur". "f\*n" matches "fn", "fission", "falcon", and anything that begins with an "f" and ends with an "n".

In case the asterisk character should be matched literally it has to be escaped by adding a backslash (\\). For example, to match exactly the string "Lorem\*ipsum" the value must be "Lorem\\*ipsum".


 ![OAuth configuration](/images/users-guide/Administration/admin-sso-8.png)

In this case the following claim will match the condition:

 ```json
 {
 ...
 "user": {
    "type": "human"
 },
 "role": [
    "ADMIN"
 ],
 ...
 }
 ```

As you can see, there is an option to verify if a value exists in a list via the "in" operator. Values can also be embedded in other objects. In this case a dot in the key implies looking into an embedded object.

##### User ID configuration

When a user logs in with an access token, the username can be derived from a JWT claim. The claim name can be configured in the **User ID configuration** window.

 ![User ID configuration](/images/users-guide/Administration/admin-sso-3.png)


![OAuth configuration](/images/users-guide/Administration/admin-sso-user-data-mappings.png)

On user login, user data like first name, last name, email and phone number can also be derived from JWT claims. Each field represents the claim name that is used to retrieve the data from JWT. The user data mapping configuration is optional and as admin manager you can only use the required fields. If the configuration is empty or the claim name cannot be found in the JWT token then the values in the user data are set as empty.

Mapping for alias is not available because it is not used in the context of single sign-on login.

##### Signature verification

Each access token is signed by a signing certificate. Currently there are the following options to configure the signing certificates.

1. By specifying the Azure AD certificate discovery address.

 ![Azure AD certificate](/images/users-guide/Administration/admin-sso-4.png)

2. By specifying the ADFS manifest address (for ADFS 3.0).

 ![ADFS certificate](/images/users-guide/Administration/admin-sso-9.png)

3. By providing the public key of a certificate manually to {{< product-c8y-iot >}}. A certificate definition requires an algorithm information, public key value and validity period.

 ![Custom certificate](/images/users-guide/Administration/admin-sso-5.png)

4. By specifying the JWKS (JSON Web Key Set) address.

 ![JWKS certificate](/images/users-guide/Administration/admin-sso-9.png)


 >**Info:** {{< product-c8y-iot >}} only supports certificates with RSA key, either as a ("n", "e") parameters pair or "x5c" certificate chain. Other key types (e.g. Elliptic-curves) are not supported.

#### Placeholders
Inside some fields you can use placeholders that are resolved by {{< product-c8y-iot >}} at runtime. Available placeholders are:

|Placeholder|Description|
|:---|:---|
|clientId|Value of the **Client ID** field
|redirectUri| Value of the **Redirect URI** field
|code|Code returned by the authorization server in response to authorization request
|refreshToken| Refresh token returned by the authorization server after token request

These placeholders can be used in authorization requests, token requests, refresh requests and logout request in the following fields:

URL, body, headers and request parameters

To use a placeholder in a field, put it inside two curly brackets preceded with a dollar sign:
![OAuth configuration](/images/users-guide/Administration/admin-sso-placeholder-standalone.png)

Placeholders can also be used as a part of text:
![OAuth configuration](/images/users-guide/Administration/admin-sso-placeholder-text.png)

Placeholders are not validated for correctness. Any not recognized or misspelled placeholder will be left in text unprocessed.

### Integration with Azure AD

#### Azure AD configuration

The integration was successfully verified against Azure AD. The configuration steps are available in [https://docs.microsoft.com/en-us/azure/active-directory/develop/v1-protocols-oauth-code](https://docs.microsoft.com/en-us/azure/active-directory/develop/v1-protocols-oauth-code).

When configuring your Azure AD, use your full domain address as redirect URI. For the purpose of this document we assume that it is "http://documentation.{{< domain-c8y >}}/tenant/oauth". The redirect URI must be set for a web application and not for a single-page application. There are no additional steps on Azure AD required.

#### Cumulocity IoT configuration

When the "Azure AD" template is selected the configuration panel will look similar to the following:

 ![OAuth configuration](/images/users-guide/Administration/admin-sso-aad-basic.png)
 ![OAuth configuration](/images/users-guide/Administration/admin-sso-aad-basic-1.png)

|Field|Description|
|:---|:---|
|Azure AD Address| Address of your Azure AD tenant
|Tenant| Azure AD tenant name
|Application ID| Application ID
|Redirect URI| Address of your {{< product-c8y-iot >}} tenant followed by /tenant/oauth
|Client secret| Azure AD client secret if applicable
|Button name| Button name
|Token issuer| Token issuer value in form of a HTTP address

Optionally single logout can be configured:

 ![OAuth configuration](/images/users-guide/Administration/admin-sso-logout-azure.png)

|Field|Description|
|:---|:---|
|Redirect after logout| Activates single logout by redirecting the user, after logout, to the authorization server logout endpoint
|Redirect URL| Address to redirect the user to after successful logout from the authorization server

The second part of the panel is the same as for the "Custom" template, where access mapping, user data mapping, user ID field selection and signature verification address are provided.

 ![OAuth configuration](/images/users-guide/Administration/admin-sso-aad-2.png)


##### Troubleshooting

It can be particularly helpful to inspect the content of the authorization token sent to the platform as some of its fields contain the information required for the correct configuration described above.

In the Administration application, navigate to **Accounts** > **Audit logs**. Here you can filter by the category "Single sign-on" and look for entries "Json web token claims".

The contexts of the token will be presented in JSON format.

![Audit token content](/images/users-guide/Administration/admin-sso-audit-token.png)
