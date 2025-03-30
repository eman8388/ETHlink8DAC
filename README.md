![snapshot](https://github.com/emna8388/ETHlink8DAC/blob/main/snapshot_PCB/3D_ETHlink8xDAC_PCB.png)
# ETHlink8DAC

An open source and open hardware 8 output ethernet audio interface.

## Main features

* Up to 32 device on local network depending on network quality
* Indentification on network based on unique serial number and MAC address (each devivce build should have a proper serial number and MAC Address) 
* NO CONTROL design, NO display, NOTHING to set up, only a dual color status led! 
* Gain of each channel, mute and DAC taste controlled via software  
* Based on STM32H750 powerfull single core Arm m7 MCU and Ethernet PHY lan8742 100BaseTX mode
* PCM512x DAC:
    * 32 bit 44100, 48000, 88100, 96000 hz sample rate 
    * stereo DAC full integrated output circuit for reduced pcb realestate and improved precision 
    * dual selectable architecture for 2 different audio taste (old or modern)
    * pseudo balanced output
* Precision clock generation with Si5351 arbitrary clock synthesizer
* PoE powered or 5V-15V DC jack (<1A)
* Improved energy efficiency, Rail-to-Rail power supply, dual 3V3 LDO for analog and digital power rail improve efficiency and noise 
* Everithing in a dual layer 120x80 mm PCB 

User manual:


Schematic: 


BOM:


Issues: 
* Depending on PCB , the TDM signal (BCK, FS, DATA_OUT) may require impedence compensation capacitor (C0G) 


E.M.
