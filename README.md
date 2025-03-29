![snapshot](https://github.com/emna8388/ETHlink8DAC/blob/main/3D_PCB1_2025-03-27.png)
# ETHlink8DAC

An open source and open hardware 8 output ethernet audio interface.

## Main features

* Up to 32 device on local network depending on network quality and computer quality
* Indentification on network based on unique serial number and mac address (each devivce buil should have a proper serial number and MAC Address) 
* NO CONTROL design NO display NOTHING to set up, only a dual color status led 
* gain of each channel, mute and DAC taste controlled via software  
* Based on STM32H750 powerfull single core Arm m7 MCU and Ethernet PHY lan8742 100BaseTX mode 
* PCM512x DAC:
    * 32 bit 44100, 48000, 88100, 96000 hz sample rate 
    * stereo DAC full integrated output circuit for reduced pcb realestate and improved precision 
    * dual selectable architecture for 2 different audio taste (old or modern)
    * pseudo balanced output 
* Precision clock generation with Si5351 arbitrary clock synthesizer 
* POE powered or 12V DC jack (1A)
* Everithing in a 120x80 mm pcb 



E.M.
