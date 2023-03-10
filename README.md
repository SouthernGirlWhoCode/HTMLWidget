# HTMLWidget
HTMLWidget's design allows developers to create themes, while writing the most minimal JavaScript code and load selected theme on iOS widget on the SpringBoard.

## Example of an HTMLWidget-based widget
HTMLWidget allows for easy value binding to attributes and contents of any HTML elments
``` html
<body>
    <p>HTMLWidget</p>
    <p>{battery.percentage}%</p>
    <p>{time}</p>
```
### Binding HTMLWidget variables to element content
``` html
<div>{variable}</div>
```
You can also use multiple of these in any given context:
``` html
<div>{variable.a}{variable.b} regular text {variable.c}</div>
```
***Note**: There must be no spaces or extra characters inside of {} for this to work. No JavaScript expressions are executed inside of brackets.*

### Binding HTMLWidget variables to attributes
``` html
<div @attr="{variable}">xxx</div>
```

### Binding boolean HTMLWidget variables to class names
This only works if the given variable only is 1 or 0; true or false or can be evaulated as boolean inside of JavaScript.
``` html
<div @class.hidden="variable">xxx</div>
```
***Note**: There are no brackets in this one. You can not use more than one inside of any given class name binding.**

### Binding HTMLWidget actions to element events
``` html
<button @onClick="media.play">play</button>
```
***Note**: This one also has no brackets.*

### Accessing the HTMLWidget JavaScript object
HTMLWidget is exposed as `window.hwidget` inside of the JavaScript context of the widget page. In order to make everything easier, we'll assign HTMLWidget to the `_` variable.
``` js
<script type="text/javascript">
    var _ = window.hwidget;
</script>
```
***Note**: All sections below will use the `_` instead of `window.hwidget`. All the code you will see should be executed inside of `<script>` unless stated otherwise. Minimal JavaScript knowledge is required.*

## Actions
HTMLWidget allows users to execute several actions.
``` js
_.action('action');
```
or:
``` js
_.action({
    action: 'name',
    other: 'arguments',
    ...
});
```
You can also bind actions to HTML elements by using the magical `@onXYZ="action"` attribute.

#### Examples
``` html
<button @class.hidden="media.playing" @onClick="media.play">play</button>
```
or:
``` html
<button @onClick="{'action': 'log', 'message': 'test log!'}">test log</button>
```

### Available Actions
This section lists all of the action that HTMLWidget supports. These can be used in many ways as shown by the examples above.

**Interaction** <br>
Enable user interaction on the widget
``` js
_.action('enableInteraction');
```
<br>

**Scrolling** <br>
Enable scrolling on the widget
``` js
_.action('enableScrolling');
```

<br>

**Scroll Indicator** <br>
Enable show scroll indicator
``` js
_.action('enableScrollingIndicator');
```

<br>

**Log**<br>
- ``log``: Allows you to log data to device's syslog (NSLog). The only argument it has is ``message``. It's shorter alias is ``_.log('message');``

**System Alert**
``` html
<button @onClick="{'action': 'systemAlert', 'title': 'HTML Widget', 'message': 'Hello, World!'}">UIAlertController</button>
```
<br>

**Media**<br>
- ``media.play``
- ``media.pause``
- ``media.stop``
- ``media.next``
- ``media.previous``

**Open**<br>
- ``open.url``: Arguements - url
- ``open.application``: Arguments - bundle (of application)
- ``open.controlcenter``
- ``open.notificationcenter``

**Compose**<br>
- ``compose.message``
- ``compose.mail``

**Capture**<br>
- ``capture.screenshot``

**Toggle**<br>
- ``toggle.airplanemode``
- ``toggle.wifi``
- ``toggle.cellular``
- ``toggle.bluetooth``
- ``toggle.lpm``
- ``toggle.appearance``
- ``toggle.flashlight``
- ``toggle.orientation``
- ``toggle.respring``
- ``toggle.safemode``

## Variables
**Time**<br>
| Variable name | Type | Description |
|---------------|------|-------------|
| time | string | Current time |
| time.withSeconds | string | Time with seconds |

**Battery**<br>
| Variable name | Type | Description |
|---------------|------|-------------|
| battery.charging | boolean | ``true`` if the phone is charging, otherwise ``false`` |
| battery.fully.charged | boolean | ``true`` if the phone is fully charged, otherwise ``false`` |
| battery.plugged.in | boolean | ``true`` if the phone is plugged in the power source, otherwise ``false`` |
| battery.percentage | string | Represents current battery percentage |

