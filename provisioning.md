# **Zoiper provisioning documentation**

## **Platform**: **Zoiper5 Desktop - Windows, macOS, Linux**

## **Version**: **1.16.1**

## Contents

<!-- TOC -->

* [Contents](#contents)
* [Overview](#overview)
  * [What is provisioning](#what-is-provisioning)
  * [What is configuration data](what-is-configuration-data)
  * [Who needs provisioning](#who-needs-provisioning)
  * [In which versions of the phone is provisioning available](#in-which-versions-of-the-phone-is-provisioning-available)
  * [Benefits of provisioning](#benefits-of-provisioning)
  * [Branding and provisioning](#branding-and-provisioning)
* [How does provisioning work?](#how-does-provisioning-work)
* [Provisioning configuration data file](#provisioning-configuration-data-file)
  * [Structure of the provisioning configuration data file](#structure-of-the-provisioning-configuration-data-file)
  * [The `options` section](#the-options-section)
* [Error response from the provisioning server](#error-response-from-the-provisioning-server)
* [Example contents of the provisioning configuration file](#example-contents-of-the-provisioning-configuration-file)
* [Example contents of the error response](#example-contents-of-the-error-response)

<!-- /TOC -->

## Overview

This document shows how to create, use and setup the *provisioning configuration data file* to be able to remotely obtain *configuration data* for the phone.

### What is provisioning

Provisioning is an easy way to remotely obtain the *configuration data* for the phone from a HTTP(S) server. We are going to refer this server as a *provisioning server*. The provisioning data (settings) i.e. the URL for the *provisioning server*, etc are taken form the *provisioning configuration data file* which is described below.

### What is configuration data

The *configuration data* is used to setup the phone i.e. what accounts or other specific settings it uses. For more info about the format and the content of the *configuration data* please refer to the *Zoiper configuration data documentation*.

Please do not confuse the *configuration data* file with the *provisioning configuration data file*.  Their contents is different (i.e. they have entirely different structures, and have no options in common): the first one contains the local *configuration data* of the phone, and the second one is described here and contains the options which determine how provisioning should be performed.

### Who needs provisioning

Provisioning is particularly useful for VoIP service providers and call centers.

### In which versions of the phone is provisioning available

Provisioning is only available in the **Premium** versions of the phone.

### Benefits of provisioning

* Relieve your users from the burden of having to configure various phone settings, like accounts, codecs, etc.
* Load-balance (distribute the load of) your VoIP servers easily, without the necessity for manually updating the locally-stored server credentials.
* Use the credentials for your website instead of the username and password for the VoIP server (i.e. it is possible for them to be separate ones).
* Update the "configuration data" of your users in accordance with your specific requirements without involving them into any action on their behalf.  This means that users might not even realize that changes in the configuration have taken place.

### Branding and provisioning

When a branding of the phone that requires provisioning of the *configuration data* is requested the customer should provide *provisioning configuration data file* to be packaged along with the branded phone.

## How does provisioning work

Provisioning is controlled by a special *provisioning configuration data file*. This file contains settings like the location of the provisioned (i.e. remote) *configuration data*, authentication method, etc.

Provisioning takes place when the phone gets started.  If the provisioning configuration file exists and is valid (i.e. is an XML document which conforms to the structure described below), the phone will download and apply the configuration from the remote *configuration data* using the parameters specified in the *provisioning configuration data file*.

When provisioning begins, the phone first loads the data stored in the local *configuration data* file (located in the `%APPDATA%\Zoiper5`) and then overlays atop it the remote *configuration data* (which gets downloaded from the *provisioning server* for that purpose). This is accomplished by obtaining the *configuration data* from the *provisioning server* using the URLs specified in the *provisioning configuration data file* and authentication credentials provided by the user.  The *provisioning server* generates and returns the response in the form of a *configuration data* XML document (please consult the *Zoiper configuration data documentation* for more information about the structure of that XML document).

Basically, this means that the values of the options from the local *configuration data* get replaced with their respective values in the remote *configuration data*, but only for values which are present in the remote *configuration data* (the values of the other ones do not get modified at all).  This scheme allows for parts of the configuration to be defined only locally for each installation of the phone (like, for example, the audio options), and for others to be provisioned for every machine (like, for example, the user account options).

If an `accounts` section is present in the remote *configuration data* and the `delete_accounts_beforehand` option is enabled, this means that only the remote accounts will be used and the local ones will get removed from the local configuration.  The same behavior is applied to the contact services when the `delete_contact_services_beforehand` option is enabled.

## Provisioning configuration data file

The *provisioning configuration data file* is named `provision.xml` and is an XML document that uses the *UTF-8* encoding. It should be located in one of the following locations (they get checked for a file named `provision.xml` in the described order):

1. The current working directory of the phone process (i.e. the **Start In** field in shortcut properties on Windows).
2. The path to the phone executable file:
   * On Windows, the executable is located in the phone installation directory (i.e. it is a single one).  By default the phone gets installed in `C:\Program Files (x86)\Zoiper5`.
   * On Linux, the executable is usually located in one of the directories designated for executable files (e.g. `/usr/bin`, `/usr/local/bin`, `/bin`, etc).
3. The phone *application data directory* for the current user.  By default it has one of the following paths, depending on the platform:
   * On Windows, the path to the directory is `%APPDATA%\Zoiper5`.  This location gets interpreted in a different way for different Windows versions (here `USER` is a placeholder for the name of the current user):
     * on Windows versions before *Vista* it gets translated to `C:\Documents and Settings\USER\Application Data\Zoiper5`;
     * on *Windows Vista* and later it gets translated to  `C:\Users\USER\AppData\Roaming\Zoiper5`.
   * On Linux, the path to the directory is `~/.Zoiper5`.  It usually gets translated to `/home/USER/.Zoiper5`, where `USER` is a placeholder for the name of the current user.

This file is not included in the standard distribution of the phone installation (i.e. it has to be created manually).

### Structure of the provisioning configuration data file

The provisioning configuration file is an XML document with the following structure:

* `provision`: the main configuration section (it corresponds to the root element of the document).
  * `base_urls`: a section with several sub-sections defining the base URLs used for provisioning, where each URL has its own `base_url` section.
    * `base_url`: this option defines a URL of a *provisioning server* used for provisioning.
      * If `url` authentication is used for the `authentication_type`, the user will be able to specify some parameter values for substitution in the URL.  The currently available parameters are `[user]` and `[pass]`.  They get substituted with one of the following things depending on the value of the `user_input_authentication` option:
        * the username and the password entered by the user, if `user_input_authentication` is enabled;
        * the  `user` and `password` options, if `user_input_authentication` is disabled.
      * If provisioning using an URL fails, the phone will try to get provisioned using the next one.  If it does not succeed with any of them, it will display an error message.
    * Type: ***string***.
    * Default value: *nothing* (i.e. an empty string).
  * `options`: a section that contains the options which control the behavior of the provisioning process.

### The `options` section

* `use_authentication`: this option determines whether authentication credentials will be provided to the *provisioning server* by the phone (i.e. whether authentication will be used at all).
  * Type: ***boolean***.
  * Possible values: `false`, `true`.
  * Default value: `false`.

* `user_input_authentication`: this option determines whether the phone should ask the user to enter his authentication credentials (i.e. username and password).  If the value of this option is `false`, the username and password from the `user` and `password` options are used for authentication.
  * Type: ***boolean***.
  * Possible values: `false`, `true`.
  * Default value: `false`.

* `authentication_type`: this option determines the way the phone supplies the authentication credentials to the *provisioning server* (i.e. how it gets *authenticated* to the *provisioning server*).
  * Type: ***text enumeration***.
  * Possible values: `basic`, `url`:
    * `basic`: basic access authentication is used for HTTP (for more information, see the relevant [Wikipedia article](https://en.wikipedia.org/wiki/Basic_access_authentication)).
    * `url`: the authentication credentials are passed as URL parameters.  (See the `base_url` option for more information.)
  * Default value: `url`.

* `user`: this option defines a preset username which is used if `user_input_authentication` is `false`.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `password`: this option defines a preset password which is used if `user_input_authentication` is `false`.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `password_format`: this option determines the format in which the password is stored.  It can either be MD5-encoded or a plaintext one.
  * Type: ***enumeration***.
  * Possible values: `0`, `1`:
    * `0`: means that the password will be *MD5*-encoded (i.e. the value of the `user` option will be a *MD5* hash of the password).
    * `1`: means that the password will be stored in plaintext (i.e. the value of the `user` option will be the actual unencrypted password).
  * Default value: `0`.

* `delete_accounts_beforehand`: this option determines whether the accounts present in the local configuration should get deleted before the provisioning happens (so that only the accounts from the provisioned configuration would be used).
  * Type: ***boolean***.
  * Possible values: `false`, `true`.
  * Default value: `true`.

* `delete_contact_services_beforehand`: this option determines whether the contact services present in the local configuration should get deleted before the provisioning happens (so that only the contact services from the provisioned configuration would be used).
  * Type: ***boolean***.
  * Possible values: `false`, `true`.
  * Default value: `false`.

## Error response from the provisioning server

If the *provisioning server* is unable to provide the remote *configuration data* for some reason, it would respond with an XML-formatted error message with the following structure:

* `error`: the root element of the document.  It should contain the error message, which is retrieved and displayed to the user by Zoiper.
  * `code`: attribute that contains a predefined integer code of the error. The possible error codes are as follows:
    * `1`: General error. If no other error suits the occurred error this one must be used with free text that is going to be shown to the end-user.

## Example contents of the provisioning configuration file

```xml
<?xml version="1.0"?>
<provision>
    <base_urls>
        <base_url>http://yourserver.com/provision/?username=[user]&amp;password=[pass]</base_url>
    </base_urls>
    <options>
            <use_authentication>false</use_authentication>
            <user_input_authentication>false</user_input_authentication>
            <authentication_type>url</authentication_type>
            <user></user>
            <password></password>
            <password_format>0</password_format>
            <delete_accounts_beforehand>true</delete_accounts_beforehand>
            <delete_contact_services_beforehand>false</delete_contact_services_beforehand>
    </options>
</provision>
```

## Example contents of the error response

```xml
<?xml version="1.0"?>
<error code="1">Provisioning failed. Some error occurred.</error>
```
