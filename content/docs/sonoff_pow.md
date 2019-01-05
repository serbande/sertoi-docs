+++
title = "Sonoff POW"
draft = false
toc = true
weight = 10
description = "Install tasmota in Sonoff POW v1"
bref = "Learn how to flash your Sonoff POW v1 device with a different firmware (Tasmota, ESPurna, ESPeasy ...) and explore the different functionalities"
+++

### Preparing hardware


- We need to access VCC RT TX and GND pins, to facilitate it we weld pins to the sonoff pcb
- To connect the sonoff to the PC we're using a USB UART TTL 3.3V Converter/Programmer (e.g. CP2102, CH340G, FT232, PL2303) 
- The schema of the connections is:
  
| Programmer| Sonoff 	|
|:--|:--|
| 3.3/VCC 	| 3.3/VCC	|
| TX 		| RX 		|
| RX		| TX		|
| GND 		| GND   	|

![Sonoff POW programming diagram](/sonoff-pow-programming-diagram.png)

<div class="message warning" data-component="message">
    <h5>WARNING!</h5>
    Do not connect AC and Serial at the same time, your computer could be damaged.
</div>

### Flashing with a new firmware

1. Install  python and package manager pip
2. Using pip install the package esptool
```
pip install --upgrade esptool
```
3. Erase the current firmware
```
python esptool.py --port COM8 --baud 115200 erase_flash
```
4. Load the new framework   
```
python esptool.py --port COM8 --baud 115200 write_flash -fs detect -fm dout 0x00000 sonoff.bin
```

TODO
Indicar como encontrar esptool.py
Decir como encontrar el COM8

### Backup and restore
After the first load of the firmware you must setup different paremeters like wifi credentials, active functionalities like mqtt... If for some reason your current installation crashes you have to repeat all the steps again. In order to evict that you can create a backup of your firmware and use it when you need to setup your sonoff again. Below are the commands to do it:

1. Create a backup image of the firmware
```
esptool.py --port COM19 --baud 115200 read_flash 0x00000 0x100000 sonoff-backup.bin
```
2. Restore the firmware from a backup image
```
python esptool.py --port COM19 --baud 115200 write_flash -fs 1MB -fm dio 0x00000 sonoff-backup.bin
```

