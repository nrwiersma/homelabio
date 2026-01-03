![Logo](http://svg.wiersma.co.za/github/project.v2?title=homelabio&tag=rack%20mount%20carrier%20board)

[![GitHub release](https://img.shields.io/github/release/nrwiersma/homelabio.svg)](https://github.com/nrwiersma/homelabio/releases)
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/nrwiersma/homelabio/master/LICENSE)

![board image](assets/assembled.png)

`HomeLabIO` is a Compute Module 4/5 carrier board designed to fit in a 19" rack using 1U of space.

Features:

* Power over Ethernet 802.3at (PoE+)
* TPM 2.0 (SLB9670)
* NVMe SSD up to 2280
* HDMI
* USB-C UART communication on the front
* PWM fan

## Why

This is my first attempt at making a PCB and was largely a practical learning experience.

The schematic is heavily inspired by the [Raspberry Pi CM4IO](https://datasheets.raspberrypi.com/cm4io/cm4io-datasheet.pdf)
and the [HomeAssistant Yellow](https://github.com/NabuCasa/yellow). 

## Config

### Fan

To configure the fan to run based on the core temperature, change the `/boot/firmware/config.txt` as follows:

#### CM4

Add the following under the `[all]` section:
```ini
dtoverlay=i2c-fan,emc2301,i2c_csi_dsi
```

#### CM5

Uncomment the following line:
```ini
dtparam=i2c_arm=on
```

Add the following under the `[all]` section:
```ini
dtoverlay=i2c-fan,emc2301,i2c_csi_dsi0
```

### TPM2

The TPM2 can be enabled by adding the following line to `/boot/firmware/config.txt` under the `[all]` section:
```ini
dtoverlay=tpm-slb9670
```

### GPIO

The following GPIOs are exposed on the board:

![pinout](assets/pinout.svg)

> [!WARNING]
> These SPI GPIOs are shared with other peripherals on the board. Make sure to check for conflicts before using them.

> [!TIP]
> The left hand pins are Raspberry Pi 4 compatible. It is possible to use some Raspberry Pi HATs with this board,
> for instance, the DS3231 Real Time Clock Module.

#### User Programmable Button

The user programmable button is connected to GPIO 20. It can be used for various purposes, such as triggering a shutdown or reset.

For example, to configure the button to trigger a shutdown when pressed, you can use the following configuration in your `/boot/firmware/config.txt`:
```ini
dtoverlay=gpio-shutdown,gpio_pin=20,active_low=1,gpio_pull=off
```

## BOM

See the [Interactive BOM](https://htmlpreview.github.io/?https://github.com/nrwiersma/homelabio/blob/main/bom/ibom.html) [(provided by InteractiveHtmlBom)
](https://github.com/openscopeproject/InteractiveHtmlBom).

## PCB Fabrication and Assembly by PCBWay

I am proudly sponsored by [PCBWay](https://www.pcbway.com/), a leading manufacturer specializing in PCB prototyping, 
low-volume production, and PCB Assembly services. Thanks to PCBWay’s sponsorship, I was able to fabricate and assemble 
my PCBs with high precision and quality. Their advanced manufacturing capabilities ensured that our design was 
translated into a robust and functional product.

![board image](assets/pcb.png)

### Why PCBWay?

PCBWay offers:

* **High-quality PCB fabrication** with multiple layer options, surface finishes, and material choices.
* **Fast turnaround times** to meet project deadlines efficiently.
* **Affordable pricing** for both prototyping and mass production.
* **Reliable PCB assembly services**, including SMT, through-hole, and mixed assembly.
* **Exceptional customer support** and a user-friendly ordering platform.

### Get Your Own PCBs from PCBWay

I highly recommend PCBWay for all your PCB fabrication and assembly needs. Their dedication to quality and customer 
satisfaction makes them an ideal partner for your projects. If you're looking for a trusted partner for PCB fabrication 
and assembly, check out PCBWay for your next project.

Special thanks to PCBWay for their generous support!

## Changelog

#### v0.1.0

* Initial schematic and board

#### v0.1.1

* Fix issue with FTDI USB connection
* Fix issue with BOM that specifies a generic fuse

#### v0.2.0

* Move fan port closer to board edge
* Switch UART chipset
* Add UART activity LEDs
* Switch PCIe power management
* Add NVMe LED
* Switch HDMI port for cheaper alternative

#### v0.3.0

* Add TPM2 support
* Add GPIO breakout header
* Add user programmable button
* Moved some components to allow for better manufacturability
* Various silkscreen improvements