**Device**<br>
| Variable name | Type | Description |
|---------------|------|-------------|
| device.name | string | Device name (i.e. the one you set in Settings) |
| device.type | string | Device type (e.g. iPhone10,4) |
| device.version | string | iOS/iPadOS version |
| device.uptime | string | System Uptime |
| device.udid | string | Device's UDID |
| device.wifi.name | string | WiFi name |
| device.wifi.ip | string | WiFi I.P address |
| device.cellular.name | string | Cellular provider name |
| device.cellular.ip | string | Cellular I.P address |
| device.brightness | string | Device's screen brightness |
| device.screen.width | string | Device's screen width in pts |
| device.screen.height | string | Device's screen height in pts |
| device.locale.country | string | Locale country |
| device.locale.language | string | Locale language |
| device.locale.timezone | string | Locale TimeZone |
| device.locale.currency | string | Locale Currency |
| device.storage.total | string | Device's total storage space |
| device.storage.used | string | Device's used storage |
| device.storage.free | string | Device's free storage |
| device.memory.physical | integer | Device's physical memory |
| device.memory.used | integer | Used memory |
| device.memory.free | integer | Free memory |
| device.memory.total | integer | Total memory (different from physical; used + free) |

**Weather**<br>
| Variable name | Type | Description |
|---------------|------|-------------|
| weather.temperature.c | string | celsius |
| weather.temperature.f | string | fahrenheit |
| weather.temperature.k | string | kelvin |
| weather.location | string | Displays the city name |
| weather.condition | string | Natural language description of the current weather condition |
| weather.conditionCode | integer | |
| weather.windSpeed | float | Displays the wind speed |
| weather.humidity | float | |
| weather.visibility | float | |
| weather.pressure | float | |
| weather.dewPoint | float | |
| weather.uvIndex | integer | |
| weather.low.c | string | celsius |
| weather.high.c | string | celsius |
| weather.low.f | string | fahrenheit |
| weather.high.f | string | fahrenheit |
| weather.low.k | string | kelvin |
| weather.high.k | string | kelvin |
| weather.feel.like.c | string | celsius |
| weather.feek.like.f | string | fahrenheit |
| weather.feel.like.k | string | kelvin |
| weather.dayForecasts | array | Array of day forecasts |
| weather.hourlyForecasts | array | Array of hourly forecasts |

**Weather day forecasts (Object in the array)**: ``weather.dayForecasts``<br>
| Variable name | Type | Description |
|---------------|------|-------------|
| day | integer | |
| dayOfTheWeek | integer | |
| icon | integer | |
| temperature.high.c | float | celsius |
| temperature.low.c | float | celsius |
| temperature.high.f | float | fahrenheit |
| temperature.low.f | float | fahrenheit |
| temperature.high.k | float | kelvin |
| temperature.low.k | float | kelvin |

**Weather hourly forecast (object in the array)**: ``weather.hourlyForecasts``<br>
| Variable name | Type | Description |
|---------------|------|-------------|
| time | string | In the format of "HH:MM" |
| conditionCode | integer | |
| temperature.c | string | celsius |
| temperature.f | string | fahrenheit |
| temperature.k | string | kelvin |
| precipitation | float |  |

**Events upcoming 7 days events (object in the array)**: ``events.upcoming``<br>
| Variable name | Type | Description |
|---------------|------|-------------|
| title | string | |
| startTime | string | In the format of "dd/MM HH:mm" |
| endTime | string | In the format of "dd/MM HH:mm" |

**Alarms (object in the array)**: ``alarm.list``<br>
| Variable name | Type | Description |
|---------------|------|-------------|
| time | string | In the format of HH:mm |
| remaining | string | In the format of HH:mm |
| enabled | boolean | Whether the alarm clock is enabled |

**Media**<br>
| Variable name | Type | Description |
|---------------|------|-------------|
| media.playing | boolean | ``true`` if any media is playing, otherwise ``false`` |
| media.album | string | Album title |
| media.artist | string | Artist name |
| media.title | string | Title |
| media.elapsed | float | Playback time in seconds |
| media.duration | float | Track duration |
| media.application | string | Bundle identifier of the currently playing application |
| media.image | string | HTML data URL with a JPEG image of the artwork |

