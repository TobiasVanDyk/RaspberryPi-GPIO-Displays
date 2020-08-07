# RaspberryPi-GPIO-Displays

A recent kernel update to version 5.4, changed the GPIO configuration for a number of small directly connected SPI LCD displays for the Raspberry Pi.
The reason seems to be that GPIO descriptors have been changed from pin numbers to labels.

I have tried to use an Waveshare 3.5" 480x320 ILI9486 display with the Waveshare driver, and also with two other drivers, without sucess with the new kerenel. Note that all these drivers work without problems with the previous 4.19 kernel.

I decided to try a completely new apprtoach, and use the FBCP SPI driver at [**Kpishere**](https://github.com/kpishere/fbcp-ili9341). This driver does not utilize the notro/fbtft framebuffer driver, i.e. dtoverlay=waveshare35xx should be removed from config.txt. This program also does not use the default SPI driver, so a line such as dtparam=spi=on in /boot/config.txt should also be removed. Likewise, if you have any touch controller related dtoverlays active, such as dtoverlay=ads7846 those should be removed. The driver has its own touch screen driver.

### I used the following to make the driver::

  $ sudo apt-get install cmake
  $ git clone https://github.com/juj/fbcp-ili9341.git
  $ cd fbcp-ili9341
  $ mkdir build
  $ cd build
  $ cmake -DSPI_BUS_CLOCK_DIVISOR=30 -DWAVESHARE35B_ILI9486=ON -DBACKLIGHT_CONTROL=ON -DDISPLAY_INVERT_COLORS=ON -DSTATISTICS=0 ..
  $ make -j
  $ sudo ./fbcp-ili9341

Also add the following lines in /boot/config.txt:

  $ hdmi_group=2
  $ hdmi_mode=87
  $ hdmi_cvt=320 240 60 1 0 0 0
  $ hdmi_force_hotplug=1

To start at boot, edit the /etc/rc.local in sudo mode, and add a line:

  $ sudo /home/pi/fbcp-ili9341/build/fbcp-ili9341 &

The result surprised me in the responsiveness I obtained, compared when using the same display with the Waveshare driver with kernel 4.19
I intend to test another display from waveshare

### Steps to install in Mint 19.1:

    $ sudo apt-get install git build-essential linux-headers-generic
    $ sudo apt-get install make gcc libelf-dev
    $ git clone -b port-to-4.15 https://github.com/kaduke/Netgear-A6210
    $ cd Netgear-A6210

Replace the rtusb_dev_id.c inside the common folder with the file version in this archive

    $ make
    $ sudo make install
    $ sudo reboot

After the reboot:

    $ sudo service network-manager restart
    
And then wait a few seconds.
