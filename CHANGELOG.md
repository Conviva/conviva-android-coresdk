
# Changelog

## 4.0.30 (09/MAR/2023)

* Improves feature of broadcasting Video Sensor Events to Conviva Application Insights SDK (No impact for the Video Sensor only integrated applications)
* Supports Auto Detection of Application Background and Foreground Events in Content and Ad Sessions
* Fixes the invalid error log message shown during session cleanup

## 4.0.29 (29/DEC/2022)

* Supports feature to broadcast Video Sensor Events to Conviva Application Insights SDK(No impact for the Video Sensor only integrated applications).

## 4.0.28 (20/DEC/2022)

* **Released on top of v4.0.26 i.e., without media3 package support.**
* Fixes Out Of Memory issue on creation of new session due to new thread creation.
* Fixes the issue of offline session not being reported due to the handler creation inside the background thread.
* Fixes the crash during Conviva initialization from a kotlin coroutine.

## 4.0.27 (11/OCT/2022)

* Added support for media3 Exoplayer (1.0.0-beta02)

## 4.0.26 (07/OCT/2022)

* Minor improvements in message logging for bitrate

## 4.0.25 (30/SEP/2022)

* Support for Android 13 GA


## 4.0.24 (12/OCT/2022)
* Hotfix on top of 4.0.23 to move the offline database calls to a worker thread to avoid Strict Mode Disk Read violations


## 4.0.23 (15/JUL/2022)

* Support for Android 13 Beta 2

## 4.0.22 (14/JUN/2022)

* Minor bug fixes.
* Performance improvements

## 4.0.21.1 (19/JAN/2023)

* Hotfix on top of 4.0.21
* Support for Android 13 GA
* Supports feature to broadcast Video Sensor Events to Conviva Application Insights SDK(No impact for the Video Sensor only integrated applications)
* Enhances the reporting of the Playing state by sending urgent HB

## 4.0.21 (6/MAY/2022)

* Improvements in offline playback.
* Added log messages for bitrate collection.

## 4.0.20 (23/FEB/2022)

* Enabled Android 12L Beta 2 Support.

## 4.0.19.1 (18/FEB/2023)

* Hotfix on top of 4.0.19
* Enhances collection by sending urgent heartbeats on following events:
   *  First PLAYING state for Content Sessions
   *  Application Background and Foreground for Content/Ad Sessions(autocollected by Conviva)
   *  FATAL/WARNING Errors for Content/Ad Sessions

## 4.0.19 (11/FEB/2022)

* Internal code enhancements.

## 4.0.18 (18/NOV/2021)

Note: This version of Core SDK is not backward compatible with Core SDK v4.0.17.197 and previous versions.

* Supports Android 12.
* Removes legacy SDK API’s.
* adType attribute added to PodStart event.
* Fixed a synchronisation issue while metric input.

## 4.0.17.197 (24/SEP/2021)

* Supports auto collection of app version.
* Supports Android 12 Beta 5.

## 4.0.16.189 (18/FEB/2023)

* Hotfix on top of 4.0.16.187
* Enhances collection by sending urgent heartbeats on following events:
   *  First PLAYING state for Content Sessions
   *  Application Background and Foreground for Content/Ad Sessions(autocollected by Conviva)
   *  FATAL/WARNING Errors for Content/Ad Sessions


## 4.0.16.187 (12/JUL/2021)

* Fixed crash that's happening on some devices on mobile data connection.

## 4.0.15.185 (24/JUN/2021)

