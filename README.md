# Voron-V0-Display-on-CANBUS
Wiring and config to run the Voron V0 display over CAN

## Setup Steps

### Compile Toolboard Firmware
- ssh to your pi console
- CD to the klipper directory
```
cd klipper
```
- Run MAKE Clean
```
make clean
```
- open menuconfig
```
make menuconfig
```
- Set the following options for CANBUS connection     

![Config](/Images/V0Display_CAN_Config.jpg)

set the DFU jumper on the V0 display and connect to the Pi by USB

Run lsusb from the command prompt

Make sure you see an STM32 in DFU mode listed
Run dfu-util --list from the command prompt

note the text inside the [xxxx:yyyy]
Remove the boot jumper.

Exit and Save
Run make clean to clean up the make environment.

Run make flash FLASH_DEVICE=xxxx:yyyy (using xxxx:yyyy from above)

Remove the USB cable, power off system and set up the CAN wiring. 

![Wiring](Images/V0Display_CAN_Wiring.jpg)
