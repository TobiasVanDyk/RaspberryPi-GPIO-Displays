# RaspberryPi-GPIO-Displays

A recent kernel update to version 5.4, changed the GPIO configuration for a number of small directly connected SPI LCD displays for the Raspberry Pi.
The reason seems to be that GPIO descriptors have been changed from pin numbers to labels. I used a nightly kernel 2020-08-06-raspios-buster-nightly-armhf with a Raspberry Pi 3B+, for the description below.

I have previously, and over many days, tried to use a Waveshare 3.5" 480x320 ILI9486 type B rev2 and type C, display with the [**Waveshare LCD B v2**](https://github.com/waveshare/LCD-show) drivers, and also with three other drivers [**GoodTFFT**](https://github.com/goodtft/LCD-show) and [**swkim01**](https://github.com/swkim01/waveshare-dtoverlays) and [**JBTEK**](https://github.com/acidjazz/jbtekoverlay), without success with the new Raspberry Pi 5.4 kernel. (Note that all these drivers work without problems with the previous 4.19 kernel.) The LCD used for the test below is the [**Waveshare display B v2**](https://www.waveshare.com/3.5inch-rpi-lcd-b.htm), which is on the left below. 

<br>
<p align="center">
<img src="images/Wave35b-v2.jpg" width="400" />  
<img src="images/Wave35c.jpg" width="400" />  
<br>

I decided to try a new approach, and use the [**FBCP SPI driver**](/fbcp) at [**Kpishere**](https://github.com/kpishere/fbcp-ili9341). This driver does not use the [**notro/fbtft**](https://github.com/notro/fbtft) framebuffer driver, i.e. lines such as dtoverlay=waveshare35xx should be removed from /boot/config.txt. This program also does not use the default SPI driver, so a line such as dtparam=spi=on in /boot/config.txt should also be removed. Similarly, if there are touch controller related dtoverlays active, such as dtoverlay=ads7846 those should be removed. The driver has its own touch screen driver.

**I used the following to make the driver:**

    $ sudo apt-get install cmake
    $ git clone https://github.com/juj/fbcp-ili9341.git
    $ cd fbcp-ili9341
    $ mkdir build
    $ cmake -DSPI_BUS_CLOCK_DIVISOR=30 -DWAVESHARE35B_ILI9486=ON -DBACKLIGHT_CONTROL=ON -DDISPLAY_INVERT_COLORS=ON -DSTATISTICS=0 ..
    $ make -j
    $ cd fbcp-ili9341
    $ sudo ./fbcp-ili9341
    
**Also add the following lines in /boot/config.txt:**

    $ hdmi_group=2
    $ hdmi_mode=87
    $ hdmi_cvt=480 320 60 1 0 0 0
    $ hdmi_force_hotplug=1

**To start at boot, edit the /etc/rc.local in sudo mode, and add a line:**

    $ sudo /home/pi/fbcp-ili9341/build/fbcp-ili9341 &
    
[**sudo-fbcp-ili9341-result.txt**](sudo-fbcp-ili9341-result.txt) shows the result of running the driver.

The result surprised me in the responsiveness I obtained, compared when using the same display with the Waveshare driver with kernel 4.19
I intend to test another display from waveshare, and test it using a Raspberry Pi 4. This second LCD is the [**Waveshare display C**](https://www.waveshare.com/3.5inch-rpi-lcd-c.htm) on the right at the very top, above.

The first image is the result of the first run of the driver, and tries to run the LCD display at 1920x1080 pixels. The second, and fourth picture, is after the cmake line: <br>$ cmake -DSPI_BUS_CLOCK_DIVISOR=30 -DWAVESHARE35B_ILI9486=ON .. <br>was used to build the driver, and shows inverted colours. The third and the fifth picture is with the corrected colour driver.
<br>
<p align="center">
<img src="images/Pi3BPWave35Bv2-1.jpg" width="300" />  
<img src="images/Pi3BPWave35Bv2-3.jpg" width="300" />  
<img src="images/Pi3BPWave35Bv2-5.jpg" width="300" />  
<br>
    
    
<br>
<p align="center">
<img src="images/Pi3BPWave35Bv2-2.jpg" width="400" />  
<img src="images/Pi3BPWave35Bv2-4.jpg" width="400" />  
<br>

Problems with these SPI LCD displays have been mentioned on the main [**Raspberry Pi kernel 5.4 update forum**] (https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=269769), but the solution presented there is only suitable for the older 3.2" type B and 3.5" type A Waveshare LCD's, which are no longer in production.
