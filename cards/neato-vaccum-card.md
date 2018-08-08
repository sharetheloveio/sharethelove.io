---
description: >-
  Here is a simple yet cool way to display your cleaning map as a card in
  homeassistant.
---

# Neato Vaccum Card

{% hint style="info" %}
This requires you to have the following component setup

* [Neato Botvac](https://www.home-assistant.io/components/neato/)

{% endhint %}

![](../.gitbook/assets/image%20%281%29.png)

```yaml
  - type: picture-elements
    image: /api/camera_proxy/camera.neato_cleaning_map?token=*
    elements:
    - type: state-icon
      tap_action: toggle
      entity: vacuum.neato
      service: vacuum.locate
      style:
        top: 5%
        left: 95%
        "--paper-item-icon-color": rgb(115, 122, 130)

    - type: state-icon
      tap_action: toggle
      entity: switch.neato_schedule
      style:
        top: 15%
        left: 95%
        "--paper-item-icon-color": rgb(115, 122, 130)

    - type: state-label
      entity: sensor.neato_battery
      style:
        top: 70%
        left: 0%
        color: rgb(115, 122, 130)
        transform: none
        pointer-events: none
        text-shadow: 1px 1px black
        font-family: Trebuchet MS
        font-size: 90%
        font-weight: bold
        border-left-style: solid
        border-color: DeepSkyBlue
        background-color: rgb(54, 65, 78)
        
    - type: state-label
      entity: sensor.neato_status
      style:
        top: 80%
        left: 0%
        color: rgb(115, 122, 130)
        transform: none
        pointer-events: none
        text-shadow: 1px 1px black
        font-family: Trebuchet MS
        font-size: 90%
        font-weight: bold
        border-left-style: solid
        border-color: Tomato
        background-color: rgb(54, 65, 78)

    - type: state-label
      entity: sensor.neato_area
      style:
        top: 90%
        left: 0%
        color: rgb(115, 122, 130)
        transform: none
        pointer-events: none
        text-shadow: 1px 1px black
        font-family: Trebuchet MS
        font-size: 90%
        font-weight: bold
        border-left-style: solid
        border-color: GreenYellow
        background-color: rgb(54, 65, 78)
```

Here is the package for the sensors

{% hint style="warning" %}
Change neato on the states for your robots name in the template sensors
{% endhint %}

```yaml
  - platform: template
    sensors:
      neato_area:
        friendly_name: "Neato: Area cleaned on last run"
        value_template: "Area Cleaned: {{ states.vacuum.neato.attributes.clean_area | round(1) }}"
        unit_of_measurement: 'm²'

  - platform: template
    sensors:
      neato_status:
        friendly_name: "Neato: Status"
        value_template: "Status: {{ states.vacuum.neato.attributes.status }}"
```
