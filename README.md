# HomeAssistant-Tipps

## Guest-Button - Device Tracker with a Toggle Button
You want to "track" a guest e.g. for a guest room, who cannot be tracked manual.
A solution can be to create a Helper Toggle Button. In the example the button will be called "Guest"
Then you create the following automation:
```yaml
alias: Guest as Button
description: ""
trigger:
  - platform: state
    entity_id:
      - input_boolean.guest
condition: []
action:
  - choose:
      - conditions:
          - condition: template
            value_template: "{{ states('input_boolean.guest') == 'on' }}"
        sequence:
          - service: device_tracker.see
            data:
              location_name: home
              dev_id: guest
    default:
      - service: device_tracker.see
        data:
          dev_id: guest
          location_name: not_home
mode: single
```
After you Toggle the Button the first time a device_tracker.guest will apear which you can use e.g. for person (with the name Guest)

[And no, @frenk is wrong, when he think you can add a input_boolean.guest button directly to track a person...
Would be a better solution if ha would allow it, but ...](https://github.com/home-assistant/home-assistant.io/pull/25152#discussion_r1045723818)
