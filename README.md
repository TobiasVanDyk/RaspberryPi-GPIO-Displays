# RaspberryPi-GPIO-Displays

**Update 11 Aug 2020:** `There are new dtoverlay driver files for the Waveshare Type C (Fast SPI) LCD` now on the [**swkim01 github page**](https://github.com/swkim01/waveshare-dtoverlays), which `functions well with kernel 5.4`.

**Kernel 5.4 is now working with new standard dtoverlay fb drivers (Raspberry Pi 3B+ and Waveshare 3.5" LCD (C) 125 MHz SPI):**      
<p align="left">
<img src="images/Pi3BK54LCDc-1.jpg" height="500" />  
<img src="images/Pi3BK54LCDc-2.jpg" height="500" />  
<br>


### Kernel 4.19
Two Waveshare 3.5" 480x320 ILI9486 type B rev2 and type C displays, were tested with the [**Waveshare LCD-show**](https://github.com/waveshare/LCD-show) drivers, and also with three other driversets: [**GoodTFFT**](https://github.com/goodtft/LCD-show) - use the MHS35-show for Waveshare (C) LCD, and [**swkim01**](https://github.com/swkim01/waveshare-dtoverlays) and [**JBTEK**](https://github.com/acidjazz/jbtekoverlay). All four driversets functioned adequately when using the previous (May 2020) Raspberry Pi Linux 4.19 kernel. None of the four driversets worked at all, with the current (July 2020) Raspberry Pi Linux kernel 5.4.

The two LCD displays used for the tests are the [**Waveshare display B v2 IPS 12 MHz SPI**](https://www.waveshare.com/3.5inch-rpi-lcd-b.htm), which is on the left below, and the [**Waveshare display C 125MHz SPI**](https://www.waveshare.com/3.5inch-rpi-lcd-c.htm) on the right.
<p align="left">
<img src="images/Wave35b-v2.jpg" width="300" />  
<img src="images/Wave35c.jpg" width="300" />  
<br>
    
The set of eight photos below, show the results of using the Waveshare drivers with the (May 2020) 4.19 kernel. This can be compared to the results when using the SPI DMA accelerated driver with the new (July 2020) kernel 5.4, as detailed in the next section.

In order to obtain dual HDMI LCD displays add the following lines in /boot/config.txt:
```
hdmi_group=2
hdmi_mode=87
hdmi_cvt=480 320 60 1 0 0 0
hdmi_force_hotplug=1
```

**Raspberry Pi 3B+ and Waveshare 3.5" LCD IPS (B) 15 MHz SPI:**
<p align="left">
<img src="images/RPi3BPK419LCDBv2-3.jpg" height="500" />  
<img src="images/RPi3BPK419LCDBv2-4.jpg" height="500" />  
<br>
    
<br>
<p align="left">
<img src="images/RPi3BPK419LCDBv2-1.jpg" width="400" />  
<img src="images/RPi3BPK419LCDBv2-2.jpg" width="400" />  
<br>

**Raspberry Pi 3B+ and Waveshare 3.5" LCD (C) 125 MHz SPI:**    
<p align="left">
<img src="images/RPi3BPK419LCDC-3.jpg" height="500" />  
<img src="images/RPi3BPK419LCDC-4.jpg" height="500" />  
<br>

<br>
<p align="left">
<img src="images/RPi3BPK419LCDC-1.jpg" width="400" />  
<img src="images/RPi3BPK419LCDC-2.jpg" width="400" />  
<br>

The more robust Raspberry Pi 3B+ was used instead of the Raspberry Pi 4, because of the latter's fiddly (*adjective: complicated or detailed and awkward to do or use*), micro-hdmi connector - I did not want to break it with frequent change-overs.
    
### Kernel 5.4  

**Update 11 Aug 2020:** There are new driver files for the Waveshare Type C (Fast SPI) now on the [**swkim01 github page**](https://github.com/swkim01/waveshare-dtoverlays), which functions well with kernel 5.4. Follow the old instructions to install the Waveshare type C LCD, then reboot and replace the waveshare35c.dtbo in /boot/overlays/ with the one from swkim01 in github. The folder swkim01 in here, contains my config.txt and the working dtbo files (as well as the source file dts).
Add the following lines in /boot/config.txt:
```
dtoverlay=waveshare35c:rotate=90
hdmi_force_hotplug=1
hdmi_group=2
hdmi_mode=1
hdmi_mode=87
hdmi_cvt 480 320 60 6 0 0 0
hdmi_drive=2
display_rotate=0
```
For more information about this kernel 5.4 fix, see the following links:
* [**Zaryob: I just changed values of reset-gpios and pendown-gpio from 0x00 to 0x01**](https://github.com/waveshare/LCD-show/pull/30)
* [**Display remains white after kernel update**](https://github.com/goodtft/LCD-show/issues/223)
* [**White Screen after every Boot, maybe because of newest Kernel?**](https://github.com/rootzoll/raspiblitz/issues/1436)


**Previously:** A recent kernel update to version 5.4, changed the GPIO configuration for a number of small directly connected SPI LCD displays for the Raspberry Pi. Refer to this [**list of Raspberry Pi discussion Forum topics**](Discussion-RaspberryPiForum-LCDKernel54.txt). The reason seems to be that GPIO descriptors have been changed from pin numbers to labels. In [**Linux Staging fbtft Switch to the gpio descriptor interface**](https://github.com/torvalds/linux/commit/c440eee1a7a1d0f2d5fc2ee6049e4a05da540f01): *"This switches the fbtft driver to use GPIO descriptors rather than numerical gpios."* 

I used a nightly kernel `2020-08-06-raspios-buster-nightly-armhf` with a `Raspberry Pi 3B+ with a Waveshare type (B) rev2 3.5" LCD`, and a `Raspberry Pi 4 (4GB) with a Waveshare type (C) fast SPI 3.5" LCD`, for the description as outlined, below.

I decided to try a new approach, and use the [**FBCP SPI driver**](/fbcp) at [**juj**](https://github.com/juj/fbcp-ili9341). This driver does not use the [**notro/fbtft**](https://github.com/notro/fbtft) framebuffer driver, i.e. lines such as dtoverlay=waveshare35xx should be removed from /boot/config.txt. This program also does not use the default SPI driver, so a line such as dtparam=spi=on in /boot/config.txt should also be removed. Similarly, if there are touch controller related dtoverlays active, such as dtoverlay=ads7846 those should be removed. The driver has its own touch screen driver.

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
To start at boot, edit the /etc/rc.local in sudo mode, and add a line:
```
sudo /home/pi/fbcp-ili9341/build/fbcp-ili9341 &
```
  
[**sudo-fbcp-ili9341-result.txt**](sudo-fbcp-ili9341-result.txt) shows the result of running the driver.

The result surprised me in the responsiveness of the LCD obtained - *subjectively measured using the user experience when moving the cursor around* - see [**VladStudio The Two and the Mouse**](https://vlad.studio/wallpaper/thetwoandthemouse) - when compared to using the same display with the Waveshare driver on kernel 4.19. For the type C display from Waveshare with a Raspberry Pi 4, the colours are inverted, and only the LCD has a display.

[**VladStudio The Two and the Mouse:**](https://vlad.studio/wallpaper/thetwoandthemouse)
The widely used user-experience **CUE factor** i.e. the cursor usability effort = drag coefficient of moving the mouse cursor to a specified point in order to complete a user task. (*In fluid dynamics, the drag coefficient is a dimensionless quantity that is used to quantify the drag or resistance of an object in a fluid environment.)*
<br>
<p align="left">
<img src="images/thetwoandthemouse.jpg" width="600" />  
<br>

### Raspberry Pi 3B+ and the Waveshare 3.5" (B) revision 2 LCD:
The first image is the result of the first run of the driver, and tries to run the LCD display at 1920x1080 pixels. The second, and fourth picture, is after the cmake line: `$ cmake -DSPI_BUS_CLOCK_DIVISOR=30 -DWAVESHARE35B_ILI9486=ON ..` was used to build the driver, and shows inverted colours. The third and the fifth picture is with the corrected colour driver.
<p align="left">
<img src="images/Pi3BPWave35Bv2-1.jpg" width="300" />  
<img src="images/Pi3BPWave35Bv2-3.jpg" width="300" />  
<img src="images/Pi3BPWave35Bv2-5.jpg" width="300" />  
<br>
   
<br>
<p align="left">
<img src="images/Pi3BPWave35Bv2-2.jpg" width="400" />  
<img src="images/Pi3BPWave35Bv2-4.jpg" width="400" />  
<br>
    
### Raspberry Pi 4 (4GB) and the Waveshare 3.5" (C) fast SPI LCD:
The first image is the result of the first run of the driver (1920x1080 resolution), and the second after a reboot. The colours are inverted, and the cmake option -DDISPLAY_INVERT_COLORS=ON will be removed.

<p align="left">
<img src="images/Pi4Wave35C-1.jpg.jpg" width="400" />  
<img src="images/Pi4Wave35C-2.jpg.jpg" width="400" />  
<br>
    
Problems with these SPI LCD displays have been mentioned on the main [**Raspberry Pi kernel 5.4 update forum**](https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=269769), but the solution presented there is only suitable for the older 3.2" type B and 3.5" type A Waveshare LCD's, which are no longer in production. *There are indications that the framebuffer driver problem may be as simple as changing the polarity of the gpio reset pin from 0 to 1.*

No - changing tft35a@0:reset-gpios:0 to tft35a@0:reset-gpios:1 with a hex editor in the dtb driver had no effect.

`Yes - a recent change (see swkim01 in github) has changed reset-gpios = <&gpio 25 1>; and the driver now functions with kernel 5.4`

<br>
<p align="left">
<img src="images/reset-gpios.png" width="800" />  
<br
    
