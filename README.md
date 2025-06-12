# PicoCalc

Information Command | Decription
:--- | :---
MM.INFO(BATTERY) | <ins>PICOCALC ONLY</ins> <br/> Returns the current battery level percentage (0-100).
MM.INFO(CHARGING) | <ins>PICOCALC ONLY</ins> <br/> Returns 1 if battery is charging on external power, 0 if battery is not charging.

Option Command | Decription
:--- | :---
OPTION BACKLIGHT KB brightness | <ins>PICOCALC ONLY</ins> <br/> Sets the brightness of the keyboard backlight. 'brightness' is a value between 0 (backlight off) and 255 (maximum brightness).


INSTALL PICO SDK
----------------
```bash
sudo apt update && sudo apt install -y cmake gcc-arm-none-eabi libnewlib-arm-none-eabi build-essential git

mkdir -p ~/pico && cd ~/pico
git clone https://github.com/raspberrypi/pico-sdk.git
cd pico-sdk
git checkout tags/2.1.1 -b sdk2.1.1
git submodule update --init

echo 'export PICO_SDK_PATH=~/pico/pico-sdk' >> ~/.bashrc
source ~/.bashrc
```

SETUP PICOCALC FIRMWARE
-----------------------
```bash
mkdir -p ~/picocalc && cd ~/picocalc
git clone https://github.com/madcock/PicoMiteAllVersions.git
cd PicoMiteAllVersions
mv ~/pico/pico-sdk/src/rp2_common/hardware_gpio/gpio.c ~/pico/pico-sdk/src/rp2_common/hardware_gpio/gpio.bak
ln -s ~/picocalc/PicoMiteAllVersions/gpio.c ~/pico/pico-sdk/src/rp2_common/hardware_gpio/gpio.c
mv ~/pico/pico-sdk/src/rp2_common/hardware_gpio/include/hardware/gpio.h ~/pico/pico-sdk/src/rp2_common/hardware_gpio/include/hardware/gpio.bak
ln -s ~/picocalc/PicoMiteAllVersions/gpio.h ~/pico/pico-sdk/src/rp2_common/hardware_gpio/include/hardware/gpio.h
mv ~/pico/pico-sdk/src/rp2_common/pico_float/float_sci_m33_vfp.S ~/pico/pico-sdk/src/rp2_common/pico_float/float_sci_m33_vfp.bak
ln -s ~/picocalc/PicoMiteAllVersions/float_sci_m33_vfp.S ~/pico/pico-sdk/src/rp2_common/pico_float/float_sci_m33_vfp.S
mv ~/pico/pico-sdk/src/rp2_common/hardware_flash/flash.c ~/pico/pico-sdk/src/rp2_common/hardware_flash/flash.bak
ln -s ~/picocalc/PicoMiteAllVersions/flash.c ~/pico/pico-sdk/src/rp2_common/hardware_flash/flash.c

```
EDIT ``~/picocalc/PicoMiteAllVersions/CMakeLists.txt`` TO CHOOSE TARGET
-----------------------------------------------------------------------
```makefile
set(PICOCALC true)

# Compile for PICO 1 Board
#set(COMPILE PICO)

# Compile for PICO 2 Board
#set(COMPILE PICORP2350)
set(COMPILE WEBRP2350)
```

BUILD PICOCALC FIRMWARE
-----------------------
```bash
cd ~/picocalc/PicoMiteAllVersions
mkdir build
cd build
cmake ..
make
```

(_original readme follows..._)

# PicoMiteRP2350
This contains files to build MMbasic V6.00.02 to run on both RP2040 and RP2350<br>
Compile with GCC 13.3.1 arm-none-eabi<br>

<b style="color:red;"> Build with sdk V2.1.1 but replace gpio.c, gpio.h, float_sci_m33_vfp.S, and flash.c with the ones included here<br></b>

Change CMakeLists.txt line 4 to determine which variant to build<br>
<br>
RP2040<br>
set(COMPILE PICO)<br>
set(COMPILE VGA)<br>
set(COMPILE PICOUSB)<br>
set(COMPILE VGAUSB)<br>
set(COMPILE WEB)<br>
<br>
RP2350<br>
set(COMPILE PICORP2350)<br>
set(COMPILE VGARP2350)<br>
set(COMPILE PICOUSBRP2350)<br>
set(COMPILE VGAUSBRP2350)<br>
set(COMPILE HDMI)<br>
set(COMPILE HDMIUSB)<br>
set(COMPILE WEBRP2350)<br>
<br>
Any of the RP2350 variants or the RP2040 variants can be built by simply changing the set(COMPILE aaaa)<br>
However, to swap between a rp2040 build and a rp2350 build (or visa versa) needs a different build directory.
The process for doing this is as follows:<br>
Close VSCode<br>
Rename the current build directory - e.g. build -> buildrp2040<br>
Rename the inactive build directory - e.g. buildrp2350 -> build<br>
edit CMakeLists.txt to choose a setting for the other chip and save it - e.g.  set(COMPILE PICO) -> set(COMPILE PICORP2350)<br>
Restart VSCode<br>

