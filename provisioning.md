# **Zoiper for Android**

## **Platform**: **Android**

## **Version**: **1.14.3**

## Contents

<!-- TOC -->

* [Contents](#contents)
  * [What is provisioning](#what-is-provisioning)
  * [What is configuration data](what-is-configuration-data)
  * [Who needs provisioning](#who-needs-provisioning)
  * [Benefits of provisioning](#benefits-of-provisioning)
  * [Branding and provisioning](#branding-and-provisioning)
* [How does provisioning work?](#how-does-provisioning-work)
* [Error response from the provisioning server](#error-response-from-the-provisioning-server)
* [Example contents of the error response](#example-contents-of-the-error-response)

<!-- /TOC -->

### What is provisioning

Provisioning is an easy way to remotely obtain the *configuration data* for the phone from a HTTP(S) server. We are going to refer this server as a *provisioning server*. Provisioning is only applicable to branded Zoiper Android[Branding and provisioning](#branding-and-provisioning).

### What is configuration data

The *configuration data* is used to setup the phone i.e. what accounts or other specific settings it uses. For more info about the format and the content of the *configuration data* please refer to the [Zoiper for Android configuration data](configuration-data.md).

### Who needs provisioning

Provisioning is particularly useful for VoIP service providers and call centers.

### Benefits of provisioning

* Relieve your users from the burden of having to configure various phone settings, like accounts, codecs, etc.
* Load-balance (distribute the load of) your VoIP servers easily, without the necessity for manually updating the locally-stored server credentials.
* Use the credentials for your website instead of the username and password for the VoIP server (i.e. it is possible for them to be separate ones).
* Update the "configuration data" of your users in accordance with your specific requirements without involving them into any action on their behalf.  This means that users might not even realize that changes in the configuration have taken place.

### Branding and provisioning

Provisioning can be used for branded versions of the application to supply *configuration data* to it. In such cases the customer should provide a valid URL and credentials for authorization.

## How does provisioning work

### Initial provisioning

A predefined URL provided by the branding customer is set in the application when it is created. The URL is in the form of:

```
https://example.com/script?user=[username]&amp;password=[password]&amp;version=[version]
```

Query parameters name is irrelevant. The query parameters values in the URL will be replaced as follow:

[username] - Username supplied by the user at the welcome(login) screen.

[password] - Password supplied by the user at the welcome(login) screen.

[version] - A backwards compatibility parameter with a value equal to the provisioning version. The value sent will be 1.14. Server should be able to handle requests with and without it for forward compatibility as the parameter can be removed in future versions.

After replacing each of the URL query parameters values the application will initiate an HTTPS GET request. If the provisioning server has valid configuration for the user identified with the *username* and *password* values it must return valid [Configuration data](#configuration-data.md). Otherwise the server must return a valid [Error response](#error-response-from-the-provisioning-server).

### Reprovisioning

After successful provisioning the *username* and *password* will be saved. On every new start the stored *username* and *password* will be used again automatically for reprovisioning to allow changes to the *configuration data*. A new start is considered application start after being explicitly stopped with "Exit".

In case of an HTTP request failure for re-provisioning due to *client error* or *server error* [HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes) existing configuration will be used.

## Error response from the provisioning server

If the *provisioning server* is unable to provide the remote *configuration data* for some reason, it would respond with an XML-formatted error message with the following structure:

* `error`: the root element of the document.  It should contain the error message, which is retrieved and displayed to the user by Zoiper.
  * `code`: attribute that contains a predefined integer code of the error. The possible error codes are as follows:
    * `1`: General error. If no other error suits the occurred error this one must be used with free text that is going to be shown to the end-user.

## Example contents of the error response

```xml
<?xml version="1.0"?>
<error code="1">Provisioning failed. Some error occurred.</error>
```
