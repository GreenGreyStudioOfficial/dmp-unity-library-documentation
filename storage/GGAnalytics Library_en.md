# GGAnalytics Library for Unity

**GGAnalitics Library** is the library used for collecting application data. The library collects product and behavioral data for its own analytics service.  

The analytics contains:

* regular events such as running the app, closing the app, minimizing the app;

* the developer’s own events.

All internal and external (partners') projects use the following taxonomy:

|Event     | Parameters                                                   |Description                                                                                                                                                     |
|----------| -------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
|install   |                                                              |Event sent upon SDK initiation                                                                                                                                  |
|id_confirm|                                                              |For ios. Group of events for idfa/adid restrictions. Can possibly be sent more than once.                                                                       |
|          | status: confirmed, declined                                  |User's agreement/disagreement to transfer the id.                                                                                                               |
|session   |                                                              |Event sent upon session start.                                                                                                                                  |
|          | session_id                                                   |Each session's unique id is indicated _in all_ events and is specified at the session start.                                                                    |
|progress  |                                                              |Progress events, which are generally levels completion (core) or account level gain (meta)                                                                      |
|          | type: core, meta, event, social                              |                                                                                                                                                                |
|          | name                                                         |Name that defines progress (game level, account level, event or guild ranking position, etc.)                                                                   |
|          | value                                                        |New value that defines progress.                                                                                                                                |
|          | status: complete, incomplete, failed, other                  |Status                                                                                                                                                          |
|economics |                                                              |Event to display each currency flow in the game                                                                                                                 |
|          | source_type: store, limited_time_offer, one_time_offer, other|Gameplay area relevant to the purchase                                                                                                                          |
|          | currency: soft, hard, other                                  |Currency type: can be customized, Archero runes-like                                                                                                            |
|          | source_name                                                  |Display, gameplay element, feature or store page where the purchase was made                                                                                    |
|          | name                                                         |Purchase name                                                                                                                                                   |
|          | value                                                        |Amount of currency received or spent                                                                                                                            |
|other     |                                                              |Every game will contain actions that do not fall into the standard progress or resources spending categories. Custom events are to be created for these purposes|
|purchase  |                                                              |in-app purchases                                                                                                                                                |
|          | type: direct, offer                                          |Type: direct in-store or offer purchase. Other options are available                                                                                            |
|          | source_name                                                  |Display, feature or store page where the purchase was made                                                                                                      |
|          | name                                                         |Purchase name                                                                                                                                                   |
|          | status: confirmed, unconfirmed                               |Purchase status: whether or not the transaction was successful                                                                                                  |
|          | revenue                                                      |Default settings: dollars, tax-free, AF self-calculated                                                                                                         |
|ab_group  | test_name                                                    |Testing name                                                                                                                                                    |
|          | test_group                                                   |User's assigned group                                                                                                                                           |
|ad_view   |                                                              |Any user interaction with advertisements                                                                                                                        |
|          | status: start, end, break, error                             |Status: start/end/break viewing or error during advertisement display                                                                                           |
|          | value                                                        |Additional information (particularly the error code)                                                                                                            |
|          | type: interstitial, rewarded, banner, other                  |                                                                                                                                                                |
|          | placement                                                    |In-game advertising placement                                                                                                                                   |
|          | revenue                                                      |Default settings: dollars, tax-free. This data is transferred in the app.                                                                              |

According to the project obligatoriness, additional events can be built in.

All the events are saved in the database and can be analyzed with **Tableau** interface.

Benefits of analytics use:

* your own free analytic service;
* storing large data searching, filtering and sorting with your own settings;
* saving time: data from different sources (mediation, marketing and product analytics services, etc.) are stored and analyzed with one service
* justified predictions: analytics services allow you to evaluate not only the project from all sides within a single ecosystem, but also compare it with other projects. More effective predicts and hypothesizes are built based on this analytics.

[Connection Requirements](#connection-requirements)

[How to configure GGAnalytics Library](#how-to-configure-gg-analytics-library)

[How to Add GGAnalytics Library to the Project](#how-to-add-gg-analytics-library-to-the-project)

[How to Track Purchase](#how-to-track-purchase)

[How to Track Random Events](#how-to-track-random-events)

[How to Check up the Successful Integration](#how-to-check-up-the-successful-integration)

## Connection Requirements

### Software Requirements

* Unity software, version 2019.x and later

* at least one work project

### How to Receive a Token

Token *analytics_project_key* is project identification key in the Analytics System.

To receive a token, send following information on a.bobrov@greengreystudio.com:

* Package ID

* Android/iOS

To connect the library see [How to configure GGAnalytics Library]()

## How to Add GGAnalytics Library to the Project

There are three ways to add **GGAnalytics Library** to the project:

* via Unity project;
* via code;
* combined.

The examples are available in **Green Gray Analytics Unity modul**:

![img7]("C:\Users\79037\Documents\GG\Documentation\GGAnalitics Library\img\img7.png")

### How to add GGAnalytics Library to the Project via Unity project

To add the library to the Unity project :

1. Open the window **Package Manager** of the dropdown menu **Window**

2. Click **+** and select **Add package from git URL…** from the dropdown menu   


  ![img1](C:\Users\v.kk\Documents\Documentation\GGAnalitics Library\img\img1.png)
  
3. Specify the following link in the opened string and click **Add**:

`https://github.com/GreenGreyStudioOfficial/dmp_unity_library.git#1.3.0`

4. Create the game object **GGAnalytics** for the actual scene.  

> :warning: To initialize game object **GGAnalytics** from the app running, go to the starting scene before creating the object  

For this go to **GreenGrey → Analytics → Create GGAnalytics GameObject**

**GGAnalytics**  will be shown in the panel **Hierarchy**:

![img3]("C:\Users\79037\Documents\GG\Documentation\GGAnalitics Library\img\img3.png")

> ℹ️ The object **GGAnalytics** manages dispatches of analytic information to the server 

  5. Click the object **GGAnalytics** to open the settings in the panel **Inspector**

  6. Specify the received token in the field **Api Key** in the panel **GG Analytics Configuration (Script)**
  
![img4]("C:\Users\79037\Documents\GG\Documentation\GGAnalitics Library\img\img4.png")


> ⚠️ **Api Key** is used while **Debug Mode** is turned on. For release **Debug Mode** must be turned off so that released keys can be used.

### How to add GGAnalytics Library to the Project via code

Initialize the library adding the following code:

```
private void InitAndSetupAnalytics()
        {
            var go = new GameObject("GGAnalytics");
            DontDestroyOnLoad(go);
            GGAnalyticsInstance.Create(go);
            var configuration = new GGAnalyticsConfigurationExampleSample3();
            GGAnalyticsInstance.Setup(configuration);
        }
```

 And set up the library using the following code:

```
public class GGAnalyticsConfigurationExampleSample3 : IGGAnalyticsConfiguration
    {
        public string ApiUri => "[https://dmp.greengreystudio.com/events](https://dmp.greengreystudio.com/events)";
        public LogLevel LogLevel => LogLevel.DEBUG;
        public string ApiKey => "779a42bdbebeccc7099de0aaff8b7298d4c22638a95028c362da50bcc5da9e67";
        public uint MaxEventsCountToSend => 10;
        public uint SendEventsTimeoutInSec => 600;
        public bool RegisterAppPause => true;
        public bool UsePrettyLogger => false;
    }
```

All parameters are described [here](#how-to-configure-gganalytics-library).

### Combined way to add GGAnalytics to the project

You also can initialize the library adding the following code:

```
private void InitAndSetupAnalytics()
        {
            var go = new GameObject("GGAnalytics");
            DontDestroyOnLoad(go);
            GGAnalyticsInstance.Create(go);
            var configuration = new GGAnalyticsConfigurationExampleSample3();
            GGAnalyticsInstance.Setup(configuration);
        }
```

And set it up in the panel **GG Analytica Configuration (Script)** of Unity project.

To check if the library is connected correctly see [How to Check up the Successful Integration](#how-to-check-up-the-successful-integration) 

### How to configure GGAnalytics Library

The library can be configured in the panel **GG Analytics Configuration (Script)**

 ![img4]("C:\Users\79037\Documents\GG\Documentation\GGAnalitics Library\img\img4.png")
 
The settings are described in the table below:

|Setting                   |Description                                                                                                                                                                                                    | Value                    |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------ |
|Api URL                   |Address of backend of statistic                                                                                                                                                                                | do not change            |
|Api Key             |Analytic key used for sending messages while developing/testing                                                                                                                                                | token                    |
|Max Events Count to Send  |Maximum number of event in the bunch to be sent to the statistic server                                                                                                                                        | 5 (minimum value)        |
|Send Events Timeout In Sec|Timeout after that the bunch will be sent to the server even if maximum number of events in it are not reached                                                                                                 | 100 (minimum value)      |
|Debug Mode                |Debug logging in the Command Prompt while integrating of the library. It is recommended to be turned on while integrating                                                                                      | turned off (recommended) |
|Register App Pause        |Fixation every time the application was minimized/paused in Debug logging. It is recommended to be turned off while integrating                                                                                | turned on (recommended)  |
|Log Level                 |Log level in Unity console. Contain the following values: DEBUG (logging all the messages), WARNING (logging warning and errors messages), ERROR (logging only error messages), OFF (logging is turned off)    | DEBUG (by default)       |

To check if the library set up correctly see [How to Check up the Successful Integration]()

## How to Track Purchase

To track any events use methods of the global object **GGAnalytics.Instance**

To track purchases use the method **LogPurchase**

`void LogPurchase(string _currency, float _value, Dictionary<string, object> _eventParams);`  
or  
`void LogPurchase(string _currency, float _value);`

| Variable    | Description                                                                      |
| ----------- | -------------------------------------------------------------------------------- |
| currency    | Three-letter currency code of the purchase according to ISO 4217. Written in ““  |
| value       | Purchase price. Specified in float format                                        |
| eventParams | User’s additional parameters                                                     |

Example:

```
GGAnalytics.Instance.LogPurchase("USD", 0.99f, new Dictionary<string, object>  
{  
   ["lot"] = "big_coins_pack",  
   ["from"] = "fullscreen_offer",  
   ["show_count"] = 2   
});  
```

To track random events see [How to Track Random Events]()

## How to Track Random Events

To track any events use methods of the global object **GGAnalytics.Instance**

To track random events use the method **LogEvent**

`void LogEvent(string _eventName, Dictionary<string, object> _eventParams);`  
or  
`void LogEvent(string _eventName);`

| Variable    | Description                  |
| ----------- | ---------------------------- |
| eventName   | Name of the event            |
| eventParams | User’s additional parameters |

Example:

```
GGAnalytics.Instance.LogEvent("SCENE_OPEN", new Dictionary<string, object>
{
   {"scene_index", currentSceneIndex},
   {"scene_name", SceneManager.GetActiveScene().name}
});
```

To track purchases see [How to Track Purchase](#how-to-track-purchase)

## How to Check up the Successful Integration

Messages about sent events must be shown in the **Command Prompt** every time **GGAnalitics.Instance** methods are used.

> ⚠️Be sure **Debug Mode** is turned on (see [How to configure GGAnalytics LIbrary](#how-to-configure-gg-analytics-library))

![img5]("C:\Users\79037\Documents\GG\Documentation\GGAnalitics Library\img\img5.png")

To check up the validity of the messages use personal dashboard in **Tableau**.

> ℹ️ Access to the dashboard is received when you receive the token from GreenGrey Manager

For this:

1. Log-in in [online.tableau.com](online.tableau.com). There are your personal dashboard filled up with all events sent from your app

2. Match that all sent messages from your side are received by a server

![img6]("C:\Users\79037\Documents\GG\Documentation\GGAnalitics Library\img\img6.png")

To connect the library correctly see [How to Add GGAnalytics Library to the Project](#how-to-add-gg-analytics-library-to-the-project)  