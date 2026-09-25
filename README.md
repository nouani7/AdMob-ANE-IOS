# 🚀 AdMob_ANE_IOS

**Native Google AdMob ANE for Adobe AIR on iOS — Banner, Interstitial, Rewarded, Rewarded Interstitial & App Open ads with UMP, ATT, lifecycle events, and full ad management.**

# 📱 AdMob ANE for Adobe AIR - iOS

A native **Google Mobile Ads (AdMob) Native Extension (ANE)** for **Adobe AIR 51.4.1.1+** on iOS.

This project provides a native **ActionScript 3 API** for integrating Google AdMob advertising into Adobe AIR applications, with a simple and consistent interface across all supported ad formats.

**Minimum supported Adobe AIR version:** `51.4.1.1`

The native iOS implementation uses **Google Mobile Ads SDK for iOS 13.10.0** and **Google User Messaging Platform (UMP) SDK 3.1.0**.

# 📋 Overview

The ANE supports the following Google advertising formats:

* **Banner Ads**
* **Interstitial Ads**
* **Rewarded Ads**
* **Rewarded Interstitial Ads**
* **App Open Ads**

It also provides APIs for:

* Google User Messaging Platform (**UMP**)
* Apple App Tracking Transparency (**ATT**)
* Ad volume and mute control
* Child-directed treatment
* Under-age-of-consent settings
* Maximum content rating
* Test devices
* Banner positioning
* Banner refreshing
* Ad lifecycle events
* Native error codes and messages
* Reward type and amount
* Ad object destruction and cleanup

# 📦 Installation

## iOS Application Descriptor Configuration

The Adobe AIR application descriptor must contain the required iOS configuration for the AdMob ANE.

The `<iPhone>` section should be configured as follows:

```xml
<iPhone>
    <InfoAdditions><![CDATA[
        <!-- Minimum iOS version -->
        <key>MinimumOSVersion</key>
        <string>15.0</string>

        <!-- iPhone + iPad -->
        <key>UIDeviceFamily</key>
        <array>
            <string>1</string>
            <string>2</string>
        </array>

        <!-- Google Mobile Ads - TEST APP ID -->
        <key>GADApplicationIdentifier</key>
        <string>ca-app-pub-3940256099942544~1458002511</string>

        <!-- App Tracking Transparency -->
        <key>NSUserTrackingUsageDescription</key>
        <string>This identifier will be used to deliver personalized ads to you.</string>

        <!-- AdMob SKAdNetwork identifiers -->
        <key>SKAdNetworkItems</key>
        <array>
            <dict>
                <key>SKAdNetworkIdentifier</key>
                <string>cstr6suwn9.skadnetwork</string>
            </dict>
        </array>

    ]]></InfoAdditions>

    <requestedDisplayResolution>high</requestedDisplayResolution>
</iPhone>
```

## Google Mobile Ads Application ID

The following key is required by Google Mobile Ads:

```xml
<key>GADApplicationIdentifier</key>
<string>ca-app-pub-3940256099942544~1458002511</string>
```

The value shown above is Google's official test AdMob Application ID.

> [!WARNING]
> ⚠️ **IMPORTANT — `GADApplicationIdentifier` is required**
>
> **Do not remove this key or use an invalid Application ID.**
>
> If `GADApplicationIdentifier` is **missing, deleted, empty, or incorrect**, the application may **crash during Google Mobile Ads initialization** instead of returning a normal initialization error.
>
> For testing, use Google's official test Application ID shown above.
>
> For production, replace it with your own valid AdMob Application ID.

# ⚙️ Basic Initialization

Import the API:

```actionscript
import com.admob.mx.AdMobANE;
import com.admob.mx.AdMobEvent;
```

Create/get the singleton instance:

```actionscript
var adMob:AdMobANE = AdMobANE.getInstance();
```

Check the native extension:

```actionscript
trace(adMob.isReady);
```

Initialize AdMob:

```actionscript
adMob.initAdMob(
    "ca-app-pub-XXXXXXXXXXXXXXXX~XXXXXXXXXX"
);
```

Initialization is asynchronous.

Listen for:

```actionscript
AdMobANE.ADMOB_INITIALIZED
```

or:

```actionscript
AdMobANE.ADMOB_INIT_FAILED
```

Example:

```actionscript
adMob.addEventListener(
    AdMobANE.ADMOB_INITIALIZED,
    onAdMobInitialized
);

adMob.addEventListener(
    AdMobANE.ADMOB_INIT_FAILED,
    onAdMobInitFailed
);

adMob.initAdMob(ADMOB_APP_ID);
```

# 1️⃣ Banner Ads

Banner advertisements are persistent native ads that can be positioned on the screen and refreshed when required.

The ANE provides complete control over:

* Banner creation
* Position
* Size
* Hide/show state
* Removal
* Position changes
* Height detection
* Refresh
* Load events
* Click events
* Impression events
* Error reporting

## 1.1 Show Banner

```actionscript
adMob.showBanner(
    BANNER_ID,
    AdMobANE.BANNER_BOTTOM,
    AdMobANE.SIZE_BANNER
);
```

Method:

```actionscript
showBanner(
    adUnitId:String,
    position:String = "BOTTOM",
    size:String = "BANNER"
):void
```

### Parameters

#### `adUnitId`

The AdMob banner ad unit ID.

Example:

```actionscript
"ca-app-pub-XXXXXXXXXXXXXXXX/XXXXXXXXXX"
```

#### `position`

Controls where the banner is displayed.

#### `size`

Controls the banner format.

## 1.2 Banner Positions

Available positions:

```actionscript
AdMobANE.BANNER_TOP
AdMobANE.BANNER_TOP_LEFT
AdMobANE.BANNER_TOP_RIGHT

AdMobANE.BANNER_BOTTOM
AdMobANE.BANNER_BOTTOM_LEFT
AdMobANE.BANNER_BOTTOM_RIGHT

AdMobANE.BANNER_CENTER
AdMobANE.BANNER_CENTER_LEFT
AdMobANE.BANNER_CENTER_RIGHT
```

