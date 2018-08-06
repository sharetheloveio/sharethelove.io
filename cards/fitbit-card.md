---
description: >-
  A great way to display your Fitbit stats
---

# Fitbit Card
![](.gitbook/assets/fitbit-card.png)

This card requires you to install and setup the following components:

{% hint style="info" %}
[Fitbit](https://www.home-assistant.io/components/sensor.fitbit/)
{% endhint %}

This card requires you to install and setup the following custom cards:

{% hint style="info" %}
[circle-sensor-card](https://github.com/custom-cards/circle-sensor-card)
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="ui-lovelace.yaml" %}
```yaml
  - type: picture-elements
    image: /local/lovelace/icons/fitbit.png
    elements:
    - type: custom:circle-sensor-card
      entity: sensor.weight_loss
      max: 65
      min: 0
      stroke_width: 15
      gradient: true
      fill: '#125054'
      name: loss
      units: ' '
      font_style:
        font-size: 1.1em
        font-color: white
        text-shadow: '2px 2px black'
      style:
        top: 5%
        left: 80%
        width: 4em
        height: 4em
        transform: none
    - type: custom:circle-sensor-card
      entity: sensor.fitbit_steps
      max: 10000
      min: 0
      stroke_width: 15
      gradient: true
      fill: '#125054'
      name: steps
      units: ' '
      font_style:
        font-size: 1.1em
        font-color: white
        text-shadow: '2px 2px black'
      color_stops:
        1: '#09C7E8'
        10000: '#0BDF0E'
      style:
        top: 70%
        left: 5%
        width: 4em
        height: 4em
        transform: none
    - type: custom:circle-sensor-card
      entity: sensor.floors
      max: 10
      min: 0
      stroke_width: 15
      gradient: true
      fill: '#125054'
      name: floors
      units: ' '
      font_style:
        font-size: 1.1em
        font-color: white
        text-shadow: '2px 2px black'
      color_stops:
        1: '#09C7E8'
        10: '#0BDF0E'
      style:
        top: 70%
        left: 24%
        width: 4em
        height: 4em
        transform: none
    - type: custom:circle-sensor-card
      entity: sensor.distance
      max: 10
      min: 0
      stroke_width: 15
      gradient: true
      fill: '#125054'
      name: miles
      units: ' '
      font_style:
        font-size: 1.1em
        font-color: white
        text-shadow: '2px 2px black'
      color_stops:
        1: '#09C7E8'
        10: '#0BDF0E'
      style:
        top: 70%
        left: 43%
        width: 4em
        height: 4em
        transform: none
    - type: custom:circle-sensor-card
      entity: sensor.fitbit_calories
      max: 3000
      min: 0
      stroke_width: 15
      gradient: true
      fill: '#125054'
      name: cals
      shadow: true
      units: ' '
      font_style:
        font-size: 1.1em
        font-color: white
        text-shadow: '2px 2px black'
      color_stops:
        1: '#09C7E8'
        3000: '#0BDF0E'
      style:
        top: 70%
        left: 62%
        width: 4em
        height: 4em
        transform: none
    - type: custom:circle-sensor-card
      entity: sensor.fitbit_minutes_active
      max: 30
      min: 0
      stroke_width: 15
      gradient: true
      fill: '#125054'
      name: active
      units: ' '
      font_style:
        font-size: 1.1em
        font-color: white
        text-shadow: '2px 2px black'
      color_stops:
        1: '#09C7E8'
        30: '#0BDF0E'
      style:
        top: 70%
        left: 80%
        width: 4em
        height: 4em
        transform: none
    - type: custom:hui-markdown-card
      content: >
        Ian
      style:
        top: 6%
        left: 1%
        font-size: 18px
        color: white
        text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black
        "--paper-card-background-color": none
        "--paper-material-elevation-1_-_box-shadow": none
        "--shadow-elevation-2dp_-_box-shadow": none
        transform: translate(0%,-50%)
    - type: state-icon
      entity: sensor.weight
      title: Weight
      style:
        left: 3%
        top: 7%
        "--iron-icon-fill-color": '#09C7E8'
        transform: none
    - type: state-label
      entity: sensor.weight
      title: Weight
      style:
        left: 12%
        top: 8%
        color: white
        transform: none
    - type: state-icon
      entity: sensor.bmi
      title: BMI
      style:
        left: 31%
        top: 7%
        "--iron-icon-fill-color": '#09C7E8'
        transform: none
    - type: state-label
      entity: sensor.bmi
      title: BMI
      style:
        left: 40%
        top: 8%
        color: white
        transform: none
    - type: state-icon
      entity: sensor.body_fat
      title: Body Fat
      style:
        left: 59%
        top: 7%
        "--iron-icon-fill-color": '#09C7E8'
        transform: none
    - type: state-label
      entity: sensor.body_fat
      title: Body Fat
      style:
        left: 67%
        top: 8%
        color: white
        transform: none
    - type: image
      entity: sensor.ionic_battery
      title: Ionic Battery
      image: /local/lovelace/icons/battery_full.png
      state_image:
        "High": /local/lovelace/icons/battery_high.png
        "Medium": /local/lovelace/icons/battery_medium.png
        "Low": /local/lovelace/icons/battery_low.png
      style:
        left: 12%
        top: 1%
        transform: none
        width: 24px
        height: 24px
    - type: state-icon
      entity: sensor.resting_heart_rate
      title: Resting HR
      style:
        left: 3%
        top: 21%
        "--iron-icon-fill-color": red
        transform: none
    - type: state-label
      entity: sensor.resting_heart_rate
      title: Resting HR
      style:
        left: 12%
        top: 22%
        color: white
        transform: none
```
{% endcode-tabs-item %}
{% endcode-tabs %}

I had to create template sensors for steps and calories as the Fitbit sensor produces comma separated string values instead of numeric values. I also created a template sensor to track my weight loss.

```yaml
sensor:
  - platform: template
    sensors:
      fitbit_steps:
        friendly_name: 'Steps'
        value_template: >
          {{ states.sensor.steps.state | replace(",","") }}
        unit_of_measurement: 'steps'
      fitbit_calories:
        friendly_name: 'Calories'
        value_template: >
          {{ states.sensor.calories.state | replace(",","") }}
        unit_of_measurement: 'calories'
      weight_loss:
        friendly_name: 'Weight Loss'
        value_template: "{{ '%.1f' | format(245.0 - (states.sensor.weight.state | float)) }}"
        unit_of_measurement: 'lbs'
```

{% hint style="info" %}
**Additional tips:**

* Be sure to change "Ian" in the markdown card to match the user's
* Change your starting weight in the template sensor
* Change your goal weight loss for the `max` value in the `circle-sensor-card` for `sensor.weight_loss` along with any other goal values for other things like steps/distance/calories
* `sensor.resting_heart_rate` and `sensor.ionic_battery` are dependent on the Fitbit device you use and may not be available to you.

{% endhint %}
