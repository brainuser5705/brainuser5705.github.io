---
name: small arduino programs
description: simons says, automatic dog feeder
tags: arduino
time: 2024-02
---

These are small Arduino projects to introduce me to basic electrical concepts.

# Simon Says

<iframe src="https://www.youtube.com/embed/M-Pe4tOBU7M?si=T-x4ymdAYy-eqiQi" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

## Features
- "touch sensors" with piezo transducers
- LCD

## Development Notes
- [Circuit diagram (excluding LCD)](https://www.digikey.com/en/schemeit/project/simon-says-51d0543eb37b4b4bafd7744fbdb79bc4)
- Used [`LiquidCrystalIO` library](https://github.com/davetcc/LiquidCrystalIO) by Adafruit
```c
#include <LiquidCrystalIO.h>
#include <IoAbstractionWire.h>
#include <Wire.h>
```

## Resources/Tutorials Used
- [How to use a piezo transducer as a touch sensor](https://docs.arduino.cc/built-in-examples/sensors/Knock/)
- [Basic workings of LCD display with I2C adapter](https://lastminuteengineers.com/i2c-lcd-arduino-tutorial/)

---

# Automatic Dog Feeder

## Prototype 1

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/3-qOSz9pvas?si=J34xJEUi--Gsj9in" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

<div class="pic">
<img src="https://i.imgur.com/I6re0rQ.jpeg">
Clockwise: 1) Funnel design for box, 2) Pockets for trapped kibble leading to jam when closing, 3) Internal circutry and wiring
</div>

## [Github](https://github.com/brainuser5705/automatic-dog-feeder)

## Features
- 3 triggers: Bluetooth, physical button, scheduled (in code)

## Development Notes
- Arduino board: [Nano 33 IOT](https://docs.arduino.cc/hardware/nano-33-iot/#tech-specs)
    - Since I used a board with the header pins setup for breadboard use, using the [pinout diagram](https://docs.arduino.cc/resources/pinouts/ABX00027-full-pinout.pdf) was helpful to see where I needed to put the connections
- Code for the **Bluetooth feature** sampled from the [offical Arduino tutorial](https://docs.arduino.cc/tutorials/nano-33-iot/bluetooth/#bluetooth-low-energy-and-bluetooth)
    - Service is the Arduino with the servo activation boolean (`0` for closed, anything else for open) as the characteristic
- Code for **automatic time activation feature** sampled from [offical Arduino tutorial](https://docs.arduino.cc/tutorials/nano-33-iot/iot-cloud/#goals) about the internal RTC of the board
    - [Thread](https://forum.arduino.cc/t/wanting-to-perform-actions-during-certain-times-of-day-how-does-one-code-this/452668/5?u=codeuser5705) that led me to the idea of using RTC
    - To keep the RTC in sync with real-world time, the code connected via WiFi to an NTP server and updates the RTC - idea taken from this [thread](https://forum.arduino.cc/t/arduino-rtc-does-not-stay-in-sync-despite-use-of-arduinocloud-getlocaltime/1204968/6?u=codeuser5705), [library with examples](https://github.com/arduino-libraries/NTPClient), [tutorial article for Nano 33 IOT](https://nerdhut.de/2021/12/15/arduino-esp32-esp8266-ntp/)
        - It uses the [`RTCZero` library](https://www.arduino.cc/reference/en/libraries/rtczero/)'s `set*` methods
- [Connecting servo motors](https://docs.arduino.cc/learn/electronics/servo-motors/)
    - Orange signal pin must wired to a *PWM* pin
    - Used the `Sweep` example with `for` loops for the open/close motion

---

# Links to Useful Tutorials/Documentation for Arduino-specific projects
- [String addition](https://docs.arduino.cc/built-in-examples/strings/StringAdditionOperator/) - Arduino does not use standard string concatentation
- [Hooking up a button](https://docs.arduino.cc/built-in-examples/digital/Button/)
    - Can get rid of the pull-up (1->0) or pull-down(0->1) resistor with [`INPUT_PULLUP` and `INPUT_PULLDOWN`](https://docs.arduino.cc/tutorials/generic/digital-input-pullup/)
- [Powering up an Arduino board](https://forum.arduino.cc/t/can-i-power-my-arduino-nano-via-usb-port-with-charger-with-5-2-v-and-2-4a-power-adapter/906838)
- [Operating voltage vs input voltage](https://forum.arduino.cc/t/whats-the-difference-between-operating-voltage-and-input-voltage/96227)