Example:

```actionscript
adMob.showBanner(
    BANNER_ID,
    AdMobANE.BANNER_TOP,
    AdMobANE.SIZE_BANNER
);
```

Bottom-right:

```actionscript
adMob.showBanner(
    BANNER_ID,
    AdMobANE.BANNER_BOTTOM_RIGHT,
    AdMobANE.SIZE_BANNER
);
```

## 1.3 Banner Sizes

Available sizes:

```actionscript
AdMobANE.SIZE_BANNER
AdMobANE.SIZE_LARGE
AdMobANE.SIZE_MEDIUM
AdMobANE.SIZE_FULL
AdMobANE.SIZE_LEADERBOARD
AdMobANE.SIZE_ADAPTIVE
```

| Constant           |            Size |
| ------------------ | --------------: |
| `SIZE_BANNER`      |        320 × 50 |
| `SIZE_LARGE`       |       320 × 100 |
| `SIZE_MEDIUM`      |       300 × 250 |
| `SIZE_FULL`        |        468 × 60 |
| `SIZE_LEADERBOARD` |        728 × 90 |
| `SIZE_ADAPTIVE`    | Adaptive height |

Example:

```actionscript
adMob.showBanner(
    BANNER_ID,
    AdMobANE.BANNER_BOTTOM,
    AdMobANE.SIZE_ADAPTIVE
);
```

`SIZE_ADAPTIVE` allows the native implementation to determine an appropriate banner height.

## 1.4 Hide Banner

Hide the current banner:

```actionscript
adMob.hideBanner();
```

The banner is hidden but not necessarily destroyed.

Event:

```actionscript
AdMobANE.BANNER_HIDDEN
```

## 1.5 Remove Banner

Remove the current banner:

```actionscript
adMob.removeBanner();
```

Event:

```actionscript
AdMobANE.BANNER_REMOVED
```

Use this when the banner is no longer required.

## 1.6 Change Banner Position

The position of the existing banner can be changed:

```actionscript
adMob.setBannerPosition(
    AdMobANE.BANNER_TOP
);
```

Available positions are the same as those used by `showBanner()`.

Success event:

```actionscript
AdMobANE.BANNER_POSITION_CHANGED
```

Failure event:

```actionscript
AdMobANE.BANNER_POSITION_FAILED
```

## 1.7 Get Banner Height

Get the current native banner height:

```actionscript
var height:int = adMob.getBannerHeight();

trace("Banner height:", height);
```

This can be useful for adjusting AIR display objects around the native banner.

## 1.8 Refresh Banner

Refresh the currently displayed banner:

```actionscript
adMob.refreshBanner();
```

Success:

```actionscript
AdMobANE.BANNER_REFRESHED
```

Failure:

```actionscript
AdMobANE.BANNER_REFRESH_FAILED
```

Example:

```actionscript
adMob.addEventListener(
    AdMobANE.BANNER_REFRESHED,
    onBannerRefreshed
);

adMob.addEventListener(
    AdMobANE.BANNER_REFRESH_FAILED,
    onBannerRefreshFailed
);
```

## 1.9 Banner Events

```actionscript
AdMobANE.BANNER_LOADED
AdMobANE.BANNER_LOAD_FAILED

AdMobANE.BANNER_OPENED
AdMobANE.BANNER_CLOSED

AdMobANE.BANNER_IMPRESSION
AdMobANE.BANNER_CLICKED

AdMobANE.BANNER_REFRESHED
AdMobANE.BANNER_REFRESH_FAILED

AdMobANE.BANNER_POSITION_CHANGED
AdMobANE.BANNER_POSITION_FAILED

AdMobANE.BANNER_HIDDEN
AdMobANE.BANNER_REMOVED
```

## 1.10 Banner Example

```actionscript
adMob.addEventListener(
    AdMobANE.BANNER_LOADED,
    onBannerLoaded
);

adMob.addEventListener(
    AdMobANE.BANNER_LOAD_FAILED,
    onBannerLoadFailed
);

adMob.addEventListener(
    AdMobANE.BANNER_CLICKED,
    onBannerClicked
);

adMob.showBanner(
    BANNER_ID,
    AdMobANE.BANNER_BOTTOM,
    AdMobANE.SIZE_ADAPTIVE
);
```

Error:

```actionscript
private function onBannerLoadFailed(
    e:AdMobEvent
):void
{
    trace("Error code:", e.errorCode);
    trace("Error message:", e.rewardType);
}
```

# 2️⃣ Interstitial Ads

Interstitial ads are full-screen advertisements.

The ANE provides:

* Load
* Loaded-state checking
* Show
* Show error
* Close notification
* Impression notification
* Destroy
* Error code/message reporting

## 2.1 Load Interstitial

```actionscript
adMob.loadInterstitial(
    INTERSTITIAL_ID
);
```

Method:

```actionscript
loadInterstitial(
    adUnitId:String
):void
```

The loading process is asynchronous.

Wait for:

```actionscript
AdMobANE.INTERSTITIAL_LOADED
```

or:

```actionscript
AdMobANE.INTERSTITIAL_LOAD_FAILED
```

## 2.2 Check Interstitial State

```actionscript
if (adMob.isInterstitialLoaded())
{
    adMob.showInterstitial();
}
```

Method:

```actionscript
isInterstitialLoaded():Boolean
```

Returns:

* `true` — an interstitial is ready
* `false` — no interstitial is currently ready

## 2.3 Show Interstitial

```actionscript
adMob.showInterstitial();
```

It is recommended to check:

```actionscript
adMob.isInterstitialLoaded()
```

before showing.

## 2.4 Destroy Interstitial

```actionscript
adMob.destroyInterstitial();
```

This destroys/releases the current native interstitial object.

Event:

```actionscript
AdMobANE.INTERSTITIAL_DESTROYED
```

## 2.5 Interstitial Events

