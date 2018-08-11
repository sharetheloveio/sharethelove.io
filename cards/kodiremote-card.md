---
description: >-
  A simple remote that will hold up under any resolution
---

# Kodi Remote Card

![kodi remote card](../.gitbook/assets/kodiremote.png)


```yaml
- type: picture-elements
  image: /local/banners/empty_placeholder.png
  elements:
    - type: image
      image: /local/icons/animated/media-card/up-arrow.png
      tap_action: call-service
      service: shell_command.kodi_up
      style:
        top: 5em
        left: 10em
        width: 54px
        height: 54px
        background-color: rgb(255, 255, 255, 0.8)
        filter: invert(0.20)
        border-radius: 4px
        z-index: 2
    - type: image
      image: /local/icons/animated/media-card/home-button.png
      tap_action: call-service
      service: shell_command.kodi_home
      style:
        top: 5em
        left: 6em
        width: 24px
        height: 24px
        padding: 15px
        background-color: rgb(200, 200, 200, 0.7)
        filter: invert(0.20)
        border-radius: 4px
        z-index: 2
    - type: image
      image: /local/icons/animated/media-card/update-button.png
      tap_action: call-service
      service: shell_command.kodi_update
      style:
        top: 5em
        left: 14em
        width: 24px
        height: 24px
        padding: 15px
        background-color: rgb(200, 200, 200, 0.7)
        filter: invert(0.20)
        border-radius: 4px
        z-index: 2
    - type: image
      image: /local/icons/animated/media-card/poweroff-button.png
      tap_action: call-service
      service: shell_command.kodi_power
      style:
        top: 5em
        left: 18em
        width: 24px
        height: 24px
        padding: 15px
        background-color: rgb(255, 255, 255, 0.8)
        filter: invert(0.20)
        border-radius: 4px
        z-index: 2
    - type: image
      image: /local/icons/animated/media-card/left-arrow.png
      tap_action: call-service
      service: shell_command.kodi_left
      style:
        top: 9em
        left: 6em
        width: 54px
        height: 54px
        background-color: rgb(255, 255, 255, 0.8)
        filter: invert(0.20)
        border-radius: 4px
        z-index: 2
    - type: image
      image: /local/icons/animated/media-card/kodi-icon.png
      entity: media_player.kodi
      style:
        top: 9em
        left: 10em
        width: 56px
        height: 56px
        z-index: 2
    - type: image
      image: /local/icons/animated/media-card/right-arrow.png
      tap_action: call-service
      service: shell_command.kodi_right
      style:
        top: 9em
        left: 14em
        width: 54px
        height: 54px
        background-color: rgb(255, 255, 255, 0.8)
        filter: invert(0.20)
        border-radius: 4px
        z-index: 2
    - type: image
      image: /local/icons/animated/media-card/reboot-button.png
      tap_action: call-service
      service: shell_command.kodi_restart
      style:
        top: 9em
        left: 18em
        width: 24px
        height: 24px
        padding: 15px
        background-color: rgb(200, 200, 200, 0.7)
        filter: invert(0.20)
        border-radius: 4px
        z-index: 2
    - type: image
      image: /local/icons/animated/media-card/down-arrow.png
      tap_action: call-service
      service: shell_command.kodi_down
      style:
        top: 13em
        left: 10em
        width: 54px
        height: 54px
        background-color: rgb(255, 255, 255, 0.8)
        filter: invert(0.20)
        border-radius: 4px
        z-index: 2
    - type: image
      image: /local/icons/animated/media-card/clean-button.png
      tap_action: call-service
      service: shell_command.kodi_clean
      style:
        top: 13em
        left: 18em
        width: 24px
        height: 24px
        padding: 15px
        background-color: rgb(255, 255, 255, 0.8)
        filter: invert(0.20)
        border-radius: 4px
        z-index: 2
    - type: image
      image: /local/icons/animated/media-card/back-button.png
      tap_action: call-service
      service: shell_command.kodi_back
      style:
        top: 13em
        left: 6em
        width: 24px
        height: 24px
        padding: 15px
        background-color: rgb(200, 200, 200, 0.7)
        filter: invert(0.20)
        border-radius: 4px
        z-index: 2
    - type: image
      image: /local/icons/animated/media-card/enter-button.png
      tap_action: call-service
      service: shell_command.kodi_enter
      style:
        top: 13em
        left: 14em
        width: 24px
        height: 24px
        padding: 15px
        background-color: rgb(200, 200, 200, 0.7)
        filter: invert(0.20)
        border-radius: 4px
        z-index: 2
```

And then some shell_command to send commands to kodi

```yaml
kodi_clean: "kodi_remote --clean"
kodi_update: "kodi_remote --update"
kodi_up: "kodi_remote --up"
kodi_down: "kodi_remote --down"
kodi_left: "kodi_remote --left"
kodi_right: "kodi_remote --right"
kodi_enter: "kodi_remote --select"
kodi_home: "kodi_remote --home"
kodi_power: "kodi_remote --poweroff"
kodi_back: "kodi_remote --back"
```
and last but not least the little shell script that handles the calls obviously swap the ip address and port in the shell script for your ip and port

```bash
#!/bin/bash
case $1 in
--back ) curl -sS --data-binary '{ "jsonrpc": "2.0", "method": "Input.Back", "id": "mybash"}' -H 'content-type: application/json;' http://127.0.0.1:80/jsonrpc ;;
--down ) curl -sS --data-binary '{ "jsonrpc": "2.0", "method": "Input.Down", "id": "mybash"}' -H 'content-type: application/json;' http://127.0.0.1:80/jsonrpc ;;
--up ) curl -sS --data-binary '{ "jsonrpc": "2.0", "method": "Input.Up", "id": "mybash"}' -H 'content-type: application/json;' http://127.0.0.1:80/jsonrpc ;;
--left ) curl -sS --data-binary '{ "jsonrpc": "2.0", "method": "Input.Left", "id": "mybash"}' -H 'content-type: application/json;' http://127.0.0.1:80/jsonrpc ;;
--right ) curl -sS --data-binary '{ "jsonrpc": "2.0", "method": "Input.Right", "id": "mybash"}' -H 'content-type: application/json;' http://127.0.0.1:80/jsonrpc ;;
--home ) curl -sS --data-binary '{ "jsonrpc": "2.0", "method": "Input.Home", "id": "mybash"}' -H 'content-type: application/json;' http://127.0.0.1:80/jsonrpc ;;
--select ) curl -sS --data-binary '{ "jsonrpc": "2.0", "method": "Input.Select", "id": "mybash"}' -H 'content-type: application/json;' http://127.0.0.1:80/jsonrpc ;;
--update ) curl -sS --data-binary '{ "jsonrpc": "2.0", "method": "VideoLibrary.Scan", "id": "mybash"}' -H 'content-type: application/json;' http://127.0.0.1:80/jsonrpc ;;
--clean ) curl -sS --data-binary '{ "jsonrpc": "2.0", "method": "VideoLibrary.Clean", "id": "mybash"}' -H 'content-type: application/json;' http://127.0.0.1:80/jsonrpc ;;
--poweroff ) curl -sS --data-binary '{ "jsonrpc": "2.0", "method": "System.Shutdown", "id": "mybash"}' -H 'content-type: application/json;' http://127.0.0.1:80/jsonrpc ;;
--restart ) curl -sS --data-binary '{ "jsonrpc": "2.0", "method": "System.OnRestart", "id": "mybash"}' -H 'content-type: application/json;' http://127.0.0.1:80/jsonrpc ;;
esac
```
