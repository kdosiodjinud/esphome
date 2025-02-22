# Good Display GDEY029T94 2.9" 296x128 ePaper
## Example
In this code is prepared deep_sleep mode. If you copy / paste code, display will be updated only when displej boot up (ideal for development)

```yaml
esphome:
  name: display-example
  comment: "Display - example"
  on_boot:
    priority: 600
    then:
      - output.turn_on: eink_power
      - component.update: eink_display
      - delay: 3s
      - output.turn_off: eink_power
      - if:
          condition:
            not:
              - api.connected: 
          then: 
            - deep_sleep.enter:
                sleep_duration: 30s     

deep_sleep:
  run_duration: 10s
  id: deep_sleep_component

esp32:
  board: esp-wrover-kit

external_components:
  - source:
      type: git
      url: https://github.com/kdosiodjinud/esphome/
      ref: main
    components: [ waveshare_epaper ]

logger:

api:
  encryption:
    key: "your-key"

ota:
  platform: esphome
  password: "your-password"

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

output:
  - platform: gpio
    pin: 2
    id: eink_power

font:
  - file: 'Comic Sans MS.ttf'
    id: font1
    size: 20
  - file: 'Bookerly Display.ttf'
    id: font2
    size: 20


spi:
  clk_pin: 18
  mosi_pin: 23

display:
  - platform: waveshare_epaper
    id: eink_display
    cs_pin: 5
    dc_pin: 17
    reset_pin: 16
    model: gdey029t94
    rotation: 90
    update_interval: 30days
    lambda: |-
      it.print(150, 10, id(font1), TextAlign::TOP_RIGHT, "Hello!");
```