```actionscript
AdMobANE.INTERSTITIAL_LOADED
AdMobANE.INTERSTITIAL_LOAD_FAILED

AdMobANE.INTERSTITIAL_SHOWED
AdMobANE.INTERSTITIAL_SHOW_FAILED

AdMobANE.INTERSTITIAL_CLOSED
AdMobANE.INTERSTITIAL_IMPRESSION

AdMobANE.INTERSTITIAL_DESTROYED
```

## 2.6 Interstitial Example

```actionscript
adMob.addEventListener(
    AdMobANE.INTERSTITIAL_LOADED,
    onInterstitialLoaded
);

adMob.addEventListener(
    AdMobANE.INTERSTITIAL_LOAD_FAILED,
    onInterstitialLoadFailed
);

adMob.addEventListener(
    AdMobANE.INTERSTITIAL_CLOSED,
    onInterstitialClosed
);

adMob.loadInterstitial(
    INTERSTITIAL_ID
);
```

Show:

```actionscript
private function onInterstitialLoaded(
    e:AdMobEvent
):void
{
    if (adMob.isInterstitialLoaded())
    {
        adMob.showInterstitial();
    }
}
```

Reload after close:

```actionscript
private function onInterstitialClosed(
    e:AdMobEvent
):void
{
    adMob.loadInterstitial(
        INTERSTITIAL_ID
    );
}
```

# 3️⃣ Rewarded Ads

Rewarded advertisements provide a reward to the user after the rewarded ad interaction is completed.

The ANE supports:

* Loading
* Loaded-state checking
* Showing
* Reward callback
* Reward type
* Reward amount
* Impression
* Close
* Show errors
* Load errors
* Destroy

## 3.1 Load Rewarded

```actionscript
adMob.loadRewarded(
    REWARDED_ID
);
```

Method:

```actionscript
loadRewarded(
    adUnitId:String
):void
```

## 3.2 Check Rewarded State

```actionscript
if (adMob.isRewardedLoaded())
{
    adMob.showRewarded();
}
```

Method:

```actionscript
isRewardedLoaded():Boolean
```

## 3.3 Show Rewarded

```actionscript
adMob.showRewarded();
```

## 3.4 Reward Event

Listen for:

```actionscript
AdMobANE.REWARDED_REWARD
```

Example:

```actionscript
private function onReward(
    e:AdMobEvent
):void
{
    trace("Reward type:", e.rewardType);
    trace("Reward amount:", e.rewardAmount);
}
```

The event provides:

```actionscript
e.rewardType
e.rewardAmount
```

For example:

```text
Reward type: coins
Reward amount: 10
```

## 3.5 Destroy Rewarded

```actionscript
adMob.destroyRewarded();
```

Event:

```actionscript
AdMobANE.REWARDED_DESTROYED
```

## 3.6 Rewarded Events

```actionscript
AdMobANE.REWARDED_LOADED
AdMobANE.REWARDED_LOAD_FAILED

AdMobANE.REWARDED_SHOWED
AdMobANE.REWARDED_SHOW_FAILED

AdMobANE.REWARDED_CLOSED
AdMobANE.REWARDED_IMPRESSION

AdMobANE.REWARDED_REWARD
AdMobANE.REWARDED_DESTROYED
```

## 3.7 Rewarded Example

```actionscript
adMob.addEventListener(
    AdMobANE.REWARDED_LOADED,
    onRewardedLoaded
);

adMob.addEventListener(
    AdMobANE.REWARDED_LOAD_FAILED,
    onRewardedLoadFailed
);

adMob.addEventListener(
    AdMobANE.REWARDED_REWARD,
    onReward
);

adMob.addEventListener(
    AdMobANE.REWARDED_CLOSED,
    onRewardedClosed
);

adMob.loadRewarded(
    REWARDED_ID
);
```

# 4️⃣ Rewarded Interstitial Ads

Rewarded Interstitial combines a full-screen advertisement with a reward callback.

The API provides:

* Load
* Loaded state
* Show
* Reward
* Reward type
* Reward amount
* Impression
* Close
* Error handling
* Destroy

## 4.1 Load

```actionscript
adMob.loadRewardedInterstitial(
    REWARDED_INTERSTITIAL_ID
);
```

## 4.2 Check Loaded State

```actionscript
if (adMob.isRewardedInterstitialLoaded())
{
    adMob.showRewardedInterstitial();
}
```

## 4.3 Show

```actionscript
adMob.showRewardedInterstitial();
```

## 4.4 Reward

Listen for:

```actionscript
AdMobANE.REWARDED_INTERSTITIAL_REWARD
```

Example:

```actionscript
private function onRewardedInterstitialReward(
    e:AdMobEvent
):void
{
    trace(
        "Reward type:",
        e.rewardType
    );

    trace(
        "Reward amount:",
        e.rewardAmount
    );
}
```

## 4.5 Destroy

```actionscript
adMob.destroyRewardedInterstitial();
```

## 4.6 Events

```actionscript
AdMobANE.REWARDED_INTERSTITIAL_LOADED
AdMobANE.REWARDED_INTERSTITIAL_LOAD_FAILED

AdMobANE.REWARDED_INTERSTITIAL_SHOWED
AdMobANE.REWARDED_INTERSTITIAL_SHOW_FAILED

AdMobANE.REWARDED_INTERSTITIAL_CLOSED
AdMobANE.REWARDED_INTERSTITIAL_IMPRESSION

AdMobANE.REWARDED_INTERSTITIAL_REWARD
AdMobANE.REWARDED_INTERSTITIAL_DESTROYED
```

# 5️⃣ App Open Ads

App Open ads are intended for displaying an advertisement when the application starts or returns to the foreground.

The ANE supports:

* Load
* Loaded state
* Show
* Show errors
* Close
* Impression
* Destroy

## 5.1 Load

```actionscript
adMob.loadAppOpen(
    APP_OPEN_ID
);
```

## 5.2 Check Loaded State

```actionscript
if (adMob.isAppOpenLoaded())
{
    adMob.showAppOpen();
}
```

## 5.3 Show

```actionscript
adMob.showAppOpen();
```

