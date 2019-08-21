---
description: >-
  Here is a simple yet cool way to display a list of items that might be broken in your install.
---

# Things That Are Probably Broken  Card

{% hint style="info" %}
This requires you to have the following custom card setup

* [Monster Card](https://github.com/ciotlosm/custom-lovelace/tree/master/monster-card)

{% endhint %}

![](../.gitbook/assets/broken-card.png)

```yaml
  - type: custom:monster-card
    card:
      type: entities
      title: Things that are broken
      show_header_toggle: false
    filter:
      include:
        - state: "unknown"
      exclude:
        - entity_id: group.*
```

