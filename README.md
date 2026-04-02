# USB Audio Interface (DAC) w/ NUCLEO-H7S3L8 & PCM-5102A

## Disclaimer
This project was completed as a thesis assignment for my degree, as a first complete project on a MCU and initially uploaded in the form it was presented in. Any next changes to be commited were done post turning it in. 

## Key features
- USB 1.0 Audio device
- Powered from USB
- Supporting 16 bit audio @ 48 kHz
- I2S protocol used for transferring audio to WMCU-5102 board containing PCM-5102A
- No synchronization endpoint, with implicit feedback

## Build instructions in CubeIDE
1. After importing, build both projects (Boot and Appli).
2. Then, for Boot: ```Run -> Run/Debug Configurations -> STM32 C/C++ Application```
3. There, in the  ```Debugger``` tab choose the external loader suitable for the XIP (Execute In Place) model: ```MX25UW25645G_NUCLEO-H7S3L8.stldr```
4. Follow steps 2 and 3 for the Appli project as well, and then, in the ```Startup``` tab, add the ```.elf``` file corresponding to the Boot project, so that executing the Appli project also launches Boot.
5. Finally, choose ```Move up``` after having selected the Appli project ```.elf``` file.

The above were based on the instructions given in the STM32CubeH7RS repository project [CDC_Standalone](https://github.com/STMicroelectronics/STM32CubeH7RS/tree/main/Projects/NUCLEO-H7S3L8/Applications/USB_Device/CDC_Standalone).

## Connecting board to DAC
Connections per pin on WMCU-5102:
- VCC: left floating, since 5V not used
- VDD: connect to 3V3_IO (CN8)
- GND: connect to GND (CN8)
- FLT: connect to GND
- DEMP: connect to GND
- SCK: connect to GND, since TI allows usage without providing a master clock
- BCK: connect to PE5 (CN9)
- DIN: connect to PC1 (CN9)
- LRCK: connect to PE4 (CN9)
- FMT: connect to GND
- XSMT: connect to 3V3_IO (CN8)

For more info on the pins of this board, and other common I2S DAC, [this page](https://github.com/pschatzmann/arduino-audio-tools/wiki/External-DAC) is quite useful.

## Software used
- STM32CubeMX V6.15.0 using STM32Cube FW_H7RS V1.2.0
- STM32CubeIDE V1.18.0

## Summary
A less performant board could have been chosen for this specific project. However, it was still used to get to grips with the then newer GPDMA controller, as well as the XSPI interface for using external memory on STM32 MCU.

This project kept most of CubeMX's generated code as is, to only work on top of it. It is clear that for any future additions, some of it is less than convenient (e.g. endpoint descriptor has to change for multiple bitrates).

Overall, this project was an introduction to producing an application on a MCU using STM32 HAL that also included familiarization with the USB protocol, I2S, using the SAI on STM32, using GPDMA and XSPI. Furthermore, it served as a first project where reading datasheets/ programmer manuals for an MCU/IC was required, useful in embeddded development.