## 5.4 Destroy

```actionscript
adMob.destroyAppOpen();
```

## 5.5 Events

```actionscript
AdMobANE.APP_OPEN_LOADED
AdMobANE.APP_OPEN_LOAD_FAILED

AdMobANE.APP_OPEN_SHOWED
AdMobANE.APP_OPEN_SHOW_FAILED

AdMobANE.APP_OPEN_CLOSED
AdMobANE.APP_OPEN_IMPRESSION

AdMobANE.APP_OPEN_DESTROYED
```

# 6️⃣ Google User Messaging Platform — UMP

The ANE provides integration with Google's User Messaging Platform.

The API supports:

* Requesting consent information
* Test mode
* Showing the consent form
* Reading consent status
* Consent update events
* Consent errors
* Consent form dismissal
* Consent form errors

## 6.1 Request Consent Information

Normal mode:

```actionscript
adMob.requestConsentInfo();
```

Test mode:

```actionscript
adMob.requestConsentInfo(true);
```

Method:

```actionscript
requestConsentInfo(
    testMode:Boolean = false
):void
```

## 6.2 Consent Status

Available constants:

```actionscript
AdMobANE.CONSENT_UNKNOWN
AdMobANE.CONSENT_REQUIRED
AdMobANE.CONSENT_NOT_REQUIRED
AdMobANE.CONSENT_OBTAINED
```

Values:

| Constant               | Value |
| ---------------------- | ----: |
| `CONSENT_UNKNOWN`      |     0 |
| `CONSENT_REQUIRED`     |     1 |
| `CONSENT_NOT_REQUIRED` |     2 |
| `CONSENT_OBTAINED`     |     3 |

Read status:

```actionscript
var status:int =
    adMob.getConsentStatus();
```

Example:

```actionscript
switch (status)
{
    case AdMobANE.CONSENT_UNKNOWN:
        trace("Consent status unknown");
        break;

    case AdMobANE.CONSENT_REQUIRED:
        trace("Consent required");
        break;

    case AdMobANE.CONSENT_NOT_REQUIRED:
        trace("Consent not required");
        break;

    case AdMobANE.CONSENT_OBTAINED:
        trace("Consent obtained");
        break;
}
```

## 6.3 Show Consent Form

```actionscript
adMob.showConsentForm();
```

## 6.4 UMP Events

```actionscript
AdMobANE.CONSENT_INFO_UPDATED
AdMobANE.CONSENT_INFO_FAILED

AdMobANE.CONSENT_FORM_DISMISSED
AdMobANE.CONSENT_FORM_FAILED
```

Example:

```actionscript
adMob.addEventListener(
    AdMobANE.CONSENT_INFO_UPDATED,
    onConsentUpdated
);

adMob.addEventListener(
    AdMobANE.CONSENT_INFO_FAILED,
    onConsentFailed
);
```

# 7️⃣ Apple App Tracking Transparency — ATT

The ANE provides access to Apple's App Tracking Transparency authorization request.

Request ATT:

```actionscript
adMob.requestATT();
```

Events:

```actionscript
AdMobANE.ATT_STATUS
AdMobANE.ATT_FAILED
```

Example:

```actionscript
adMob.addEventListener(
    AdMobANE.ATT_STATUS,
    onATTStatus
);

adMob.addEventListener(
    AdMobANE.ATT_FAILED,
    onATTFailed
);

adMob.requestATT();
```

The ATT status is returned through:

```actionscript
e.errorCode
```

Example:

```actionscript
private function onATTStatus(
    e:AdMobEvent
):void
{
    trace("ATT status:", e.errorCode);
}
```

Failure:

```actionscript
private function onATTFailed(
    e:AdMobEvent
):void
{
    trace(
        "ATT error:",
        e.errorCode,
        e.rewardType
    );
}
```

The application must also provide the required iOS tracking usage description in its application configuration when ATT is used.

# 8️⃣ Global AdMob Settings

These settings apply to the native AdMob implementation.

## Volume

```actionscript
adMob.setVolume(1.0);
```

Accepted range:

```text
0.0 – 1.0
```

Values outside the range are automatically clamped.

Examples:

```actionscript
adMob.setVolume(0.0);
adMob.setVolume(0.5);
adMob.setVolume(1.0);
```

## Muted

Mute:

```actionscript
adMob.setMuted(true);
```

Unmute:

```actionscript
adMob.setMuted(false);
```

## Child Directed

Enable:

```actionscript
adMob.setChildDirected(true);
```

Disable:

```actionscript
adMob.setChildDirected(false);
```

## Under Age of Consent

Enable:

```actionscript
adMob.setUnderAgeOfConsent(true);
```

Disable:

```actionscript
adMob.setUnderAgeOfConsent(false);
```

## Maximum Content Rating

Available ratings:

```actionscript
AdMobANE.RATING_G
AdMobANE.RATING_PG
AdMobANE.RATING_T
AdMobANE.RATING_MA
```

Values:

| Constant    | Value |
| ----------- | ----: |
| `RATING_G`  |     0 |
| `RATING_PG` |     1 |
| `RATING_T`  |     2 |
| `RATING_MA` |     3 |

Example:

```actionscript
adMob.setMaxContentRating(
    AdMobANE.RATING_PG
);
```

# 9️⃣ Test Devices

Add a test device:

```actionscript
adMob.addTestDevice(
    "DEVICE_ID"
);
```

Example:

```actionscript
adMob.addTestDevice(
    "YOUR_TEST_DEVICE_ID"
);
```

If the device ID is `null` or empty, the call is ignored.

During development, use Google's official test ad unit IDs or configure the device as a test device.

# 🔟 AdMobEvent

All native events are converted into:

```actionscript
AdMobEvent
```

The class extends:

```actionscript
flash.events.Event
```

and provides:

```actionscript
public var rewardType:String;
public var rewardAmount:int;
public var errorCode:int;
```

## rewardType

For rewarded advertisements, this contains the reward type.

Example:

```actionscript
trace(e.rewardType);
```

