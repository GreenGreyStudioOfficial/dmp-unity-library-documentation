# Green Grey Analytics Library for Unity

Библиотека для сбора и отправки событий игровой аналитики в системе DMP.

## Текущая версия

1.1.0

## 0. Что понадобится для интеграции?

**analytics_project_key** - ключ идентификации проекта в системе аналитики

Его вы можете получить у менеджера Green Grey

## 1. Добавление в проект

1.1 В панели Package Manager выберите Add package from git URL

![](../images/index_0.png)

1.2 В открывшемся окне вставьте ссылку [https://github.com/GreenGreyStudioOfficial/dmp_unity_library.git#1.1.0](https://github.com/GreenGreyStudioOfficial/dmp_unity_library.git#1.1.0)

1.3 Выберите пункт меню GreenGrey → Analytics → Create GGAnalytics GameObject.

![](../images/index_1.png)

Это создаст в текущей сцене объект GGAnalytics, управляющий отправкой аналитики на сервер.

1.4 В свойствах объекта GGAnalytics укажите свой **analytics_project_key** - ключ идентификации проекта в системе аналитики.

-   DebugApiKey - ключ аналитики для отправки событий при разработке/тестировании приложения.
-   AndroidApiKey - ключ аналитики для отправки событий релизной версии android приложения.
-   IosApiKey - ключ аналитики для отправки событий релизной версии ios приложения.

> Важно: заполнение полей DebugApiKey, AndroidApiKey и IosApiKey обязательно. Ключи могут содержать одно и то же значение, но желательно разделять тестовое и релизное окружение во избежании засорения актуальных данных тестовыми событиям.

> DebugApiKey используется всегда при активном DebugMode. При сборке релизного приложения, для использования соответствующих релизных ключей, его необходимо отключать.

Также здесь можно поменять управлять настройками библиотеки аналитики:

![](../images/index_2.png)

**Api Uri** - адрес бэкенда статистики (не менять)

**Debug Mode** - синализирует что нужно отправлять события только по DebugApiKey ключу.

**Debug Api Key** - ключ аналитики для отправки событий при разработке/тестировании приложения.

**Android Api Key** - ключ аналитики для отправки событий релизной версии android приложения.

**Ios Api Key** - ключ аналитики для отправки событий релизной версии ios приложения.

**Max Events Count To Send** - количество событий, после которых произойдет отправка на сервер статистики.

**Send Events Timeout In Sec** - таймаут, после которого события будут отправлены на сервер, даже если не достигнуто максимальное количество.

**Register App Pause** - добавлять или нет событие APP_ENABLE (при отладке в редакторе)

**Log Level** - уровень логирования в Unity console:

-   DEBUG - логировать все сообщения
-   WARNING - логировать сообщения уровня WARNING и ERROR
-   ERROR - логировать только сообщения уровня ERROR
-   OFF - выключение логирования

## 2. Использование

Чтобы трекать события используйте методы глобального объекта DmpAnalytics.Instance

### Трекинг покупок

Для трекинга покупок используйте методы LogPurchase:

```
void LogPurchase(string _currency, float _value, Dictionary<string, object> _eventParams);
void LogPurchase(string _currency, float _value);
```

где

_currency - трехбуквенный код валюты покупки по [ISO_4217](https://en.wikipedia.org/wiki/ISO_4217#Active_codes)

_value - сумма покупки

_eventParams - произвольные дополнительные параметры

Пример:

```
DmpAnalytics.Instance.LogPurchase("USD", 0.99f, new Dictionary<string, object>
{
   ["lot"] = "big_coins_pack",
   ["from"] = "fullscreen_offer",
   ["show_count"] = 2
});
```

### Трекинг произвольных событий

Для трекинга событий используйте метод LogEvent:

```
void LogEvent(string _eventName, Dictionary<string, object> _eventParams = null);
```

где:

_eventName - имя события

_eventParams - произвольные дополнительные параметры

Пример:

```
DmpAnalytics.Instance.LogEvent("SCENE_OPEN", new Dictionary<string, object>
{
   {"scene_index", currentSceneIndex},
   {"scene_name", SceneManager.GetActiveScene().name}
});
```

## 3. Проверка интеграции

При вызове методов DmpAnalytics.Instance в консоли должны отображаться сообщения об отправке событий (убедитесь, что флаг Debug Mode установлен в настройках аналитики - см. п. 1.4):

![](../images/index_3.png)

Проверить валидность отправленных событий вы сможете в персональном дашборде в Tableau, доступ к которому вы получите от менеджера Green Grey вместе с ключами приложения.