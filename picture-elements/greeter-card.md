# Greeter Card

_A simple greeter card that changes the welcome message based on time of day, it also uses weather from minutecast with animated icons._

![greeter-card](../.gitbook/assets/greeter-card.png)

This card requires you to install and setup the following components:

{% hint style="info" %}

* [Yahoo Weather](https://www.home-assistant.io/components/weather.yweather/)
* [Google Calendar](https://www.home-assistant.io/components/calendar.google/)

{% endhint %}

```yaml
    - type: picture-elements
      image: /local/banners/vacation.jpg
      elements:
        - type: state-label
          entity: sensor.time
          style:
            top: 8%
            left: 10%
            color: rgb(249, 251, 255)
            text-shadow: 2px 2px DarkSlateGrey
            font-family: Trebuchet MS
            font-weight: bold
            pointer-events: none
            text-rendering: optimizeLegibility
            -moz-osx-font-smoothing: grayscale
            font-smoothing: antialiased
            -webkit-font-smoothing: antialiased

        - type: state-icon
          entity: sensor.outside_temp
          style:
            top: 7.5%
            left: 82%
            pointer-events: none
            max-width: 24px
            max-height: 24px
            color: rgb(249, 251, 255)
        - type: state-label
          entity: sensor.outside_temp
          style:
            top: 8%
            left: 90%
            font-weight: bold
            color: rgb(249, 251, 255)
            font-family: Trebuchet MS
            text-shadow: 2px 2px DarkSlateGrey
            text-rendering: optimizeLegibility
            -moz-osx-font-smoothing: grayscale
            font-smoothing: antialiased
            -webkit-font-smoothing: antialiased

        - type: state-label
          entity: sensor.time_of_day
          style:
            top: 40%
            left: 50%
            color: rgb(249, 251, 255)
            font-size: 250%
            pointer-events: none
            text-shadow: 2px 2px DarkSlateGrey
            font-family: Trebuchet MS
            font-style: oblique
            font-weight: 400
            text-rendering: optimizeLegibility
            -moz-osx-font-smoothing: grayscale
            font-smoothing: antialiased
            -webkit-font-smoothing: antialiased

        - type: state-label
          entity: sensor.outside_alerts
          style:
            top: 52%
            left: 50%
            color: rgb(249, 251, 255)
            font-family: Trebuchet MS
            font-style: oblique
            text-shadow: 2px 2px DarkSlateGrey
            font-size: 120%
            pointer-events: none
            font-weight: 400
            text-rendering: optimizeLegibility
            -moz-osx-font-smoothing: grayscale
            font-smoothing: antialiased
            -webkit-font-smoothing: antialiased

        - type: state-label
          entity: sensor.greeter_card_info
          style:
            top: 82%
            left: 0%
            color: rgb(255, 255, 255)
            transform: none
            font-family: Trebuchet MS
            text-shadow: 2px 2px black
            font-size: 90%
            pointer-events: none
            font-weight: bold
            border-left-style: solid
            box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19)
            border-color: rgb(34, 154, 210)
            background-color: rgb(54, 65, 78)
            opacity: 0.8
```

Here are the package with sensors

{% hint style="warning" %}
Please change the following states on the follwing sensors

* greeter\_card\_info  
* time\_of\_day  


  ```yaml
  states('yourawesome_device_tracker')
  as_timestamp(states.calendar.your_awesome_mail.attributes.start_time)
  states.calendar.your_awesome_mail.attributes.message
  change User to wanted name
  ```

{% endhint %}

```yaml
  - platform: time_date
    display_options:
      - 'time'

  - platform: template
    sensors:
      time_of_day:
        value_template: >
          {% set current_hour = strptime(states('sensor.time'), "%H:%M").hour %}
          {% if current_hour < 12 %}
            Good Morning, User
          {% elif 12 <= current_hour < 18 %}
            Good Afternoon, User
          {% elif 18 <= current_hour < 23 %}
            Good Evening, User
          {% else %}
            Good Night, User
          {% endif %}
  
  - platform: template
    sensors:
      greeter_card_info:
        value_template: >
          Current Location: {{ states('yourawesome_device_tracker') }}

          Next Event: {{ as_timestamp(states.calendar.your_awesome_mail.attributes.start_time) | timestamp_custom('%Y/%m/%d at %H:%M') }} - {{ states.calendar.your_awesome_mail.attributes.message }}
      
  - platform: command_line
    name: "Outside: Alerts"
    icon: alert-outline
    command: "/home/.homeassistant/homeassistant/includes/script/shell/check-weather.sh"
    scan_interval: 180 
    
  - platform: template
    sensors:
      outside_temp:
        friendly_name: "Outside Temperature"
        unit_of_measurement: '°C'
        value_template: "{{ states.weather.yweather.attributes.temperature }}"
        icon_template: >
          {% if is_state("weather.yweather", "sunny") -%}
             mdi:weather-sunny
          {% elif is_state('weather.yweather', 'clear-night') -%}
             mdi:weather-night
          {% elif is_state('weather.yweather', 'rain') -%}
             mdi:weather-rainy
          {% elif is_state('weather.yweather', 'snow') -%}
             mdi:weather-snowy
          {% elif is_state('weather.yweather', 'sleet') -%}
             mdi:weather-snowy-rainy
          {% elif is_state('weather.yweather', 'wind') -%}
             mdi:weather-windy-variant
          {% elif is_state('weather.yweather', 'fog') -%}
             mdi:weather-fog
          {% elif is_state('weather.yweather', 'cloudy') -%}
             mdi:weather-cloudy
          {% elif is_state('weather.yweather', 'partly-cloudy-day') -%}
             mdi:weather-partlycloudy
          {% elif is_state('weather.yweather', 'hail') -%}
             mdi:weather-hail
          {% elif is_state('weather.yweather', 'thunderstorm') -%}
             mdi:weather-lightning
          {% else %}
             mdi:help-circle
          {% endif %}

        entity_picture_template: >
          {% if is_state("weather.yweather", "sunny") -%}
             /local/weather_icons/static/sunny.svg
          {% elif is_state('weather.yweather', 'clear-night') -%}
             /local/weather_icons/static/clear-night.svg
          {% elif is_state('weather.yweather', 'rain') -%}
             /local/weather_icons/static/rain.svg
          {% elif is_state('weather.yweather', 'snow') -%}
             /local/weather_icons/static/snow.svg
          {% elif is_state('weather.yweather', 'sleet') -%}
             /local/weather_icons/static/sleet.svg
          {% elif is_state('weather.yweather', 'wind') -%}
             /local/weather_icons/static/wind.svg
          {% elif is_state('weather.yweather', 'fog') -%}
             /local/weather_icons/static/fog.svg
          {% elif is_state('weather.yweather', 'cloudy') -%}
             /local/weather_icons/static/cloudy.svg
          {% elif is_state('weather.yweather', 'partly-cloudy-day') -%}
             /local/weather_icons/static/partly-cloudy-day.svg
          {% elif is_state('weather.yweather', 'hail') -%}
             /local/weather_icons/static/hail.svg
          {% elif is_state('weather.yweather', 'thunderstorm') -%}
             /local/weather_icons/static/thunderstorm.svg
          {% else %}
             /local/weather_icons/static/help.svg
          {% endif %}
```

Here is the bash script for Accuweather change the url to your city

```bash
#!/bin/bash
SET_AGENT="Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:60.0) Gecko/20100101 Firefox/60.0"
SET_URL="https://www.accuweather.com/en/se/boras/315908/minute-weather-forecast/315908"

weather_alert () {

curl -A "$SET_AGENT" -s $SET_URL |\
         perl -wne 'print if /<div class="mc-summary/ .. /<\/div>/' |\
         perl -wne 'print if /<p>/ .. /<\/p>/' |\
         sed 's/<\/p>//' | sed 's/<p>//' | grep -oE "[A-Z].*"
}

weather_alert
```

{% hint style="info" %}
**Additional tips:**   
  
This package can use more customize-able input like [https://philhawthorne.com/making-home-assistants-presence-detection-not-so-binary/](https://philhawthorne.com/making-home-assistants-presence-detection-not-so-binary/) that will make the location status messages appear more dynamic. To animate the icons used for temperature use this icon pack [https://www.amcharts.com](https://www.amcharts.com) and put it in your WWW directory in Homeassistant.
{% endhint %}

{% file src="../.gitbook/assets/10922815\_725782897533748\_7579215957418386485\_o.jpg" caption="the image thats used for this card" %}