For error events, the same property contains the native error message.

Therefore:

```actionscript
e.rewardType
```

has two possible roles:

* Reward type for reward events
* Error message for error events

## rewardAmount

Contains the reward amount for:

```text
REWARDED_REWARD
REWARDED_INTERSTITIAL_REWARD
```

Example:

```actionscript
trace(e.rewardAmount);
```

## errorCode

Contains the native error/status code.

Example:

```actionscript
trace(e.errorCode);
```

For events without an error/status code, this is normally:

```text
0
```

# 1️⃣1️⃣ Error Handling

Error events provide:

```actionscript
e.errorCode
e.rewardType
```

Example:

```actionscript
private function onLoadFailed(
    e:AdMobEvent
):void
{
    trace(
        "Error code:",
        e.errorCode
    );

    trace(
        "Error message:",
        e.rewardType
    );
}
```

Error events include:

```text
ADMOB_INIT_FAILED

BANNER_LOAD_FAILED
BANNER_REFRESH_FAILED
BANNER_POSITION_FAILED

INTERSTITIAL_LOAD_FAILED
INTERSTITIAL_SHOW_FAILED

REWARDED_LOAD_FAILED
REWARDED_SHOW_FAILED

REWARDED_INTERSTITIAL_LOAD_FAILED
REWARDED_INTERSTITIAL_SHOW_FAILED

APP_OPEN_LOAD_FAILED
APP_OPEN_SHOW_FAILED

CONSENT_INFO_FAILED
CONSENT_FORM_FAILED

ATT_FAILED
```

# 1️⃣2️⃣ Complete Example

```actionscript
import com.admob.mx.AdMobANE;
import com.admob.mx.AdMobEvent;

var adMob:AdMobANE =
    AdMobANE.getInstance();

adMob.addEventListener(
    AdMobANE.ADMOB_INITIALIZED,
    onAdMobInitialized
);

adMob.addEventListener(
    AdMobANE.ADMOB_INIT_FAILED,
    onAdMobInitFailed
);

adMob.addEventListener(
    AdMobANE.BANNER_LOADED,
    onBannerLoaded
);

adMob.addEventListener(
    AdMobANE.BANNER_LOAD_FAILED,
    onBannerLoadFailed
);

adMob.addEventListener(
    AdMobANE.INTERSTITIAL_LOADED,
    onInterstitialLoaded
);

adMob.addEventListener(
    AdMobANE.INTERSTITIAL_CLOSED,
    onInterstitialClosed
);

adMob.addEventListener(
    AdMobANE.REWARDED_LOADED,
    onRewardedLoaded
);

adMob.addEventListener(
    AdMobANE.REWARDED_REWARD,
    onReward
);

adMob.initAdMob(
    "ca-app-pub-XXXXXXXXXXXXXXXX~XXXXXXXXXX"
);

function onAdMobInitialized(
    e:AdMobEvent
):void
{
    trace("AdMob initialized.");

    adMob.showBanner(
        "ca-app-pub-XXXXXXXXXXXXXXXX/XXXXXXXXXX",
        AdMobANE.BANNER_BOTTOM,
        AdMobANE.SIZE_ADAPTIVE
    );

    adMob.loadInterstitial(
        "ca-app-pub-XXXXXXXXXXXXXXXX/XXXXXXXXXX"
    );

    adMob.loadRewarded(
        "ca-app-pub-XXXXXXXXXXXXXXXX/XXXXXXXXXX"
    );
}

function onAdMobInitFailed(
    e:AdMobEvent
):void
{
    trace(
        "AdMob initialization failed:",
        e.errorCode,
        e.rewardType
    );
}

function onBannerLoaded(
    e:AdMobEvent
):void
{
    trace("Banner loaded.");
}

function onBannerLoadFailed(
    e:AdMobEvent
):void
{
    trace(
        "Banner failed:",
        e.errorCode,
        e.rewardType
    );
}

function onInterstitialLoaded(
    e:AdMobEvent
):void
{
    if (adMob.isInterstitialLoaded())
    {
        adMob.showInterstitial();
    }
}

function onInterstitialClosed(
    e:AdMobEvent
):void
{
    adMob.loadInterstitial(
        "ca-app-pub-XXXXXXXXXXXXXXXX/XXXXXXXXXX"
    );
}

function onRewardedLoaded(
    e:AdMobEvent
):void
{
    trace("Rewarded loaded.");
}

function onReward(
    e:AdMobEvent
):void
{
    trace(
        "Reward type:",
        e.rewardType
    );

    trace(
        "Reward amount:",
        e.rewardAmount
    );
}
```

# 1️⃣3️⃣ Complete API Reference

## Core

```actionscript
getInstance():AdMobANE
isReady:Boolean

initAdMob(appId:String):void
dispose():void
```

## Banner

```actionscript
showBanner(
    adUnitId:String,
    position:String = "BOTTOM",
    size:String = "BANNER"
):void

hideBanner():void
removeBanner():void

setBannerPosition(
    position:String
):void

getBannerHeight():int

refreshBanner():void
```

## Interstitial

```actionscript
loadInterstitial(
    adUnitId:String
):void

showInterstitial():void

isInterstitialLoaded():Boolean

destroyInterstitial():void
```

## Rewarded

```actionscript
loadRewarded(
    adUnitId:String
):void

showRewarded():void

isRewardedLoaded():Boolean

destroyRewarded():void
```

## Rewarded Interstitial

```actionscript
loadRewardedInterstitial(
    adUnitId:String
):void

showRewardedInterstitial():void

isRewardedInterstitialLoaded():Boolean

destroyRewardedInterstitial():void
```

## App Open

```actionscript
loadAppOpen(
    adUnitId:String
):void

showAppOpen():void

isAppOpenLoaded():Boolean

destroyAppOpen():void
```

## Settings

```actionscript
setVolume(
    volume:Number
):void

setMuted(
    muted:Boolean
):void

setChildDirected(
    directed:Boolean
):void

setUnderAgeOfConsent(
    underAge:Boolean
):void

setMaxContentRating(
    rating:int
):void

addTestDevice(
    deviceId:String
):void
```

