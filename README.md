![snapshot](https://github.com/emna8388/ETHlink8DAC/blob/main/snapshot_PCB/3D_ETHlink8xDAC_PCB.png)
# ETHlink8DAC

An open source and open hardware 8 output ethernet audio interface, remote controlled, interface of the ETHlink audio interface family.


## Main features
* computer side application ETH8DACV2 vst3 for sending audio directly from daw, NO DRIVER REQUIRED. project:  
* Up to 32 device on local network depending on network quality
* Very very very low latency: 32 sample per buffer 
* Indentification on network based on unique serial number and MAC address (each devivce build should have a proper serial number and MAC Address) 
* NO CONTROL design, NO display, NOTHING to set up, only a dual color status led! 
* Gain of each channel and mute are controlled via software  
* Based on STM32H743 powerfull single core Arm m7 MCU and Ethernet PHY lan8742 100-BaseTX mode
* PCM512x DAC:
    * 32 bit 44100, 48000, 88100, 96000 hz sample rate 
    * stereo DAC full integrated output circuit for reduced pcb real-estate and improved precision 
    * pseudo balanced output
* Precision clock generation with Si5351 arbitrary clock synthesizer
* PoE powered or 5V-15V DC jack (<1A)
* Improved energy efficiency, Rail-to-Rail power supply, dual 3V3 LDO for analog and digital power rail improve efficiency and noise 
* Everithing in a dual layer 120x80 mm PCB 

## Hardware

The pcb is cost effective optimized using simplified architecture improving manufacturing. 

 Schematic: 
 https://github.com/emna8388/ETHlink8DAC/tree/main/PCB_schematic

 BOM:
 https://github.com/emna8388/ETHlink8DAC/tree/main/PCB_BOM

 Gerber file
 https://github.com/emna8388/ETHlink8DAC/tree/main/PCB_fabrication_file%20

 Issues: 
* Depending on PCB , the TDM signals (BCK, FS, DATA_OUT) may require impedence compensation capacitor in my case i had to had C238 (2.2nF cap) on FS line.
* Depending on the status led type, resistor R148, R149 have to be reduced. 

### Example application device 

 https://github.com/emna8388/ETHlink8DAC/tree/main/snapshot_device_example

## Firmware 

 Developed with STM32IDE in C based on FreeRTOS.
 Can be used STM32H743VIT6 or the cheper alternative (with 128kbyte rom) STM32H750VBT6 regenerating the project for it.

 Project is ready to build, MAC address should be assigned like this:
 00:E8:DA:C0:00:00 serial number 0 
 00:E8:DA:C0:00:01 serial number 1
 the last 2 byte of the MAC address rapresent the serial number of the device (16bit value).

### Function description
 The comunication between the device and computer is based on Raw Ethernet frame (LAYER-2) with custom protocol (0x0802), so the identifier of the device in local network is ONLY the MAC address.
 The device receive a packet conteining 64byte header (wich contein the command) and audio data (TDM fomrat 32bit audio 8 channel int32_t format PCM , 8 sample each channe) so the device check and apply the command, send the audio data to a dynamic queue to be played by TDM interface, and send back each second a packet conteining only the heder (first 64byte of the received packet) to the device wich send the first packet, if no packet is received in 1 second the comunication stop.
 The packets received is composed in this way: 
 
 ETHERNET LAYER 2 HEDER (14BYTE) - APPLICATION HEDER (50 BYTE) - AUDIO DATA (8 CHANNEL * 8 SAMPLE * 4 BYTE PER SAMPLE (32BIT) MULTIPLEXED LIKE TDM (so frame 1 (8 channel), frame 2 ...))
 
 For detail, see main.c and ethernitif.c . 

 Issues:
 * 8 sample should be send every 8/FS wich is a very short time << 1ms, the FreeRTOS scheduler should be set at least to 10000 Tic per second, this may cause instability. In tihis specific application seems to work fine anyway.   

### Instruction for flashing 
 Download the Folder ETH8DAC_STM32DE_FILES.
 Open STM32IDE and create a new project based on ETH8DAC.ioc configuration file, replace the file in your project with the files that you can find in the ETH8DAC_STM32DE_FILES folder.
 For modifying the MAC address go to ethernetif.c (LWIP->TARGET), function static void low_level_init(struct netif *netif); (line 219) and modify the MACAddr[] array;
 Set Optimization for the project to -Ofast (right cluck on the project->properties->C/C++ Build->Settings->MCU/MPU GCC Compiler->Optimization).
 Connect STlinkV2 to the pin Heder at Top left of the board via GND SWDIO SWCLK (pin 4 1 2) build and flash the project.
 
 For testing purpose, ther's the elf binary in the folder TEST redy to flash with STM32CubeProgrammer.    

Eugenio Mancini, mancini97email@gmail.com 
