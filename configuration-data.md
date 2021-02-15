# **Zoiper5 configuration data documentation**

## **Platform**: **Zoiper5 Desktop - Windows, macOS, Linux**

## **Version**: **1.16.2**

## Contents

<!-- TOC -->

* [Contents](#contents)
* [Introduction](#introduction)
* [Provisioning](#provisioning)
* [Format](#format)
* [Compatibility with other platforms](#compatibility-with-other-platforms)
* [Structure](#structure)
  * [Indentation](#indentation)
  * [Details](#details)
* [Main sections](#main-sections)
* [General options (the `general` section)](#general-options-the-general-section)
* [Audio options (the `audio` section)](#audio-options-the-audio-section)
* [HID options (the `hid` section)](#hid-options-the-hid-section)
* [Global codec options (the `codec` section)](#global-codec-options-the-codec-section)
* [Account options (the `account` section)](#account-options-the-account-section)
  * [Generic (protocol-independent) account options](#generic-protocol-independent-account-options)
  * [SIP-specific account options](#sip-specific-account-options)
  * [IAX-specific account options](#iax-specific-account-options)
  * [HTTP Phone Control-specific account options](#http-phone-control-specific-account-options)
  * [Codec account options](#codec-account-options)
  * [STUN account options](#stun-account-options)
  * [Registration- and subscription-related account options](#registration--and-subscription-related-account-options)
* [SIP options (the `sip_options` section)](#sip-options-the-sipoptions-section)
* [IAX options (the `iax_options` section)](#iax-options-the-iaxoptions-section)
* [RTP options (the `rtp_options` section)](#rtp-options-the-rtpoptions-section)
* [Global STUN options (the `stun` section)](#global-stun-options-the-stun-section)
* [Diagnostic options (the `diagnostics` section)](#diagnostic-options-the-diagnostics-section)
* [Network options (the `network` section)](#network-options-the-network-section)
* [Chat options (the `chat` section)](#chat-options-the-chat-section)
* [Provisioning options (the `provision` section)](#provisioning-options-the-provision-section)
* [Popup options (the `popup` section)](#popup-options-the-popup-section)
* [Video options (the `video` section)](#video-options-the-video-section)
* [Skin options (the `skin` section)](#skin-options-the-skin-section)
* [Call forwarding and auto-answer options (the `forwarding_and_auto_answer` section)](#call-forwarding-and-auto-answer-options-the-forwardingandautoanswer-section)
* [Open-URL-on-event options (the `open_url_on_event` section)](#open-url-on-event-options-the-openurlonevent-section)
* [GUI options (the `gui` section)](#gui-options-the-gui-section)
* [Presence profile options (the `profile` section)](#presence-profile-options-the-profile-section)
* [Contact service options (the `contact_service` section)](#contact-service-options-the-contactservice-section)
  * [Generic contact service options](#generic-contact-service-options)
  * [Options specific to the `local` contact service type](#options-specific-to-the-local-contact-service-type)
  * [Options specific to the `ldap` contact service type](#options-specific-to-the-ldap-contact-service-type)
  * [Options specific to the `outlook` contact service type](#options-specific-to-the-outlook-contact-service-type)
  * [Options specific to the `mac` contact service type](#options-specific-to-the-mac-contact-service-type)
  * [Options specific to the `google` contact service type](#options-specific-to-the-google-contact-service-type)
  * [Options specific to the `windows` contact service type](#options-specific-to-the-windows-contact-service-type)
  * [Options specific to the `xml` contact service type](#options-specific-to-the-xml-contact-service-type)
* [Google Analytics options (the `google_analytics` section)](#google-analytics-options-the-googleanalytics-section)
* [Crash handler options (the `crash_handler` section)](#crash-handler-options-the-crashhandler-section)
* [Proxy options (the `proxy` section)](#proxy-options-the-proxy-section)
* [History options (the `history` section)](#history-options-the-history-section)
* [Example contents of the configuration data](#example-contents-of-the-configuration-data)

<!-- /TOC -->

## Introduction

The phone uses the *configuration data* to setup and configure itself. The *configuration data* is stored as XML data using the *UTF-8* encoding. Locally the phone holds its configuration data in an XML file. The name of the file is `Config.xml` and it is located in the `%APPDATA%\Zoiper5` directory (also known as the *application data directory* for the phone).

This location gets interpreted in a different way for different versions of Windows (here `USER` is a placeholder for the name of the current Windows user):

* on Windows versions before *Vista* it gets translated to `C:\Documents and Settings\USER\Application Data\Zoiper5`;
* on *Windows Vista* and later it gets translated to `C:\Users\USER\AppData\Roaming\Zoiper5`.

By default the phone creates *configuration data* file with default values if the file is not present.

This document explains the structure and the meaning of the elements used in the *configuration data*.

## Provisioning

The phone can be automatically configured using *configuration data* located on a remote machine (more specifically, an HTTP server), which is called *provisioning*.

For more info about how provisioning works and how to set it up, please refer to the [Zoiper5 provisioning documentation](provisioning.md).

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

* *UI*: this is name used in the user interface (UI) for the option (e.g. **Minimize to tray**), as well as its location there (e.g. located in **Settings** -> **GUI** -> **Look and Feel** -> **Behaviour**).  If it is not named explicitly in the UI, the interface element used for modifying the option's value will be described.
* *Type*: this is the data type used for storing the option's value.  The following data types are recognized :
  * ***integer***: this is an option whose value is an integer (e.g. `0`, `3`, `-5`, *etc*).  It is usually represented in the UI by an input field (i.e. a textbox) in which only digits can be entered.
  * ***string***: this is an option whose value is a string (i.e. a sequence of characters).  It is usually represented in the UI by an input field (i.e. a textbox).
  * ***boolean***: this is an *on*/*off* option (i.e. with two possible values).  It is usually represented by a checkbox in the UI.  The possible values for this type are as follows:
    * `true`: means that the option is turned *on* (i.e. a checked checkbox in the UI).
    * `false`: means that the option is turned *off* (i.e. an unchecked checkbox in the UI).
  * ***date/time***: this is an option whose value is a date-time object (e.g. `2006-01-02 15:04:05`).
  * ***enumeration***: this is an option whose value is a number from a given range (*enumeration*) of numbers (e.g. `3` from the enumeration `0`-`11`).
  * ***text enumeration***: this is an option whose value is a string from a given set (*enumeration*) of strings (e.g. `udp` from the enumeration {`udp`, `tcp`, `tls`}).
* *Default value*: this is the value which gets assigned to the option when the *configuration data* is created (e.g. on first start), or when a new account, *etc* is added.

## Main sections

* `options`: the main configuration section (it corresponds to the root element of the document).
  * `general`: a section for general options.
  * `audio`: a section for options related to audio.
  * `hid`: a section for options related to telephony HIDs.
  * `codecs`: a section with several sub-sections defining the global codecs used, where each codec has its own `codec` section.
    * This section is also encountered in the SIP- and IAX-specific parts of the `account` options section.
    * The options in this section are not account-specific.  They are rather used for calls which are not created from or matched with a specific account.
    * `codec`: a section for options related to a specific codec.
  * `accounts`: a section with several sub-sections defining the accounts used, where each account has its own `account` section.
    * `account`: a section for options related to a specific account.
  * `sip_options`: a section for options related to the SIP protocol.
    * The options in this section are not account-specific.
  * `iax_options`: a section for options related to the IAX protocol.
    * The options in this section are not account-specific.
  * `rtp_options`: a section for options related to the RTP protocol.
  * `stun`: a section for options related to the default STUN server.
  * `diagnostics`: a section for options related to diagnostics.
  * `network`: a section for options related to networking.
  * `chat`: a section for options related to the chat functionality.
  * `provision`: a section for options related to the provisioning functionality.
  * `popup`: a section for options related to popup dialogs.
  * `video`: a section for options related to video.
  * `skin`: a section for options related to skins.
  * `forwarding_and_auto_answer`: a section with options which are relevant when in FAAM (Forwarding and Auto Answer Mode).
  * `open_url_on_event`: a section with several sub-sections defining the URLs which should get opened when a given event occurs, where each URL has its own `open_url_on_event_item` section.
  * `gui`: a section for options related to the graphical user interface (GUI).
  * `profile`: a section for options related to the presence profile of the user.
  * `contact_services`: a section with several sub-sections defining the contact services, where each account has its own `contact_service` section.
  * `google_analytics`: a section for options related to the Google Analytics data collection functionality.
  * `crash_handler`: a section for options related to the crash reporting facility (i.e. Crashpad).
  * `proxy`: a section for options related to the proxy server.

## General options (the `general` section)

* `account`: this option defines the default selected account.
  * If no specific account is chosen for some action, usually this one is used.
  * UI: the option's value is changed using the **Choose account for dialing** dropdown.
  * Type: ***string*** (more specifically, a valid account identifier string).
  * Default value: initially *nothing* (i.e. an empty string).  When the first account gets created, its identifier becomes the ID of the default selected account.

* `auto_away`: this option defines the time of inactivity in seconds after which presence profile state gets switched to **Away**.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***integer***.
  * Default value: `180`.

* `minimize_to_tray`: this option determines whether the UI window disappears, leaving only the system tray icon, when the **Minimize** button gets clicked.
  * UI: **Minimize to tray** (located in **Settings** -> **GUI** -> **Behaviour** -> **Behaviour**).
  * Type: ***boolean***.
  * Default value: `false`.

* `minimize_on_close`: this option determines whether the UI window actually gets minimized instead of being closed when the **Close** button gets clicked.
  * UI: **Minimize on close** (located in **Settings** -> **GUI** -> **Behaviour** -> **Behaviour**).
  * Type: ***boolean***.
  * Default value: `true` (`false` on *Apple macOS*).

* `record_calls`: this option determines whether calls are recorded automatically (i.e. without the user explicitly pressing the **Record** button).
  * UI: **Enable call recording for all calls** (located in **Settings** -> **Features** -> **Automation** -> **Call events options**).
  * Type: ***boolean***.
  * Default value: `false`.

* `record_path`: this option defines the location where calls are recorded (i.e. in which directory).
  * This option only makes sense if `record_calls` is enabled.
  * UI: **Directory to store the recorded conversations** (located in **Settings** -> **Features** -> **Automation** -> **Call events options**).
  * Type: ***string*** (more specifically, a valid path to an existing directory).  It is selected using a **Browse For Folder** dialog which appears when the text field is clicked.
  * Default value: the path to the application data directory (for more info, see the *Introduction* section).

* `record_filename`: this option defines the format used for the filenames of call recordings.
  * This option only makes sense if `record_calls` is enabled.
  * In order to generate the final filename(s) for each recording, the following templates are substituted in the format string:
    * `{YYYY}`: the year of the recording;
    * `{MM}`: the month of the recording;
    * `{DD}`: the day (of the month) of the recording;
    * `{HH}`: the hour of the recording;
    * `{NN}`: the month of the recording;
    * `{SS}`: the second of the recording;
    * `{recording_part}`: which part of the recording is contained in this specific file.  If there is only one part, `{recording_part}` just gets substituted with `part1`.
  * UI: **File names for the recorded conversations** (located in **Settings** -> **Features** -> **Automation** -> **Call events options**).
  * Type: ***string*** (more specifically, a format string for a filename).
  * Default value: `recorded_conversation_{YYYY}-{MM}-{DD}-{HH}_{NN}_{SS}_part{recording_part}`.

* `record_format`: this option defines the format in which the recording will be written. Not the filename but the actual data.
  * This option only makes sense if `record_calls` is enabled.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***text enumeration***.
  * Possible values:
    * `wav`: 16-bit PCM uncompressed single channel (mono) wave audio.
    * `mp3`: MPEG layer 3 single channel (mono) audio. **Not available in all versions**
  * Default value: `wav`.

* `record_mode`: this option defines the mode in which the recording will be written. Not the filename but the actual data.
  * This option only makes sense if `record_calls` is enabled.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***text enumeration***.
  * Possible values:
    * `mixed`: the local and the remote audio will be mixed together in one channel.
    * `local`: only the local audio will be recorded. **Not available in all versions**
    * `remote`: only the remote audio will be recorded. **Not available in all versions**
    * `stereo`: the local and the remote audio will be recorded but in two channels. **Not available in all versions**
  * Default value: `mixed`.

* `record_path_bookmark`: this option defines the name of the *macOS Finder* bookmark for the call recording directory.
  * This option only makes sense if `record_calls` is enabled.
  * This option is only available on and specific to *Apple macOS*.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `always_on_top`: this option determines whether the UI window always stays on top of other windows.
  * UI: **Always on top** (located in **Settings** -> **GUI** -> **Behaviour** -> **Behaviour**).
  * Type: ***boolean***.
  * Default value: `false`.

* `reject_anonymous_calls`: this option determines whether the phone rejects anonymous calls (i.e. where the phone number of the caller cannot be seen).
  * UI: *none* (the option's value cannot be changed using UI interface).
  * Type: ***boolean***.
  * Default value: `false`.

* `auto_reject_calls`: this option determines whether the phone automatically rejects calls.
  * This option serves as the main switch for the `reject_call_on_*` options (i.e. they only work if this option is turned on).
      If the option is enabled, but none of the `reject_call_on_*` options are switched on, all calls are rejected.
  * UI: **Auto reject calls if status is set to** (located in **Settings** -> **Features** -> **Automation** -> **Call events options**).
  * Type: ***boolean***.
  * Default value: `false`.

* `reject_call_on_invisible`: this option determines whether the phone rejects calls when the presence profile state is **Invisible**.
  * This option only makes sense if `auto_reject_calls` is enabled.
  * UI: **Invisible** (located in **Settings** -> **Features** -> **Automation** -> **Call events options** -> **Auto reject calls if status is set to**).
  * Type: ***boolean***.
  * Default value: `false`.

* `reject_call_on_away`: this option determines whether the phone rejects calls when the presence profile state is **Away**.
  * This option only makes sense if `auto_reject_calls` is enabled.
  * UI: **Away** (located in **Settings** -> **Features** -> **Automation** -> **Call events options** -> **Auto reject calls if status is set to**).
  * Type: ***boolean***.
  * Default value: `false`.

* `reject_call_on_busy`: this option determines whether the phone rejects calls when the presence profile state is **Busy**.
  * This option only makes sense if `auto_reject_calls` is enabled.
  * UI: **Busy** (located in **Settings** -> **Features** -> **Automation** -> **Call events options** -> **Auto reject calls if status is set to**).
  * Type: ***boolean***.
  * Default value: `false`.

* `reject_call_on_phone`: this option determines whether the phone rejects calls when the presence profile state is **On the phone**.
  * This option only makes sense if `auto_reject_calls` is enabled.
  * UI: **On the phone** (located in **Settings** -> **Features** -> **Automation** -> **Call events options** -> **Auto reject calls if status is set to**).
  * Type: ***boolean***.
  * Default value: `false`.

* `reject_call_on_lunch`: this option determines whether the phone rejects calls when the presence profile state is **Out to lunch**.
  * This option only makes sense if `auto_reject_calls` is enabled.
  * UI: **Out to lunch** (located in **Settings** -> **Features** -> **Automation** -> **Call events options** -> **Auto reject calls if status is set to**).
  * Type: ***boolean***.
  * Default value: `false`.

* `reject_call_on_brb`: this option determines whether the phone rejects calls when the presence profile state is **Be right back**.
  * This option only makes sense if `auto_reject_calls` is enabled.
  * UI: **Be right back** (located in **Settings** -> **Features** -> **Automation** -> **Call events options** -> **Auto reject calls if status is set to**).
  * Type: ***boolean***.
  * Default value: `false`.

* `new_call_auto_popup`: this option determines whether the UI window gets focused when an incoming call is received.
  * UI: **Focus window on incoming call** (located in **Settings** -> **GUI** -> **Behaviour** -> **Behaviour**).
  * Type: ***boolean***.
  * Default value: `false`.

* `new_call_blink`: this option determines whether the UI window blinks when an incoming call is received.
  * UI: **Blink window on incoming call** (located in **Settings** -> **GUI** -> **Behaviour** -> **Behaviour**).
  * Type: ***boolean***.
  * Default value: `true`.

* `command_line_call_auto_popup`: this option determines whether the UI window gets focused when a call is started from the command line.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `false`.

* `command_line_call_blink`: this option determines whether the UI window blinks when a call is started from the command line.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `false`.

* `check_for_updates`: this option determines whether the phone regularly checks whether there are any updates for the application.
  * UI: **Check for updates** (located in **Settings** -> **Features** -> **Automation** -> **General options**).
  * Type: ***boolean***.
  * Default value: `true`.

* `catch_protocol_requests`: this option determines whether the phone gets started (or whether the prescribed action is performed, if already started) when an URL gets opened which begins with (i.e. whose scheme is) `callto:`, `tel:`, or `zoiper:`.
  * This option is only available on *Microsoft Windows* and *Apple macOS*.
  * This option requires additional changes in the configuration of *Microsoft Windows* (i.e. the *Windows Registry*) to be made in order for it to work properly, and that's why it is preferable to change its value from the phone (which applies these modifications automatically).
  * UI: **Register Callto, sip: tel: URI's with operating system** (located in **Settings** -> **Features** -> **Automation** -> **Integration options**).
  * Type: ***boolean***.
  * Default value: `true`.

* `integrate_into_outlook`: this option determines whether the phone is integrated into *Microsoft Outlook* (i.e. whether the Zoiper Outlook plugin is authorized to communicate with Outlook).
  * This option is only available on and specific to *Microsoft Windows*.  In addition, *Microsoft Office* (with Outlook) is required to be installed in order for the option to work.
  * This option requires additional changes in the configuration of *Microsoft Outlook* to be made in order for it to work properly, and that's why it is preferable to change its value from the phone (which applies these changes automatically).
  * UI: **Integrate Zoiper5 into Microsoft Outlook** (located in **Settings** -> **Features** -> **Automation** -> **Integration options**).
  * Type: ***boolean***.
  * Default value: `true`.

* `integrate_into_addressbook`: this option determines whether the phone is integrated with the addressbook on *macOS* (i.e. whether it is authorized to use the addressbook as a contact service).
  * This option is only available on and specific to *Apple macOS*.
  * This option requires additional changes in the configuration of *macOS* to be made in order for it to work properly, and that's why it is preferable to change its value from the phone (which applies these changes automatically).
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `true`.

* `add_to_firewall`: this option determines whether the phone is present in the authorized application list for the *Windows Firewall* (so that the phone would be allowed to communicate through the firewall, i.e. create and accept network connections).
  * This option is only available on and specific to *Microsoft Windows*.
  * This option requires additional changes in the configuration of the *Windows Firewall* to be made in order for it to work properly, and that's why it is preferable to change its value from the phone (which applies these changes automatically).
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `true`.

* `start_with_os`: this option determines whether the phone is started every time the operating system boots.
  * This option requires additional changes in the operating system configuration to be made in order for it to work properly, and that's why it is preferable to change its value from the phone (which applies these changes automatically).
  * UI: **Start Zoiper5 with the operating system** (located in **Settings** -> **Features** -> **Automation** -> **General options**).
  * Type: ***boolean***.
  * Default value: `true`.

* `start_minimized`: this option determines whether the phone is started in minimized mode.
  * UI: **Start minimized** (located in **Settings** -> **GUI** -> **Behaviour** -> **Behaviour**).
  * Type: ***boolean***.
  * Default value: `false`.

* `use_custom_browser`: this option determines whether the phone uses a user-specified web browser to open URLs.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `false`.

* `custom_browser`: this option defines the browser which the phone uses to open URLs.
  * This option only makes sense if `use_custom_browser` is enabled.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `on_transfer_request_style`: this option determines whether the phone always accepts or always rejects call transfer requests.
  * UI: **On transfer request mode** (located in **Settings** -> **Features** -> **Automation** -> **Call events options**).
  * Type: ***text enumeration***.
  * Possible values:
    * `accept`: always accept transfer requests.
    * `reject`: always reject transfer requests.
  * Default value: `reject`

* `open_url_from_network`: this option determines whether the phone is able to open URLs received over a network (e.g. the Internet).
  * UI: **Handle open URLs sent in IAX header** (located in **Settings** -> **Features** -> **Automation** -> **General options**).
  * Type: ***boolean***.
  * Default value: `false`.

* `automatic_open_url_from_network`: this option determines whether the phone automatically opens URLs received over a network (e.g. the Internet).
  * This option only makes sense if `open_url_from_network` is enabled.
  * UI: **Automatically open URLs sent in IAX header** (located in **Settings** -> **Features** -> **Automation** -> **General options**).
  * Type: ***boolean***.
  * Default value: `false`.

## Audio options (the `audio` section)

The audio options are located on the **Settings** -> **Media** -> **Audio** page of the UI.

* `selected_input_device`: a section defining the selected audio input device which the phone uses for capturing voice.
  * `use_default`: this option defines if the system default input device should always be preferred over the named device.
    * Type: ***boolean***.
    * Default value: `true`.
  * `name`: this option defines the name of the audio input device to be used in case `use_default` is false.
    * Type: ***string*** (more specifically, a valid input device name).
    * Default value: *nothing* (i.e. an empty string).

* `selected_output_device`: a section defining the selected audio output device which the phone uses for outputting voice.
  * `use_default`: this option defines if the system default ouput device should always be preferred over the named device.
    * Type: ***boolean***.
    * Default value: `true`.
  * `name`: this option defines the name of the audio output device to be used in case `use_default` is false.
    * Type: ***string*** (more specifically, a valid output device name).
    * Default value: *nothing* (i.e. an empty string).

* `selected_ringing_device`: a section defining the selected audio ringing device which the phone uses for ringing on incoming calls.
  * `use_default`: this option defines if the system default output device should always be preferred over the named device.
    * Type: ***boolean***.
    * Default value: `true`.
  * `name`: this option defines the name of the audio ringing device to be used in case `use_default` is false.
    * Type: ***string*** (more specifically, a valid ringing device name).
    * Default value: *nothing* (i.e. an empty string).

* `selected_speaker_input_device`: a section defining the selected audio speaker input device which the phone uses for capturing voice.
  * `use_default`: this option defines if the system default input device should always be preferred over the named device.
    * Type: ***boolean***.
    * Default value: `true`.
  * `name`: this option defines the name of the audio speaker input device to be used in case `use_default` is false.
    * Type: ***string*** (more specifically, a valid speaker input device name).
    * Default value: *nothing* (i.e. an empty string).

* `selected_speaker_output_device`: a section defining the selected audio speaker output device which the phone uses for outputting voice.
  * `use_default`: this option defines if the system default output device should always be preferred over the named device.
    * Type: ***boolean***.
    * Default value: `true`.
  * `name`: this option defines the name of the audio speaker output device to be used in case `use_default` is false.
    * Type: ***string*** (more specifically, a valid speaker output device name).
    * Default value: *nothing* (i.e. an empty string).

* `use_mic_boost`: this option determines whether microphone boost is used when capturing voice.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `false`.

* `use_echo_cancellation`: this option determines whether echo cancellation is applied to the captured voice.
  * UI: **Echo cancellation** (located in the **Audio device selection** section).
  * Type: ***boolean***.
  * Default value: `true`.

* `ring_tone_file`: this option defines the path to the audio file used for the phones ringtone.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string*** (more specifically, a valid path to an existing file).
  * Default value: *nothing* (i.e. an empty string).  This means that no ringtone file is used and that the ringtone is generated by the phone instead.

* `pc_speaker_ring`: this option determines whether the phone also plays the ringtone through the integrated PC speaker in addition to ringing through an external audio device.
  * UI: **Ring also through pc speaker** (located in the **Extra features** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `mute_on_early_media`: this option determines whether the audio output of the phone gets muted when early media arrives for a call (so that the user won't hear it).
  * UI: **Mute early media (outgoing calls)** (located in the **Extra features** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `ring_when_talking`: this option determines whether the phone plays the ringtone when there already is an active call.
  * UI: **Ring when talking (incoming calls)** (located in the **Extra features** section).
  * Type: ***boolean***.
  * Default value: `true`.

* `disable_dtmf_sounds`: this option determines whether the phone abstains from playing DTMF sounds while dialing a number.
  * UI: **Disable DTMF sounds** (located in the **Extra features** section).
  * Type: ***boolean*** (`true` means *disable* and `false` means *do not disable*).
  * Default value: `false`.

* `input_volume`: this option defines the volume level for the input device.
  * UI: **Setup Input Device** (located on the dialog which appears after the *Play* button for the **Input Device** in the **Audio device selection** section is clicked).
  * Type: ***integer***.  The value is selected using a slider in the UI.
  * Default value: `50` (i.e. the middle of the slider in the UI).

* `output_volume`: this option defines the volume level for the output device.
  * UI: **Setup Output Device** (located on the dialog which appears after the *Play* button for the **Output Device** in the **Audio device selection** section is clicked).
  * Type: ***integer***.  The value is selected using a slider in the UI.
  * Default value: `50` (i.e. the middle of the slider in the UI).

* `use_alternate_timer`: this option determines whether the phone uses an alternate timer for audio devices.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `false`.

* `use_auto_mic_selection`: this option determines whether the phone selects the microphone (i.e. input) device automatically (i.e. whether it hides the choice from the user).
  * UI: **Automatic microphone selection** (located in the **Audio device selection** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `use_agc`: this option determines whether the phone uses automatic gain control to adjust audio input device volume.
  * UI: **Automatic gain control** (located in the **Audio device selection** section).
  * Type: ***boolean***.
  * Default value: `true`.

* `use_noise_suppression`: this option determines whether the phone applies noise suppression to audio output.
  * UI: **Noise suppression** (located in the **Audio device selection** section).
  * Type: ***boolean***.
  * Default value: `true`.

* `disable_ringing_sound`: this option determines whether the phone abstains from playing the ringtone at all.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean*** (`true` means *disable* and `false` means *do not disable*).
  * Default value: `false`.

## HID options (the `hid` section)

The HID options are located on the **Settings** -> **Features** -> **HID Integration** page of the UI.

* `use_generic_hid`: this option determines whether the phone is used with a generic telephony HID.
  * UI: **Use generic HID**.
  * Type: ***boolean***.
  * Default value: `false`.

* `use_jabra_hid`: this option determines whether the phone is used with a Jabra telephony HID.
  * UI: **Use Jabra HID**.
  * Type: ***boolean***.
  * Default value: `true`.

* `use_plantronics_hid`: this option determines whether the phone is used with a Plantronics telephony HID.
  * UI: **Use Plantronics HID (32-bit only)**.
  * Type: ***boolean***.
  * Default value: `true`.

* `use_sennheiser_hid`: this option determines whether the phone is used with a Sennheiser telephony HID.
  * UI: **Use Sennheiser HID**.
  * Type: ***boolean***.
  * Default value: `true`.

## Global codec options (the `codec` section)

The global codec options data is the same as the account codec options data but is used when no specific account can be matched for incoming calls. See [Codec account options](#codec-account-options).

## Account options (the `account` section)

The account options are located on the **Settings** -> **Accounts** page of the UI.  There is an entry for each account in the left pane for which the options are displayed in the right pane.

### Generic (protocol-independent) account options

These options are present for every account (regardless of its type).

* `ident`: this option defines the ID of the account.
  * This option's value should be unique.
  * This option is immutable (i.e. its value cannot be changed).
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string*** (more specifically, a valid account identifier).
  * Default value: initially *nothing* (i.e. an empty string).  The value gets automatically assigned by the phone during the creation of the account.

* `name`: this option defines the name of the account (as seen in the phone).
  * UI: The value of this option can be changed by clicking on the title of the right pane.
  * Type: ***string***.
  * Default value: initially *nothing* (i.e. an empty string).  The value gets assigned by the user during the creation of the account.

* `save_username`: this option determines whether the entered username for the account gets persisted (i.e. saved) in the configuration.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `true`.

* `username`: this option defines the username for the account (as used by its respective protocol).
  * UI: **Username** (located in the *protocol* **Credentials** section).
  * Type: ***string***.
  * Default value: initially *nothing* (i.e. an empty string).  The value gets assigned by the user during the creation of the account.

* `save_password`: this option determines whether the entered password for the account gets persisted (i.e. saved) in the configuration.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `true`.

* `password`: this option defines the password for the account (as used by its respective protocol).
  * UI: **Password** (located in the *protocol* **Credentials** section).
  * Type: ***string*** (more specifically, a password).
  * Default value: initially *nothing* (i.e. an empty string).  The value gets assigned by the user during the creation of the account.

* `register_on_startup`: this option determines whether the account gets registered when the phone gets started.
  * UI: **Register on startup** (located in the **Features** section).
  * Type: ***boolean***.
  * Default value: `true`.

* `do_not_play_ringback_tones`: this option determines whether the phone abstains from playing a ringback tone for calls made or received from this specific account.
  * UI: **Don't play ringback tones** (located in the **Features** section).
  * Type: ***boolean*** (`true` means *disable* and `false` means *do not disable*).
  * Default value: `false`.

* `voicemail_check_extension`: this option defines the extension that the phone dials on the server to listen to voicemail messages from this specific account.
  * UI: **Check voicemail** (located in the **Pre-configured Extensions** section).
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `voicemail_transfer_extension`: this option defines the extension that the phone dials on the server to leave a voicemail message from this specific account.
  * UI: **Transfer to voicemail** (located in the **Pre-configured Extensions** section).
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `force_rfc3264`: this option determines whether the phone forcibly holds calls made or received from this specific account according to RFC3264 (Cisco Unified Communications Manager).
  * UI: **Force rfc3264 hold (Cisco Unified Communications Manager)** (located in the **Compatibility modes** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `use_kpml`: this option determines whether the phone uses KPML with this specific account.
  * UI: **Send KPML (Cisco Unified Communications Manager)** (located in the **Compatibility modes** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `use_overlap_dialing`: this option determines whether the phone uses overlap dialing from this specific account.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `false`.

* `use_custom_ringtone`: this option determines whether the phone uses a custom ringtone (from an audio file) for calls made or received from this specific account.
  * UI: **Use custom ringtone** (located in the **Features** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `custom_ringtone_location`: this option defines the path to the audio file used as a custom ringtone for this specific account.
  * This option only makes sense when `use_custom_certificate` is enabled.
  * UI: **Custom ringtone** (located in the **Features** section).
  * Type: ***string*** (more specifically, a valid path to an existing audio file).
  * Default value: *nothing* (i.e. an empty string).  This means that no custom ringtone file is used and that the default ringtone is played instead by the phone.

* `use_custom_certificate`: this option determines whether and how the phone uses a custom certificate for encryption with this specific account.
  * UI: **Use certificate as** (located in the **Encryption** section).
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
  * UI: **Certificate file** (located in the **Encryption** section).
  * Type: ***string*** (more specifically, a valid path to an existing certificate file).
  * Default value: *nothing* (i.e. an empty string).  This means that no custom certificate file is used.

* `custom_certificate`: this option defines the custom certificate itself used for this specific account.
  * This option only makes sense when the value of the `use_custom_certificate` option is `configuration`.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string*** (more specifically, a valid certificate specification).
  * Default value: *nothing* (i.e. an empty string).  This means that no custom certificate is used.

* `mwi_subscribe_usage`: this option determines whether and how the phone subscribes for Message Waiting Information (MWI) to the server.  If the server supports it, the phone will inform the user of new voicemail messages received from this specific account.
  * UI: **'Voicemail Message Waiting Indicator (MWI)** (located in the **Features** section).
  * Type: ***text enumeration***.
  * Possible values:
    * `disabled`: This value means that no MWI subscription takes place.
    * `before`: This value means that the MWI subscription takes place before the account gets registered.
    * `after`: This value means that the MWI subscription takes place after the account gets registered.
    * `both`: This value means that the MWI subscription takes place both before and after the account gets registered.
  * Default value: `both`.

* `use_number_rewriting`: this option determines whether a default country code is used for numbers dialed from this specific account that do not have one, and whether an international call prefix is used for them.
  * UI: **Enable default country code and international prefix** (located in the **Number Rewriting** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `number_rewriting_country`: this option defines the default country code used for numbers dialed from this specific account that do not have one.
  * This option only makes sense when `use_number_rewriting` is enabled.
  * UI: **Default country for numbers without country code** (located in the **Number Rewriting** section).
  * Type: ***string*** (more specifically, a valid country code).
  * Default value: the country code for the current location.

* `number_rewriting_prefix`: this option defines the prefix used when dialing international numbers from this specific account.
  * This option only makes sense when `use_number_rewriting` is enabled.
  * UI: **Prefix to use for international calls** (located in the **Number Rewriting** section).
  * Type: ***string*** (more specifically, a valid phone number prefix).
  * Default value: *nothing* (i.e. an empty string).  This means that no international call prefix is used.

* `use_strip_dial_chars`: this option determines whether certain characters designated as delimiters are stripped from numbers dialed from this specific account.
  * UI: **Strip dial chars** (located in the **Number Rewriting** section).
  * Type: ***boolean***.
  * Default value: `true`.

* `strip_dial_chars`: this option defines the characters to be stripped from numbers dialed from this specific account.
  * This option only makes sense when `use_strip_dial_chars` is enabled.
  * UI: **Strip dial chars** (located in the **Number Rewriting** section; namely, the text field).
  * Type: ***string*** (more specifically, a set of characters to strip).
  * Default value: ` .-()[]{}`.

* `protocol`: this option determines which communication protocol is used when dialing from this specific account (i.e. the type of the account).
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***text enumeration***.
  * Possible values:
    * `sip`: SIP account.
    * `iax2`: IAX2 account.
    * `http_phone_control`: HTTP control account. **Not available in all versions**
  * Default value: `sip`.

* `rtcp_profile_type`: this option determines whether a RTCP feedback mechanism is used and the RTP profile type used for RTCP-based feedback.
  * UI: **RTCP Feedback** (located in the **Compatibility modes** section).
  * Type: ***text enumeration***.
  * Possible values:
    * `avp`: This value means that RTCP feedback is turned off.
    * `avpf`: This value means that RTCP feedback is turned on.  In this case only the RTP/AVPF RTP profile is offered through SDP (if the other peer does not support feedbacks, the call will be drop immediately).
    * `both`: This value means that RTCP feedback is turned on.  In this case both the RTP/AVP and RTP/AVPF RTP profiles are offered (media lines are duplicated for backward compatibility) through SDP (if the other peer supports RTCP feedbacks, it will be used with priority).
  * Default value: `avp`.

* `enabled_video_fmtp`: this option determines whether the phone should use the video FMTP protocol for the SIP server used for this account.
  * UI: **Enable video FMTP** (located in the **Compatibility modes** section).
  * Type: ***boolean***.
  * Default value: `true`.

#### SIP-specific account options

These options are only present for SIP accounts.

* `SIP_domain`: this option defines the hostname/IP address of the SIP registrar server used for this specific account.
  * UI: **Domain** (located in the **SIP Credentials** section).
  * Type: ***string*** (more specifically, a valid hostname or an IP address).  The hostname/IP address can optionally be followed by a colon and a port number, like this: `host:port`.
  * Default value: *nothing* (i.e. an empty string).

* `SIP_use_outbound_proxy`: this option determines whether the SIP server used for this specific account gets accessed using an outbound proxy.
  * UI: **Use outbound proxy** (located in the **Optional SIP credentials** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `SIP_outbound_proxy`: this option defines the hostname/IP address of the outbound proxy server used for accessing the SIP server used for this specific account.
  * This option forces the phone to use an optional proxy server instead of the automatically detected one.
  * This option only makes sense when `SIP_use_outbound_proxy` is enabled.
  * UI: **Outbound proxy** (located in the **Optional SIP credentials** section).
  * Type: ***string*** (more specifically, a valid hostname or an IP address).  The hostname/IP address can optionally be followed by a colon and a port number, like this: `host:port`.
  * Default value: *nothing* (i.e. an empty string).

* `SIP_transport_type`: this option determines the transport-layer protocol used for sending/receiving SIP packets from this specific account.
  * UI: **Transport** (located in the **Network related** section).
  * Type: ***text enumeration***.
  * Possible values:
    * `udp`: This value means that the User Datagram Protocol (UDP) is used for sending and receiving SIP packets.
    * `tcp`: This value means that the Transmission Control Protocol (TCP) is used for sending and receiving SIP packets.
    * `tls` This value means that the Transport Layer Security (TLS) protocol over TCP is used for sending and receiving of encrypted SIP packets.
  * Default value: `udp`.

* `SIP_use_auth_username`: this option determines whether an authentication username should be used for the SIP server used for this specific account.
  * UI: **Use auth. username** (located in the **Optional SIP credentials** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `SIP_auth_username`: this option defines the authentication username for the SIP server used for this specific account.
  * This is an optional username which is used when responding to a SIP authentication challenge.
  * It is recommended that the value of this option be left empty unless you have been explicitly instructed to fill this field by your provider or PBX.
  * This option only makes sense when `SIP_use_auth_username` is enabled.
  * UI: **Use auth. username** (located in the **Optional SIP credentials** section; namely, the text field).
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `SIP_callerId`: this option defines the caller ID name for the SIP server used for this specific account.
  * This is the name that gets displayed to somebody who does not have you on their contact list when you call them.
  * UI: **Caller ID Name** (located in the **Features** section).
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

<!-- The `SIP_callerNumber` option is deliberately omitted because its usage is deprecated (more specifically, it was not conforming to the SIP standard). -->

* `SIP_use_rport`: this option determines whether the phone uses the *rport* extension parameter for the SIP server used for this specific account.
  * This option is used so that the server can discover the public address and port of the user if there is a NAT between the user and the server.
  * This option is mainly useful with normal unfirewalled TCP and TLS connections.  That is why it is highly recommended to enable it for those protocols.
  * The default is to have *rport* disabled for UDP connections.  If *rport* is enabled for UDP connections along with STUN, STUN is preferred.
  * UI: **Use rport** (located in the **Network related** section).
  * Type: ***boolean***.
  * Default value: `true`.

* `SIP_use_rport_media`: this option determines whether the phone uses *rport*-discovered public addresses for media negotiations performed from this specific account.
  * This option is the last resort for NAT-related problems with missing audio for some broken implementations (e.g. when the client is behind a symmetric NAT in combination with a CUCM server).
  * This option can also be useful for some firewall/NAT/VPN setups where the port is not changed, only the private address is replaced with a public one.  When both rport and STUN are enabled, STUN is preferred.
  * It is recommended for this option to be disabled.  Enable it only if you absolutely know what you're doing.
  * UI: **Use rport media** (located in the **Network related** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `SIP_srtp_mode`: this option determines whether and how the phone uses SRTP to encrypt media exchanged via this specific account.
  * SRTP only works if used in combination with encrypted transport (i.e. TLS).
  * UI: **SRTP key negotiation** (located in the **Encryption** section).
  * Type: ***text enumeration***.
  * Possible values:
    * `none`: This value means that SRTP won't be used at all.
    * `sdes`: This value means SRTP with SDES is used.
  * Default value: `none`.

* `SIP_dtmf_style`: this option determines whether and how the phone sends DTMF tones (i.e. the DTMF band it uses) from this specific account.
  * UI: **DTMF mode** (located in the **Compatibility modes** section).
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
  * UI: **Use BLF** (located in the **Features** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `SIP_publish_presence`: this option determines whether the phone publishes the status of its presence profile to contacts on the contact list.
  * UI: **Publish presence** (located in the **Features** section).
  * Type: ***boolean***.
  * Default value: `false` (`true` if the `presence` feature is enabled).

* `SIP_subscribe_presence`: this option determines whether the phone subscribes for the presence profile status of contacts on the contact list.
  * UI: **Subscribe presence** (located in the **Features** section).
  * Type: ***boolean***.
  * Default value: `false` (`true` if the `presence` feature is enabled).

* `SIP_keep_alive_mode`: this option determines whether and how often the phone should send Keep-Alive requests to the SIP server used for this account.
  * UI: **Keep alive time-out** (located in the **Network related** section).
  * Type: ***text enumeration***.
  * Possible values:
    * `disabled`: This value means that no Keep-Alive requests are sent by the phone.
    * `default`: This value means that Keep-Alive requests are sent every 30 seconds for UDP or every 180 seconds for TCP.
    * `custom`: This value means that Keep-Alive requests are sent on a user-specified interval (configured in the `SIP_keep_alive_timeout` option).
  * Default value: `default`.

* `SIP_keep_alive_timeout`: this option defines a custom interval (in seconds) for Keep-Alive requests sent by the phone to the SIP server used for this account.
  * UI: **Keep alive custom interval** (located in the **Network related** section).
  * Type: ***integer***.
  * Default value: `30` (this one is for UDP).

* `SIP_use_cisco`: this option determines whether the phone should use Cisco-style server-side call forwarding.
  * This option only works if you configure the phone type on the **Cisco Call Manager** as **Cisco Softphone** instead of the standard 3rd-party SIP softphone.
  * UI: **Cisco Call forwarding (Cisco Unified Communications Manager)** (located in the **Compatibility modes** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `SIP_cisco_device_name`: this option defines the name of the Cisco device that should be used for Cisco-style server-side call forwarding.
  * This option only makes sense when `SIP_use_cisco` is enabled.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `zrtp`: a section for options related to the ZRTP media encryption protocol.
  * The ZRTP protocol is used along with SIP protocol only.
  * UI: The ZRTP options are located in the **Encryption** -> **ZRTP** section.

  * `enabled`: this option determines whether ZRTP is used for this account.
    * UI: **Enable ZRTP** (located in the **Encryption** section).
    * Type: ***boolean***.
    * Default value: `false`.

  * `hash_algorithms`: a section with several sub-sections defining the ZRTP hash algorithms used for this specific account.

    * `hash_algorithm`: a section for options related to a specific hash algorithm.
      * `name`: this option defines the name of the hash algorithm (as seen in the phone).
        * UI: *none* (the option's value cannot be changed using the UI).
        * Type: ***string***.
        * Default value: *nothing* (i.e. an empty string).

      * `id`: this option determines the type of the hash algorithm.
        * UI: the hash algorithm type (which determines the name of the hash algorithm) is listed in the **Selected Hash algorithms** column.
        * Type: ***enumeration*** (the hash algorithm type is chosen from a predefined list).
        * Possible values:
          * `0`: represents the *S256* hash algorithm.
          * `1`: represents the *S384* hash algorithm.
          * `2`: represents the *N256* hash algorithm (not supported yet).
          * `3`: represents the *N384* hash algorithm (not supported yet).
        * Default value: `0`.

      * `priority`: this option defines the priority of the hash algorithm among the others.  The phone orders the hash algorithms by priority when trying to establish a ZRTP-encrypted stream.  The lesser the number, the more likely for the hash algorithm to be chosen.
        * UI: the hash algorithm priority is manipulated by dragging the name of the hash algorithm in the *Available Hash algorithms* or *Selected Hash algorithms* columns.
        * Type: ***integer***.
        * Default value: `0`.

      * `selected`: this option determines whether the hash algorithm is selected (i.e. whether it is used at all).
        * UI: selected hash algorithms are placed in the *Selected Hash algorithms* column, while deselected ones are placed in the *Available Hash algorithms* column.
        * Type: ***boolean***.
        * Default value: `false`.

    * `cipher_algorithms`: a section with several sub-sections defining the ZRTP cipher algorithms used for this specific account.

      * `cipher_algorithm`: a section for options related to a specific cipher algorithm.

        * `name`: this option defines the name of the cipher algorithm (as seen in the phone).
          * UI: *none* (the option's value cannot be changed using the UI).
          * Type: ***string***.
          * Default value: *nothing* (i.e. an empty string).

        * `id`: this option determines the type of the cipher algorithm.
          * UI: the cipher algorithm type (which determines the name of the cipher algorithm) is listed in the **Selected Cipher algorithms** column.
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
          * UI: the cipher algorithm priority is manipulated by dragging the name of the cipher algorithm in the *Available Cipher algorithms* or *Selected Cipher algorithms* columns.
          * Type: ***integer***.
          * Default value: `0`.

        * `selected`: this option determines whether the cipher algorithm is selected (i.e. whether it is used at all).
          * UI: selected cipher algorithms are placed in the *Selected Cipher algorithms* column, while deselected ones are placed in the *Available Cipher algorithms* column.
          * Type: ***boolean***.
          * Default value: `false`.

    * `auth_tags`: a section with several sub-sections defining the ZRTP authentication tags used for this specific account.

      * `auth_tag`: a section for options related to a specific authentication tag.

        * `name`: this option defines the name of the authentication tag (as seen in the phone).
          * UI: *none* (the option's value cannot be changed using the UI).
          * Type: ***string***.
          * Default value: *nothing* (i.e. an empty string).

        * `id`: this option determines the type of the authentication tag.
          * UI: the authentication tag type (which determines the name of the authentication tag) is listed in the **Selected Auth. tags** column.
          * Type: ***enumeration*** (the authentication tag type is chosen from a predefined list).
          * Possible values:
            * `0`: represents the *HS32* authentication tag.
            * `1`: represents the *HS80* authentication tag.
            * `2`: represents the *SK32* authentication tag (not supported yet).
            * `3`: represents the *SK64* authentication tag (not supported yet).
        * Default value: `0`.

      * `priority`: this option defines the priority of the authentication tag among the others.  The phone orders the authentication tags by priority when trying to establish a ZRTP-encrypted stream.  The lesser the number, the more likely for the authentication tag to be chosen.
        * UI: the authentication tag priority is manipulated by dragging the name of the authentication tag in the *Available Auth. tags* or *Selected Auth. tags* columns.
        * Type: ***integer***.
        * Default value: `0`.

      * `selected`: this option determines whether the authentication tag is selected (i.e. whether it is used at all).
        * UI: selected authentication tags are placed in the *Selected Auth. tags* column, while deselected ones are placed in the *Available Auth. tags* column.
        * Type: ***boolean***.
        * Default value: `false`.

    * `key_agreement_methods`: a section with several sub-sections defining the ZRTP key agreement methods used for this specific account.

      * `key_agreement_method`: a section for options related to a specific key agreement method.
        * `name`: this option defines the name of the key agreement method (as seen in the phone).
          * UI: *none* (the option's value cannot be changed using the UI).
          * Type: ***string***.
          * Default value: *nothing* (i.e. an empty string).

      * `id`: this option determines the type of the key agreement method.
        * UI: the key agreement method type (which determines the name of the key agreement method) is listed in the **Selected Key agreement methods** column.
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
        * UI: the key agreement method priority is manipulated by dragging the name of the key agreement method in the *Available Key agreement methods* or *Selected Key agreement methods* columns.
        * Type: ***integer***.
        * Default value: `0`.

      * `selected`: this option determines whether the key agreement method is selected (i.e. whether it is used at all).
        * UI: selected key agreement methods are placed in the *Selected Key agreement methods* column, while deselected ones are placed in the *Available Key agreement methods* column.
        * Type: ***boolean***.
        * Default value: `false`.

    * `sas_encodings`: a section with several sub-sections defining the ZRTP SAS encodings used for this specific account.

      * `sas_encoding`: a section for options related to a specific SAS encoding.
        * `name`: this option defines the name of the SAS encoding (as seen in the phone).
          * UI: *none* (the option's value cannot be changed using the UI).
          * Type: ***string***.
          * Default value: *nothing* (i.e. an empty string).

        * `id`: this option determines the type of the SAS encoding.
          * UI: the SAS encoding type (which determines the name of the SAS encoding) is listed in the **Selected SAS encodings** column.
          * Type: ***enumeration*** (the SAS encoding type is chosen from a predefined list).
          * Possible values:
            * `0`: represents the *B32* SAS encoding.
            * `1`: represents the *B256* SAS encoding.
          * Default value: `0`.

        * `priority`: this option defines the priority of the SAS encoding among the others.  The phone orders the SAS encodings by priority when trying to establish a ZRTP-encrypted stream.  The lesser the number, the more likely for the SAS encoding to be chosen.
          * UI: the SAS encoding priority is manipulated by dragging the name of the SAS encoding in the *Available SAS encodings* or *Selected SAS encodings* columns.
          * Type: ***integer***.
          * Default value: `0`.

        * `selected`: this option determines whether the SAS encoding is selected (i.e. whether it is used at all).
          * UI: selected SAS encodings are placed in the *Selected SAS encodings* column, while deselected ones are placed in the *Available SAS encodings* column.
          * Type: ***boolean***.
          * Default value: `false`.

#### IAX-specific account options

These options are only present for IAX accounts.

* `IAX2_host`: this option defines the hostname or the IP address of the IAX server used for this account.
  * UI: **Server Hostname/IP** (located in the **IAX Credentials** section).
  * Type: ***string*** (more specifically, a valid hostname or an IP address).  The hostname/IP address can optionally be followed by a colon and a port number, like this: `host:port`.
  * Default value: *nothing* (i.e. an empty string).  The value gets assigned by the user during the creation of the account.

* `IAX2_context`: this option defines the context for the IAX server used for this account.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string*** (more specifically, a valid hostname or an IP address).  The hostname/IP address can optionally be followed by a colon and a port number, like this: `host:port`.
    * Default value: *nothing* (i.e. an empty string).

* `IAX2_callerId`: this option defines the caller ID name for the IAX server used for this account.
  * This is the name that gets displayed to somebody who does not have you on their contact list when you call them.
  * UI: **Caller ID Name** (located in the **Features** section).
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `IAX2_callerNumber`: this option defines the caller ID phone number for the IAX server used for this account.
  * This is the phone number that gets displayed to somebody who does not have you on their contact list when you call them.
  * UI: **Caller ID Number** (located in the **Features** section).
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `IAX2_dtmf_style`: this option determines whether and how the phone should send DTMF tones (i.e. which DTMF band it should use).
  * UI: **DTMF mode** (located in the **Compatibility modes** section).
  * Type: ***text enumeration***.
  * Possible values:
    * `outband`: This value means that DTMF tones are sent out-of-band.
    * `inband`: This value means that DTMF tones are sent in-band within the media (i.e. audio data) as sound.
    * `disabled`: This value means that no DTMF tones are sent by the phone.
  * Default value: `outband`.

#### Account options specific for the HTTP Phone Control protocol

* `HTTP_PHONE_CONTROL_host`: this option defines the hostname or the IP address of the HTTP Phone Control server used for this account.
  * UI: **Host** (located in the **Deskphone account** section).
  * Type: ***string*** (more specifically, a valid hostname or an IP address).  The hostname/IP address can optionally be followed by a colon and a port number, like this: `host:port`.
  * Default value: *nothing* (i.e. an empty string).

* `HTTP_PHONE_CONTROL_path`: this option defines the context (path) for the HTTP Phone Control server used for this account.
  * UI: **Path** (located in the **Deskphone account** section).
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `HTTP_PHONE_CONTROL_number`: this option defines the name of the "number" HTTP parameter used with the HTTP Phone Control protocol by this account.
  * UI: **Number parameter** (located in the **Action parameters** section).
  * Type: ***string***.
  * Default value: `number=`.

* `HTTP_PHONE_CONTROL_outgoing_uri`: this option defines the name of the "outgoing URI" HTTP parameter used with the HTTP Phone Control protocol by this account.
  * UI: **Outgoing URI parameter** (located in the **Action parameters** section).
  * Type: ***string***.
  * Default value: `outgoing_uri=`.

* `HTTP_PHONE_CONTROL_answer`: this option defines the name of the "answer" HTTP parameter used with the HTTP Phone Control protocol by this account.
  * UI: **Answer parameter** (located in the **Action parameters** section).
  * Type: ***string***.
  * Default value: `key=F4`.

* `HTTP_PHONE_CONTROL_hangup`: this option defines the name of the "hangup" HTTP parameter used with the HTTP Phone Control protocol by this account.
  * UI: **Hangup parameter** (located in the **Action parameters** section).
  * Type: ***string***.
  * Default value: `key=F4`.

### Codec account options

* `codecs`: a section with several sub-sections defining the codecs used for this specific account, where each codec has its own `codec` section.
  * UI: The options from this section are located in the **Advanced** -> **Audio codecs** / **Video codecs** sections.

  * `codec`: a section for options related to a specific codec.

    * `codec_id`: this option determines the type of the codec.
      * UI: the codec type (which determines the name of the codec) is listed in the **Selected codecs** column.
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
        * UI: the codec priority is manipulated by dragging the name of the codec in the **Available codecs** or **Selected codecs** columns.
        * Type: ***integer***.
        * Default value: `0`.

      * `enabled`: this option determines whether the codec is enabled (i.e. whether it is used at all).
        * UI: enabled codecs are placed in the **Selected codecs** column, while disabled ones are placed in the **Available codecs** column.
        * Type: ***boolean***.
        * Default value: `false`.

      * `bps`: this option defines the bitrate (*bps*) at which data is passed through the codec.
        * UI: *none* (the option's value cannot be changed using the UI).
        * Type: ***integer***.
        * Default value: `0`.

      * `dtx`: this option determines whether DTX is used with the codec.
        * UI: *none* (the option's value cannot be changed using the UI).
        * Type: ***boolean***.
        * Default value: `false`.

      * `vbr`: this option determines whether VBR is used with the codec.
        * UI: *none* (the option's value cannot be changed using the UI).
        * Type: ***boolean***.
        * Default value: `false`.

### STUN account options

* `stun`: a section for options related to the STUN server options per account.
  * UI: The per-account STUN options are on the **Account** settings page (located in the **Network related** section).

  * `use_stun`: this option determines whether and how the phone uses the STUN protocol.
    * UI: **Use STUN**.
    * Type: ***text enumeration***.
    * Possible values:
      * `disabled`: This value means that the account will not use STUN at all or there will not be a global STUN server.
      * `default`: This value means that the account will use the global STUN server. This value is not used in the case of a global STUN server itself.
      * `custom`: This value means that will use a custom STUN server i.e. the STUN server defined per-account.
    * Default value: `default`.

  * `stun_host`: this option defines the hostname or the IP address of the STUN server.
    * UI: **STUN server**.
    * Type: ***string*** (more specifically, a valid hostname).
    * Default value: `stun.zoiper.com`.

  * `stun_port`: this option defines the number of the port which the phone uses to communicate with the STUN server.
    * UI: **STUN port**.
    * Type: ***integer*** (more specifically, a valid port number, i.e. a number between `1` and `65535`).
    * Default value: `3478`.

  * `stun_refresh_period`: this option defines the interval (in seconds) on which *refresh* requests are sent by the phone to the STUN server.
    * UI: **STUN refresh period**.
    * Type: ***integer***.
    * Default value: `30`.

### Registration- and subscription-related account options

* `reregistration_mode`: this option determines whether and how often the registration of this account should expire (i.e. how often re-registration requests should be sent by the phone).
  * UI: **Registration expiry mode** (located in the **Network related** section).
  * Type: ***text enumeration***.
  * Possible values:
    * `default`: This value means that re-registration requests are sent every 30 seconds for UDP or every 600 seconds for TCP.
    * `custom`: This value means that re-registration requests are sent on a user-specified interval (configured in the `reregistration_time` option).
  * Default value: `default`.

* `reregistration_time`: this option defines a custom interval (in seconds) for re-registration requests sent by the phone to the SIP server used for this account.
  * UI: **Registration expiry** (located in the **Network related** section).
  * Type: ***integer***.
  * Default value: `60` when UDP is used, `600` when TCP is used.

* `resubscription_mode`: this option determines whether and how often the subscription of this account should expire (i.e. how often resubscription requests should be sent by the phone).
  * UI: **Subscription expiry mode** (located in the **Network related** section).
  * Type: ***text enumeration***.
  * Possible values:
    * `default`: This value means that resubscription requests are sent every 30 seconds for UDP or every 600 seconds for TCP.
    * `custom`: This value means that resubscription requests are sent on a user-specified interval (configured in the `resubscription_time` option).
  * Default value: `default`.

* `resubscription_time`: this option defines a custom interval (in seconds) for resubscription requests sent by the phone to the SIP server used for this account.
  * UI: **Subscription expiry** (located in the **Network related** section).
  * Type: ***integer***.
  * Default value: `60` when UDP is used, `600` when TCP is used.

* `send_typing_notification`: this option determines whether the phone should send typing notifications.
  * UI: **Enable sending of typing notification** (located in the **Features** section).
  * Type: ***boolean***.
  * Default value: `true`.

## SIP options (the `sip_options` section)

* `port`: this option defines the number of the port which the phone uses to communicate with SIP servers.
  * UI: **Port** (located in **Settings** -> **Features** -> **Advanced** -> **Network** -> **SIP options**).
  * Type: ***integer*** (more specifically, a valid port number, i.e. a number between `1` and `65535`).
  * Default value: `5060`.

* `use_random_port`: this option determines whether the phone uses a random port to communicate with SIP servers.
  * UI: **Open random port above 32000** (located in **Settings** -> **Features** -> **Advanced** -> **Network** -> **SIP options**).
  * Type: ***boolean***.
  * Default value: `true`.

* `tls_certificate_file`: this option defines the path to the file containing the TLS certificate used for TLS-encrypted SIP connections.
  * This option only makes sense when the value of the `SIP_transport_type` option for the account is set to `tls`.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string*** (more specifically, a valid path to an existing certificate file).
  * Default value: *nothing* (i.e. an empty string).  This means that no certificate file is used.

* `override_domain_name`: this option determines whether the phone overrides the domain name from the TLS certificate with a custom one.
  * UI: **Override domain name** (located in **Settings** -> **Features** -> **Advanced** -> **TLS Options**).
  * Type: ***boolean***.
  * Default value: `false`.

* `domain`: this option defines a custom domain name to override the one from the TLS certificate.
  * This option only makes sense when `override_domain_name` is enabled.
  * UI: **Domain Name** (located in **Settings** -> **Features** -> **Advanced** -> **TLS Options**).
  * Type: ***string*** (more specifically, a valid domain name).
  * Default value: *nothing* (i.e. an empty string).

* `use_domain_certificate`: this option determines whether the phone uses a custom certificate for the specified TLS domain name.
  * UI: **Load domain certificate** (located in **Settings** -> **Features** -> **Advanced** -> **TLS Options**).
  * Type: ***boolean***.
  * Default value: `false`.

* `domain_certificate_file`: this option defines the path to the file containing the custom certificate for the specified TLS domain name.
  * This option only makes sense when `use_domain_certificate` is enabled.
  * UI: **Certificate file** (located in **Settings** -> **Features** -> **Advanced** -> **TLS Options**).
  * Type: ***string*** (more specifically, a valid path to an existing certificate file).
  * Default value: *nothing* (i.e. an empty string).  This means that no certificate file is used.

* `use_only_strong_cipher`: this option determines whether the phone should only use strong cipher algorithms for TLS encryption.
  * UI: **Use only strong ciphers** (located in **Settings** -> **Features** -> **Advanced** -> **TLS Options**).
  * Type: ***boolean***.
  * Default value: `false`.

* `protocol_suite`: this option determines which transport-layer data encryption protocol is used by the phone.
  * UI: **Protocol suite** (located in **Settings** -> **Features** -> **Advanced** -> **TLS Options**).
  * Type: ***text enumeration***.
  * Possible values:
    * `sslv23-insecure`: This value means that the Secure Socket Layer (SSL) protocol is used for transport-layer data encryption.
      * This protocol is deprecated and should not be used. It is kept for legacy compatibility reasons.
    * `tlsv1`: This value means that the Transport Layer Security (TLS) protocol version 1.0 is used for transport-layer data encryption.
    * `tlsv1_1`: This value means that the Transport Layer Security (TLS) protocol version 1.1 is used for transport-layer data encryption.
    * `tlsv1_2`: This value means that the Transport Layer Security (TLS) protocol version 1.2 is used for transport-layer data encryption.
    * `tlsv1_3`: This value means that the Transport Layer Security (TLS) protocol version 1.3 is used for transport-layer data encryption.
  * Default value: `tlsv1_2`.

## IAX options (the `iax_options` section)

The global IAX options are located in the **Settings** -> **Features** -> **Advanced** -> **Network** -> **IAX options** section of the UI.

* `port`: this option defines the number of the port which the phone uses to communicate with IAX servers.
* UI: **Port**.
  * Type: ***integer*** (more specifically, a valid port number, i.e. a number between `1` and `65535`).
  * Default value: `4569`.

## RTP options (the `rtp_options` section)

The RTP options are located in the **Settings** -> **Features** -> **Advanced** -> **Network** -> **RTP options** section of the UI.

* `port`: this option defines the number of the port which the phone uses to communicate with RTP peers.
  * UI: **Port**.
  * Type: ***integer*** (more specifically, a valid port number, i.e. a number between `1` and `65535`).
    * Default value: `8000`.

* `use_random_port`: this option determines whether the phone uses a random port to communicate with RTP peers.
  * UI: **Open random port above 32000**.
  * Type: ***boolean***.
  * Default value: `false`.

* `session_name`: this option defines the session name which the phone uses when communicating with RTP peers.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string***.
  * Default value: `Z`.

* `user_name`: this option defines the username which the phone uses when communicating with RTP peers.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string***.
  * Default value: `Z`.

* `url`: this option defines the URL which the phone uses when communicating with RTP peers.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string*** (more specifically, a valid URL).
  * Default value: `http://www.zoiper.com`.

* `email`: this option defines the email address which the phone uses when communicating with RTP peers.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string*** (more specifically, a valid email address).
  * Default value: `sales@zoiper.com`.

## Global STUN options (the `stun` section)

The global STUN options are located in the **Settings** -> **Features** -> **Advanced** -> **Global STUN** section of the UI.

The global STUN options data is the same as the account STUN options data but is used when no specific account can be matched for calls. See [STUN account options](#stun-account-options).

## Diagnostic options (the `diagnostics` section)

The diagnostic options are located in the **Settings** -> **Help/About** -> **Diagnostic** -> **Diagnostic** section of the UI.

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

* `enable_verbose_event_logging`: this option enables logging in the event scheduler, enabling it will increase the size of log files by a lot, use carefully.
  * This option only makes sense if `enable_debug_log` is enabled.
  * Type: ***boolean***.
  * Default value: `false`.

## Network options (the `network` section)

There is no dedicated section for the network options in the UI.

* `signal_dscp`: this option determines the type of the signal DSCP.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***text enumeration***.
  * Possible values: `none`, `CS0`, `CS1`, `CS2`, `CS3`, `CS4`, `CS5`, `CS6`, `CS7`, `AF11`, `AF12`, `AF13`, `AF21`, `AF22`, `AF23`, `AF31`, `AF32`, `AF33`, `AF41`, `AF42`, `AF43`, `EF`.
    * Default value: `none`.

* `media_dscp`: this option determines the type of the media DSCP.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***text enumeration***.
  * Possible values: `none`, `CS0`, `CS1`, `CS2`, `CS3`, `CS4`, `CS5`, `CS6`, `CS7`, `AF11`, `AF12`, `AF13`, `AF21`, `AF22`, `AF23`, `AF31`, `AF32`, `AF33`, `AF41`, `AF42`, `AF43`, `EF`.
  * Default value: `none`.

## Chat options (the `chat` section)

* `play_sound`: this option determines whether the phone plays the *new message* sound when an incoming chat message is received.
  * UI: **On new chat message** (located in **Settings** -> **Media** -> **Audio** -> **Play sounds**).
  * Type: ***boolean***.
  * Default value: `true`.

* `hide_offline`: this option determines whether the phone hides the chat message input box for a contact when the presence profile state goes offline.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `false`.

* `new_message_auto_popup`: this option determines whether the the phone window gets focused when an incoming chat message is received.
  * UI: **Focus window on incoming chat** (located in **Settings** -> **GUI** -> **Behaviour** -> **Behaviour**).
  * Type: ***boolean***.
  * Default value: `false`.

* `new_message_blink`: this option determines whether the the phone window gets blinked when an incoming chat message is received.
  * UI: **Blink window on incoming chat** (located in **Settings** -> **GUI** -> **Behaviour** -> **Behaviour**).
  * Type: ***boolean***.
  * Default value: `true`.

* `new_message_sound_file`: this option defines the path to the audio file used for the phone's *new message* sound.
  * This option only makes sense if `play_sound` is enabled.
  * UI: the option is represented in the UI by the text field under the **On new chat message** checkbox (located in **Settings** -> **Media** -> **Audio** -> **Play sounds**).
  * Type: ***string*** (more specifically, a valid path to an existing file).  It is selected using the **Select custom message sound file** dialog which appears when the text field is clicked.
  * Default value: `NewMessage.wav`.

## Provisioning options (the `provision` section)

The provisioning options are located in the **Settings** -> **Features** -> **Lockdown & Provisioning** -> **Provisioning** settings section of the UI.

* `remember_username_password`: this option determines whether the entered provisioning credentials (i.e. username and password) get persisted (i.e. saved) in the configuration.
  * UI: **Remember me**.
  * Type: ***boolean***.
  * Default value: `false`.

* `username`: this option defines the username used for provisioning the phone.
  * UI: **Username**.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `password`: this option defines the password used for provisioning the phone.
  * UI: **Password**.
  * Type: ***string*** (more specifically, a password).
  * Default value: *nothing* (i.e. an empty string).

* `login_automatically`: this option determines whether automatic login is enabled for the provisioning server.
  * UI: **Auto Login**.
  * Type: ***boolean***.
  * Default value: `false`.

* `provisioned`: this option determines whether the phone has already been provisioned (in which case a second provisioning won't be necessary).
  * UI: *none* (the option's value cannot be changed using the UI).  The value of this option is automatically assigned by the phone.
  * Type: ***boolean***.
  * Default value: `false`.

## Popup options (the `popup` section)

The popup options are located in the **Settings** -> **GUI*** -> **Behaviour** -> **Show pop-up notifications when** settings section of the UI.

* `check_for_focus`: this option determines whether popup notifications are displayed only if the UI window is not focused.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `true`.

* `peer_status`: this option determines whether a popup notification are displayed by the phone when a contact goes online.
  * UI: **A contact comes online**.
  * Type: ***boolean***.
  * Default value: `true`.

* `im`: this option determines whether a popup notification are displayed by the phone when an incoming chat message gets received.
  * UI: **A chat message is received**.
  * Type: ***boolean***.
  * Default value: `true`.

* `call`: this option determines whether a popup notification are displayed by the phone when an incoming call gets received.
  * UI: **There is an incoming call**.
  * Type: ***boolean***.
  * Default value: `true`.

* `voice_mail`: this option determines whether a popup notification are displayed by the phone when an incoming voicemail message gets received.
  * UI: **There is a new voicemail**.
  * Type: ***boolean***.
  * Default value: `true`.

* `audio_device`: this option determines whether a popup notification are displayed by the phone when an audio device gets (dis)connected.
  * UI: **An audio device is (dis)connected**.
  * Type: ***boolean***.
  * Default value: `true`.

* `network`: this option determines whether a popup notification are displayed by the phone when a network change gets detected.
  * UI: **A network change is detected**.
  * Type: ***boolean***.
  * Default value: `true`.

* `use_native_popups`: this option determines whether the phone displays notifications using the native look-and-feel for the current platform.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `true`.

* `auto_close_time`: this option defines the time (in seconds) for which notifications are displayed by the phone.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***integer***.
  * Default value: `15`.

## Video options (the `video` section)

The video options are located in the **Settings** -> **Media** -> **Video** -> **Video options** section of the UI.

* `camera_device`: this option defines the name of the video input device (i.e. camera) which the phone uses for capturing video.
  * UI: **Camera device**.
  * Type: ***string*** (more specifically, a valid video input device name).  It is selected using a dropdown in the UI.
  * Default value: initially *nothing* (i.e. an empty string).  After the phone manages to discover a functioning video input device, this option's value becomes the name of the device.

* `capture_width`: this option determines the width (in pixels) of the video captured from the camera by the phone.
  * UI: **Capture size**.
  * Type: ***integer***.
  * Default value: `640`.

* `capture_height`: this option determines the height (in pixels) of the video captured from the camera by the phone.
  * UI: **Capture size**.
  * Type: ***integer***.
  * Default value: `480`.

* `capture_fps`: this option determines the frame rate of the video captured from the camera by the phone.
  * UI: **Capture frame rate**.
  * Type: ***text enumeration***.
  * Possible values:
    * `5fps`: This value means that video is captured from the camera by the phone at a rate of 5 fps (frames per second).
    * `15fps`: This value means that video is captured from the camera by the phone at a rate of 15 fps (frames per second).
    * `30fps`:  This value means that video is captured from the camera by the phone at a rate of 30 fps (frames per second).
  * Default value: `5fps`.

* `bit_rate`: this option defines the bit rate (in bps) of the video captured from the camera by the phone.
  * UI: **Bit rate**.
  * Type: ***integer***.
  * Default value: `256000`.

* `always_accept_video`: this option determines whether the phone always accepts video when offered.
  * UI: **Always accept video**.
  * Type: ***boolean***.
  * Default value: `false`.

## Skin options (the `skin` section)

The skin options are located in the **Settings** -> **GUI*** -> **Appearance** -> **Change theme** section of the UI.

* `selected`: this option defines the name of the skin used for the phone's UI (user interface).
  * UI: **Theme**.
  * Type: ***string*** (more specifically, an existing skin name).  The option is represented in the UI by a dropdown.
  * Default value: `default`.

## Call forwarding and auto-answer options (the `forwarding_and_auto_answer` section)

The call forwarding and auto-answer options are located on the **Settings** -> **Features** -> **Calls** page of the UI.

* `mode`: this option determines whether and how the phone forwards or auto-answers incoming calls.
  * This option only makes sense if the `enabled` option is `true`.
  * UI: **Call forwarding** and **Auto answer**.
  * Type: ***text enumeration***.
  * Possible values: `none`, `answer_delay`, `answer_instant`, `forward_delay`, `forward_instant`.  The option is represented in the UI by one checkbox and two radio button groups with the following choices:
    * `forward_instant`: This value means that the phone will forward incoming calls instantly after receiving them.
    * `forward_delay`: This value means that the phone will forward incoming calls after a user-defined timeout.
    * `answer_instant`:  This value means that that the phone will answer incoming calls instantly after receiving them.
    * `answer_delay`: This value means that the phone will answer incoming calls after a user-defined timeout.
    * `none`: This value means that the phone will neither forward nor auto-answer incoming calls.
  * Default value: `answer_instant`.

* `forward_seconds`: this option defines the timeout (in seconds) after which the phone forwards an incoming call.
  * This option only makes sense if the `enabled` option is `true` and the value of the `mode` option is `forward_delay`.
  * UI: **Forward after (1 - 600 secs)** (located in the **Call forwarding** section).
  * Type: ***integer***.
  * Default value: `30`.

* `forward_extension`: this option defines the extension to which the phone forwards the call.
  * This option only makes sense if the `enabled` option is `true` and the value of the `mode` option is `forward_delay` or `forward_instant`.
  * UI: **Forward target** (located in the **Call forwarding** section).
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `autoanswer_seconds`: this option defines the timeout (in seconds) after which the phone answers an incoming call.
  * This option only makes sense if the `enabled` option is `true` and value of the `mode` option is `answer_delay`.
  * UI: **Answer after (1 - 600 secs)** (located in the **Auto answer** section).
  * Type: ***integer***.
  * Default value: `30`.

* `play_sound_on_auto_answer`: this option determines whether the phone plays the *new message* sound when an incoming chat message is received.
  * This option only makes sense if the `enabled` option is `true` and the value of the `mode` option is `answer_delay` or `answer_instant`.
  * UI: **Play sound on auto answer**.
  * Type: ***boolean***.
  * Default value: `true`.

* `auto_answer_keep_setting`: this option determines whether the phone persists (i.e. save) the changes to the auto-answer settings to the configuration (so that they are available after restarting the phone).
  * This option only makes sense if the `enabled` option is `true` and the value of the `mode` option is `answer_delay` or `answer_instant`.
  * UI: **Keep settings after restart**.
  * Type: ***boolean***.
  * Default value: `false`.

* `enabled`: this option determines whether incoming call handling (i.e. forwarding or auto-answering incoming calls) is enabled in the phone.
  * UI: **Enable incoming call handling** (located in the **Call settings** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `auto_answer_with_video`: this option determines whether the phone uses video when auto-answering incoming calls.
  * This option only makes sense if the `enabled` option is `true` and the value of the `mode` option is `answer_delay` or `answer_instant`.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `false`.

* `accept_server_auto_answer`: this option determines whether the phone accepts server-side auto-answer requests.
  * This option only makes sense if the `enabled` option is `true` and the value of the `mode` option is `answer_delay` or `answer_instant`.
  * UI: **Accept server-side auto answer** (located in the **Call events options** section).
  * Type: ***boolean***.
  * Default value: `false`.

## Open-URL-on-event options (the `open_url_on_event` section)

The open-URL-on-event options are located in the **Settings** -> **Features** -> **Automation** -> **Event rules** section of the UI.  A dialog titled **Edit Open URL Rule** appears when the user clicks on an event rule entry from which the values of the various options associated with the event rule can be modified.

* `open_url_on_event_item`: a section for options related to a specific URL which gets opened (more specifically, on which an action is performed) when an event happens.
  * This section defines a single event reaction rule.
  * The rule-specific options are located on the **Edit Open URL Rule** dialog which gets opened when a specific rule gets clicked on the **Event rules** section or when the **Add rule** button gets clicked.
  * The options in this section only make sense if the value of the `enabled` option is `true`.

  * `type`: this option determines the type of the event to which the phone reacts.
    * UI: **On**.
    * Type: ***text enumeration***.
    * Possible values: `never`, `call`, `presence`.  The option is represented in the UI by a dropdown with the following choices:
      * `call`: This value means that the phone will react to changes in the state of the active call.
      * `presence`:  This value means that the phone will react to changes in the state of the presence profile.
      * `never`: This value means that this specific event reaction rule is disabled.
    * Default value: `never`.

  * `call_state`: this option determines the state to which the call transitions in order for the phone to react.
    * This option only makes sense (and is only displayed) if the value of the `type` option is `call`.
    * UI: **Call State changes to**.
    * Type: ***text enumeration***.
    * Possible values:
      * `ringing`: This value means that the phone will react when the active call becomes a ringing one.
      * `answer`:  This value means that the phone will react when the active call gets answered.
      * `rejected`: This value means that the phone will react when the active call gets rejected.
      * `hangup`: This value means that the phone will react when the active call is hung up.
    * Default value: `ringing`.

  * `call_dir`: this option determines the direction of the calls to whose state transitions the phone reacts.
    * This option only makes sense (and is only displayed) if the value of the `type` option is `call`.
    * UI: **And call direction is**.
    * Type: ***text enumeration***.
    * Possible values:
      * `both`: This value means that the phone will react to the state transitions of incoming and outgoing calls.
      * `in`: This value means that the phone will react to the state transitions of incoming calls.
      * `out`:  This value means that the phone will react to the state transitions of outgoing calls.
    * Default value: `both`.

  * `presence`: this option determines the state to which the presence profile transitions in order for the phone to react.
    * This option only makes sense (and is only displayed) if the value of the `type` option is `presence`.
       UI: **Presence changes to**.
    * Possible values: `offline`, `invisible`, `online`, `away`, `busy`, `phone`, `lunch`, `be_back`.

  * `method`: this option determines the action which the phone performs on the specified URL (in the `url` option) when the given event occurs.
    * UI: **Do action**.
    * Type: ***text enumeration***.
    * Possible values:
      * `url`: This value means that the phone will open the URL when the event occurs.
      * `program`:  This value means that the phone will open the application specified in the URL field (as a shell command) when the event occurs.
      * `rest_api`:  This value means that the phone will send a request to the REST API endpoint specified in the URL field when the event occurs.
    * Default value: `url`.

  * `filter`: this option defines a filter for the specified URL.
    * UI: **Filter**.
    * Type: ***string***.
    * Default value: *nothing* (i.e. an empty string).

  * `url`: this option defines the specific URL on which the given action is performed (e.g. opening).
    * UI: **Open URL / Run**.
    * Type: ***string*** (more specifically, a valid URL or a valid shell command).
    * Default value: *nothing* (i.e. an empty string).

## GUI options (the `gui` section)

The GUI options are located on the **Settings** -> **GUI*** page of the UI.

* `switch_contact_on_transfer`: this option determines whether the contact for a call gets changed when transferring the call.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `false`.

* `enable_chat_support`: this option determines whether chat is enabled for the phone.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `true`.

* `collapse_on_hangup`: this option determines whether the the phone window collapses after a call hangup.
  * UI: **Collapse on hangup** (located in the **Behaviour** -> **Behaviour** section).
  * Type: ***boolean***.
  * Default value: `false`.

* `language`: this option defines the language used for the phone's UI (user interface).
  * UI: **Language** (located in the **Appearance** -> **Language** section).
  * Type: ***string*** (more specifically, a valid language name in English).  The option is represented in the UI by a dropdown.
  * Default value: `english`.

* `auto_focus_window_on_incoming_chat`: this option determines whether the the phone window automatically gets focused on receiving an incoming chat message.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `true`.

* `auto_focus_window_on_incoming_call`: this option determines whether the the phone window automatically gets focused on receiving an incoming call.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***boolean***.
  * Default value: `true`.

* `custom_properties`: a section for custom options used by the phone's UI.
  * This section **must** have an attribute named `type` with value `subtree`.
  * The structure of this section (i.e. whatever options it contains) is defined by the UI with some rules being imposed by the phone.
  * Any element contained in this section **must** have an attribute named `type` which represents its type.  Possible values for this attribute: `boolean`, `integer`, `string`, `subtree`.
  * Elements with type subtree have the same requirements as the `custom_properties` section.

* `show_upgrade_wizard`: this option determines whether the phone displays the **Upgrade to Premium** wizard on startup.
  * UI: **Show PRO screen on start up** (located in the **Behaviour** -> **Behaviour** section).
  * Type: ***boolean***.
  * Default value: `true`.

## Presence profile options (the `profile` section)

* `last_online_manual_state`: this option determines the state of the phone's presence profile when the user switches back to *online* (i.e. registered) mode.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***text enumeration*** (the profile state is chosen from a predefined list).
  * Possible values: `invisible`, `online`, `away`, `busy`, `phone`, `lunch`, `be_back`.
  * Default value: `online`.

## Contact service options (the `contact_service` section)

The contact service options are located on the **Settings** -> **Contacts** page of the UI.  There is an entry for each contact service in the left pane for which the options are displayed in the right pane.

### Generic contact service options

These options are present for every contact service (regardless of its type).

* `ident`: this option defines the ID of the contact service.
  * This option's value should be unique.
  * This option is immutable (i.e. its value cannot be changed).
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string*** (more specifically, a valid contact service identifier).
  * Default value: *nothing* (i.e. an empty string).  The value gets automatically assigned by the phone during the creation of the contact service.

* `name`: this option defines the name of the contact service (as seen in the phone).
  * UI: The value of this option can be changed by clicking on the title of the right pane (except for the `local` and `windows` contact services where the value of the option is immutable).
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).  The value gets assigned by the user during the creation of the account.

* `type`: this option determines the type of the contact service.
  * This option is immutable (i.e. its value cannot be changed).
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***text enumeration***.
  * Possible values:
    * `local`:  This service is used for contacts stored locally by the phone.
      * There should be only one service of type `local`.
    * `ldap`:  This service is used for contacts sourced from a LDAP server.
    * `outlook`:  This service is used for contacts sourced from Microsoft Outlook's addressbook.
      * This option only makes sense on and is specific to *Microsoft Windows*.
    * `mac`:  This service is used for contacts sourced from the macOS addressbook.
      * This option only makes sense on and is specific to *Apple macOS*.
      * There should be only one service of type `mac`.
    * `google`:  This service is used for contacts sourced from a Google account.
    * `windows`:  This service is used for contacts sourced from the Windows addressbook (i.e. the `Contacts` folder in the Windows user directory).
      * This option only makes sense on and is specific to *Microsoft Windows*.
      * There should be only one service of type `windows`.
    * `xml`:  This service is used for contacts sourced from an XML file.
  * Default value: no default value.

* `enabled`: this option determines whether the contact service is enabled (i.e. whether it is used at all).
  * The value of this option is fixed to `true` for the `local` contact service (i.e. it cannot be disabled).
  * UI: **Enable**.
  * Type: ***boolean***.
  * Default value: `true`.  The option's value is determined by the account creation wizard.

* `account_mapping_type`: this option determines whether and how a dial account gets assigned for the contact service.
  * UI: **Use this account for dialing**.
  * Type: ***text enumeration***.
  * Possible values:
    * `none`: This value means that no dial account gets assigned for the contact service.
    * `default`: This value means that the default account for the phone will get assigned as a dial account for the contact service.
    * `custom`: This value means the account to be assigned as a dial account for the contact service gets stored in the `account_ident` option.
  * Default value: `default`.

* `account_ident`: this option defines the ID of the dial account assigned for the contact service.
  * This option's value should be unique.
  * This option only makes sense when the value of the `account_mapping_type` option is `custom`.
  * UI: **Use this account for dialing**.
  * Type: ***string*** (more specifically, a valid account identifier).
  * Default value: *nothing* (i.e. an empty string).

* `presence_account_mapping_type`: this option determines whether and how a presence account gets assigned for the contact service.
  * UI: **Use this account for presence**.
  * Type: ***text enumeration***.
  * Possible values:
    * `none`: This value means that no presence account gets assigned for the contact service.
    * `custom`: This value means the account to be assigned as a presence account for the contact service gets stored in the `presence_account_ident` option.
  * Default value: `none`.

* `presence_account_ident`: this option defines the ID of the presence account assigned for the contact service.
  * This option's value should be unique.
  * This option only makes sense when the value of the `presence_account_mapping_type` option is `custom`.
  * UI: **Use this account for presence**.
  * Type: ***string*** (more specifically, a valid account identifier).  
  * Default value: *nothing* (i.e. an empty string).

* `hide_contacts_without_name`: this option determines whether contacts from this service which do not have a name (i.e. it is an empty string) are hidden from the contact list.
  * UI: **Hide contacts without name** (located in the **Filters** section).
  * Type: ***boolean***.
  * Default value: `true`.

* `hide_contacts_without_phone`: this option determines whether contacts from this service which do not have a phone number (i.e. it is an empty string) are hidden from the contact list.
  * UI: **Hide contacts without phone** (located in the **Filters** section).
  * Type: ***boolean***.
  * Default value: `true`.

* `restart_time`: this option determines the restart interval used when retrying failed contact service connections.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***integer***.
  * Default value: `30`.

### Options specific to the `local` contact service type

There are no options which are only present for **Local** contact services.

* The value of the `ident` option is supposed to be `ContactServiceLocal`.

* The default value for the `hide_contacts_without_name` option is `false`.

* The default value for the `hide_contacts_without_phone` option is `false`.

### Options specific to the `ldap` contact service type

These options are only present for **LDAP** contact services.

* `limit`: this option determines the limit for the number of contacts which can be retrieved using a single query of this service by the phone.
  * This is the number of contacts from this service that are displayed in the contact list after the service gets enabled.
  * In addition, this is the maximum number of contacts from this service which can be found when searching through the contact list.
  * UI: **Limit the number of search results to**.
  * Type: ***integer***.
  * Default value: `0`.

* `host`: this option defines the hostname or the IP address of the LDAP server used for this contact service.
  * UI: **Hostname**.
  * Type: ***string*** (more specifically, a valid hostname or an IP address).  The hostname/IP address can optionally be followed by a colon and a port number, like this: `host:port`.
  * Default value: *nothing* (i.e. an empty string).

* `username`: this option defines the username for the LDAP server used for this contact service.
  * UI: **Username**.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `password`: this option defines the password for the LDAP server used for this contact service.
  * UI: **Password**.
  * Type: ***string*** (more specifically, a password).
  * Default value: *nothing* (i.e. an empty string).

* `root`: this option defines the Domain Component (DC) of the LDAP server used for this contact service.
  * UI: **DC**.
  * Type: ***string*** (more specifically, a valid DC name string).
  * Default value: *nothing* (i.e. an empty string).

* `attribute_mappings`: this section defines the names of the contact properties to which LDAP attributes are mapped (i.e. *attribute mappings*).
  * UI: The attribute mappings are located in the **Assign attribute** section.

  * `attribute_mapping`: this section defines an attribute mapping.

    * `property_type`: this option defines the contact property to which the LDAP attribute is mapped.
      * UI: *none* (the option's value cannot be changed using the UI).
      * Type: ***text enumeration*** (the contact property is chosen from a predefined list).
      * Possible values: `last_name`, `first_name`, `middle_name`, `display_name`, `company`, `position`, `profession`, `home_fax`, `work_fax`, `home_mail`, `work_mail`, `home_cell`, `work_cell`, `home_phone`, `work_phone`, `home_pager`, `work_pager`, `home_custom`, `work_custom`, `home_ipphone`, `work_ipphone`, `home_zip`, `work_zip`, `home_city`, `work_city`, `home_state`, `work_state`, `home_street`, `work_street`, `home_country`, `work_country`.

    * `attribute_name`: this option defines the name of the LDAP attribute which is being mapped.
      * UI: the text boxes in the **Settings** -> **Contacts** -> *contact service name* -> **Assign attribute** section.
      * Type: ***string*** (more specifically, a valid LDAP attribute name).
      * Default value: *nothing* (i.e. an empty string).

    * `enabled`: this option determines whether the attribute mapping is enabled (i.e. whether it is used at all).
      * UI: the checkboxes in the **Settings** -> **Contacts** -> *contact service name* -> **Assign attribute** section.
      * Type: ***boolean***.
      * Default value: `true`.

  * The default mappings which are present when the LDAP contact service is created are as follows:
    * `property_type`: `first_name`, `attribute_name`: `givenName`.
    * `property_type`: `middle_name`, `attribute_name`: `initials`.
    * `property_type`: `last_name`, `attribute_name`: `sn`.
    * `property_type`: `display_name`, `attribute_name`: `cn`.
    * `property_type`: `company`, `attribute_name`: `company`.
    * `property_type`: `position`, `attribute_name`: `department`.
    * `property_type`: `profession`, `attribute_name`: `title`.
    * `property_type`: `work_country`, `attribute_name`: `co`.
    * `property_type`: `work_city`, `attribute_name`: `l`.
    * `property_type`: `work_mail`, `attribute_name`: `mail`.
    * `property_type`: `work_phone`, `attribute_name`: `telephoneNumber`.
    * `property_type`: `work_cell`, `attribute_name`: `mobile`.
    * `property_type`: `work_pager`, `attribute_name`: `pager`.
    * `property_type`: `work_ipphone`, `attribute_name`: `ipPhone`.
    * `property_type`: `work_fax`, `attribute_name`: `facsimileTelephoneNumber`.

* `authentication_flags`: this option determines the kind of authentication used for this contact service.
  * UI: **Authentication type**.
  * Type: ***text enumeration***.
  * Possible values:
    * `simple`: This value means that Simple authentication will be used for the contact service.
    * `sasl_plain`: This value means that SASL authentication using the PLAIN mechanism will be used for the contact service.
    * `sasl_login`: This value means that SASL authentication using the LOGIN mechanism will be used for the contact service.
    * `sasl_digest_md5` This value means that SASL authentication using the DIGEST-MD5 mechanism will be used for the contact service.
  * Default value: `simple`.

### Options specific to the `outlook` contact service type

These options are only present for **Outlook** contact services.

* `profile`: this option defines the name of the Outlook profile used for this contact service.
  * UI: **Outlook Profile"**.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `password`: this option defines the password for the Outlook profile used for this contact service.
  * UI: **Outlook Profile Password"**.
  * Type: ***string*** (more specifically, a password).
  * Default value: *nothing* (i.e. an empty string).

### Options specific to the `mac` contact service type

There are no options which are only present for **Mac** contact services.

### Options specific to the `google` contact service type

These options are only present for **Google** contact services.

* `limit`: this option defines the limit for the number of contacts which can be retrieved using a single query of this service by the phone.
  * This is the number of contacts from this service that are displayed in the contact list after the service gets enabled.
  * In addition, this is the maximum number of contacts from this service which can be found when searching through the contact list.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***integer***.
  * Default value: `0`.

* `access_token`: this option defines the access token for the Google account used for this contact service.
  * UI: **Enter your access token here** (located on the **Authenticate Google Contacts** dialog which appears after the **Generate**/**Change** button is clicked; the **Open URL** button on the dialog should also be clicked for the textbox to appear). The user receives the access token after clicking the **Generate** (or **Change**, if the service has already been authenticated) button on the contact service page and following the steps to authenticate the phone for that account.
  * Type: ***string*** (more specifically, an access token).
  * Default value: *nothing* (i.e. an empty string).

* `refresh_token`: this option defines the refresh token for the Google account used for this contact service.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string*** (more specifically, a refresh token).
  * Default value: *nothing* (i.e. an empty string).

### Options specific to the `windows` contact service type

There are no options which are only present for **Windows** contact services.

### Options specific to the `xml` contact service type

These options are only present for **XML** contact services.

* `uri`: this option defines the URI of (i.e. the URL/path to) the XML document used for this contact service.
  * UI: **Local path/URL**.
  * Type: ***string*** (more specifically, a valid URI, i.e. a valid URL or a valid path to an existing XML file).
  * Default value: *nothing* (i.e. an empty string).

* `username`: this option defines the username used to access the XML used for this contact service.
  * This option makes sense only when the XML is accessed using HTTP (i.e. when the URL is an HTTP one).
  * UI: **Username**.
  * Type: ***string***.
  * Default value: *nothing* (i.e. an empty string).

* `password`: this option defines the password used to access the XML used for this contact service.
  * This option makes sense only when the XML is accessed using HTTP (i.e. when the URL is an HTTP one).
  * UI: **Password**.
  * Type: ***string*** (more specifically, a password).
  * Default value: *nothing* (i.e. an empty string).

* `auth_type`: this option determines how the phone gets authenticated in order to access the XML used for this contact service.
  * This option makes sense only when the XML is accessed using HTTP (i.e. when the URL is an HTTP one).
  * UI: **Authentication type**.
  * Type: ***text enumeration***.
  * Possible values:
    * `none`: This value means that the phone will not use authentication for HTTP.
    * `basic`: This value means that the phone will use basic access authentication for HTTP (using the `WWW-Authenticate` and `Authorization` headers).
    * `url`: This value means that the phone will encode the authentication parameters for HTTP in the URL itself.
  * Default value: `none`.

## Google Analytics options (the `google_analytics` section)

The Google Analytics options are located in the **Settings** -> **Features** -> **Advanced** -> **Google Analytics*** section of the UI.

* `ident`: this option defines the ID of the Google Analytics object.
  * This option's value should be unique.
  * This option is immutable (i.e. its value cannot be changed).
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***string*** (more specifically, a valid Google Analytics identifier).
  * Default value: initially *nothing* (i.e. an empty string).  The value gets automatically assigned by the phone during startup.

* `enabled`: this option determines whether Google Analytics is enabled.
  * UI: **Enable Google Analytics**.
  * Type: ***boolean***.
  * Default value: `false` (`true` if the `google_analytics` feature is enabled).

* `queue_message_count`: this option defines the number of Google Analytics messages to be kept in the phone's message queue.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***integer***.
  * Default value: `10`.

## Crash handler options (the `crash_handler` section)

The crash handler options are located in the **Settings** -> **Help/About** -> **Diagnostic** -> **Diagnostic** section of the UI.

* `enabled`: this option determines whether the crash handler is enabled.
  * *Crashpad* is the crash dump facility for the phone on Windows and Mac.
  * *Breakpad* is the crash dump facility for the phone on Windows and Mac.
  * UI: **Enable crash dump**.
  * Type: ***boolean***.
  * Default value: `true`.

* `upload_dumps`: this option determines whether the crash handler sends crash dump reports to the phone's crash server.
  * UI: **Automatically send crash dump to Zoiper5 server"**.
  * Type: ***boolean***.
  * Default value: `true`.

## Proxy options (the `proxy` section)

There is no dedicated section for the proxy options in the UI.

* `mode`: this option determines whether and how the proxy server is used.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***text enumeration***.
  * Possible values: `disabled`, `manual`, `system`.
    * `disabled`: This value means that no proxy server will be used.
    * `manual`: This value means that the proxy server specified by the `custom_proxy` option will be used.
    * `system`: This value means that the proxy server specified in the network settings of the OS will be used.
  * Default value: `disabled`.

* `custom_proxy`: this section defines the proxy server to be used.
  * `hostname`: this option defines the hostname or the IP address of the proxy server.
    * UI: *none* (the option's value cannot be changed using the UI).
    * Type: ***string*** (more specifically, a valid hostname or an IP address).
    * Default value: *nothing* (i.e. an empty string).
  * `port`: this option defines the port number of the proxy server.
    * UI: *none* (the option's value cannot be changed using the UI).
    * Type: ***integer***.
    * Default value: `0`.

## History options (the `history` section)

There is no dedicated section for the history options in the UI.

* `limit_mode`: This option defines the mode of limiting the history count
  * UI: *none* (the option's value cannot be changed using the UI)
  * Type: ***text enumeration***.
  * Possible Values: default or custom.
  * Default value: `default`.

* `limit`: This option defines the number of allowed history entries  
  * UI: *none* (the option's value cannot be changed using the UI)
  * Type: ***integer***.
  * Default value: `2000`.

* `allowed_overflow_percent_mode`: This option define the mode of extra allowed history entries above the specified limit before zoiper automatically delete them.
  * UI: *none* (the option's value cannot be changed using the UI).
  * Type: ***text enumeration***.
  * Possible Values:
    * `default`: This value means that default of 20% overflow will be used.
    * `custom`: This value means that a custom user-specified percent will be used  (configured in the `allowed_overflow_percent` option).
  * Default value: `default`.

* `allowed_overflow_percent` this option define the number of allowed entries above the specified limit before zoiper automatically deletes them in bulks, this option only applicable when "custom" value is configured for  "allowed_overflow_percent_mode"
  * UI: *none* (the option's value cannot be changed using the UI)
  * Type: ***integer***.
  * Default value: `20`.

## Example contents of the configuration data

```xml
<?xml version="1.0"?>
<options>
  <general>
    <account>Z599e3b298c0b03439f9d85cf</account>
    <auto_away>180</auto_away>
    <minimize_to_tray>false</minimize_to_tray>
    <minimize_on_close>true</minimize_on_close>
    <record_calls>false</record_calls>
    <record_path>/home/radoslav/.Zoiper5</record_path>
    <record_filename>recorded_conversation_{YYYY}-{MM}-{DD}-{HH}_{NN}_{SS}_part{recording_part}</record_filename>
    <always_on_top>false</always_on_top>
    <reject_anonymous_calls>false</reject_anonymous_calls>
    <auto_reject_calls>false</auto_reject_calls>
    <reject_call_on_invisible>false</reject_call_on_invisible>
    <reject_call_on_away>false</reject_call_on_away>
    <reject_call_on_busy>false</reject_call_on_busy>
    <reject_call_on_phone>false</reject_call_on_phone>
    <reject_call_on_lunch>false</reject_call_on_lunch>
    <reject_call_on_brb>false</reject_call_on_brb>
    <new_call_auto_popup>false</new_call_auto_popup>
    <new_call_blink>true</new_call_blink>
    <command_line_call_auto_popup>false</command_line_call_auto_popup>
    <command_line_call_blink>false</command_line_call_blink>
    <check_for_updates>true</check_for_updates>
    <catch_protocol_requests>true</catch_protocol_requests>
    <start_with_os>true</start_with_os>
    <start_minimized>false</start_minimized>
    <use_custom_browser>false</use_custom_browser>
    <custom_browser/>
    <on_transfer_request_style>reject</on_transfer_request_style>
    <open_url_from_network>false</open_url_from_network>
    <automatic_open_url_from_network>false</automatic_open_url_from_network>
  </general>
  <audio>
    <input_device>Autoselect (Logitech BRIO Analog Stereo)</input_device>
    <output_device>Autoselect (Plantronics Blackwire 3220 Series Analog Stereo)</output_device>
    <ringing_device/>
    <speaker_input_device/>
    <speaker_device/>
    <use_mic_boost>false</use_mic_boost>
    <use_echo_cancellation>true</use_echo_cancellation>
    <ring_tone_file/>
    <pc_speaker_ring>false</pc_speaker_ring>
    <mute_on_early_media>false</mute_on_early_media>
    <ring_when_talking>true</ring_when_talking>
    <disable_dtmf_sounds>false</disable_dtmf_sounds>
    <input_volume>50</input_volume>
    <output_volume>50</output_volume>
    <use_alternate_timer>false</use_alternate_timer>
    <use_auto_mic_selection>false</use_auto_mic_selection>
    <use_agc>true</use_agc>
    <use_noise_suppression>true</use_noise_suppression>
    <disable_ringing_sound>false</disable_ringing_sound>
  </audio>
  <hid/>
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
  <sip_options>
    <port>5060</port>
    <use_random_port>true</use_random_port>
    <tls_certificate_file/>
    <override_domain_name>false</override_domain_name>
    <domain/>
    <use_domain_certificate>false</use_domain_certificate>
    <domain_certificate_file/>
    <use_only_strong_cipher>false</use_only_strong_cipher>
    <protocol_suite>tlsv1_2</protocol_suite>
  </sip_options>
  <iax_options>
    <port>4569</port>
  </iax_options>
  <rtp_options>
    <port>8000</port>
    <use_random_port>false</use_random_port>
    <session_name>Z</session_name>
    <user_name>Z</user_name>
    <url>http://www.zoiper.com</url>
    <email>sales@zoiper.com</email>
  </rtp_options>
  <stun>
    <use_stun>default</use_stun>
    <stun_host>stun.zoiper.com</stun_host>
    <stun_port>3478</stun_port>
    <stun_refresh_period>30</stun_refresh_period>
  </stun>
  <diagnostics>
    <enable_debug_log>false</enable_debug_log>
    <enable_extra_dmp>false</enable_extra_dmp>
    <enable_audio_debug>false</enable_audio_debug>
  </diagnostics>
  <network>
    <signal_dscp>CS0</signal_dscp>
    <media_dscp>CS0</media_dscp>
  </network>
  <chat>
    <play_sound>true</play_sound>
    <hide_offline>false</hide_offline>
    <new_message_auto_popup>false</new_message_auto_popup>
    <new_message_blink>true</new_message_blink>
    <new_message_sound_file>NewMessage.wav</new_message_sound_file>
  </chat>
  <provision>
    <remember_username_password>false</remember_username_password>
    <login_automatically>false</login_automatically>
    <provisioned>false</provisioned>
  </provision>
  <popup>
    <check_for_focus>true</check_for_focus>
    <peer_status>true</peer_status>
    <im>true</im>
    <call>true</call>
    <voice_mail>true</voice_mail>
    <audio_device>true</audio_device>
    <network>true</network>
    <use_native_popups>true</use_native_popups>
    <auto_close_time>15</auto_close_time>
  </popup>
  <video>
    <camera_device>Logitech BRIO</camera_device>
    <capture_width>0</capture_width>
    <capture_height>480</capture_height>
    <capture_fps>5fps</capture_fps>
    <bit_rate>256000</bit_rate>
    <always_accept_video>false</always_accept_video>
  </video>
  <skin>
    <selected>Default</selected>
  </skin>
  <forwarding_and_auto_answer>
    <mode>answer_instant</mode>
    <forward_seconds>30</forward_seconds>
    <forward_extension/>
    <autoanswer_seconds>30</autoanswer_seconds>
    <play_sound_on_auto_answer>true</play_sound_on_auto_answer>
    <auto_answer_keep_setting>false</auto_answer_keep_setting>
    <auto_answer_with_video>false</auto_answer_with_video>
    <accept_server_auto_answer>false</accept_server_auto_answer>
  </forwarding_and_auto_answer>
  <open_url_on_event/>
  <gui>
    <switch_contact_on_transfer>false</switch_contact_on_transfer>
    <enable_chat_support>true</enable_chat_support>
    <collapse_on_hangup>false</collapse_on_hangup>
    <language>en_US</language>
    <skin>default</skin>
    <background/>
    <auto_focus_window_on_incoming_chat>true</auto_focus_window_on_incoming_chat>
    <auto_focus_window_on_incoming_call>true</auto_focus_window_on_incoming_call>
    <custom_properties type="subtree">
      <devices_tested type="boolean">true</devices_tested>
    </custom_properties>
    <show_upgrade_wizard>true</show_upgrade_wizard>
  </gui>
  <profile>
    <last_online_manual_state>online</last_online_manual_state>
  </profile>
  <contact_services>
    <contact_service>
      <ident>ContactServiceLocal</ident>
      <name>Zoiper5 Contact Service</name>
      <type>local</type>
      <enabled>true</enabled>
      <account_mapping_type>default</account_mapping_type>
      <account_ident/>
      <presence_account_mapping_type>custom</presence_account_mapping_type>
      <presence_account_ident>Z599e3b298c0b03439f9d85cf</presence_account_ident>
      <hide_contacts_without_name>false</hide_contacts_without_name>
      <hide_contacts_without_phone>false</hide_contacts_without_phone>
      <restart_time>30</restart_time>
    </contact_service>
  </contact_services>
  <google_analytics>
    <ident>Zc869d7e74561ceb978b80ef</ident>
    <enabled>true</enabled>
    <queue_message_count>10</queue_message_count>
  </google_analytics>
  <proxy>
    <mode>disabled</mode>
    <custom_proxy>
      <hostname/>
      <port>0</port>
    </custom_proxy>
  </proxy>
  <history>
    <limit_mode>default</limit_mode>
    <limit>2000</limit>
    <allowed_overflow_percent_mode>default</allowed_overflow_percent_mode>
    <allowed_overflow_percent>20</allowed_overflow_percent>
  </history>
</options>

```