## UMP

```actionscript
requestConsentInfo(
    testMode:Boolean = false
):void

showConsentForm():void

getConsentStatus():int
```

## ATT

```actionscript
requestATT():void
```

# 1️⃣4️⃣ Event Reference

## AdMob Initialization

```actionscript
ADMOB_INITIALIZED
ADMOB_INIT_FAILED
```

## Banner

```actionscript
BANNER_LOADED
BANNER_LOAD_FAILED
BANNER_OPENED
BANNER_CLOSED
BANNER_IMPRESSION
BANNER_CLICKED
BANNER_REFRESHED
BANNER_REFRESH_FAILED
BANNER_POSITION_CHANGED
BANNER_POSITION_FAILED
BANNER_HIDDEN
BANNER_REMOVED
```

## Interstitial

```actionscript
INTERSTITIAL_LOADED
INTERSTITIAL_LOAD_FAILED
INTERSTITIAL_SHOWED
INTERSTITIAL_SHOW_FAILED
INTERSTITIAL_CLOSED
INTERSTITIAL_IMPRESSION
INTERSTITIAL_DESTROYED
```

## Rewarded

```actionscript
REWARDED_LOADED
REWARDED_LOAD_FAILED
REWARDED_SHOWED
REWARDED_SHOW_FAILED
REWARDED_CLOSED
REWARDED_IMPRESSION
REWARDED_REWARD
REWARDED_DESTROYED
```

## Rewarded Interstitial

```actionscript
REWARDED_INTERSTITIAL_LOADED
REWARDED_INTERSTITIAL_LOAD_FAILED
REWARDED_INTERSTITIAL_SHOWED
REWARDED_INTERSTITIAL_SHOW_FAILED
REWARDED_INTERSTITIAL_CLOSED
REWARDED_INTERSTITIAL_IMPRESSION
REWARDED_INTERSTITIAL_REWARD
REWARDED_INTERSTITIAL_DESTROYED
```

## App Open

```actionscript
APP_OPEN_LOADED
APP_OPEN_LOAD_FAILED
APP_OPEN_SHOWED
APP_OPEN_SHOW_FAILED
APP_OPEN_CLOSED
APP_OPEN_IMPRESSION
APP_OPEN_DESTROYED
```

## UMP

```actionscript
CONSENT_INFO_UPDATED
CONSENT_INFO_FAILED
CONSENT_FORM_DISMISSED
CONSENT_FORM_FAILED
```

## ATT

```actionscript
ATT_STATUS
ATT_FAILED
```

## Settings

```actionscript
UNDER_AGE_SET
MAX_RATING_SET
```

# 1️⃣5️⃣ Singleton Architecture

`AdMobANE` uses the Singleton pattern.

Correct:

```actionscript
var adMob:AdMobANE =
    AdMobANE.getInstance();
```

Do not instantiate it directly:

```actionscript
new AdMobANE();
```

The constructor prevents multiple instances and throws an error if another instance already exists.

# 1️⃣6️⃣ Cleanup

When the application no longer needs the ANE:

```actionscript
adMob.dispose();
```

The `dispose()` method:

1. Removes the native status event listener.
2. Calls the native `dispose` method.
3. Disposes the AIR `ExtensionContext`.
4. Clears the Singleton instance.

A new instance can subsequently be obtained with:

```actionscript
AdMobANE.getInstance();
```

# 1️⃣7️⃣ Advertisement Lifecycle

## Interstitial / Rewarded / Rewarded Interstitial / App Open

The general lifecycle is:

```text
Initialize
    │
    ▼
Load
    │
    ├── Load Failed
    │
    ▼
Loaded
    │
    ▼
Show
    │
    ├── Show Failed
    │
    ▼
Showed
    │
    ▼
Impression
    │
    ▼
Closed
    │
    ▼
Load Again
```

## Banner

Banner lifecycle:

```text
showBanner()
     │
     ▼
BANNER_LOADED
     │
     ├── BANNER_IMPRESSION
     ├── BANNER_CLICKED
     ├── BANNER_OPENED
     └── BANNER_CLOSED
```

Banner can additionally be:

```text
Refreshed
Moved
Hidden
Removed
```

# 1️⃣8️⃣ Recommended Project Structure

# 🎨 Adding AdMobANE to Adobe Animate

If you are using **Adobe Animate** to develop your Adobe AIR application, you can add `AdMobANE.ane` directly to the Animate project and use the ActionScript API provided by the extension.

## 1. Copy the ANE into Your Project

Copy:

```text
AdMobANE.ane
```

into your Adobe Animate project directory.

For example:

```text
MyProject/
│
├── MyProject.fla
├── AdMobANE.ane
└── ...
```

You can also keep ANEs inside a dedicated folder:

```text
MyProject/
│
├── MyProject.fla
├── ANE/
│   └── AdMobANE.ane
└── ...
```

Using a dedicated `ANE` folder is recommended when the project contains multiple native extensions.

## 2. Add the ANE to Adobe Animate

Open your `.fla` project in **Adobe Animate**.

Go to:

```text
File → ActionScript Settings...
```

In the **ActionScript Settings** window, select the:

```text
Library path
```

tab.

Click:

```text
Browse to ANE...
```

or the corresponding browse/add button available in your Animate version, then select:

```text
AdMobANE.ane
```

from your project directory.

After adding it, the ANE should appear in the project's library path.

> **Important:** Do not simply copy the ANE into the Animate installation directory. Add the `.ane` to the project through **ActionScript Settings**.

## 3. Add the ANE to the AIR Packaging Configuration

Adding the ANE to **ActionScript Settings → Library path** makes the ActionScript API available to the project.

The ANE must also be included when packaging the AIR application.

When publishing the AIR application, make sure:

```text
AdMobANE.ane
```

is included in the application's **Native Extensions / ANE** configuration.

The exact location of this option can differ between Adobe Animate versions.

The important requirement is that the final AIR package contains the ANE for the target platform.

