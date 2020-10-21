# **Zoiper5 plugin configuration documentation**

## **Platform**: **Zoiper5 Desktop - Windows, macOS, Linux**

## **Version**: **1.0.0**

## Contents

<!-- TOC -->

* [Contents](#contents)
* [Overview](#overview)
* [Plugin configuration directory](#plugin-configuration-directory)
* [Plugin configuration file](#plugin-configuration-file)
  * [XML Structure](#xml-structure)
    * [`zoiper_plugin`](#zoiper-plugin)
      * [`name`](#name)
      * [`version`](#version)
      * [`vendor_name`](#vendor-name)
      * [`signature`](#signature)
      * [`intermediate_PEM`](#intermediate-pem)
      * [`PEM`](#pem)
      * [`description`](#description)
      * [`plugin_path`](#plugin-path)
      * [`architecture`](#architecture)
* [Example contents of a plugin configuration file](#example-contents-of-a-plugin-configuration-file)

<!-- /TOC -->

## Overview

This document shows how to create plugin configuration files, allowing third party plugins to be loaded.

## Plugin configuration file

A *plugin configuration file* can have any name and is an XML document that uses the *UTF-8* encoding. It is recommended that the files end with the `.xml` extension and contain the name of the providing vendor.


### XML structure

All of the following sections are required to be present in a valid plugin configuration, unless specified otherwise.

#### `zoiper_plugin`

  * The root element of the document.

##### `name`

  * The plugin's name, this will be displayed to the user when managing the plugin;
  * Type: ***string***.

#### `version`
  * The plugin's version;
  * Type: ***integer***.

#### `vendor_name`
  * The name of the vendor which created the plugin;
  * Type: ***string***.

#### `signature`
  * The signature of the plugin. Details on how to sign a plugin are provided in a separate document;
  * Type: ***string***.

#### `intermediate_PEM`
  * A single intermediate PEM certificate, part of the certificate chain used to sign the plugin;
  * More than one of these elements can be present in the file, their order in the file does not matter;
  * **Optional**;
  * Type: ***string***.

#### `PEM`
  * The certificate with which to verify the plugin's signature. Must be signed by Securax's root CA;
  * Type: ***string***.

#### `description`
  * The plugin's description. This will be used by the user to manage the plugin;
  * Type: ***string***.

#### `plugin_path`
  * The path to the plugin itself;
  * Type: ***string***.

#### `architecture`
  * The plugin's target architecture. Specifies which architecture the plugin is compiled for;
  * Type: ***string***. Possible values: `x86` and `x86_64`.

## Plugin configuration directory

The plugin configuration files are located in specific directories on each operating system. The directories are listed and all the readable files are checked if they are valid plugin configuration files.

The directories in which the plugin configurations are located are as follows:

	* Windows: "C:\Program Data\ZoiperPlugins"
	* macOS: "/Library/ZoiperPlugins"
	* Linux: "/etc/zoiper/plugins"

## Example contents of a plugin configuration file

```xml
<zoiper_plugin>
  <name>Sample Plugin</name>
  <version>420</version>
  <vendor_name>Securax</vendor_name>
  <signature>6371c37d75ef5e7b62993419562a3eed3e0eaf74dae60834988b9ab79398092d136cc887287ae674a7faf35ac15b377cf6db86596270e5b2cff5f68f0748bbd3c9b3aba963551ca9265e9dc661c867195a236c526c7abc6f2bfc1abba2a1f5ce1401186eecbe1942792f4098032ba463771976297724ca8705b328433b7f784aad3aa6e529d2dc931283917e3ebd928f6117990b93baee657ac1c632f908dff55618fd4522dd250285956847cea5d2acabf78a57149f20aac805d21ba5de09e4c7d7ddc6d41b6e6fc6d7f6793b7906978337a8e192167b3377d90a8c0985478479d56ea683fc6081b20a39ee9eb0c3c219f27dfaef35b937647c078095f9c9bc</signature>
  <intermediate_PEM>-----BEGIN CERTIFICATE-----
MIIHATCCBOmgAwIBAgIUdJsXZWZU9WzQUJieCpPYGcL7QqowDQYJKoZIhvcNAQEN
BQAwXzELMAkGA1UEBhMCQkcxDjAMBgNVBAcMBVNvZmlhMQ8wDQYDVQQKDAZab2lw
ZXIxDzANBgNVBAsMBlpvaXBlcjEeMBwGA1UEAwwVcGx1Z2lucy1jYS56b2lwZXIu
Y29tMB4XDTIwMTAxMjExNDgwMFoXDTMwMTAxMDExNDQwMFowXzELMAkGA1UEBhMC
QkcxDjAMBgNVBAcMBVNvZmlhMQ8wDQYDVQQKDAZab2lwZXIxDzANBgNVBAsMBlpv
aXBlcjEeMBwGA1UEAwwVcGx1Z2lucy1jYS56b2lwZXIuY29tMIICIjANBgkqhkiG
9w0BAQEFAAOCAg8AMIICCgKCAgEAurIzBlF/RruNYIwbD9KB/C9/v0dVFchHLpr/
SvY9Osj6JpHCMB1LbC6LseWvrl8ZiSaF6xC2r+x571c8z0GS21PyvLWN/AnpMfk8
MZ4f19Cw157OICZpiWvZp8Kd1SL2bvgddE2kZoOeeDH81MP4se7mXbuY0nLSQ2Cr
O99RnrwqOjyDKP4kOoCsTP4TVIZ1L8RpXCgsZ3PNk29Per7L+LupYIYziZWfuV80
gWefQPvhyw3k6tIYuMEaV8R/CLbEDnkjn+WHpfdxnYry/hIjgf/V1OejTEwUJxFc
Yj3j47GZE48GLsCvJ/8UC2m68lO4EErL4ZbGaUKA/3fpyk0yUcrwkYV5vDnIFGZT
Pk9TeEt9RRAu9wEahpKEx7Xn8C+CrCykaLXyYYSgKjyH1U+yRgspnLLpp4N0kZxY
VzRPRmhM5DlzIkcCUVTO1FWyAKZjo8WpNpymMGmehq4EkWjcrEnVxGG8fk8gdyg9
Bp924yoBGOPHncY3aqgGosQgnOAdyIXJSAuMQDLSNeB72rE1fAvRzdCSiQifHWdG
EzTnWRczoag1EhdhFiUR0oHJSFD8HiLJJlsigI5vFO2Gah+mf74BD8sqRcx8ZKAQ
fX3Ceq+Z8eI89DDg3EYR/rqD7/MMeOQLL3j/5XllLtXON3L8Giv8icB6aOM3p/KP
+X9/G4ECAwEAAaOCAbMwggGvMBIGA1UdEwEB/wQIMAYBAf8CAQIwDgYDVR0PAQH/
BAQDAgEGMB0GA1UdDgQWBBSvrL6CS+csdpXZR/nPZph8QTjkjzAfBgNVHSMEGDAW
gBSTQ2y9tP8q+WhLr3Oa7liBBbzjoTBoBgNVHR8EYTBfMF2gW6BZhldodHRwOi8v
cGx1Z2lucy1jYS56b2lwZXIuY29tL2RqYW5nb19jYS9jcmwvY2EvNTBFMTAwNjNG
QTE0NTI4NDI2MzYxRUUxQjdEMEI0N0YyODY5Q0YxNC8wgd4GCCsGAQUFBwEBBIHR
MIHOMGQGCCsGAQUFBzABhlhodHRwOi8vcGx1Z2lucy1jYS56b2lwZXIuY29tL2Rq
YW5nb19jYS9vY3NwLzUwRTEwMDYzRkExNDUyODQyNjM2MUVFMUI3RDBCNDdGMjg2
OUNGMTQvY2EvMGYGCCsGAQUFBzAChlpodHRwOi8vcGx1Z2lucy1jYS56b2lwZXIu
Y29tL2RqYW5nb19jYS9pc3N1ZXIvNTBFMTAwNjNGQTE0NTI4NDI2MzYxRUUxQjdE
MEI0N0YyODY5Q0YxNC5kZXIwDQYJKoZIhvcNAQENBQADggIBAFBiee5Kwbuk4SDP
myakKP53jE9GASAa7M4yNtPOGJhrgVfU6OC4moG3jG5R8esE39oJDoqq/gtMzAg2
Ldu8GlNkvmFkz/z5wrIkRHOiwrBEvWzyEyESw5UgA9D4A15dyT4KKbtSeubs49Z0
6sGf/F4xI4HkPua/0yX4hx91oGzE+IgnAJfATX4UV79zbYrYHDpE6+OuH36hv3FW
CN529mEWcQZcJyhQqoBYiFWDCUUW3RWhQkDP2JuB9Oit+z+Muak2Bz79Bl/Axww1
eynGHR1u6fcstBNLlCRg69/XmIk3lm3tof/VS8NKO5BpEPQq3LIHwm8O0flvN1Le
ovUHF3HVQmy8PAZpwMD1ZdZjqye5AFKr0J++tvwxzb4PWSlI90WC9pCoRXQWJfyU
JL52i1+lWgw46uTO1sjQss+7c7dzMgEdW/vJgoq+2kfr2BapiUmbJ29dLIisxkVZ
noGiX1slpHt9TE95ijdKAJ0DKsGLRfb8n8sItA7nUZrtzufhVL1M4gsKTscvrq3N
mJ9j3GE9eO5a2pNY665ugH8TsbVDdNoHQChxGXQ/rR9LlNnbg46wkzuWhBmBjLVE
wQGgP947HcgUIkYdV8nVUe9CSBs4K8UGUEvfEcXbB/aLSVI7JVohzUlNxZqFIkfi
xXR/guVdTVeldXv9Lpee1bTLuFMk
-----END CERTIFICATE-----</intermediate_PEM>
  <PEM>-----BEGIN CERTIFICATE-----
MIIF/jCCA+agAwIBAgIUKaaieBTGBf9nTT82Hn+hkvBKkxowDQYJKoZIhvcNAQEN
BQAwXzELMAkGA1UEBhMCQkcxDjAMBgNVBAcMBVNvZmlhMQ8wDQYDVQQKDAZab2lw
ZXIxDzANBgNVBAsMBlpvaXBlcjEeMBwGA1UEAwwVcGx1Z2lucy1jYS56b2lwZXIu
Y29tMB4XDTIwMTAxMjEzMzYwMFoXDTI4MTAxMDEzMzYwMFowHDEaMBgGA1UEAwwR
a3Jpc3RpeWFuLnBleWNoZXYwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIB
AQDplB6yNOWurhe8RQvxb/QXoQU5JCkF5McFW0jNfGV1fol5XRR1mJEGYG1M/cNf
gykYJtIqVM5H25ypp9XFBlW9Cb1NhiSYX+3vAL0MkCQ9DhVFDbPtkyGtW3etHc38
JcmjQb+UxTMxwan1V26v96GPI0QctiltSrMPhOD3P0awn1x3PYM9V2Dw7zHTXE1/
dqlDsoeZyOMCn4zk4aIe2pDLjDrVpRLvJLcJ5YrRcPEvkGHJ8U+dEfTtbhQUT//k
CmmPX5ei09pH3573XypnoYIgmFTmSO4c9YD6te7oivF/O8Jl0/mKAb/cGVaJ1EAD
pquT0XE//kdi8x1sn0SkB10XAgMBAAGjggHzMIIB7zAOBgNVHQ8BAf8EBAMCBLAw
JwYDVR0lBCAwHgYIKwYBBQUHAwIGCCsGAQUFBwMDBggrBgEFBQcDBDAMBgNVHRMB
Af8EAjAAMBwGA1UdEQQVMBOCEWtyaXN0aXlhbi5wZXljaGV2MB8GA1UdIwQYMBaA
FK+svoJL5yx2ldlH+c9mmHxBOOSPMGUGA1UdHwReMFwwWqBYoFaGVGh0dHA6Ly9w
bHVnaW5zLWNhLnpvaXBlci5jb20vZGphbmdvX2NhL2NybC83NDlCMTc2NTY2NTRG
NTZDRDA1MDk4OUUwQTkzRDgxOUMyRkI0MkFBLzCB4AYIKwYBBQUHAQEEgdMwgdAw
ZgYIKwYBBQUHMAKGWmh0dHA6Ly9wbHVnaW5zLWNhLnpvaXBlci5jb20vZGphbmdv
X2NhL2lzc3Vlci81MEUxMDA2M0ZBMTQ1Mjg0MjYzNjFFRTFCN0QwQjQ3RjI4NjlD
RjE0LmRlcjBmBggrBgEFBQcwAYZaaHR0cDovL3BsdWdpbnMtY2Euem9pcGVyLmNv
bS9kamFuZ29fY2Evb2NzcC83NDlCMTc2NTY2NTRGNTZDRDA1MDk4OUUwQTkzRDgx
OUMyRkI0MkFBL2NlcnQvMB0GA1UdDgQWBBSue4NoLFI0X2WHuMVqruUtHO37lDAN
BgkqhkiG9w0BAQ0FAAOCAgEAq1Z1CuBvQvZOv9OVGFDd17vvbZX6MqzTn23dxBfp
t3Ibqx59r7PX3zN+YybkhkApvoLX5mdTN/IK+5WDfGvnQ8TrrH/n4x3icC1nSP1W
i4QXmHbUZNH9FHZzVXBfGA512r4DwwI+2O4Qu7vX1UdB5HQRjennBarWJ82Rqt1g
tzl8GmyxjRvgtsSRgFyg8bUS+JkaFav0q8+u8GwHHtKakwGrP0Z2B/x05ePpUSmt
dNvF60Ec3xPtn0z9OM++7Eq43CAsV04cBYwku1Q9zDSsaG8xHR1as2OVjLHmMul0
Gt5z/5x0WPYRov6cesEqNBkN7bMBSg5rk1BTATXR47q6mogKM6/UKoB9Q7dtnBCu
NZ7SGlMeomA/IU0x0zi4PcW528/vmz0jzEaj49YqsudzqrpbALsjJmSc6YXbW5/Q
Cg1hw90WzBtKfn4gKmCSomXjOLHqUxZhcIw/G1AtFw9f03z3U/w3ejdGriPe1vc+
sPBLcOf/GlhYj0CbKOYRyP1kF9Z1GqmvOcIRUbFO8R98QCZj6B8bkzTd79fRewLm
jzfjErfO8XtNGou7WTk8z4npLEy+6YVitVGl4y4VcM2erXAtFuRkZ2rK1yVWP2ST
FVdYAwR5xORBM/3pEITGXshKokFbQZkg6NIGE/2/WRaUHoW6je8LzbKx1xKU25QH
NzE=
-----END CERTIFICATE-----</PEM>
  <description>A sample plugin</description>
  <plugin_path>/plugins/libPlugin.so</plugin_path>
</zoiper_plugin>
```