# Settings
If you are familar with xenhtml's settings ```Config.json``` then it should be relatively simple for you to implement your own settings for the widget. Take a look at the Example folder located at /Library/HTMLWidget/Widgets/Example/ to have a better understanding on how it works and it's pretty much similar to xenhtml but with a few adjustments.

## Resources
You will need to put all your image assets in the Resources folder for the settings i.e icons and banner.

## Retrieve variable from the settings
``` js
document.body.style.backgroundColor = config.examplecolourpickerkey;
```

## Config.json
##### This is the important part for the widget metadata!

Name of the widget
``` json
"name": "Example Widget"
```

Author
``` json
"author": "SGWC"
```

Bundle Identifier, you must add this for the widget to load saved settings
``` json
"bundleID" : "com.sgwc.examplewidget"
```

Version
``` json
"version" : "1.0.0"
```

Description of the widget
``` json
"description" : "Example description of the widget"
```

Widget family the one you want your widget to support iOS widget size
``` json
"widgetFamily": [
    "small",
    "medium",
    "large"
]
```

### Optional: add a banner to the main page of your settings

``` json
"banner": {
    "image": "cover",
    "height": 200.0
}
  ```
  
# Pages and Cell Types
The full Config.json should look like this

``` json
{
  "name": "Example Widget",
  "author": "SGWC",
  "bundleID" : "com.sgwc.examplewidget",
  "version" : "1.0.0",
  "description" : "Example description of the widget",
  "banner": {
      "image": "cover",
      "height": 200.0
  },
  "widgetFamily": [
    "small",
    "medium",
    "large"
],
  "options": [
      {
          "type": "title",
          "text": "General"
      },
      {
          "type": "page",
          "text": "Example Page",
          "options": [
              {
                  "type": "switch",
                  "text": "Example switch",
                  "default": true,
                  "key": "switchKey"
              }
          ]
      },
      {
        "type": "applist",
        "text": "Example Applist 1",
        "multiselection" : false,
        "default": "com.apple.AppStore",
        "key": "applistKey1"
      },
      {
        "type": "applist",
        "text": "Example Applist 2",
        "multiselection" : true,
        "default": [
            "com.apple.AppStore",
            "com.apple.Preferences",
            "com.apple.camera"
        ],
        "key": "applistKey2"
      },
      {
          "type": "switch",
          "text": "Example switch",
          "default": true,
          "key": "switchKey"
      },
      {
          "type": "color",
          "text": "Example colour",
          "default": "#000000",
          "key": "colourpickerkey"
      },
      {
          "type": "text",
          "text": "Example text",
          "default": "Hello, World!",
          "key": "textkey",
          "placeholder": "Example text..."
      },
      {
          "type": "text",
          "text": "Example short text",
          "default": "Hello, World!",
          "key": "textkey2",
          "placeholder": "Example text...",
          "mode": "short"
      },
      {
          "type": "number",
          "text": "Example number",
          "default": 1,
          "key": "numberkey"
      },
      {
          "type": "slider",
          "text": "Example slider",
          "min" : 0,
          "max" : 2,
          "default": 1,
          "key": "sliderkey"
      },
      {
          "type": "stepper",
          "text": "Example stepper",
          "min" : 1,
          "max" : 10,
          "default": 5,
          "key": "stepperkey"
      },
      {
          "type": "option",
          "text": "Example options",
          "comment": "Example footer comment",
          "default": "one",
          "key": "optionskey",
          "options": [
              {
                  "text": "Example value 1",
                  "value": "one"
              },
              {
                  "text": "Example value 2",
                  "value": "two"
              }
          ]
      },
      {
          "type": "multi",
          "text": "Example multi",
          "key": "multikey",
          "default": [
              "1",
              "2"
          ],
          "options": [
              {
                  "value": "1",
                  "text": "Example 1"
              },
              {
                  "value": "2",
                  "text": "Example 2"
              },
              {
                  "value": "3",
                  "text": "Example 3"
              }
          ]
      },
      {
          "type": "gap"
      },
      {
          "type": "title",
          "text": "Support"
      },
      {
          "type": "avatarlink",
          "icon": "icon",
          "text": "Example @Twitter",
          "url": "https://www.twitter.com"
      },
      {
          "type": "title",
          "text": "Donation"
      },
      {
          "type": "link",
          "text": "Buy me a coffee",
          "url": "https://www.paypal.com/"
      }
  ]
}

```
