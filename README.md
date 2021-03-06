# // weatherbit
![GitHub release (latest by date including pre-releases)](https://img.shields.io/github/v/release/briis/weatherbit?include_prereleases&style=flat-square) [![hacs_badge](https://img.shields.io/badge/HACS-Default-orange.svg?style=flat-square)](https://github.com/custom-components/hacs) [![](https://img.shields.io/badge/COMMUNITY-FORUM-success?style=flat-square)](https://community.home-assistant.io/t/weatherbit-io-current-weather-and-forecast-data/200224)

The weatherbit integration adds support for the [weatherbit.io](https://www.weatherbit.io/) web service as a source for meteorological data for your location.

There is currently support for the following device types within Home Assistant:
* Weather
* Sensor

There is only support for *daily* forecasts, as the hourly forecast requires a paid API Key.

## Installation

### HACS Installation
This Integration is part of the default HACS store, so search for *Weatherbit* in HACS.

### Manual Installation

To add Weatherbit to your installation, create this folder structure in your /config directory:

`custom_components/weatherbit`.
Then, drop the following files into that folder:

```yaml
__init__.py
manifest.json
weather.py
sensor.py
entity.py
config_flow.py
const.py
string.json
translation (Directory with all files)
```
## Configuration
The Weatherbit weather service is free under the [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 Generic License](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode). Weather data will be pulled once every 30 minutes.

To add Weatherbit weather forecast to your installation, go to the Integrations page inside the configuration panel and add a location by providing the API Key for Weatherbit and longitude/latitude of your location.

If the location is configured in Home Assistant, it will be selected as the default location. After that, you can add additional locations.

During setup you will have the option of installing Individual sensors for each of the *Current Day* values plus the next seven days of Forecast. This will be setup by default, but you can opt not to install them by deselecting the checkbox. If you deselect, and later want the sensors installed, you will have to remove the Integration and then set it up again.

You will also have the option of adding *Weather Alerts* for your location. It is de-selected by default, but if you mark the box, an additional sensor will be created for Weather Alerts, showing the number of Alerts in the State, and the details for these alerts in the attributes. If you want to add or remove this sensor, delete the Integrations and re-add it to the system. For a very basic example of using the alerts in Lovelace, I modified some code from @eggman and you can find the [example here](https://github.com/briis/weatherbit/blob/master/weather_alert_markdown.yaml)

The units used are defined by the Unit System set in the *General* section of the *Configuration* page. If you change the unit system here, you will have to restart Home Assistent for this Integration to reflect the changes.

**You can only add locations through the integrations page, not in configuration files.**

### Remove Sensors that are not needed
The Integration creates a significant amount of sensors, so if there are any of these you don't need, and want to clean-up your installation, you can disable the sensors, so that they are not created in Home Assistant.

In order to do that:
* Go to the *Configuration* tab, select *Integrations*
* Find the *Weatherbit* Integration. Now click on *x entities* where x shows the number of entities created for Weatherbit.
* Select the sensors you don't need by marking the box to the left.
* Once you are done, click *DISABLE SELECTED* on the top right, and the sensors will be removed and not enabled on next restart.

Should you want them back, you can click on the *Filter* symbol on the top right, and select *Show disabled entities* and you can now re-add them again.

## API Key for Weatherbit
This integration requires an API Key that can be retrieved for free from the Weatherbit Webpage. Please [go here](https://www.weatherbit.io/account/create) to apply for your personal key.
This key allows you to make 500 calls pr. day, and as this Integration uses 4 calls per hour (96 pr day) with the default update interval, and with that, you can add a maximum of 5 locations to your setup, without exceeding the limit per day.
If you activate the *Weather Alerts* that will add another 2 calls per hour with the default settings.

**API KEY ESTIMATOR**<br>
* **Forecast** 1 call pr.cycle, so default 2 per hour with a 30 min update interval.
* **Current Data** 1 call pr.cycle, so default 2 per hour with a 30 min update interval.
* **Weather Alerts** 1 call pr.cycle, so default 2 per hour with a 30 min update interval.

If you set Forecast Update Interval to every 60 min, and Current Data Update Interval to every 10 min, and activate the Weather Alerts, the calculation would look like the following:<br>

* *Forecast* 1 * 60/60min * 24hrs = 24 calls per day
* *Current Data* 1 * 60/10min * 24hrs = 144 calls per day
* *Weather Alerts* 1 * 60/60min * 24hrs = 24 calls per day
* **Total**: 192 calls per day. So with this setup, you can have 2 locations in total configured.

### CONFIGURATION VARIABLES
**API Key**<br>
&nbsp;&nbsp;*(string)(Required)*<br>
&nbsp;&nbsp;Specify your Weatherbit API Key.

&nbsp;&nbsp;*Default value:*<br>
&nbsp;&nbsp;None

**latitude**<br>
&nbsp;&nbsp;*(float)(Required)*<br>
&nbsp;&nbsp;Manually specify latitude.

&nbsp;&nbsp;*Default value:*<br>
&nbsp;&nbsp;Provided by Home Assistant configuration

**longitude**<br>
&nbsp;&nbsp;*(float)(Required)*.<br>
&nbsp;&nbsp;Manually specify longitude.

&nbsp;&nbsp;*Default value:*<br>
&nbsp;&nbsp;Provided by Home Assistant configuration

**Wind Unit**<br>
&nbsp;&nbsp;*(string)(Optional)*.<br>
&nbsp;&nbsp;Select the Wind Unit, if Home Assistant is set to the Metric Unit System. Possible values are: m/s and km/h. If Unit System is Imperial, this will always be *mph (Miles per Hour)*

&nbsp;&nbsp;*Default value:*<br>
&nbsp;&nbsp;m/s (Meters per second)

**Update Interval - Current Data**<br>
&nbsp;&nbsp;*(int)(Optional)*.<br>
&nbsp;&nbsp;Specify the time in minutes for the Current Data being updated. Value between 4 and 60 minutes. If you set it in the low end, be carefull not to run over your daily limit of 500 calls.

&nbsp;&nbsp;*Default value:*<br>
&nbsp;&nbsp;30 minutes

**Update Interval - Forecast Data**<br>
&nbsp;&nbsp;*(int)(Optional)*.<br>
&nbsp;&nbsp;Specify the time in minutes for the Forecast Data being updated. Value between 30 and 120 minutes.

&nbsp;&nbsp;*Default value:*<br>
&nbsp;&nbsp;30 minutes

**Forecast Language**<br>
&nbsp;&nbsp;*(string)(Optional)*.<br>
&nbsp;&nbsp;Specify the language that Weatherbit should return the Forecast strings in. Se list of supported languages below.

&nbsp;&nbsp;*Default value:*<br>
&nbsp;&nbsp;en (English)

**Add Sensors**<br>
&nbsp;&nbsp;*(bool)Optional)*.<br>
&nbsp;&nbsp;Deselect this checkbox of you don't want the sensors added to Home Assistant.

&nbsp;&nbsp;*Default value:*<br>
&nbsp;&nbsp;True

**Activate Alerts**<br>
&nbsp;&nbsp;*(bool)Optional)*.<br>
&nbsp;&nbsp;Select this checkbox of you want the Weather Alerts sensor added to Home Assistant.

&nbsp;&nbsp;*Default value:*<br>
&nbsp;&nbsp;False

### Supported Forecast Languages

Here is the list of languages that Weatherbit can return Forecast strings in:
* en - English
* ar - Arabic
* az - Azerbaijani
* be - Belarusian
* bg - Bulgarian
* bs - Bosnian
* ca - Catalan
* cz - Czech
* da - Danish
* de - German
* fi - Finnish
* fr - French
* el - Greek
* es - Spanish
* et - Estonian
* hr - Croation
* hu - Hungarian
* id - Indonesian
* it - Italian
* is - Icelandic
* iw - Hebrew
* kw - Cornish
* lt - Lithuanian
* nb - Norwegian Bokmål
* nl - Dutch
* pl - Polish
* pt - Portuguese
* ro - Romanian
* ru - Russian
* sk - Slovak
* sl - Slovenian
* sr - Serbian
* sv - Swedish
* tr - Turkish
* uk - Ukrainian
* zh - Chinese (Simplified)
* zh-tw - Chinese (Traditional)