## 4. Recommended Project Structure

A typical Adobe Animate project can look like this:

```text
MyProject/
│
├── MyProject.fla
├── MyProject-app.xml
│
├── ANE/
│   └── AdMobANE.ane
│
├── src/
│   └── ...
│
└── assets/
    └── ...
```

The `.fla`, AIR application descriptor, source files, and ANE can therefore remain inside the same project structure.

## 5. Building the iOS Application

Adobe Animate can be used to develop and configure the AIR project.

For the final iOS application, especially when using native iOS ANEs and frameworks, it is recommended to perform the final AIR packaging and signing on **macOS**.

> [!WARNING]
> **iOS Packaging — macOS Recommended**
>
> For the final iOS `.ipa` build, use **macOS with Xcode and the required AIR SDK environment**.
>
> Building or exporting the final iOS application on Windows may result in:
>
> * Code-signing errors
> * Linker errors
> * IPA packaging errors
> * Invalid application bundles
> * App Store validation errors
>
> This is particularly important when the ANE contains native iOS frameworks.

## 10. Quick Setup

The complete workflow is:

```text
Adobe Animate
      │
      ▼
Create/Open AIR project
      │
      ▼
Copy AdMobANE.ane
      │
      ▼
ActionScript Settings
      │
      ▼
Add AdMobANE.ane to Library Path
      │
      ▼
Add ANE to AIR packaging configuration
      │
      ▼
Configure application.xml
      │
      ├── GADApplicationIdentifier
      ├── NSUserTrackingUsageDescription
      ├── SKAdNetworkItems
      └── iPhone settings
      │
      ▼
Import AdMobANE classes
      │
      ▼
Initialize AdMob
      │
      ▼
ADMOB_INITIALIZED
      │
      ▼
Load / Show advertisements
      │
      ▼
Final iOS packaging
      │
      ▼
macOS + AIR SDK + Xcode
      │
      ▼
IPA
```


# 1️⃣9️⃣ Important Notes

### Initialization

Do not assume that calling:

```actionscript
initAdMob()
```

means AdMob is immediately ready.

Wait for:

```actionscript
ADMOB_INITIALIZED
```

before starting the normal ad workflow.

### Loading

Interstitial, Rewarded, Rewarded Interstitial and App Open ads are asynchronous.

Always wait for their corresponding:

```text
*_LOADED
```

event before showing them.

### Error Handling

Always handle:

```text
*_LOAD_FAILED
*_SHOW_FAILED
```

when implementing production advertising logic.

Use:

```actionscript
e.errorCode
```

for the native error code and:

```actionscript
e.rewardType
```

for the error message on error events.

### Reward Handling

Only grant the application's reward from:

```text
REWARDED_REWARD
```

or:

```text
REWARDED_INTERSTITIAL_REWARD
```

and use:

```actionscript
e.rewardType
e.rewardAmount
```

to determine the reward.

# 2️⃣0️⃣ License and Third-Party Software

This project is an independent Adobe AIR Native Extension.

It is not affiliated with or endorsed by Google, Apple, or Adobe.

Google Mobile Ads SDK, Google User Messaging Platform, Apple frameworks, Adobe AIR, and their associated trademarks and software are owned by their respective owners.

Use of Google advertising services remains subject to the applicable Google policies and terms.

Developers are responsible for complying with:

* Google AdMob policies
* Google consent requirements
* Apple App Store requirements
* Apple's privacy requirements

## 🛡️ Ad Policy & Content Controls

To help ensure that this project is used in a beneficial way and remains respectful of Islamic values, **Google AdMob** provides several controls that allow publishers to manage and limit the types of advertisements displayed in their applications.

### 🔒 Blocking Controls

From your **Google AdMob** account, go to:

**Apps → Select your app → Blocking controls**

You can use the available controls to manage advertising content:

* **Sensitive Categories** — Block specific sensitive advertising categories.
* **General Categories** — Block broader advertising categories.
* **Advertiser URLs** — Block advertisements associated with specific advertisers or websites.
* **Ad Review Center** — Review individual advertisements and block or report inappropriate ads.

### 📋 When an Inappropriate Ad Appears

You can review individual advertisements from:

**AdMob → Apps → Select your app → Blocking controls → Ad Review Center**

Review the advertisement and use **Block** or **Report** as appropriate.

It is also recommended to review your **Sensitive Categories** and **General Categories** settings periodically rather than relying only on blocking individual advertisements after they appear.

> ⚠️ **Important**
>
> Google classifies advertisements automatically. Therefore, blocking a specific category does not guarantee that every advertisement containing similar content will be prevented.
>
> Regularly reviewing advertisements through the **Ad Review Center** provides additional control over the content displayed in your application.

> **Disclaimer**
>
> This ANE provides the technical integration with **Google AdMob** only. It does not directly control which advertisements the AdMob network selects and displays.
>
> The responsibility for configuring blocking settings and reviewing advertisements rests with the **AdMob account owner**.

# 📋 Requirements

* Adobe AIR 51.4.x
* AdMob App ID
* AdMob Ad Unit IDs
* Compiled `AdMobANE.ane`

> [!WARNING]
> 
> ⚠️ **iOS Build — macOS Recommended**
>
> For iOS application packaging, it is strongly recommended to build and export the final AIR application on **macOS**.
>
> Building or exporting the iOS application on **Windows** may cause **code-signing, packaging, linker, or IPA validation errors**, especially when using native iOS ANEs and frameworks.
>
> For the most reliable iOS build process, use **macOS with Xcode and the required AIR SDK environment**.

# 🧩 SDKs and Native Libraries

The iOS implementation is based on the **latest available stable Google Mobile Ads SDK for iOS used by this project**, together with the corresponding Google User Messaging Platform SDK.

This ANE is built using the following technologies and SDKs:

- **Google Mobile Ads SDK for iOS:** `13.10.0`
- **Google User Messaging Platform (UMP) SDK for iOS:** `3.1.0`
- **Apple App Tracking Transparency (ATT):** Apple iOS SDK / Framework
- **Adobe AIR SDK:** `51.4.1.1`
- **Adobe AIR Native Extension API:** Included with Adobe AIR SDK

