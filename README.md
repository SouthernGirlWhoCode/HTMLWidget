# HTMLWidget
HTMLWidget's design allows developers to create themes, while writing the most minimal JavaScript code and load selected theme on iOS widget on the SpringBoard.

## Example of an HTMLWidget-based widget
HTMLWidget allows for easy value binding to attributes and contents of any HTML elments
```
<body>
    <p>HTMLWidget</p>
    <p>{battery.percentage}%</p>
    <p>{time}</p>
```
### Binding HTMLWidget variables to element content
```
<div>{variable}</div>
```
You can also use multiple of these in any given context:
```
<div>{variable.a}{variable.b} regular text {variable.c}</div>
```
***Note**: There must be no spaces or extra characters inside of {} for this to work. No JavaScript expressions are executed inside of brackets.*

### Binding HTMLWidget variables to attributes
```
<div @attr="{variable}">xxx</div>
```

### Binding boolean HTMLWidget variables to class names
This only works if the given variable only is 1 or 0; true or false or can be evaulated as boolean inside of JavaScript.
```
<div @class.hidden="variable">xxx</div>
```
***Note**: There are no brackets in this one. You can not use more than one inside of any given class name binding.**

### Binding HTMLWidget actions to element events
```
<button @onClick="media.play">play</button>
```
***Note**: This one also has no brackets.*

### Accessing the HTMLWidget JavaScript object
HTMLWidget is exposed as `window.hwidget` inside of the JavaScript context of the widget page. In order to make everything easier, we'll assign HTMLWidget to the `_` variable.
```
<script type="text/javascript">
    var _ = window.hwidget;
</script>
```
***Note**: All sections below will use the `_` instead of `window.hwidget`. All the code you will see should be executed inside of `<script>` unless stated otherwise. Minimal JavaScript knowledge is required.*

## Actions
HTMLWidget allows users to execute several actions.
```
_.action('action');
```
or:
```
_.action({
    action: 'name',
    other: 'arguments',
    ...
});
```
You can also bind actions to HTML elements by using the magical `@onXYZ="action"` attribute.

#### Examples
```
<button @class.hidden="media.playing" @onClick="media.play">play</button>
```
or:
```
<button @onClick="{'action': 'log', 'message': 'test log!'}">test log</button>
```

### Available Actions
This section lists all of the action that HTMLWidget supports. These can be used in many ways as shown by the examples above.

**Interaction** <br>
Enable user interaction on the widget
```
_.action('enableInteraction');
```
<br>

**Scrolling** <br>
Enable scrolling on the widget
```
_.action('enableScrolling');
```

<br>

**Scroll Indicator** <br>
Enable show scroll indicator
```
_.action('enableScrollingIndicator');
```

<br>

**Log**<br>
- ``log``: Allows you to log data to device's syslog (NSLog). The only argument it has is ``message``. It's shorter alias is ``_.log('message');``

**System Alert**
```
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
| weather.location | string | Displays the city name |
| weather.condition | string | Natural language description of the current weather condition |
| weather.conditionCode | integer | |
| weather.windSpeed | float | Displays the wind speed |
| weather.humidity | float | |
| weather.visibility | float | |
| weather.pressure | float | |
| weather.dewPoint | float | |
| weather.uvIndex | integer | |
| weather.low | string | |
| weather.high | string | |
| weather.dayForecasts | array | Array of day forecasts |
| weather.hourlyForecasts | array | Array of hourly forecasts |

**Weather day forecasts (Object in the array)**: ``weather.dayForecasts``<br>
| Variable name | Type |
|---------------|------|
| day | integer |
| dayOfTheWeek | integer |
| icon | integer |
| temperature.high | float |
| temperature.low | float |

**Weather hourly forecast (object in the array)**: ``weather.hourlyForecasts``<br>
| Variable name | Type | Description |
|---------------|------|-------------|
| time | string | In the format of "HH:MM" |
| conditionCode | integer | |
| temperature | float | |

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
