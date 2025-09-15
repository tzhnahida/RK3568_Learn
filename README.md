# Learn Core Board

## Hardware Overview
This Learn Core Board is designed for high-performance embedded applications, featuring dual Ethernet, USB 3.0/2.0 support, HDMI 2.0, and advanced power management.

For detailed schematics and diagrams, refer to the sections below.

## Project Overview
This project is based on the RK3568 processor and aims to explore and practice high-performance embedded hardware design.  
I independently completed the schematic design, PCB layout, and basic bring-up testing of the core board.  

The board is intended for embedded learning and system development, with representative educational and practical value.  


## Project Showcase

## Core Features
1. **PMIC**: RK809-5 + Discrete Power  
2. **RAM**: DDR4 2x16Bit  
3. **ROM**: eMMC  
4. **Storage**: 1 x Micro SD Card  
5. **USB**:  
   - 1 x USB3.0 OTG  
   - 1 x USB3.0 HOST1  
   - 2 x USB2.0 HOST  
6. **Display**: 1 x HDMI 2.0 TX  
7. **Networking**:  
   - 2 x 10/100/1000 Ethernet (RGMI10 & RGMI11)  
   - WiFi 5 (SDIO, 2x2)  
8. **Debug**: 1 x UART  
9. **Indicators**: Power LED, CPU LED, GPU LED, NPU LED  
10. **Controls**: Reset, Power on/off Key  


## Power Sequence Summary

| Power Supply      | PMIC Channel    | Supply Limit | Power Name          | Time Slot | Default Voltage | Default ON/OFF | Work Voltage       | Peak Current | Sleep Current |
|-------------------|-----------------|--------------|---------------------|-----------|-----------------|----------------|--------------------|--------------|---------------|
| VCC3V3_SYS        | RK809_BUCK1     | 2.5A         | VDD_LOGIC           | Slot:1    | 0.9V            | ON             | 0.9V               | TBD          | TBD           |
| VCC3V3_SYS        | RK809_BUCK2     | 2.5A         | VDD_GPU             | Slot:2    | 0.9V            | ON             | DVFS<sup>1</sup>   | TBD          | TBD           |
| VCC3V3_SYS        | RK809_BUCK3     | 1.5A         | VCC_DDR             | Slot:3    | ADJ FB=0.8V<sup>2</sup> | ON | 1.3V (DDR4)        | TBD          | TBD           |
| VCC3V3_SYS        | RK809_BUCK4     | 1.5A         | VDD_NPU             | N/A       | ON              | OFF            | DVFS<sup>1</sup>   | TBD          | TBD           |
| VCC3V3_SYS        | RK809_LDO1      | 0.4A         | VDDA0V9_INAGE       | N/A       | ON              | OFF            | 0.9V               | TBD          | TBD           |
| VCC3V3_SYS        | RK809_LDO2      | 0.4A         | VDDA_0V9            | Slot:1    | 0.9V            | ON             | 0.9V               | TBD          | TBD           |
| VCC3V3_SYS        | RK809_LDO3      | 0.1A         | VDDA0V9_PMU         | Slot:1    | 0.9V            | ON             | 0.9V               | TBD          | TBD           |
| VCC3V3_SYS        | RK809_LDO4      | 0.4A         | VCCIO_ACODEC        | N/A       | ON              | OFF            | 3.3V               | TBD          | TBD           |
| VCC3V3_SYS        | RK809_LDO5      | 0.4A         | VCCIO_SD            | Slot:4    | 3.3V            | ON             | 3.3V, 1.8V<sup>3</sup> | TBD    | TBD           |
| VCC3V3_SYS        | RK809_LDO6      | 0.4A         | VCC3V3_PMU          | Slot:2    | 3.3V            | ON             | 3.3V               | TBD          | TBD           |
| VCC3V3_SYS        | RK809_LDO7      | 0.4A         | VCCA_1V8            | Slot:2    | 1.8V            | ON             | 1.8V               | TBD          | TBD           |
| VCC3V3_SYS        | RK809_LDO8      | 0.4A         | VCCA1V8_PMU         | Slot:2    | 1.8V            | ON             | 1.8V               | TBD          | TBD           |
| VCC3V3_SYS        | RK809_LDO9      | 0.4A         | VCCA1V8_INAGE       | N/A       | ON              | OFF            | 1.8V               | TBD          | TBD           |
| VCC3V3_SYS        | RK809_SW1       | 2.1A         | VCC3V3_SD           | Slot:4    | 3.3V            | ON             | 3.3V               | TBD          | TBD           |
| VCC3V3_SYS        | RK809_SW2       | 2.1A         | VCC_3V3             | Slot:4    | 3.3V            | ON             | 3.3V               | TBD          | TBD           |
| VCC3V3_SYS        | RK809_BUCK5     | 2.5A         | VCC_1V8             | Slot:2    | 1.8V            | ON             | 1.8V               | TBD          | TBD           |
| VIN_5V            | EXT BUCK        | 3.0A         | VCC3V3_SYS          | Slot:0    | 3.3V            | ON             | 3.3V               | TBD          | TBD           |
| VCC3V3_SYS        | EXT LDO         | 0.3A         | VCC2VS_DDR          | Slot:2A   | 2.5V            | ON             | 2.5V               | TBD          | TBD           |

### Notes:
1. **DVFS**: Dynamic Voltage and Frequency Scaling (Voltage adjusted based on workload).  
2. **ADJ FB=0.8V**: Adjustable voltage with a base reference of 0.8V.  
3. **1.8V**: Voltage may switch between 3.3V and 1.8V depending on SD card mode.  
4. **TBD**: To be determined (Values will be updated in future revisions).  

### Board (Front View)
![Board Front](Images\board_front.jpg)

### Testing in Progress
![Board Test](Images\board_test.jpg)

## Known Issue
There is a schematic design error in the ETH0/ETH1 power supply section:  
- The IO supply pins, which should have been powered at **1.0V**, were mistakenly connected to **3.3V**.  
- This caused the Ethernet interfaces to fail to function properly.  
  

## Revision History
| Version | Date       | Change Description                                   | Approved By |
| ------- | ---------- | --------------------------------------------------- | ----------- |
| V1.0    | 2024-08-06 | Preliminary version                                  |             |
| V1.1    | 2024-09-08 | - Modified ETH0/ETH1 matching resistors<br>- Updated ETH0/ETH1 net naming |             |

---  
Feel free to contribute or report issues via the repository.  