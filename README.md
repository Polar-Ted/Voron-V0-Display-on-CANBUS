# Voron V0 Display on CANBUS now with CanBoot!

![Working](/Images/V0_Disply_on_CAN.jpg)
Wiring and config to run the Voron V0 display over CAN with CanBoot

## Setup Steps

### Compile V0 Display Canboot Firmware
- ssh to your pi console
- CD to the klipper directory
```
cd klipper
```

- open menuconfig
```
make menuconfig
```
- Set the following options for CANBUS connection  
    Note: If you already have a CAN implementation set the CANBUS speed to match your existing configuration. Common speeds are 250000 and 500000

![Config](/Images/V0_display_canboot_can.png)

- Set the DFU boot jumper on the V0 display and connect to the Pi by USB

- Run lsusb from the command prompt. Make sure you see a device labeled STM32 in DFU mode listed
```
lsusb
```

  

- Run dfu-util --list from the command prompt
``` 
dfu-util --list
``` 
      
      note the text inside the [xxxx:yyyy]

- Exit and Save      

- Run make clean to clean up the make environment.
```
make clean
```

- Run make 
```
make
```

- Run DFU util to flash the canboot.bin file
```
sudo dfu-util -a 0 -D ~/CanBoot/out/canboot.bin --dfuse-address 0x08000000:force:mass-erase:leave -d 0483:df11
```
- Remove the DFU boot jumper.

- Remove the USB cable, power off system and set up the CAN wiring. 

### Compile V0 Display Klipper Firmware with canboot
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
    Note: If you already have a CAN implementation set the CANBUS speed to match your existing configuration. Common speeds are 250000 and 500000

![Config](/Images/V0_display_klipper_can.png)

- Get your UUID
```
python3 ~/CanBoot/scripts/flash_can.py -q
```
- Flash Klipper to the V0 Display
```
python3 ~/CanBoot/scripts/flash_can.py -i can0 -f ~/klipper/out/klipper.bin -u {your uuid}
```

## Wiring Diagram

    Note: The MCP2551 does not have 120 ohm resistors. If this is the only device on your CANBUS you will need to wire a 120 ohm resistor between CANH and CANL at the MCP2551 connector.  You CANBUS should measure 60 ohms between CAN H and CAN L with all the devices connected. ( measure while powered off) 
    
For those that are interested I have designed a carrier board for the MCP2551 to make wiring and jumpering in the 120 ohm resister easy. [Link to PCBWAY project](https://www.pcbway.com/project/shareproject/Voron_V0_display_CANBUS_Transceiver_5da4f6dd.html) if you want to order bare PCB boards. 

![VO_DIsplay_EZ_CAN board](https://pcbwayfile.s3.us-west-2.amazonaws.com/web/22/10/25/0114274907101m.jpg)

![Wiring](Images/V0Display_CAN_Wiring.jpg)      

![V0 Display Com pins](/Images/V0_Display_com_pins.jpg)

![Display Wiring IMage](/Images/V0_Display_Wiring.jpg)

![MPC2551 Wiring](/Images/MCP2551_CAN_Tran_wiring.jpg)


## Links  

  [V0 Display EZ Can board by Polar_Ted](https://www.pcbway.com/project/shareproject/Voron_V0_display_CANBUS_Transceiver_5da4f6dd.html)

  [MCP2551 board](https://www.aliexpress.com/item/2255800362518857.html?spm=a2g0o.order_list.0.0.21ef1802WJAiGd)
  
  [Voron Hardware V0 Display](https://github.com/VoronDesign/Voron-Hardware/tree/master/V0_Display)
  
  [Maz0r CanBus Configuratuion and troubleshooting guides](https://maz0r.github.io/klipper_canbus/)
  
  [rootiest guide to SKR Pico as a USB Can bridge](https://www.reddit.com/r/klippers/comments/wl4t93/skrpico_as_canbus_bridge/)



