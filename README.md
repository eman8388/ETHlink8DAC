![snapshot](https://github.com/emna8388/ETHlink8DAC/blob/main/snapshot_PCB/3D_ETHlink8xDAC_PCB.png)
# ETHlink8DAC

An open source and open hardware 8 output ethernet audio interface, remote controlled, interface of the ETHlink audio interface family.

## Main features

* Up to 32 device on local network depending on network quality
* Very very very low latency: 32 sample per buffer 
* Indentification on network based on unique serial number and MAC address (each devivce build should have a proper serial number and MAC Address) 
* NO CONTROL design, NO display, NOTHING to set up, only a dual color status led! 
* Gain of each channel, mute and DAC taste controlled via software  
* Based on STM32H750 powerfull single core Arm m7 MCU and Ethernet PHY lan8742 100BaseTX mode
* PCM512x DAC:
    * 32 bit 44100, 48000, 88100, 96000 hz sample rate 
    * stereo DAC full integrated output circuit for reduced pcb real-estate and improved precision 
    * dual selectable architecture for 2 different audio taste (old or modern)
    * pseudo balanced output
* Precision clock generation with Si5351 arbitrary clock synthesizer
* PoE powered or 5V-15V DC jack (<1A)
* Improved energy efficiency, Rail-to-Rail power supply, dual 3V3 LDO for analog and digital power rail improve efficiency and noise 
* Everithing in a dual layer 120x80 mm PCB 

## Hardware

The pcb is cost effective optimized using simplified architecture improving easy manufacturing. 

 Schematic: 
 https://github.com/emna8388/ETHlink8DAC/tree/main/PCB_schematic

 BOM:
 https://github.com/emna8388/ETHlink8DAC/tree/main/PCB_BOM

 Gerber file
 https://github.com/emna8388/ETHlink8DAC/tree/main/PCB_fabrication_file%20

 Issues: 
* Depending on PCB , the TDM signals (BCK, FS, DATA_OUT) may require impedence compensation capacitor 

## Example application device 

 https://github.com/emna8388/ETHlink8DAC/tree/main/snapshot_device_example

## Firmware 

 Developed with STM32IDE in C based on FreeRTOS. 

 Project is ready to build, MAC address should be assigned like this:
 00:E8:DA:C0:00:00 serial number 0 .
 00:E8:DA:C0:00:01 serial number 1 .
 the last 2 byte of the MAC address rapresent the serial number of the device (16bit value).

### Functioning description
 The comunication between the device and computer is based on Raw ethernet frame with custom protocol (0x0802), so the identifier of the device in local network is ONLY the MAC address






E.M.
