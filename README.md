# GamePi20 Drivers for Linux Kernel 5.4 and Above

First, enable the SPI interface by using `sudo raspi-config` and go to `Interface Options > SPI`

or by editing `/boot/config.txt` and add the following line `dtparam=spi=on`



## TFT Drivers / Device Tree Overlay Source (DTS) for GamePi20
 
NOTE:The old modules method doesnt work on linux kernel 5.4. The device tree overlay for gamepi20 tft is updated for linux kernel 5.4, 

Compile ST7789V GamePi TFT device tree overlay source with this command:
```
 sudo dtc -@ -I dts -O dtb -o /boot/overlays/gamepi20.dtbo gamepi20.dts
```

After compiling, add the gamepi20 dtoverlay to /boot/config.txt 

First `sudo nano /boot/config.txt` to open text editor in superuser, then add the following code end of the line

```
dtoverlay=gamepi20
```
### How to check Drivers is successsfully loaded

The command `ls \dev\` should show `\dev\fb1`

The command  `lsmod | grep st7789` should showup

The command `dmesg | grep st7789` should show no errors
