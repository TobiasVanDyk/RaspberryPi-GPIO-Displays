# Raspberry Pi ILI9486 GPIO Displays

**Update 11 February 2021:**<br>
The Waveshare Raspberry Pi ILI9486 3.5" Type (C) Fast-SPI LCD works unchanged with the new **kernel 5.10.11.v71+**. Details are below.

**Update 10 October 2020:** <br>
**Configuration details for using the Waveshare Raspberry Pi ILI9486 3.5" Type (B) revision 2 LCD with the Arduino Teensy 3.6.** Details are at the end of this page.

**Update 27 Sep 2020:** <br>
[**Waveshare have updated all their newer LCD display-dtbs to work with kernel 5.45**](https://github.com/waveshare/LCD-show).<br>
[**GoodTFT also updated their LCDshow dtb drivers**](https://github.com/goodtft/LCD-show).

**Update 16 Aug 2020:** <br>
**The standard dt-overlay drivers have now been confirmed to be also working for a Raspberry Pi 4B (4GB):** See the [**Video of Raspberry Pi 4B and kernel 5.45 with Waveshare LCD type C**](../VideoPi4BK54LC35C.mp4) or refer to [**Display 1**](https://github.com/goodtft/LCD-show/issues/223#issuecomment-670673815) and [**Display 2**](https://github.com/goodtft/LCD-show/issues/223#issuecomment-667666150). Instructions are as detailed below.

**Update 11 Aug 2020:** <br>
`There are new device tree FBCP driver files for the both the Waveshare Type C (125MHz SPI) LCD, and the Waveshare Type B revision 2 LCD`. For the first go to [**swkim01 github page**](https://github.com/swkim01/waveshare-dtoverlays), or else both LCD displays have new source (dts) and compiled (dtb) drivers here in the Waveshare folder. Both of these ILI9486 LCD displays function well with the new kernel 5.4 - and tested here with the RaspberryPi 3B+ and 4B. 

*Compilation details using the latest version of dtc, to build new working drivers for kernel 5.4, are given further down this page.*

### Kernel 5.10
A Waveshare 3.5" 480x320 ILI9486 type C Fast-SPI display, were tested with the a Raspberry Pi 4B and the [**Waveshare LCD-show LCD35C-show**](https://github.com/waveshare/LCD-show) drivers, compiled as specified in the [**Waveshare Wiki**](https://www.waveshare.com/wiki/3.5inch_RPi_LCD_(C)). This driver functioned well when using the new (Feb 2021) Raspberry Pi Linux 5.10.11 kernel. Refer to the two photos below.

<p align="left">
<img src="images/LCD-C-K51011a.png" width="350" />  
<img src="images/LCD-C-K51011b.png" width="350" />  
<br>

### Kernel 4.19
Two Waveshare 3.5" 480x320 ILI9486 type B rev2 and type C displays, were tested with the [**Waveshare LCD-show**](https://github.com/waveshare/LCD-show) drivers, and also with three other driversets: [**GoodTFT**](https://github.com/goodtft/LCD-show) - use the MHS35-show for Waveshare (C) LCD, and [**swkim01**](https://github.com/swkim01/waveshare-dtoverlays) and [**JBTEK**](https://github.com/acidjazz/jbtekoverlay). All four driversets functioned adequately when using the previous (May 2020) Raspberry Pi Linux 4.19 kernel. None of the four driversets worked at all, with the current (July 2020) Raspberry Pi Linux kernel 5.4.

The two LCD displays used for the tests are the [**Waveshare display B v2 IPS 12 MHz SPI**](https://www.waveshare.com/3.5inch-rpi-lcd-b.htm), which is on the left below, and the [**Waveshare display C 125MHz SPI**](https://www.waveshare.com/3.5inch-rpi-lcd-c.htm) on the right.

All configuration details for the two Waveshare LCDs also apply the two LCDWiki ILI9486 displays: [**MHS3521**](http://www.lcdwiki.com/MHS-3.5inch_RPi_Display) and [**MPI3501**](http://www.lcdwiki.com/3.5inch_RPi_Display), as shown on the right below.

<p align="left">
<img src="images/Wave35b-v2.jpg" width="200" />  
<img src="images/Wave35c.jpg" width="200" />  
<img src="images/MPI3501.jpg" width="200" />  
<img src="images/MHS3521.jpg" width="200" />      
<br>
       
The set of eight photos below, show the results of using the Waveshare drivers with the (May 2020) 4.19 kernel. This can be compared to the results when using the SPI DMA accelerated driver with the new (July 2020) kernel 5.4, as detailed in the next section.

In order to obtain dual (HDMI and SPI-LCD) displays, add the following lines in /boot/config.txt:
```
hdmi_group=2
hdmi_mode=87
hdmi_cvt=480 320 60 1 0 0 0
hdmi_force_hotplug=1
```

**Raspberry Pi 3B+ and Waveshare 3.5" LCD IPS (B) 15 MHz SPI:**
<p align="left">
<img src="images/RPi3BPK419LCDBv2-3.jpg" width="200" />  
<img src="images/RPi3BPK419LCDBv2-4.jpg" width="200" /> 
<img src="images/RPi3BPK419LCDBv2-1.jpg" width="200" />  
<img src="images/RPi3BPK419LCDBv2-2.jpg" width="200" />    
<br>

**Raspberry Pi 3B+ and Waveshare 3.5" LCD (C) 125 MHz SPI:**    
<p align="left">
<img src="images/RPi3BPK419LCDC-3.jpg" width="200" />  
<img src="images/RPi3BPK419LCDC-4.jpg" width="200" /> 
<img src="images/RPi3BPK419LCDC-1.jpg" width="200" />  
<img src="images/RPi3BPK419LCDC-2.jpg" width="200" />     
<br>

The more robust Raspberry Pi 3B+ was mainly used instead of the Raspberry Pi 4B, because of the latter's fiddly (*adjective: complicated or detailed and awkward to do or use*), micro-hdmi connector - I did not want to break it with frequent change-overs. The Raspberry Pi 4B does work with both displays using kernel 4.19 and 5.45.
    
### Kernel 5.45  

**Update 16 Aug 2020:** **The standard dt-overlay drivers are now confirmed to be working for a Raspberry Pi 4B as well:**

After the git clone of the Zaryob github [**Zaryob LCD-show New Waveshare Type C LCD**](https://github.com/Zaryob/LCD-show), edit his local copy of LCD35C-show and comment out the sudo reboot at the end. Then proceed as shown below, but before the sudo reboot, edit /boot/config.txt and correct the line hdmi_cvt=480 320 60 1 0 0 0 (i.e add the =).

```
git clone https://github.com/Zaryob/LCD-show.git
cd LCD-show/
chmod +x LCD35C-show
sudo ./LCD35C-show
sudo reboot
```
The result is shown below - working for a Raspberry Pi 4B (4GB) and a Waveshare 3.5" LCD type C using the nightly kernel 2020-08-12-raspios-buster-nightly-armhf.img with all updates up to 16 August 2020. Also see the [**Video of Raspberry Pi 4B and kernel 5.4 with Waveshare LCD C**](../VideoPi4BK54LC35C.mp4). Folder [**pi4B-lcdC**](pi4B-lcdC) contains all the relevant configuration files as used for this particular setup.

**Kernel 5.4 + new dtoverlay fbcp drivers (Raspberry Pi 4B and Waveshare 3.5" LCD type (C):** 
<p align="left">
<img src="images/Pi4K54LCD35c-1.jpg" width="300" />  
<img src="images/Pi4K54LCD35c-2.jpg" width="300" />     
<br>

**Update 11 Aug 2020:** There are new driver files for both the Waveshare Type C (Fast SPI) and type B LCD displays. For the first it is on the [**swkim01 github page**](https://github.com/swkim01/waveshare-dtoverlays), and the type B version 2 display is here in the folder Waveshare. Both these displays function well with kernel 5.4 (using a Raspberry Pi 3B+ and 4B for the type C display). 

As an alternative use one of the following two methods for working kernel 5.4 and Raspberry Pi3B+ dtoverlay drivers:

(1) Follow the Waveshare Wiki instructions to install the Waveshare type C or B rev 2 LCD, then reboot and replace the waveshare35c.dtbo or waveshare35b-v2.dtbo in /boot/overlays/ with the one from swkim01 in github, or for the Type B revision 2 LCD, from here in the folder Waveshare. 

(2) Follow the instructions here in the goodTFT folder to install the Waveshare type C or B rev 2 LCD, and then reboot.

**Compiling new SPI LCD drivers for kernel 5.4:** 
* Edit the dts files for the type C and B revision 2 LCD displays from the swkim01 Github page, and change the 0 to 1 for both the reset and pendown GPIO's (reset-gpios = <&gpio 25 1>;  pendown-gpio = <&gpio 17 1>;). 
* Get a new version of dtc:  git clone git://git.kernel.org/pub/scm/utils/dtc/dtc.git, and then do a make.
* Copy the new dts files into the dtc folder as well as the dtc.sh script from swkim01. 
* Then do: sudo dtc -@ -I dts -O dtb -o waveshare35c.dtbo waveshare35c.dts. Ignore the spidev warning. 
* The dtbo files can now be copied to the /boot/overlays/ folder - refer to the Waveshare folder for filenames.

Add the following lines in /boot/config.txt if they are not already there (change the waveshare35c to waveshare35b-v2 if required):
```
dtoverlay=waveshare35c:rotate=90
hdmi_force_hotplug=1
hdmi_group=2
hdmi_mode=1
hdmi_mode=87
hdmi_cvt=480 320 60 6 0 0 0
hdmi_drive=2
display_rotate=0
```
Make sure the line hdmi_cvt=480 320 60 6 0 0 0 has the equal sign, because the waveshare script is incorrect.

The folders swkim01 and Waveshare, contain working config.txt and the dtbo files (as well as the dts source files).

For more information about this kernel 5.4 fix, see the following links:
* [**Waveshare 2.8" lcd and latest Pi OS**](https://www.raspberrypi.org/forums/viewtopic.php?f=38&t=281023)
* [**Zaryob: I just changed values of reset-gpios and pendown-gpio from 0x00 to 0x01**](https://github.com/waveshare/LCD-show/pull/30)
* [**Display remains white after kernel update**](https://github.com/goodtft/LCD-show/issues/223)
* [**White Screen after every Boot, maybe because of newest Kernel?**](https://github.com/rootzoll/raspiblitz/issues/1436)

**Kernel 5.4 + new dtoverlay fbcp drivers (Raspberry Pi 3B+ and Waveshare 3.5" LCD type (C) and type (B) revision 2):**      
<p align="left">
<img src="images/Pi3BK54LCDc-1.jpg" width="200" />  
<img src="images/Pi3BK54LCDc-2.jpg" width="200" />     
<img src="images/Pi3BK54LCDbv2-1.jpg" width="200" />  
<img src="images/Pi3BK54LCDbv2-2.jpg" width="200" />  
<br>

**Previously:** A recent kernel update to version 5.4, changed the GPIO configuration for a number of small directly connected SPI LCD displays for the Raspberry Pi. Refer to this [**list of Raspberry Pi discussion Forum topics**](../Discussion-RaspberryPiForum-LCDKernel54.txt). The reason seems to be that GPIO descriptors have been changed from pin numbers to labels. In [**Linux Staging fbtft Switch to the gpio descriptor interface**](https://github.com/torvalds/linux/commit/c440eee1a7a1d0f2d5fc2ee6049e4a05da540f01): *"This switches the fbtft driver to use GPIO descriptors rather than numerical gpios."* 

I used a nightly kernel `2020-08-06-raspios-buster-nightly-armhf` with a `Raspberry Pi 3B+ with a Waveshare type (B) rev2 3.5" LCD`, and a `Raspberry Pi 4 (4GB) with a Waveshare type (C) fast SPI 3.5" LCD`, for the description as outlined, below.

A new approach is to use the [**Custom SPI LCD driver**](/fbcp) at [**juj**](https://github.com/juj/fbcp-ili9341). This driver does not use the [**notro/fbtft**](https://github.com/notro/fbtft) framebuffer copy (FBCP) driver, i.e. lines such as dtoverlay=waveshare35xx should be removed from /boot/config.txt. This program also does not use the default SPI driver, so a line such as dtparam=spi=on in /boot/config.txt should also be removed. Similarly, if there are touch controller related dtoverlays active, such as dtoverlay=ads7846 those should be removed. There is not provision for a touch interface with this driver.

**I used the following to make the driver:**

    $ sudo apt-get install cmake
    $ git clone https://github.com/juj/fbcp-ili9341.git
    $ cd fbcp-ili9341
    $ mkdir build
    $ cmake -DSPI_BUS_CLOCK_DIVISOR=30 -DWAVESHARE35B_ILI9486=ON -DBACKLIGHT_CONTROL=ON -DDISPLAY_INVERT_COLORS=ON -DSTATISTICS=0 ..
    $ make -j
    $ cd fbcp-ili9341
    $ sudo ./fbcp-ili9341
    
Also add the following lines in /boot/config.txt:
```
hdmi_group=2
hdmi_mode=87
hdmi_cvt=480 320 60 1 0 0 0
hdmi_force_hotplug=1
```
To start at boot (this is different from the the previously discussed drivers which add the line fbcp &), edit the /etc/rc.local in sudo mode, and add the line:
```
sudo /home/pi/fbcp-ili9341/build/fbcp-ili9341 &
```
  
[**sudo-fbcp-ili9341-result.txt**](sudo-fbcp-ili9341-result.txt) shows the result of running the driver.

The result surprised me in the responsiveness of the LCD obtained - *subjectively measured using the user experience when moving the cursor around* - see [**VladStudio The Two and the Mouse**](https://vlad.studio/wallpaper/thetwoandthemouse) - when compared to using the same display with the Waveshare driver on kernel 4.19. For the type C display from Waveshare with a Raspberry Pi 4, the colours are inverted, and only the LCD has a display.

[**VladStudio The Two and the Mouse:**](https://vlad.studio/wallpaper/thetwoandthemouse)
The widely used user-experience **CUE factor** i.e. the cursor usability effort = drag coefficient of moving the mouse cursor to a specified point in order to complete a user task. (*In fluid dynamics, the drag coefficient is a dimensionless quantity that is used to quantify the drag or resistance of an object in a fluid environment.)*
<br>
<p align="left">
<img src="images/thetwoandthemouse.jpg" width="400" />  
<br>

### Raspberry Pi 3B+ and the Waveshare 3.5" (B) revision 2 LCD:
The first image is the result of the first run of the driver, and tries to run the LCD display at 1920x1080 pixels. The second, and fourth picture, is after the cmake line: `$ cmake -DSPI_BUS_CLOCK_DIVISOR=30 -DWAVESHARE35B_ILI9486=ON ..` was used to build the driver, and shows inverted colours. The third and the fifth picture is with the corrected colour driver.
<p align="left">
<img src="images/Pi3BPWave35Bv2-1.jpg" width="200" />  
<img src="images/Pi3BPWave35Bv2-3.jpg" width="200" />  
<img src="images/Pi3BPWave35Bv2-5.jpg" width="200" />  
<br>
   
<br>
<p align="left">
<img src="images/Pi3BPWave35Bv2-2.jpg" width="300" />  
<img src="images/Pi3BPWave35Bv2-4.jpg" width="300" />  
<br>
    
### Raspberry Pi 4B (4GB) and the Waveshare 3.5" (C) fast SPI LCD:
The first image is the result of the first run of the driver (1920x1080 resolution), and the second after a reboot. The colours are inverted, and the cmake option -DDISPLAY_INVERT_COLORS=ON will be removed.

<p align="left">
<img src="images/Pi4Wave35C-1.jpg.jpg" width="300" />  
<img src="images/Pi4Wave35C-2.jpg.jpg" width="300" />  
<br>
    
Problems with these SPI LCD displays have been mentioned on the main [**Raspberry Pi kernel 5.4 update forum**](https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=269769), but the solution presented there is only suitable for the older 3.2" type B and 3.5" type A Waveshare LCD's, which are no longer in production. *There are indications that the framebuffer driver problem may be as simple as changing the polarity of the gpio reset pin from 0 to 1.*

`Yes - a recent change (see swkim01 in github) has changed reset-gpios = <&gpio 25 1>; and the driver now functions with kernel 5.4`
[**Zaryob: I just changed values of reset-gpios and pendown-gpio from 0x00 to 0x01**](https://github.com/waveshare/LCD-show/pull/30)


 ### Waveshare 3.5" Type (B) revision 2 LCD and Arduino Teensy
Connection details are as below for a Teensy 3.6 using the Adafruit ILI9486 library. Waveshare 3.5" LCD (B) rev 2 for raspberry Pi (ILI9486 chipset) with [Adafruit ILI9486 library](Library-LCD35inch(B)rev2-Teensy36.zip) and example graphicstest. See [LCD35inch(B)rev2-Teensy36.ino](LCD35inch(B)rev2-Teensy36.ino) for details.

Also see [ImpulseAdventure Waveshare_ILI9486](https://github.com/ImpulseAdventure/Waveshare_ILI9486) and  [ImpulseAdventure TFT_eSPI](https://github.com/ImpulseAdventure/TFT_eSPI) for two alternate drivers for this LCD. The second link also has instructions for increasing the response speed of this display: *The library supports TFT displays designed for the Raspberry Pi (RPi) that are based on a ILI9486 or ST7796 driver chip with a 480 x 320 pixel screen. The ILI9486 RPi display must be of the Waveshare design and use a 16 bit serial interface based on the 74HC04, 74HC4040 and 2 x 74HC4094 logic chips. A modification to these displays is possible (see mod image in Tools folder) to make many graphics functions much faster (e.g. 23ms to clear the screen, 1.2ms to draw a 72 pixel high numeral).*

Table 2. Connect the Waveshare LCD 3,5" type (B) revision 2 display to Teensy 3.6:

|   RPi LCD  | Pin  | Teensy | Pin  |  
|------------|------|--------|------| 
| Vcc +5v    | 2 	| +5v    | 5v   |
| Gnd 	     | 6    | Gnd    | Gnd  |
| SCLK       | 23 	| SCK    | 13   |
| MISO       | 21   | MISO   | 12   |  
| MOSI       | 19 	| MOSI   | 11   |
| CE0        | 24	| LCD-CD | 10   | 
| GPIO24     | 18 	| DC     | 9    |
| GPIO25     | 22	| Reset  | 8    | 
    

The working result is as shown below:
<p align="left">
<img src="images/LCD35inch(B)rev2-Teensy36.jpg" width="300" />  
<br>   
