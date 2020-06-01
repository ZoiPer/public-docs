# **Title**

## **Platform**: **platform**

## **Version**: **version**

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

Provisioning is an easy way to remotely obtain the *configuration data* for the phone from a HTTP(S) server. We are going to refer this server as a *provisioning server*.

### What is configuration data

The *configuration data* is used to setup the phone i.e. what accounts or other specific settings it uses. For more info about the format and the content of the *configuration data* please refer to the *PUT PROPER DOCUMENTATION REFERENCE HERE*.

### Who needs provisioning

Provisioning is particularly useful for VoIP service providers and call centers.

### Benefits of provisioning

* Relieve your users from the burden of having to configure various phone settings, like accounts, codecs, etc.
* Load-balance (distribute the load of) your VoIP servers easily, without the necessity for manually updating the locally-stored server credentials.
* Use the credentials for your website instead of the username and password for the VoIP server (i.e. it is possible for them to be separate ones).
* Update the "configuration data" of your users in accordance with your specific requirements without involving them into any action on their behalf.  This means that users might not even realize that changes in the configuration have taken place.

### Branding and provisioning

## How does provisioning work

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
