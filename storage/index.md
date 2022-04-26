# Green Grey Analytics Library for Unity
## Current version

1.1.0

## 0. What you need for integration

**analytics_project_key** - the project ID key in the analytics system

You may obtain it from your manager in Green Grey

## 1.Adding to the project

1.1 In the Package Manager panel, select Add package from git URL

![](../images/index_0.png)

1.2 In the window that opens, insert the link [https://github.com/GreenGreyStudioOfficial/dmp_unity_library.git#1.1.0](https://github.com/GreenGreyStudioOfficial/dmp_unity_library.git#1.1.0)

1.3 Select the menu item GreenGrey → Analytics → Create GGAnalytics GameObject.

![](../images/index_1.png)

This will create a current scene GGAnalytics object that controls sending analytics to the server.

1.4 In the properties of the GGAnalytics object, specify **analytics_project_key** - the project ID key in the analytics system.

-   DebugApiKey - analytics key to send events when developing or testing the app (Android Key can be used for this too).
-   AndroidApiKey - analytics key to send events from the release version of the Android app.
-   IosApiKey - analytics key to send events from the release version of the iOS app.

> Important: filling in the DebugApiKey, AndroidApiKey and iosApiKey is obligatory. The keys can contain the same value, but it’s better to separate test and release environment to evade scrambling valid data with test events.

> DebugApiKey is used always when DebugMode is active. When assembling the release app, to use the correct release analytics keys, DebugMode needs to be disabled.

Here you can also manage settings of the analytics library:

![](../images/index_2.png)

**Api Uri** - the backend statistics address (do not change)

**Debug Mode** - use or do not use DebugApiKey.

**Debug Api Key** - analytics key to send events when developing or testing the app.

**Android Api Key** - analytics key to send events from the release version of the Android app.

**Ios Api Key** - analytics key to send events from the release version of the iOS app.

**Max Events Count To Send** - the maximum number of events before they are sent to the statistics server.

**Send Events Timeout In Sec** - timeout after which events will be sent to the server even if they haven't reached the maximum number.

**Register App Pause** - add or not the APP_ENABLE event (when debugging in the editor)

**Log Level** - log level for Unity console:

-   DEBUG - log all messages
-   WARNING - log only WARNING и ERROR messages
-   ERROR - log only ERROR messages
-   OFF - do not log any messages

## 2. Usage

For event tracking, use the methods of the DmpAnalytics.Instance global object

### Трекинг покупок

For purchase tracking, use LogPurchase methods:

```
void LogPurchase(string _currency, float _value, Dictionary<string, object> _eventParams);
void LogPurchase(string _currency, float _value);
```

where

_currency is the three-letter code of the purchase currency according to [ISO_4217](https://en.wikipedia.org/wiki/ISO_4217#Active_codes)

_value is the purchase amount

_eventParams are arbitrary additional parameters

Example:

```
DmpAnalytics.Instance.LogPurchase("USD", 0.99f, new Dictionary<string, object>
{
   ["lot"] = "big_coins_pack",
   ["from"] = "fullscreen_offer",
   ["show_count"] = 2
});
```

### Tracking arbitrary events

For event tracking, use the LogEvent method:

```
void LogEvent(string _eventName, Dictionary<string, object> _eventParams = null);
```

where

_eventName is the event name

_eventParams are arbitrary additional parameters

Example:

```
DmpAnalytics.Instance.LogEvent("SCENE_OPEN", new Dictionary<string, object>
{
   {"scene_index", currentSceneIndex},
   {"scene_name", SceneManager.GetActiveScene().name}
});
```

## 3. Integration check

When invoking DmpAnalytics.Instance methods, the console should display messages of events being sent (make sure Debug Mode is enabled in the analytics settings – see paragraph 1.4):

![](../images/index_3.png)

You can check the validity of the sent events in your personal Tableau dashboard. The access to the dashboard is granted by the Green Grey manager along with the application keys.

-   To do so, log in with the provided credentials at online.tableau.com.
-   You will see a dashboard with a single view that displays all the events received from your application.
-   Make sure all the events received by the server are the same as sent on your side.