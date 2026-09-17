
# Changelog
## 4.1.0 (17/SEP/2026)
- Introduces API to report live latency (BETA) as a playback metric and an is-at-live-edge flag (BETA) through reportPlaybackMetric.

### Live Latency and Is At Live Edge (CWS 2.9)
- Protocol version raised from 2.8 to 2.9.
- Two new playback metric keys, reported through the existing reportPlaybackMetric API. They are
  independent: neither call requires, orders itself against, or gates the other.
      videoAnalytics.reportPlaybackMetric(ConvivaSdkConstants.PLAYBACK.LIVE_LATENCY, 8200);
      videoAnalytics.reportPlaybackMetric(ConvivaSdkConstants.PLAYBACK.IS_AT_LIVE_EDGE, true);

#### ConvivaSdkConstants.PLAYBACK.LIVE_LATENCY - wire key "lat" in HB.
- A single positive integer: milliseconds behind the live edge, computed and pushed by the
  application as wall clock now minus the programDateTime of the fragment on screen.
- Accepts boxed Integer or Long only. Zero, negatives, null, strings, and floats are discarded
  with a debug log. Under the expected wall-clock-minus-programDateTime derivation zero latency
  is not achievable, so a zero indicates a broken derivation rather than a viewer at the edge.
- Carried on a CwsDataSamplesEvent inside evs. It never appears at heartbeat top level: a sample
  describes one measured instant, so restating it in a later heartbeat would assert a moment nobody
  measured. When heartbeats are lost the backend sees a gap rather than a stale value.
- Each accepted call emits exactly one sample. Values are not de-duplicated, because an unchanged
  value is evidence the viewer is still that far behind, and no value is stored between calls.
- Report it every 5 seconds whenever the number is valid, at the live edge or deep in the DVR
  window. It is not restricted to live-edge viewing; distinguishing the two is IS_AT_LIVE_EDGE's job.
- Reporting stops by not calling. There is no "off" call, no sentinel and no flag.

#### ConvivaSdkConstants.PLAYBACK.IS_AT_LIVE_EDGE - wire key "ule"
- A single boxed Boolean: whether the viewer is tracking the live edge, by the application's
  definition, as opposed to having deliberately time-shifted. Only true and false are accepted;
  anything else is discarded with a debug log rather than coerced.
- Stateful and carried in new/old on a CwsStateChangeEvent, and restated at heartbeat top level
  once known, so a lost transition event is recoverable.
- Emitted only on a change. Repeating the current value is a successful no-op.
- The default is Unknown, and Unknown is expressed by the absence of "ule" rather than by a sentinel.
  It is not a reportable value, so a session that never calls stays Unknown for its whole life.
  Report IS_AT_LIVE_EDGE once at the start of every live session and then on every transition.

#### Common to both
- Live content only. On a session known to be VOD the value is rejected and an error is logged once
  per session, per metric. While the stream type is still unresolved the value is accepted.
  Switching to VOD mid-session stops new reports and omits beacon "ule" until the session is live
  again (last-known is kept in memory).
- Discarded during a client-side ad break, since the player has left the live stream. Reported
  normally under server-side ad insertion, where the player never leaves it — so both split across a
  client-side break and are continuous across a server-side one. A client-side break does not clear
  the last known IS_AT_LIVE_EDGE flag. A new SSAI ad session copies last-known IS_AT_LIVE_EDGE.
  After content becomes VOD, live metrics are not mirrored.
- Reporting either key through reportAdMetric is a documented no-op.

## 4.0.51 (26/MAR/2026)
- Improves error synchronisation between SSAI Ad Sessions and Video Sessions

## 4.0.50 (16/DEC/2025)
- Supports reporting the Ad session events such as, Attempt, End, etc., to DPI.

