---
description: Floorplan card using the picture-elements card
---

# 3D Floorplan Card
This simple card displays a 3D floorplan and overlays black boxes to create the effect of lights turning on and off in the selected rooms.


![](../.gitbook/assets/floorplan.gif)

{% hint style="info" %}
Change the pictures and sensors to your own.
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="ui-lovelace.yaml" %}
```yaml
- type: picture-elements
  image: /local/Floorplan.png
  elements:

  - type: image
    tap_action: toggle
    entity: light.kitchen
    image: /local/Floorplan/kitchen.png
    state_filter:
      "off": opacity(50%)
      "on": opacity(1%)
    style:
      top: 80%
      left: 27.25%
      width: 39.75%

  - type: image
    tap_action: toggle
    entity: light.hallway
    image: /local/Floorplan/hallway.png
    state_filter:
      "off": opacity(50%)
      "on": opacity(1%)
    style:
      top: 73.7%
      left: 70.5%
      width: 43.75%

  - type: image
    tap_action: toggle
    entity: light.living_room
    image: /local/Floorplan/living_room.png
    state_filter:
      "off": opacity(50%)
      "on": opacity(1%)
    style:
      top: 36.25%
      left: 50%
      width: 85%

```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
Change the pictures and sensors to your own.

The 3D floorplan was made using the free website https://www.sweethome3d.com
The black boxes were cut out using the free software from https://www.gimp.org
{% endhint %}