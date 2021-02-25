# GamePi20 Drivers for Linux Kernel 5.4 and Above
For RetroPie 4.7 and Raspbian Buster and above

First, enable the SPI interface by using `sudo raspi-config` and go to `Interface Options > SPI`

or by editing `/boot/config.txt` and add the following line `dtparam=spi=on`

To verify spi is enabled, `lsmod | grep spi_` will show something, otherwise it won't show up anything



## TFT Drivers / Device Tree Overlay Source (DTS) for GamePi20
 
NOTE:The old modules method doesnt work on linux kernel 5.4. So device tree overlay is required to enable gamepi20 tft
The device tree overlay for gamepi20 tft is updated for linux kernel 5.4, 

Compile ST7789V GamePi TFT device tree overlay source with this command:
```
 sudo dtc -@ -I dts -O dtb -o /boot/overlays/gamepi20.dtbo gamepi20.dts
```

After compiling, add the gamepi20 dtoverlay to /boot/config.txt 

First `sudo nano /boot/config.txt` to open text editor in superuser, then add the following code end of the line

```
dtoverlay=gamepi20
```
Lastly reboot to take effect by typing `sudo reboot`

### How to check Drivers is successsfully loaded

The command `ls /dev/` should show `/dev/fb1`

The command  `lsmod | grep st7789` should showup

The command `dmesg | grep st7789` should show no errors

## Framebuffer Copy |fbcp
You need to install fbcp to allow `/dev/fb0` to copy the framebuffer to `/dev/fb1` which is the ST7789V TFT LCD Panel

First make sure  you have cmake and git install
```
sudo apt-get install cmake git
```
Then cd to home-directory for cloning the fbcp, compiling the source code and also to install the fbcp to `/usr/local/bin/`
```
cd ~
git clone https://github.com/tasanakorn/rpi-fbcp
cd rpi-fbcp/
mkdir build
cd build/
cmake ..
make
sudo install fbcp /usr/local/bin/fbcp
```
### How to check fbcp is working
run `fbcp` in command and it should show up the same image as the HDMI output

if not, go back and check whether device tree overlay is successfully installed.

`CTRL+C` to exit `fbcp` after verifying is working

### Install fbcp as services
First we will save `fbcp&` to `/etc/rc.local`

```
sudo nano /etc/rc.local
```

Edit the file so that `fbcp&` is before exit 0. 

REMEMBER IT SHOULD BE `fbcp&` , THE `&` IS VERY IMPORTANT. 
THIS WILL MAKE SURE OTHER SYSTEM PROCESSES CAN CONTINUE LOADING WITHOUT WAITING FBCP TO CLOSE

![alt text](https://github.com/darrenliew96/gamepi20_drivers/blob/main/docs/image/fbcp.rc.local.png?raw=true)

### Edit `/boot/config.txt` so that the words would not be too small, and will fit into the TFT Panel
Copy and paste this into `/boot/config.txt`

```
hdmi_force_hotplug=1
hdmi_cvt=640 480 60 1 0 0 0
hdmi_group=2
hdmi_mode=1
hdmi_mode=87
dtoverlay=pwm
```
- `dtoverlay=pwm` sets the Audio output via the PWM GPIO pin 12 `GPIO=18 (PWM CLK)`

Then `sudo reboot` and the TFT should show up the same image as HDMI output

## Setup the GPIO pins for GAMEPI20 Joystick/Controller
run this command in subsequent to setup the kernel joystick driver

```
git clone https://github.com/waveshare/mk_arcade_joystick_rpi.git
cd mk_arcade_joystick_rpi
sudo ./install.sh 3
sudo reboot
```

Finally after the reboot setup the controller via the initial input configuration.

Short press the button to save input, Long press a button to skip
