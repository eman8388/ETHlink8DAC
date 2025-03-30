[snapshot](https://github.com/emna8388/ETHlink8DAC/blob/main/snapshot_PCB/3D_ETHlink8xDAC_PCB.png)

# ETHlink8DAC

ETHlink8DAC is an open-source, open-hardware 8-output Ethernet audio interface, remotely controlled, and part of the ETHlink audio interface family.

## Main Features

- **No Driver Required**: The ETH8DACV2 VST3 plugin enables direct audio streaming from your DAW. Project: https://github.com/eman8388/ETH8DACV2 .
- **Scalability**: Supports up to 32 devices on a local network, depending on network quality.
- **Ultra-Low Latency**: Only 32 samples per buffer.
- **Unique Identification**: Each device is identified on the network via a unique serial number and MAC address.
- **Minimalist Design**: No controls, no display—just a dual-color status LED!
- **Software-Controlled Gain & Mute**
- **Powerful Hardware**:
  - STM32H743 high-performance single-core Arm Cortex-M7 MCU
  - Ethernet PHY LAN8742 in 100-BaseTX mode
  - PCM512x DAC featuring:
    - 32-bit resolution with support for 44.1kHz, 48kHz, 88.2kHz, and 96kHz sample rates
    - Fully integrated stereo DAC output for reduced PCB footprint and enhanced precision
    - Pseudo-balanced output
  - High-precision clock generation using the Si5351 arbitrary clock synthesizer
- **Flexible Power Options**: PoE-powered or 5V-15V DC jack (<1A).
- **Optimized for Efficiency**: Rail-to-rail power supply, dual 3.3V LDO regulators for analog and digital rails, improving efficiency and reducing noise.
- **Compact Design**: Everything fits on a dual-layer 120x80 mm PCB.

## Hardware

The PCB is cost-optimized with a simplified architecture to improve manufacturability.

- **Schematic:**\
  [View Schematic](https://github.com/emna8388/ETHlink8DAC/tree/main/PCB_schematic)

- **EasyEDA Project:**\
  [View Project](https://github.com/emna8388/ETHlink8DAC/tree/main/PCB_EasyEDAPro_PROJECT)

- **Bill of Materials (BOM):**\
  [View BOM](https://github.com/emna8388/ETHlink8DAC/tree/main/PCB_BOM)

- **Gerber Files:**\
  [Download Gerber Files](https://github.com/emna8388/ETHlink8DAC/tree/main/PCB_fabrication_file%20)

### Known Hardware Issues

- Depending on the PCB layout, TDM signals (BCK, FS, DATA\_OUT) may require impedance compensation. In my case, I had to add a 2.2nF capacitor (C238) on the FS line.
- The resistor values for the status LED (R148, R149) may need to be adjusted based on the LED type.

### Example Application Device

[View Example](https://github.com/emna8388/ETHlink8DAC/tree/main/snapshot_device_example)

## Firmware

Developed in **C** using **STM32IDE**, based on **FreeRTOS**.

- **Supported MCUs:** STM32H743VIT6 or the cost-effective STM32H750VBT6 (128KB ROM, requires project regeneration).
- **MAC Address Assignment:**
  - `00:E8:DA:C0:00:00` → Serial number 0
  - `00:E8:DA:C0:00:01` → Serial number 1
  - The last two bytes of the MAC address represent the device's serial number (16-bit value).

### Communication Protocol

ETHlink8DAC communicates with the computer using **Raw Ethernet Frames (Layer-2)** with a custom protocol (`0x0802`). The only network identifier is the **MAC address**.

- The device receives a packet containing:
  - **64-byte header** (containing commands)
  - **Audio data** (TDM format, 32-bit PCM, 8 channels, 8 samples per channel)
- The device:
  1. Processes the command.
  2. Sends audio data to a dynamic queue for playback via the TDM interface.
  3. Sends a response packet once per second containing only the 64-byte header to the sender.
  4. If no packet is received within one second, communication stops.

**Packet Structure:**

```
[ETHERNET LAYER 2 HEADER (14 BYTES)] - [APPLICATION HEADER (50 BYTES)] - [AUDIO DATA (8 CHANNELS × 8 SAMPLES × 4 BYTES PER SAMPLE)]
```


For details, refer to **main.c** and **ethernetif.c**.

### Known Firmware Issues

- Audio samples must be transmitted every **8/FS**, which is **<<1ms**. To accommodate this, the **FreeRTOS scheduler must be set to at least 10,000 ticks per second**, which might cause system instability. However, in this specific application, it functions reliably.

## Flashing Instructions

1. **Download and extract** `ETH8DAC_STM32IDE_FILES.zip`.
2. **Open STM32IDE** and create a new project using `ETH8DAC.ioc` configuration.
3. **Replace project files** with those from the `ETH8DAC_STM32IDE_FILES` folder.
4. **Modify MAC Address**:
   - Open `ethernetif.c` (under `LWIP->TARGET`).
   - Locate `static void low_level_init(struct netif *netif);` (line 219).
   - Update the `MACAddr[]` array with your desired address.
5. **Optimize the build**:
   - Right-click on the project → Properties → `C/C++ Build` → Settings → `MCU/MPU GCC Compiler` → Optimization → Set to `-Ofast`.
6. **Connect ST-Link V2**:
   - Pin header at the **top-left of the board**.
   - Connect **GND, SWDIO, SWCLK** (Pins 4, 1, 2).
   - Build and flash the project.

For testing, a **precompiled ELF binary** is available in the `TEST` folder with serial numebr 0, ready to be flashed with **STM32CubeProgrammer**.

---

Eugenio Mancini\
 [mancini97email@gmail.com](mailto\:mancini97email@gmail.com)


