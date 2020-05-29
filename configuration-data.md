# **Title**

## **Platform**: **platform**

## **Version**: **version**

## Contents

<!-- TOC -->

* [Contents](#contents)
* [Format](#format)
* [Compatibility with other platforms](#compatibility-with-other-platforms)
* [Structure](#structure)
  * [Indentation](#indentation)
  * [Details](#details)
* [Main sections](#main-sections)
* [Account options (the `account` section)](#account-options-the-account-section)
  * [Generic (protocol-independent) account options](#generic-protocol-independent-account-options)
  * [SIP-specific account options](#sip-specific-account-options)
  * [IAX-specific account options](#iax-specific-account-options)
  * [HTTP Phone Control-specific account options](#http-phone-control-specific-account-options)
  * [Codec account options](#codec-account-options)
  * [STUN account options](#stun-account-options)
  * [Registration- and subscription-related account options](#registration--and-subscription-related-account-options)
* [Diagnostic options (the `diagnostics` section)](#diagnostic-options-the-diagnostics-section)
* [Example contents of the configuration data](#example-contents-of-the-configuration-data)

<!-- /TOC -->

## Format

XML documents usually contain the following things:

* *Tags*: a tag is a label (i.e. a string which does not contain whitespace) enclosed in angular brackets.  For example, `<options>` is a tag.  They are further subdivided into:
  * *opening tags*: elements begin with them and they look like this: `<account>`.
  * *closing tags*: elements end with them and they look like this: `</account>`.
  * *shorttags*: they are used in empty elements and look like this: `<account/>`.
* *Elements*: there are two types of them:
  * *normal elements*: they consist of the following things in the *given* order:
    * an opening tag;
      * content: can include *one or more* of the following things in *any* order:
      * other elements: they are called *children* of the given element, which is a *parent* of them;
      * *plaintext*: this is just a literal string of characters which do not include `<`, `>`, or `&`.
    * a closing tag.
  * *empty elements*: they consist of a single *shorttag*.

Every XML document contains a main element which is called a *root element*.  It has no parent element and contains all other elements in the document.

**Note**:  The actual XML specification is substantially larger and more complex.  However, the other features of XML are simply irrelevant to our case because they are not used for the configuration of the phone, and that's why they are not documented here.

## Compatibility with other platforms

Zoiper is available for Android and iOS platforms. Some of the sections are compatible across all platforms as follows:

* accounts
* account
* diagnostics

## Structure

The configuration is organized in several subnodes called *sections*.  Every section corresponds to an XML element and contains either or both of the following two things:

* other sections: they are called *subsections* of that section;
* *options*: those are elements which do not contain other sections.  They only have plaintext inside them (which represents the value of the option).

In general, every section should occur only once (i.e. its corresponding element at the specific position in the hierarchy of the document should be the only one with the given name).  There are exceptions to this rule, and they are explicitly documented here.

The configuration sections are listed in the *Main sections* section of this document, along with a short description of what they are responsible for.

### Indentation

The indentation used in this document is only used to demonstrate the tree structure of the *configuration data*, and therefore is not needed to be present in the actual *configuration data*.

### Details

For each individual option, there are a few details which are specified in addition to its description:

* *Type*: this is the data type used for storing the option's value.  The following data types are recognized :
  * ***integer***: this is an option whose value is an integer (e.g. `0`, `3`, `-5`, *etc*).
  * ***string***: this is an option whose value is a string (i.e. a sequence of characters).
  * ***boolean***: this is an *on*/*off* option (i.e. with two possible values).
    * `true`: means that the option is turned *on*.
    * `false`: means that the option is turned *off*.
  * ***date/time***: this is an option whose value is a date-time object (e.g. `2006-01-02 15:04:05`).
  * ***enumeration***: this is an option whose value is a number from a given range (*enumeration*) of numbers (e.g. `3` from the enumeration `0`-`11`).
  * ***text enumeration***: this is an option whose value is a string from a given set (*enumeration*) of strings (e.g. `udp` from the enumeration {`udp`, `tcp`, `tls`}).
* *Default value*: this is the value which gets assigned to the option when the *configuration data* is created (e.g. on first start), or when a new account, *etc* is added.

## Main sections

* `options`: the main configuration section (it corresponds to the root element of the document).
  * `accounts`: a section with several sub-sections defining the accounts used, where each account has its own `account` section.
    * `account`: a section for options related to a specific account.
  * `diagnostics`: a section for options related to diagnostics.

## Account options (the `account` section)

### Generic (protocol-independent) account options

These options are present for every account (regardless of its type).

* `ident`: this option defines the ID of the account.
  * This option's value should be unique.
  * This option is immutable (i.e. its value cannot be changed).
  * Type: ***string*** (more specifically, a valid account identifier).
  * Default value: initially *nothing* (i.e. an empty string).  The value gets automatically assigned by the phone during the creation of the account.

* `name`: this option defines the name of the account (as seen in the phone).
  * Type: ***string***.
  * Default value: initially *nothing* (i.e. an empty string).  The value gets assigned by the user during the creation of the account.

* `save_username`: this option determines whether the entered username for the account gets persisted (i.e. saved) in the configuration.
  * Type: ***boolean***.
  * Default value: `true`.

* `username`: this option defines the username for the account (as used by its respective protocol).
  * Type: ***string***.
  * Default value: initially *nothing* (i.e. an empty string).  The value gets assigned by the user during the creation of the account.

* `save_password`: this option determines whether the entered password for the account gets persisted (i.e. saved) in the configuration.
  * Type: ***boolean***.
  * Default value: `true`.

* `password`: this option defines the password for the account (as used by its respective protocol).
  * Type: ***string*** (more specifically, a password).
  * Default value: initially *nothing* (i.e. an empty string).  The value gets assigned by the user during the creation of the account.

* `register_on_startup`: this option determines whether the account gets registered when the phone gets started.
  * Type: ***boolean***.
  * Default value: `true`.

* `do_not_play_ringback_tones`: this option determines whether the phone abstains from playing a ringback tone for calls made or received from this specific account.
  * Type: ***boolean*** (`true` means *disable* and `false` means *do not disable*).
  * Default value: `false`.

* `voicemail_check_extension`: this option defines the extension that the phone dials on the server to listen to voicemail messages from this specific account.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `voicemail_transfer_extension`: this option defines the extension that the phone dials on the server to leave a voicemail message from this specific account.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `force_rfc3264`: this option determines whether the phone forcibly holds calls made or received from this specific account according to RFC3264 (Cisco Unified Communications Manager).
  * Type: ***boolean***.
  * Default value: `false`.

* `use_kpml`: this option determines whether the phone uses KPML with this specific account.
  * Type: ***boolean***.
  * Default value: `false`.

* `use_overlap_dialing`: this option determines whether the phone uses overlap dialing from this specific account.
  * Type: ***boolean***.
  * Default value: `false`.

* `use_custom_ringtone`: this option determines whether the phone uses a custom ringtone (from an audio file) for calls made or received from this specific account.
  * Type: ***boolean***.
  * Default value: `false`.

* `custom_ringtone_location`: this option defines the path to the audio file used as a custom ringtone for this specific account.
  * This option only makes sense when `use_custom_certificate` is enabled.
  * Type: ***string*** (more specifically, a valid path to an existing audio file).
  * Default value: *nothing* (i.e. an empty string).  This means that no custom ringtone file is used and that the default ringtone is played instead by the phone.

* `use_custom_certificate`: this option determines whether and how the phone uses a custom certificate for encryption with this specific account.
  * Type: ***text enumeration***.
  * Possible values:
    * `none`: This value means that no certificate is used at all.
    * `common`: This value means that the default certificate from the server is used.
    * `location`: This value means that a custom certificate from a certificate file is used.
    * `configuration`: This value means that the phone will generate a self-signed certificate for this account.
    * `self_signed`: It means that a custom certificate which is directly stored in the configuration is used.
  * Default value: `none`.

* `custom_certificate_location`: this option defines the path to the certificate file used for the custom certificate for this specific account.
  * This option only makes sense when the value of the `use_custom_certificate` option is `location`.
  * Type: ***string*** (more specifically, a valid path to an existing certificate file).
  * Default value: *nothing* (i.e. an empty string).  This means that no custom certificate file is used.

* `custom_certificate`: this option defines the custom certificate itself used for this specific account.
  * This option only makes sense when the value of the `use_custom_certificate` option is `configuration`.
  * Type: ***string*** (more specifically, a valid certificate specification).
  * Default value: *nothing* (i.e. an empty string).  This means that no custom certificate is used.

* `mwi_subscribe_usage`: this option determines whether and how the phone subscribes for Message Waiting Information (MWI) to the server.  If the server supports it, the phone will inform the user of new voicemail messages received from this specific account.
  * Type: ***text enumeration***.
  * Possible values:
    * `disabled`: This value means that no MWI subscription takes place.
    * `before`: This value means that the MWI subscription takes place before the account gets registered.
    * `after`: This value means that the MWI subscription takes place after the account gets registered.
    * `both`: This value means that the MWI subscription takes place both before and after the account gets registered.
  * Default value: `both`.

* `use_number_rewriting`: this option determines whether a default country code is used for numbers dialed from this specific account that do not have one, and whether an international call prefix is used for them.
  * Type: ***boolean***.
  * Default value: `false`.

* `number_rewriting_country`: this option defines the default country code used for numbers dialed from this specific account that do not have one.
  * This option only makes sense when `use_number_rewriting` is enabled.
  * Type: ***string*** (more specifically, a valid country code).
  * Default value: the country code for the current location.

* `number_rewriting_prefix`: this option defines the prefix used when dialing international numbers from this specific account.
  * This option only makes sense when `use_number_rewriting` is enabled.
  * Type: ***string*** (more specifically, a valid phone number prefix).
  * Default value: *nothing* (i.e. an empty string).  This means that no international call prefix is used.

* `use_strip_dial_chars`: this option determines whether certain characters designated as delimiters are stripped from numbers dialed from this specific account.
  * Type: ***boolean***.
  * Default value: `true`.

* `strip_dial_chars`: this option defines the characters to be stripped from numbers dialed from this specific account.
  * This option only makes sense when `use_strip_dial_chars` is enabled.
  * Type: ***string*** (more specifically, a set of characters to strip).
  * Default value: ` .-()[]{}`.

* `protocol`: this option determines which communication protocol is used when dialing from this specific account (i.e. the type of the account).
  * Type: ***text enumeration***.
  * Possible values:
    * `sip`: SIP account.
    * `iax2`: IAX2 account.
    * `http_phone_control`: HTTP control account. **Not available in all versions**
  * Default value: `sip`.

* `rtcp_profile_type`: this option determines whether a RTCP feedback mechanism is used and the RTP profile type used for RTCP-based feedback.
  * Type: ***text enumeration***.
  * Possible values:
    * `avp`: This value means that RTCP feedback is turned off.
    * `avpf`: This value means that RTCP feedback is turned on.  In this case only the RTP/AVPF RTP profile is offered through SDP (if the other peer does not support feedbacks, the call will be drop immediately).
    * `both`: This value means that RTCP feedback is turned on.  In this case both the RTP/AVP and RTP/AVPF RTP profiles are offered (media lines are duplicated for backward compatibility) through SDP (if the other peer supports RTCP feedbacks, it will be used with priority).
  * Default value: `avp`.

* `enabled_video_fmtp`: this option determines whether the phone should use the video FMTP protocol for the SIP server used for this account.
  * Type: ***boolean***.
  * Default value: `true`.

#### SIP-specific account options

These options are only present for SIP accounts.

* `SIP_domain`: this option defines the hostname/IP address of the SIP registrar server used for this specific account.
  * Type: ***string*** (more specifically, a valid hostname or an IP address).  The hostname/IP address can optionally be followed by a colon and a port number, like this: `host:port`.
  * Default value: *nothing* (i.e. an empty string).

* `SIP_use_outbound_proxy`: this option determines whether the SIP server used for this specific account gets accessed using an outbound proxy.
  * Type: ***boolean***.
  * Default value: `false`.

* `SIP_outbound_proxy`: this option defines the hostname/IP address of the outbound proxy server used for accessing the SIP server used for this specific account.
  * This option forces the phone to use an optional proxy server instead of the automatically detected one.
  * This option only makes sense when `SIP_use_outbound_proxy` is enabled.
  * Type: ***string*** (more specifically, a valid hostname or an IP address).  The hostname/IP address can optionally be followed by a colon and a port number, like this: `host:port`.
  * Default value: *nothing* (i.e. an empty string).

* `SIP_transport_type`: this option determines the transport-layer protocol used for sending/receiving SIP packets from this specific account.
  * Type: ***text enumeration***.
  * Possible values:
    * `udp`: This value means that the User Datagram Protocol (UDP) is used for sending and receiving SIP packets.
    * `tcp`: This value means that the Transmission Control Protocol (TCP) is used for sending and receiving SIP packets.
    * `tls` This value means that the Transport Layer Security (TLS) protocol over TCP is used for sending and receiving of encrypted SIP packets.
  * Default value: `udp`.

* `SIP_use_auth_username`: this option determines whether an authentication username should be used for the SIP server used for this specific account.
  * Type: ***boolean***.
  * Default value: `false`.

* `SIP_auth_username`: this option defines the authentication username for the SIP server used for this specific account.
  * This is an optional username which is used when responding to a SIP authentication challenge.
  * It is recommended that the value of this option be left empty unless you have been explicitly instructed to fill this field by your provider or PBX.
  * This option only makes sense when `SIP_use_auth_username` is enabled.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `SIP_callerId`: this option defines the caller ID name for the SIP server used for this specific account.
  * This is the name that gets displayed to somebody who does not have you on their contact list when you call them.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

<!-- The `SIP_callerNumber` option is deliberately omitted because its usage is deprecated (more specifically, it was not conforming to the SIP standard). -->

* `SIP_use_rport`: this option determines whether the phone uses the *rport* extension parameter for the SIP server used for this specific account.
  * This option is used so that the server can discover the public address and port of the user if there is a NAT between the user and the server.
  * This option is mainly useful with normal unfirewalled TCP and TLS connections.  That is why it is highly recommended to enable it for those protocols.
  * The default is to have *rport* disabled for UDP connections.  If *rport* is enabled for UDP connections along with STUN, STUN is preferred.
  * Type: ***boolean***.
  * Default value: `true`.

* `SIP_use_rport_media`: this option determines whether the phone uses *rport*-discovered public addresses for media negotiations performed from this specific account.
  * This option is the last resort for NAT-related problems with missing audio for some broken implementations (e.g. when the client is behind a symmetric NAT in combination with a CUCM server).
  * This option can also be useful for some firewall/NAT/VPN setups where the port is not changed, only the private address is replaced with a public one.  When both rport and STUN are enabled, STUN is preferred.
  * It is recommended for this option to be disabled.  Enable it only if you absolutely know what you're doing.
  * Type: ***boolean***.
  * Default value: `false`.

* `SIP_srtp_mode`: this option determines whether and how the phone uses SRTP to encrypt media exchanged via this specific account.
  * SRTP only works if used in combination with encrypted transport (i.e. TLS).
  * Type: ***text enumeration***.
  * Possible values:
    * `none`: This value means that SRTP won't be used at all.
    * `sdes`: This value means SRTP with SDES is used.
  * Default value: `none`.

* `SIP_dtmf_style`: this option determines whether and how the phone sends DTMF tones (i.e. the DTMF band it uses) from this specific account.
  * Type: ***text enumeration***.
  * Possible values:
    * `inband`: This value means that DTMF tones are sent in-band within the media (i.e. audio data) as sound.
    * `outband` or `rfc_2833`: This value means that DTMF tones are sent out-of-band embedded in RTP packets according to RFC 2833.
    * `SIP_info_numeric`: These values mean that DTMF tones are sent out-of-band embedded in SIP packets as numbers.
    * `SIP_info_ascii`: These values mean that DTMF tones are sent out-of-band embedded in SIP packets as ASCII symbols.
    * `disabled`: This value means that no DTMF tones are sent by the phone.
  * Default value: `rfc_2833`.

* `SIP_use_blf`: this option determines whether the phone uses the Busy Lamp Field (BLF) functionality.
  * When an extension configured with BLF is busy, its presence status is seen as busy in the contacts list.
  * Type: ***boolean***.
  * Default value: `false`.

* `SIP_publish_presence`: this option determines whether the phone publishes the status of its presence profile to contacts on the contact list.
  * Type: ***boolean***.
  * Default value: `false` (`true` if the `presence` feature is enabled).

* `SIP_subscribe_presence`: this option determines whether the phone subscribes for the presence profile status of contacts on the contact list.
  * Type: ***boolean***.
  * Default value: `false` (`true` if the `presence` feature is enabled).

* `SIP_keep_alive_mode`: this option determines whether and how often the phone should send Keep-Alive requests to the SIP server used for this account.
  * Type: ***text enumeration***.
  * Possible values:
    * `disabled`: This value means that no Keep-Alive requests are sent by the phone.
    * `default`: This value means that Keep-Alive requests are sent every 30 seconds for UDP or every 180 seconds for TCP.
    * `custom`: This value means that Keep-Alive requests are sent on a user-specified interval (configured in the `SIP_keep_alive_timeout` option).
  * Default value: `default`.

* `SIP_keep_alive_timeout`: this option defines a custom interval (in seconds) for Keep-Alive requests sent by the phone to the SIP server used for this account.
  * Type: ***integer***.
  * Default value: `30` (this one is for UDP).

* `SIP_use_cisco`: this option determines whether the phone should use Cisco-style server-side call forwarding.
  * This option only works if you configure the phone type on the **Cisco Call Manager** as **Cisco Softphone** instead of the standard 3rd-party SIP softphone.
  * Type: ***boolean***.
  * Default value: `false`.

* `SIP_cisco_device_name`: this option defines the name of the Cisco device that should be used for Cisco-style server-side call forwarding.
  * This option only makes sense when `SIP_use_cisco` is enabled.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `zrtp`: a section for options related to the ZRTP media encryption protocol.
  * The ZRTP protocol is used along with SIP protocol only.

  * `enabled`: this option determines whether ZRTP is used for this account.
    * Type: ***boolean***.
    * Default value: `false`.

  * `hash_algorithms`: a section with several sub-sections defining the ZRTP hash algorithms used for this specific account.

    * `hash_algorithm`: a section for options related to a specific hash algorithm.
      * `name`: this option defines the name of the hash algorithm (as seen in the phone).
        * Type: ***string***.
        * Default value: *nothing* (i.e. an empty string).

      * `id`: this option determines the type of the hash algorithm.
        * Type: ***enumeration*** (the hash algorithm type is chosen from a predefined list).
        * Possible values:
          * `0`: represents the *S256* hash algorithm.
          * `1`: represents the *S384* hash algorithm.
          * `2`: represents the *N256* hash algorithm (not supported yet).
          * `3`: represents the *N384* hash algorithm (not supported yet).
        * Default value: `0`.

      * `priority`: this option defines the priority of the hash algorithm among the others.  The phone orders the hash algorithms by priority when trying to establish a ZRTP-encrypted stream.  The lesser the number, the more likely for the hash algorithm to be chosen.
        * Type: ***integer***.
        * Default value: `0`.

      * `selected`: this option determines whether the hash algorithm is selected (i.e. whether it is used at all).
        * Type: ***boolean***.
        * Default value: `false`.

    * `cipher_algorithms`: a section with several sub-sections defining the ZRTP cipher algorithms used for this specific account.

      * `cipher_algorithm`: a section for options related to a specific cipher algorithm.

        * `name`: this option defines the name of the cipher algorithm (as seen in the phone).
          * Type: ***string***.
          * Default value: *nothing* (i.e. an empty string).

        * `id`: this option determines the type of the cipher algorithm.
          * Type: ***enumeration*** (the cipher algorithm type is chosen from a predefined list).
          * Possible values:
            * `0`: represents the *AES1* cipher algorithm (only this one is supported yet).
            * `1`: represents the *AES2* cipher algorithm.
            * `2`: represents the *AES3* cipher algorithm.
            * `3`: represents the *2FS1* cipher algorithm.
            * `4`: represents the *2FS2* cipher algorithm.
            * `5`: represents the *2FS3* cipher algorithm.
          * Default value: `0`.

        * `priority`: this option defines the priority of the cipher algorithm among the others.  The phone orders the cipher algorithms by priority when trying to establish a ZRTP-encrypted stream.  The lesser the number, the more likely for the cipher algorithm to be chosen.
          * Type: ***integer***.
          * Default value: `0`.

        * `selected`: this option determines whether the cipher algorithm is selected (i.e. whether it is used at all).
          * Type: ***boolean***.
          * Default value: `false`.

    * `auth_tags`: a section with several sub-sections defining the ZRTP authentication tags used for this specific account.

      * `auth_tag`: a section for options related to a specific authentication tag.

        * `name`: this option defines the name of the authentication tag (as seen in the phone).
          * Type: ***string***.
          * Default value: *nothing* (i.e. an empty string).

        * `id`: this option determines the type of the authentication tag.
          * Type: ***enumeration*** (the authentication tag type is chosen from a predefined list).
          * Possible values:
            * `0`: represents the *HS32* authentication tag.
            * `1`: represents the *HS80* authentication tag.
            * `2`: represents the *SK32* authentication tag (not supported yet).
            * `3`: represents the *SK64* authentication tag (not supported yet).
        * Default value: `0`.

      * `priority`: this option defines the priority of the authentication tag among the others.  The phone orders the authentication tags by priority when trying to establish a ZRTP-encrypted stream.  The lesser the number, the more likely for the authentication tag to be chosen.
        * Type: ***integer***.
        * Default value: `0`.

      * `selected`: this option determines whether the authentication tag is selected (i.e. whether it is used at all).
        * Type: ***boolean***.
        * Default value: `false`.

    * `key_agreement_methods`: a section with several sub-sections defining the ZRTP key agreement methods used for this specific account.

      * `key_agreement_method`: a section for options related to a specific key agreement method.
        * `name`: this option defines the name of the key agreement method (as seen in the phone).
          * Type: ***string***.
          * Default value: *nothing* (i.e. an empty string).

      * `id`: this option determines the type of the key agreement method.
        * Type: ***enumeration*** (the key agreement method type is chosen from a predefined list).
        * Possible values:
          * `0`: represents the *DH3K* key agreement method.
          * `1`: represents the *DH2K* key agreement method.
          * `2`: represents the *EC25* key agreement method.
          * `3`: represents the *EC38* key agreement method.
          * `4`: represents the *PRSH* key agreement method (not supported yet).
          * `5`: represents the *MULT* key agreement method (not supported yet).
        * Default value: `0`.

      * `priority`: this option defines the priority of the key agreement method among the others.  The phone orders the key agreement methods by priority when trying to establish a ZRTP-encrypted stream.  The lesser the number, the more likely for the key agreement method to be chosen.
        * Type: ***integer***.
        * Default value: `0`.

      * `selected`: this option determines whether the key agreement method is selected (i.e. whether it is used at all).
        * Type: ***boolean***.
        * Default value: `false`.

    * `sas_encodings`: a section with several sub-sections defining the ZRTP SAS encodings used for this specific account.

      * `sas_encoding`: a section for options related to a specific SAS encoding.
        * `name`: this option defines the name of the SAS encoding (as seen in the phone).
          * Type: ***string***.
          * Default value: *nothing* (i.e. an empty string).

        * `id`: this option determines the type of the SAS encoding.
          * Type: ***enumeration*** (the SAS encoding type is chosen from a predefined list).
          * Possible values:
            * `0`: represents the *B32* SAS encoding.
            * `1`: represents the *B256* SAS encoding.
          * Default value: `0`.

        * `priority`: this option defines the priority of the SAS encoding among the others.  The phone orders the SAS encodings by priority when trying to establish a ZRTP-encrypted stream.  The lesser the number, the more likely for the SAS encoding to be chosen.
          * Type: ***integer***.
          * Default value: `0`.

        * `selected`: this option determines whether the SAS encoding is selected (i.e. whether it is used at all).
          * Type: ***boolean***.
          * Default value: `false`.

#### IAX-specific account options

These options are only present for IAX accounts.

* `IAX2_host`: this option defines the hostname or the IP address of the IAX server used for this account.
  * Type: ***string*** (more specifically, a valid hostname or an IP address).  The hostname/IP address can optionally be followed by a colon and a port number, like this: `host:port`.
  * Default value: *nothing* (i.e. an empty string).  The value gets assigned by the user during the creation of the account.

* `IAX2_context`: this option defines the context for the IAX server used for this account.
  * Type: ***string*** (more specifically, a valid hostname or an IP address).  The hostname/IP address can optionally be followed by a colon and a port number, like this: `host:port`.
    * Default value: *nothing* (i.e. an empty string).

* `IAX2_callerId`: this option defines the caller ID name for the IAX server used for this account.
  * This is the name that gets displayed to somebody who does not have you on their contact list when you call them.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `IAX2_callerNumber`: this option defines the caller ID phone number for the IAX server used for this account.
  * This is the phone number that gets displayed to somebody who does not have you on their contact list when you call them.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `IAX2_dtmf_style`: this option determines whether and how the phone should send DTMF tones (i.e. which DTMF band it should use).
  * Type: ***text enumeration***.
  * Possible values:
    * `outband`: This value means that DTMF tones are sent out-of-band.
    * `inband`: This value means that DTMF tones are sent in-band within the media (i.e. audio data) as sound.
    * `disabled`: This value means that no DTMF tones are sent by the phone.
  * Default value: `outband`.

#### Account options specific for the HTTP Phone Control protocol

* `HTTP_PHONE_CONTROL_host`: this option defines the hostname or the IP address of the HTTP Phone Control server used for this account.
  * Type: ***string*** (more specifically, a valid hostname or an IP address).  The hostname/IP address can optionally be followed by a colon and a port number, like this: `host:port`.
  * Default value: *nothing* (i.e. an empty string).

* `HTTP_PHONE_CONTROL_path`: this option defines the context (path) for the HTTP Phone Control server used for this account.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `HTTP_PHONE_CONTROL_number`: this option defines the name of the "number" HTTP parameter used with the HTTP Phone Control protocol by this account.
  * Type: ***string***.
  * Default value: `number=`.

* `HTTP_PHONE_CONTROL_outgoing_uri`: this option defines the name of the "outgoing URI" HTTP parameter used with the HTTP Phone Control protocol by this account.
  * Type: ***string***.
  * Default value: `outgoing_uri=`.

* `HTTP_PHONE_CONTROL_answer`: this option defines the name of the "answer" HTTP parameter used with the HTTP Phone Control protocol by this account.
  * Type: ***string***.
  * Default value: `key=F4`.

* `HTTP_PHONE_CONTROL_hangup`: this option defines the name of the "hangup" HTTP parameter used with the HTTP Phone Control protocol by this account.
  * Type: ***string***.
  * Default value: `key=F4`.

### Codec account options

* `codecs`: a section with several sub-sections defining the codecs used for this specific account, where each codec has its own `codec` section.
  * `codec`: a section for options related to a specific codec.

    * `codec_id`: this option determines the type of the codec.
      * Type: ***enumeration*** (the codec type is chosen from a predefined list).
      * Possible values:
        * `-1`: represents an unknown or invalid codec;
        * `0`: represents the *G.711 mu-law (PCMU)* free audio codec;
        * `1`: represents the *GSM* free audio codec;
        * `6`: represents the *G.711 a-law (PCMA)* free audio codec;
        * `7`: represents the *G.722* free audio codec;
        * `16`: represents the *G.729* patented audio codec;
        * `18`: represents the *MJPEG* free video codec;
        * `24`: represents the *Speex @ 8000 Hz (Speex Narrow)* free audio codec;
        * `25`: represents the *Speex @ 16000 Hz (Speex Wide)* free audio codec;
        * `26`: represents the *Speex @ 32000 Hz (Speex Ultra)* free audio codec;
        * `27`: represents the *iLBC 30* free audio codec;
        * `28`: represents the *iLBC 20* free audio codec;
        * `29`: represents the *G.726 @ 32 kbps* free audio codec;
        * `30`: represents the *H.263+ (H263-1998)* free video codec;
        * `31`: represents the *VP8* free video codec;
        * `32`: represents the *H.264* patented video codec;
        * `33`: represents the *RFC 2833/4733 DTMF (telephone-event)* free DTMF audio codec;
        * `34`: represents the *Opus @ 8000 Hz (Opus Narrow)* free audio codec;
        * `35`: represents the *Opus @ 16000 Hz (Opus Wide)* free audio codec;
        * `36`: represents the *Opus @ 24000 Hz (Opus Super)* free audio codec;
        * `37`: represents the *Opus @ 48000 Hz (Opus Full)* free audio codec;
        * `38`: represents the *AMR* patented audio codec;
        * `39`: represents the *AMR-WB* patented audio codec;
        * `40`: represents the *H.264 (hardware-accelerated)* video codec;
        * `41`: represents the *RFC 2833/4733 @ 16000 Hz (telephone-event 16)* free DTMF audio codec;
        * `42`: represents the *RFC 2833/4733 @ 32000 Hz (telephone-event 32)* free DTMF audio codec;
        * `43`: represents the *RFC 2833/4733 @ 48000 Hz (telephone-event 48)* free DTMF audio codec.
      * Default value: `0`.

      * `priority`: this option defines the priority of the codec among the others.  the phone orders the codecs by priority when trying to establish an audio/video stream.  The lesser the number, the more likely for the codec to be chosen.
        * Type: ***integer***.
        * Default value: `0`.

      * `enabled`: this option determines whether the codec is enabled (i.e. whether it is used at all).
        * Type: ***boolean***.
        * Default value: `false`.

      * `bps`: this option defines the bitrate (*bps*) at which data is passed through the codec.
        * Type: ***integer***.
        * Default value: `0`.

      * `dtx`: this option determines whether DTX is used with the codec.
        * Type: ***boolean***.
        * Default value: `false`.

      * `vbr`: this option determines whether VBR is used with the codec.
        * Type: ***boolean***.
        * Default value: `false`.

### STUN account options

* `stun`: a section for options related to the STUN server options per account.

  * `use_stun`: this option determines whether and how the phone uses the STUN protocol.
    * Type: ***text enumeration***.
    * Possible values:
      * `disabled`: This value means that the account will not use STUN at all or there will not be a global STUN server.
      * `default`: This value means that the account will use the global STUN server. This value is not used in the case of a global STUN server itself.
      * `custom`: This value means that will use a custom STUN server i.e. the STUN server defined per-account.
    * Default value: `default`.

  * `stun_host`: this option defines the hostname or the IP address of the STUN server.
    * Type: ***string*** (more specifically, a valid hostname).
    * Default value: `stun.zoiper.com`.

  * `stun_port`: this option defines the number of the port which the phone uses to communicate with the STUN server.
    * Type: ***integer*** (more specifically, a valid port number, i.e. a number between `1` and `65535`).
    * Default value: `3478`.

  * `stun_refresh_period`: this option defines the interval (in seconds) on which *refresh* requests are sent by the phone to the STUN server.
    * Type: ***integer***.
    * Default value: `30`.

### Registration- and subscription-related account options

* `reregistration_mode`: this option determines whether and how often the registration of this account should expire (i.e. how often re-registration requests should be sent by the phone).
  * Type: ***text enumeration***.
  * Possible values:
    * `default`: This value means that re-registration requests are sent every 30 seconds for UDP or every 600 seconds for TCP.
    * `custom`: This value means that re-registration requests are sent on a user-specified interval (configured in the `reregistration_time` option).
  * Default value: `default`.

* `reregistration_time`: this option defines a custom interval (in seconds) for re-registration requests sent by the phone to the SIP server used for this account.
  * Type: ***integer***.
  * Default value: `60` when UDP is used, `600` when TCP is used.

* `resubscription_mode`: this option determines whether and how often the subscription of this account should expire (i.e. how often resubscription requests should be sent by the phone).
  * Type: ***text enumeration***.
  * Possible values:
    * `default`: This value means that resubscription requests are sent every 30 seconds for UDP or every 600 seconds for TCP.
    * `custom`: This value means that resubscription requests are sent on a user-specified interval (configured in the `resubscription_time` option).
  * Default value: `default`.

* `resubscription_time`: this option defines a custom interval (in seconds) for resubscription requests sent by the phone to the SIP server used for this account.
  * Type: ***integer***.
  * Default value: `60` when UDP is used, `600` when TCP is used.

* `send_typing_notification`: this option determines whether the phone should send typing notifications.
  * Type: ***boolean***.
  * Default value: `true`.

## Diagnostic options (the `diagnostics` section)

* `enable_debug_log`: this option determines whether the phone should write debug messages (*debug log*) to a log file.
  * Type: ***boolean***.
  * Default value: `false`.

* `enable_extra_dmp`: this option determines whether the phone should append additional information (*extended dump*) to the debug log.
  * This option only makes sense if `enable_debug_log` is enabled.
  * Type: ***boolean***.
  * Default value: `false`.

* `enable_audio_debug`: this option determines whether the phone should write audio debug messages to the log file.
  * This option only makes sense if `enable_debug_log` is enabled.
  * Type: ***boolean***.
  * Default value: `false`.

## Example contents of the configuration data

```xml
<?xml version="1.0"?>
<options>
  <accounts>
    <account>
      <ident>Z599e3b298c0b03439f9d85cf</ident>
      <name>username@10.2.1.99:6060</name>
      <save_username>true</save_username>
      <username>username</username>
      <save_password>true</save_password>
      <password>4Az/iSiiZmVzApH6Nra2jQ==
</password>
      <register_on_startup>true</register_on_startup>
      <do_not_play_ringback_tones>false</do_not_play_ringback_tones>
      <voicemail_check_extension/>
      <voicemail_transfer_extension/>
      <force_rfc3264>false</force_rfc3264>
      <use_kpml>false</use_kpml>
      <use_overlap_dialing>false</use_overlap_dialing>
      <use_custom_ringtone>false</use_custom_ringtone>
      <custom_ringtone_location/>
      <use_custom_certificate>none</use_custom_certificate>
      <custom_certificate_location/>
      <custom_certificate/>
      <mwi_subscribe_usage>both</mwi_subscribe_usage>
      <use_number_rewriting>false</use_number_rewriting>
      <number_rewriting_country>BG</number_rewriting_country>
      <number_rewriting_prefix/>
      <use_strip_dial_chars>true</use_strip_dial_chars>
      <strip_dial_chars> .-()[]{}</strip_dial_chars>
      <protocol>sip</protocol>
      <SIP_domain>10.2.1.99:6060</SIP_domain>
      <SIP_use_outbound_proxy>false</SIP_use_outbound_proxy>
      <SIP_outbound_proxy/>
      <SIP_transport_type>tcp</SIP_transport_type>
      <SIP_use_auth_username>false</SIP_use_auth_username>
      <SIP_auth_username/>
      <SIP_callerId/>
      <SIP_callerNumber/>
      <SIP_use_rport>true</SIP_use_rport>
      <SIP_use_rport_media>false</SIP_use_rport_media>
      <SIP_srtp_mode>none</SIP_srtp_mode>
      <SIP_dtmf_style>outband</SIP_dtmf_style>
      <SIP_use_blf>false</SIP_use_blf>
      <SIP_publish_presence>true</SIP_publish_presence>
      <SIP_subscribe_presence>true</SIP_subscribe_presence>
      <SIP_keep_alive_mode>default</SIP_keep_alive_mode>
      <SIP_keep_alive_timeout>30</SIP_keep_alive_timeout>
      <SIP_use_cisco>false</SIP_use_cisco>
      <SIP_cisco_device_name/>
      <enabled_video_fmtp>true</enabled_video_fmtp>
      <codecs>
        <codec>
          <codec_id>35</codec_id>
          <priority>1</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>7</codec_id>
          <priority>2</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>34</codec_id>
          <priority>3</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>0</codec_id>
          <priority>4</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>6</codec_id>
          <priority>5</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>16</codec_id>
          <priority>6</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>1</codec_id>
          <priority>7</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>39</codec_id>
          <priority>12</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>38</codec_id>
          <priority>15</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>25</codec_id>
          <priority>18</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>28</codec_id>
          <priority>21</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>27</codec_id>
          <priority>22</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>24</codec_id>
          <priority>23</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>26</codec_id>
          <priority>24</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>29</codec_id>
          <priority>25</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>36</codec_id>
          <priority>26</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>37</codec_id>
          <priority>27</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>31</codec_id>
          <priority>1001</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>32</codec_id>
          <priority>1002</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
        <codec>
          <codec_id>30</codec_id>
          <priority>1003</priority>
          <enabled>true</enabled>
          <bps>0</bps>
          <dtx>false</dtx>
          <vbr>false</vbr>
        </codec>
      </codecs>
      <stun>
        <use_stun>default</use_stun>
        <stun_host>stun.zoiper.com</stun_host>
        <stun_port>3478</stun_port>
        <stun_refresh_period>30</stun_refresh_period>
      </stun>
      <zrtp>
        <enabled>false</enabled>
        <hash_algorithms>
          <hash_algorithm>
            <name>S256</name>
            <id>0</id>
            <priority>0</priority>
            <selected>true</selected>
          </hash_algorithm>
          <hash_algorithm>
            <name>S384</name>
            <id>1</id>
            <priority>1</priority>
            <selected>true</selected>
          </hash_algorithm>
        </hash_algorithms>
        <cipher_algorithms>
          <hash_algorithm>
            <name>AES3</name>
            <id>2</id>
            <priority>0</priority>
            <selected>true</selected>
          </hash_algorithm>
          <hash_algorithm>
            <name>AES2</name>
            <id>1</id>
            <priority>1</priority>
            <selected>true</selected>
          </hash_algorithm>
          <hash_algorithm>
            <name>AES1</name>
            <id>0</id>
            <priority>2</priority>
            <selected>true</selected>
          </hash_algorithm>
        </cipher_algorithms>
        <auth_tags>
          <hash_algorithm>
            <name>HS32</name>
            <id>0</id>
            <priority>0</priority>
            <selected>true</selected>
          </hash_algorithm>
          <hash_algorithm>
            <name>HS80</name>
            <id>1</id>
            <priority>1</priority>
            <selected>true</selected>
          </hash_algorithm>
        </auth_tags>
        <key_agreement_methods>
          <hash_algorithm>
            <name>DH2K</name>
            <id>1</id>
            <priority>0</priority>
            <selected>true</selected>
          </hash_algorithm>
          <hash_algorithm>
            <name>EC25</name>
            <id>2</id>
            <priority>1</priority>
            <selected>true</selected>
          </hash_algorithm>
          <hash_algorithm>
            <name>DH3K</name>
            <id>0</id>
            <priority>2</priority>
            <selected>true</selected>
          </hash_algorithm>
          <hash_algorithm>
            <name>EC38</name>
            <id>3</id>
            <priority>3</priority>
            <selected>true</selected>
          </hash_algorithm>
        </key_agreement_methods>
        <sas_encodings>
          <hash_algorithm>
            <name>B256</name>
            <id>1</id>
            <priority>0</priority>
            <selected>true</selected>
          </hash_algorithm>
          <hash_algorithm>
            <name>B32</name>
            <id>0</id>
            <priority>1</priority>
            <selected>true</selected>
          </hash_algorithm>
        </sas_encodings>
      </zrtp>
      <reregistration_mode>default</reregistration_mode>
      <reregistration_time>0</reregistration_time>
      <resubscription_mode>default</resubscription_mode>
      <resubscription_time>0</resubscription_time>
      <send_typing_notification>true</send_typing_notification>
      <rtcp_profile_type>avp</rtcp_profile_type>
    </account>
  </accounts>
  <diagnostics>
    <enable_debug_log>false</enable_debug_log>
    <enable_extra_dmp>false</enable_extra_dmp>
    <enable_audio_debug>false</enable_audio_debug>
  </diagnostics>
</options>

```
