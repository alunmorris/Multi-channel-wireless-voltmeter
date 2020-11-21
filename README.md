# Multi-channel-wireless-voltmeter
A multi-channel wireless voltmeter display using ATTINY402, Arduino nano, NRF24L01+ and 128x32 I2C OLED.
The transmitter is optimised for low power and will run from a battery for weeks or months depending on useage, sample frequency and of course battery capacity.
Range is a few meters to 100m depending on how well the radio module is isolated from the controller, and the power setting.
Developed on Auduino

## Notable design features
* Pin saving by multiple use of microcontroller pins:
  * SPI chip-enable, LED drive, negative input test mode and switch A/B detection on one pin
  * MISO and analog input on one pin
  * UPDI programming and switch input on one pin (without needing a high-voltage UPDI programmer)
* Measuring microcontroller Vdd without using an input pin
* Very-low power sleep mode of ATTINY 0 and 1 series
* Very small form factors

## Overview
The display receives voltage data from up to 4 transmitters (one channel each) and shows them on a 128x32 OLED display.

Features:
* 0-20V range, can be configured by resistor selection. 0.01V resolution.
* Accuracy better than 1% (by calibration). May deteriorate - depends on resistor quality.
* Positive voltage only. Over voltage and undervoltage (negative) indication. +/-100V input tolerant.
* 1Mohm input impedance, can be configured by resistor selection
* Sample rate once/s, configurable
* Minimum and maximum display
* Transmitter low battery warning and battery voltage display. Low battery protection.
* Transmitter auto power-off
* Display modes: all channels or single channel with more detais

## Transmitter operation
* On and Off buttons. Off is sleep mode
* Configurable auto power-off period. LED flashes 8 times when powering off.
* Powers off if battery below critical voltage - configurable
* Disable auto power-off by long press of On button. LED flashes 4 times.
* Microntoller sums four voltage readings to reduce noise and to improved resolution of the 10-bit ADC
* LED flashes 20 times if radio IC not responding on power-up

## Receiver operation
* Displays version info, radio channel and lost-signal timeout on power-up/ reset
* Then shows Summary display of all available voltage channels
* Each channel voltage field is blank until data received
* Low battery, critical battery and lost signal indication
* Single channel display shows voltage, Min, Max, low battery, critical battery and lost signal
* Single display also shows Transmitter battery voltage (Vdd) for a few seconds
* Mode button rotates around display modes from Summary to each single channel in rotation

### Summary display (up to 4 channels)
 Row 1: '1 vv.vv*?2 nn.nn*?
 Row 2: '3 vv.vv*?4 nn.nn*?
 where vv.vv = voltage. *=battery alert, ? = timeout AKA lost signal alert. 16 pixel rows
 Over voltage shows as -----, under voltage as -0.00, no data is just blank
![Summary](https://user-images.githubusercontent.com/4630866/99881511-32e78800-2c12-11eb-9359-a8b6a553febb.png) 
 
### Single channel display
 Row 1: N vv.vv *   Min
 Row 2:             mm.mm
 Row 3:         ?   Max
 Row 4: Bat b.bb    mm.mm

 where N=channel, nn.nn = volatage,, mm.mm= min or max voltage, 
 b.bb=sender's Vdd (bat) voltage (appears for a few seconds only to reduce clutter)
 Rows are 8 pixels high in Single view, though N and vv.vv are taller.

A customised font file is used with battery state and signal timeout chars
Tested using Arduino nano and Auduino IDE 1.8.13