## 4.0.49 (15/SEP/2025)
- Support for new [Media3 IMA](https://github.com/Conviva/conviva-android-media3-imasdk) and [ExoPlayer IMA](https://github.com/Conviva/conviva-android-exoplayer-imasdk) modules.
  
## 4.0.48 (21/AUG/2025)
- Introduced a new method `reportAdType(ConvivaSdkConstants.AdType adType)` to `ConvivaVideoAnalytics` to allow reporting the type of ad to the video session.
  
## 4.0.47 (13/AUG/2025)
Introduces a new constant `ConvivaSdkConstants.PLAYBACK.AUTO_REPORT_ERRORS` to turn off the default error reporting behavior of the ExoPlayer and Media3 modules. Pass this constant in the player info through the `ConvivaVideoAnalytics.setPlayer` API.
Note: Applicable only to ExoPlayer modules 4.1.6 and above, and Media3 modules 4.0.1 and above.

## 4.0.46 (11/JUL/2025)
- Resolved a NullPointerException that occurred when calling getSessionId under certain conditions.
- Introduced a new method getSessionIdAsync(SessionIdCallback callback) to fetch the session ID asynchronously.
  
## 4.0.45 (02/JUN/2025)
- Support for new [Media3](https://github.com/Conviva/conviva-android-media3) module.
  
## 4.0.44 (01/APR/2025)
* Support for Android 16.
* Internal updates and improvements.
  
## 4.0.43 (10/JAN/2025)
* Fixes the issue of last HB missing framework name and version, module name and module version metadata.
* Fixes the issue of language metadata not being reset for a subsequent new session in same client instance.
* Changed the minimum delay for urgent Heartbeats to 200ms.

## 4.0.42 (02/DEC/2024)
* Fixes the ANR issue in the Network Callbacks
* Fixes the caught exception in urgent HB timer

## 4.0.41 (17/OCT/2024)
* Fixes issue in resetting buffer length to 0 on ad end, reducing inflated errors for content / ad sessions.
* Enhances collection by sending urgent heartbeats on following events:
   *  First PLAYING state for Content Sessions
   *  Application Background for Content/Ad Sessions(autocollected by Conviva)
   *  FATAL Errors for Content/Ad Sessions

## 4.0.39 (26/JUL/2024)
* Support for Android 15. (Updated: 13/SEP/2024)
* Supports playback tracking in the background.
* Set the`ConvivaSdkConstants.ALLOW_BACKGROUND_PLAYBACK` to true, to continue the tracking in the background.

## 4.0.38 (24/JUL/2024)
* Resolves the ANR issue due to deadlock between Conviva sdk and UI thread.
 
## 4.0.37 (04/APR/2024)
* Resolves the NullPointerException due to synchronisation issues in the Android Modules.

## 4.0.36 (08/MAR/2024)
* Resolves the metadata retention issue by cleaning up at the end of the session.

## 4.0.35 (11/OCT/2023)
* Supports Android 14

## 4.0.34 (04/OCT/2023)
* Fixes the thread synchronization issues in the setCallback function.

## 4.0.33 (05/JUL/2023)
* Supports media3 Exoplayer.

## 4.0.32 (19/JUN/2023)
* Fixes the crash issue when the app is backgrounded or foregrounded after release is called.
* Fixes the thread synchronization issues in the init and release lifecycle.
* Fixes the issue of Ad asset name not being reported during the zeroth heartbeat.

## 4.0.31 (10/APR/2023)
* Introduces new keys ConvivaSdkConstants.PLAYBACK.AUDIO_LANGUAGE, ConvivaSdkConstants.PLAYBACK.SUBTITLES_LANGUAGE, ConvivaSdkConstants.PLAYBACK.CLOSED_CAPTIONS_LANGUAGE within reportPlaybackMetric() API for setting audio track changes, subtitle track changes and closed caption track changes respectively.

## 4.0.30 (09/MAR/2023)

* Improves feature of broadcasting Video Sensor Events to Conviva Application Insights SDK (No impact for the Video Sensor only integrated applications)
* Supports Auto Detection of Application Background and Foreground Events in Content and Ad Sessions
* Fixes the invalid error log message shown during session cleanup

### DEPRECATED VERSION
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

### DEPRECATED VERSION
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
