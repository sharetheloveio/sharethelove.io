---
description: >
  Jinja2 script to help you create/migrate the existing config/setup to lovelace automagically.
---

{% hint style="info" %}

The following is a jinja2 script that you need to run in the Developer Tools -> Templates (template editor), then copy and paste the output into your `ui-lovelace.yaml` file. Copying the following jinja code/script directly into the `ui-lovelace.yaml` file will give you severe headaches and possibly long-term Migraines. 

{% endhint %}

# Code to auto-generate Lovelace Script

Use the following script to automagically generate the lovelace cards based on what you already have. Go to your Home Assistant, click on the hamburger icon on the top left, under `Developer Tools`, click on `Templates`, and then replace what's out there with the following code. You should see the output right next to it. Copy that output, and paste it in the `ui-lovelace.yaml` file in your config folder and go to https://<<ha>>:8123/lovelace url and hit refresh.

```yaml
{# @Author: Mahasri Kalavala (a.k.a @skalavala #}

{%- macro plural(name) -%}
  {%- if name[(name|length|int -1):] == "y" -%}
  {{- name[:-1] -}}ies
{%- else -%}
  {{- name -}}s
{%- endif -%}
{%- endmacro -%}

title: My Lovely Home Automation
views:
  - title: Home
    cards:
{%- set domains = states | map(attribute='domain') |list | unique | list %}
{%- for item in states['camera'] %}
      - type: picture-entity
        title: {{ item.name }}
        entity: {{ item.entity_id }}
        camera_image: {{ item.entity_id }}
        show_info: true
        tap_action: dialog
{% endfor %}
{%- for item in states['media_player'] %}
      - type: media-control
        entity: {{ item.entity_id }}
{% endfor %}
{%- for item in domains if item != "camera" and item != "media_player" %}
      - type: {% if item == "device_tracker" -%}
        entity-filter
        {%- elif item == "camera" -%}
        picture-entity
        {%- else -%}
          entities
        {%- endif %}
{%- if states[item]|list |length|default(0)|int > 1 %}
        title: {{ plural(item.replace('_', ' ') | title)  }}
{%- else %}
        title: {{ item.replace('_', ' ') | title }}
{%- endif %}
        show_header_toggle: true
        entities:
{%- for e in states[item] %}
          - {{ e.entity_id }}
{%- endfor %}
{%- if item == "device_tracker" %}
        state_filter:
          - 'home'
        card:
          type: glance
          title: My Device Trackers
{% endif %}
{% endfor -%}
```
If you need additional support for other custom cards, please feel free to do a PR or contact @skalavala. 