* Removed SDK included permissions.Previously SDK was including permissions that we listed on community and asked customers to include them in the apps.
* Fixed the ignoring of PauseJoin when false play state is reported prior to Preroll Ad impacting VST metric.
* Fixed Database insert failure of the data with special symbols(\`) while storing Hbs in offline playback mode.
* Fixed Security audit issue related to Sqlite injection attack.

## 4.0.13.171 (28/MAY/2021)
* Fixed CSR-6029: Android SDK - Offline data not reported

## 4.0.12.155 (22/MAR/2021)
* Enhancement to determine version based on API(Legacy/Simplified) being used for integration.

## 4.0.11.150 (10/FEB/2021)
* Supports 5G network type detection.
* Added IS_OFFLINE_PLAYBACK constant to enable offline monitoring.
* Added support for reporting average bit rate using PLAYBACK.AVG_BITRATE constant.
* IS_LIVE key can accept boolean as well as enum.
* Introduces new versioning of Major.Minor.PatchL(Eg.. 4.1.2L) for the legacy Conviva SDK Integrations to be able to differentiate from the Simplified     Integrations. (Existing)

## 4.0.10.141 (05/OCT/2020)
* Supports Android 11
* Supports auto collection of Screen Resolution
* Added API to report Dropped frames

## 4.0.9.132 (30/JUL/2020)
* Bug Fixes and improvements
* Supports Slate tracking

## 4.0.8.127 (22/JUL/2020)
* Fixed crash while initializing the SDK.

## 4.0.7.118 (19/JUN/2020)
* Supports auto collection of OS version for FireOS.
* Supports custom device info for unsupported devices.
* Supports to fetch Framework name & version without player object availability.

## 4.0.5 (03/APR/2020)
* Supports auto detection of NexStreaming and Brightcove Modules.

## 4.0.4 (23/MAR/2020)
* Supports auto detection of CDN Edge Server IP by Conviva Player Modules.

## 4.0.3 (13/MAR/2020)
* Adds the API setAdListener to support the Google IMA Module.
* Updates for bug fixes and improvements.

## 4.0.2 (14/FEB/2020)
* Introduces a major upgrade to the SDK architecture that simplifies and accelerates Conviva integration.
* Introduces ConvivaAnalytics, ConvivaVideoAnalytics, and ConvivaAdAnalytics to simplify the integration of the SDK.

## 2.145.4 (13/DEC/2019)
* Supports data collection and data compliance as per General Data Protection Regulation (GDPR), and California Consumer Privacy Act (CCPA).
* Introduces setUserPreferenceForDataCollection() API for setting user preferences to opt-out of user data collection.
* Introduces setUserPreferenceForDataDeletion() API for setting user preferences to delete previously collected user data.

## 2.145.3 (29/AUG/2019)
* Supports Android 10 (updated 23/SEP/2019).
* Introduces setCDNServerIP() API to support setting of CDN Edge Server's IP address.

## 2.145.2 (25/JUL/2019)
* Supports Android Q Beta 4 (10).
* Introduces a new feature to log error for missing late metadata during session creation and rendering of first frame.
* Fixes the memory leak issue found in the Conviva SDK.
* Fixes the heartbeat interval issue that updates only the latest amongst the sessions in a single client instance.
* Changes the application permission required to fetch Signal Strength to ACCESS_FINE_LOCATION in Android Q.

## 2.145.1 (12/SEPT/2018)
* Supports Android 9 Pie.
* Introduces Link Encryption changes to support security on Android P devices.
* Improves custom event error handling for non-string data types.
* Fixes missing state change event for assetName and playerName in the first heartbeat.

## 2.143.0.36122 (20/JUN/2018) (manual download)
## 2.145.0 (30/JUN/2018) (Gradle install)
* Supports Gradle dependency manager. (update 30/JUN/2018)
* Supports Android 9 Pie, developer preview 2.
* Fixes exception issue when custom tags value was set to null.
* Improves handling of concurrent modification of custom tags hash map, by two separate threads.

## 2.141.0.35947 (05/JUN/2018)
* Moves updateContentMetadata API to Client, to allow metadata updates during the entire session lifetime.
* Deprecates updateContentMetadata API from PlayerStateManager class.

## 2.138.0.35421 (14/MAR/2018)
* Supports Kotlin. (update 24/MAY/2018)
* Supports Ad Experience, part of Ad Insights.
* Removes detection and collection of Wi-Fi SSID.

## 2.133.0.34793 (27/DEC/2017)
* Supports customized gatewayUrl using each customer's CUSTOMER_KEY.
* Removed defaultDevelopmentGatewayUrl field from ClientSettings class.

## 2.129.0.34308 (15/SEP/2017)
* Supports Android Oreo (8.0.0).
* Fixed race condition issue relating to Average Frame Rate detection.
* Improved Android Core SDK with additional exception handling.
* Added API level check to capture cellular signal strength for WCDMA.
* Added Picture-in-Picture (PiP) mode support for Custom Android Player.

## 2.127.0.33850 (15/AUG/2017)
* Supports Android O Preview 3.

## 2.125.0.33492 (07/JUN/2017)
* Enhanced Frame Rate collection from Player.
* Customer set values for metadata take priority over automatically detected values by the Conviva player drop-in module.
* Validated on Android O Preview 2. (update 07/JULY/2017)

## 2.123.0.33157 (24/APR/2017)
* More accurate reporting of Average Frame Rate.

## 2.122.0.32826 (31/MAR/2017)
* Fix for incorrect reporting of content Duration.

## 2.121.0.32528 (16/MAR/2017)
* Synchronization improvement in Core SDK.

## 2.120.0.32345 (13/FEB/2017)
* Improvement to core library protocol implementation.
* Supports auto detection of Advertising ID (update: this has been removed on release SDK_2.121.0.32528 due to privacy concerns).
* Ability to update an existing ContentMetadata object with updateContentMetadata(contentmetadata) API call (mutable metadata).
* Deprecated the following APIs:
    * setDuration( int duration) API from PlayerStateManager.
    * setEncodedFrameRate(int encodedFrameRate) from PlayerStateManager.
    * updateContentMetadata(int sessionKey, ContentMetadata contentMetadata) from Client.
  Use updateContentMetadata(ContentMetadata _contentMetadata) API in PlayerStateManager to update Duration, Encoded Frame Rate and other metadata.
* Deprecated the following field:
  public int defaultBitrateKbps field from ContentMetadata is deprecated. Bitrate can be set using setBitrateKbps(int newBitrateKbps) API in PlayerStateManager.

## 2.119.0.32040 (04/JAN/2017)
* Improvement to core library protocol implementation.