The native iOS implementation uses Google's Google Mobile Ads SDK and User Messaging Platform SDK together with Apple's native frameworks and the Adobe AIR Native Extension API.

The Google libraries are integrated on the native iOS side of the ANE rather than implemented in ActionScript.

---

## 🛡️ سياسة الإعلانات والمحتوى

حرصًا على أن يكون استخدام هذا المشروع نافعًا ومحترمًا للقيم الإسلامية، توفر **Google AdMob** مجموعة من أدوات التحكم التي تساعد أصحاب التطبيقات على إدارة المحتوى الإعلاني والحد من ظهور الإعلانات غير المناسبة.

### 🔒 إعدادات حظر الإعلانات

من حساب **Google AdMob** انتقل إلى:

**Apps → اختر التطبيق → Blocking controls**

ومن هناك يمكنك استخدام الأدوات المتاحة لإدارة المحتوى الإعلاني:

* **Sensitive Categories** — لحظر فئات إعلانية حساسة محددة.
* **General Categories** — لحظر فئات إعلانية عامة.
* **Advertiser URLs** — لحظر الإعلانات المرتبطة بمعلنين أو مواقع محددة.
* **Ad Review Center** — لمراجعة الإعلانات الفردية وحظر أو الإبلاغ عن الإعلانات غير المناسبة.

### 📋 عند ظهور إعلان غير مناسب

يمكنك مراجعة الإعلانات الفردية من:

**AdMob → Apps → التطبيق → Blocking controls → Ad Review Center**

ثم مراجعة الإعلان واستخدام خيار **Block** أو **Report** حسب الحالة.

كما يُنصح بمراجعة إعدادات **Sensitive Categories** و**General Categories** بشكل دوري، وعدم الاعتماد فقط على حظر الإعلانات بعد ظهورها.

> ⚠️ **ملاحظة مهمة**
>
> تقوم Google بتصنيف الإعلانات آليًا، ولذلك فإن حظر فئة معينة لا يضمن منع جميع الإعلانات التي قد تحتوي على محتوى مشابه.
>
> تساعد المراجعة الدورية للإعلانات من خلال **Ad Review Center** على توفير قدر أكبر من التحكم في المحتوى الإعلاني المعروض داخل التطبيق.

> **تنبيه**
>
> هذا الـ **ANE** يوفر التكامل التقني مع **Google AdMob** فقط، ولا يتحكم بشكل مباشر في الإعلانات التي تختار شبكة AdMob عرضها.
>
> تقع مسؤولية إعداد أدوات الحظر ومراجعة الإعلانات على عاتق **صاحب حساب AdMob**.

نسأل الله أن يجعل هذا العمل نافعًا، وأن يوفقنا إلى استعمال العلم والتقنية فيما فيه الخير والإصلاح، وألا نجعلها سببًا في نشر ما يخالف ديننا أو قيمنا.

---

## 🤲 الحمد لله

> **الحمد لله الذي بنعمته تتم الصالحات، والحمد لله على توفيقه وفضله وكرمه.**
>
> اللهم لك الحمد على ما علّمت ووفّقت ويسّرت، ولك الشكر على نعمك التي لا تُحصى.
>
> اللهم اجعل هذا العلم والعمل نافعًا للناس، واجعله حجةً لنا لا علينا، وبارك لنا فيه، واهدنا إلى استخدامه فيما يرضيك وينفع عبادك.
>
> اللهم لا تجعل ما علّمتنا سببًا في ضرر أحد أو ظلم أحد أو إفساد في الأرض، وأعنّا على استعماله في الخير والإصلاح، واجعلنا ممن يستعملون نعمك فيما تحب وترضى.
>
> **قال تعالى:**
>
> **﴿وَلَا تَبْغِ الْفَسَادَ فِي الْأَرْضِ ۖ إِنَّ اللَّهَ لَا يُحِبُّ الْمُفْسِدِينَ﴾**
>
> *القصص: 77*
>
> **وقال تعالى:**
>
> **﴿وَلَا تَعَاوَنُوا عَلَى الْإِثْمِ وَالْعُدْوَانِ ۚ وَاتَّقُوا اللَّهَ﴾**
>
> *المائدة: 2*
>
> **وقال تعالى:**
>
> **﴿إِنَّ السَّمْعَ وَالْبَصَرَ وَالْفُؤَادَ كُلُّ أُولَٰئِكَ كَانَ عَنْهُ مَسْئُولًا﴾**
>
> *الإسراء: 36*
>
> **اللهم اجعل هذا العمل علمًا نافعًا، وعملًا صالحًا، وأثرًا طيبًا، وانفع به من يحتاج إليه.**
>
> **رَبِّ أَوْزِعْنِي أَنْ أَشْكُرَ نِعْمَتَكَ الَّتِي أَنْعَمْتَ عَلَيَّ وَأَنْ أَعْمَلَ صَالِحًا تَرْضَاهُ.**
>
> **وَمَا تَوْفِيقِي إِلَّا بِاللَّهِ ۖ عَلَيْهِ تَوَكَّلْتُ وَإِلَيْهِ أُنِيبُ.**


## ☕ Support the Project

If this project has been useful to you, consider supporting its continued development. Your contribution helps maintain the project, fix bugs, improve compatibility, and develop new features.

### 💙 Support via PayPal

[![Support via PayPal](https://img.shields.io/badge/Support%20via%20PayPal-Donate-0070BA?logo=paypal\&logoColor=white)](https://www.paypal.com/ncp/payment/YFEPDXW4YH7HN)

Every contribution, regardless of size, helps support:

* 🛠️ Continued development and maintenance
* 🐛 Bug fixes and compatibility improvements
* 📱 Testing across iOS, macOS, and Adobe AIR versions
* 📚 Documentation and examples
* 🚀 New features and future improvements

Thank you for supporting the project and open-source development.

