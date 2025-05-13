# Dormo

The Dormo is a tiny IoT device with extremely long battery life and snappy response time, making it ideal for a multi-button smart home remote.
This project started as a fork of [Picoclick-C3](https://github.com/makermoekoe/Picoclick-C3) which focuses on overall size and offers limited expandability.

## Current State

The short term goal is to have a ready-to order PCB at JLCPCB with the respective parts for assembly.
After inital PCB design a housing for the remote will follow. If you have suggestions simply open a feature request issue.

- [x] Update schematic in KiCad
- [ ] Draw PCB
- [ ] Validate prototype PCB
- [ ] Design 3D-printable housing
  



## GPIOs

Function | GPIO C3T | GPIO C3 | Mode
-------- | -------- | -------- | --------
LED | GPIO6 | GPIO6 (CLK) + GPIO7 (SDI) | Output
Latch* | GPIO3 | ** | Output
Button | GPIO5 | GPIO5 | Input
Charge Stat. | GPIO1  | ext. LED | Input
Bat Voltage | GPIO4 | GPIO4 | Input
Bat Voltage EN | -- | GPIO3 | Output

*Enabling the LDO can be done by pressing the button of the device or turning the latch high. In most use cases, the latch GPIO should be turned on as the first task of the Picoclick. Once the task is completed the device can be powered off by turning the latch off (e.g. pulling it low).

**The Picoclick C3 doesn't need a latching GPIO because it uses the embedded flash's power supply pin as a reference. It can be depowered with putting the ESP32 in deepsleep. To reduce the power consumption of the ESP32 this function will disable the power of the embedded flash (`VDD_SPI`) which in result will depower the Picoclick itself. The deepsleep calling function is only necessary to pulling the `VDD_SPI` line low, not to use the deepsleep mode of the ESP32.

## Reducing power consumption

In order to reduce the power consumption of the Picoclick the following points can be done:

- Disable WiFi while not in use (e.g. if using LED animations only): `WiFi.mode(WIFI_OFF);`
- Reducing CPU frequency after WiFi stuff is completed (crystal is 40MHz): `setCpuFrequencyMhz(10); //reduce to 10MHz`
- Reducing brightness of the LEDs (can be done right after FastLED init): `FastLED.setBrightness(150);`



## Flashing firmware to the ESP32 (C3T)

**- Press and hold the button during the complete flashing process! Otherwise the ESP32 will be loose power and the upload process will crash!**

**- A battery or a power supply has to be applied to the battery pads (3.5v - 5.5v) in order to flash the device!**

Except the above, the Picoclick behaves like a normal development board. No need to get the ESP32 into download mode or pressing any reset button.

## Speed up boot process

Things makermoekoe has done so far:
- Set *Flash SPI mode* from *DIO* to *QIO* (in Serial flasher config)
- Set *Log output* from *Info* to *Warning* (in component config)
- Set *Bootloader log verbosity* from *Info* to *Warning* (in Bootloader config)
- Enable *Skip image validation from power on* (in Bootloader config)

These points result in a boot up time of around 68ms which is almost quite fantastic. The test I've done so far were quite sufficient. If it is possible to make it even faster or if you have other ideas which could lead into the right direction then please let me know!


## Battery for the Picoclick

** TODO **

As the Picoclick C3 comes with an embedded battery protection, the protection of the LiPo battery itself is redundant. Therefore you can use these tiny cells which are used by BT headsets. They come in a 401010 or 301010 package.


## Cases for the C3T

** TODO **

# FAQ

[Main documentation of the Picoclick-C3 can be found here!](https://makermoekoe.gitbook.io/picoclick-c3/)


