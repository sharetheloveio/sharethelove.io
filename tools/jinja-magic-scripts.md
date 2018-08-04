---
description: >-
  A bunch of Jinja2 migration scripts to help you create the lovelace configuration for you automagically.
---

{% hint style="info" %}
This page contains a bunch of scripts that you need to run in the template editor, and copy and paste the output into your `ui-lovelace.yaml` file. Copying the jinja code/script directly into the `ui-lovelace.yaml` file will give you severe headaches and possibly long-term Migraines. 
{% endhint %}

# Jinja2 Script Helpers

Use the following script to automatically generate the entity cards based on your current setup.
```yaml
{% set domains = states | map(attribute='domain') |list | unique | list %}
{%- for item in domains -%}
- type: entities
  title: {{ item.replace('_', ' ') | title }}
  show_header_toggle: true
  entities:
{%- for e in states[item] %}
    - {{ e.entity_id }}
      name: {{ e.name }}
{%- endfor %}

{% endfor -%}
```
