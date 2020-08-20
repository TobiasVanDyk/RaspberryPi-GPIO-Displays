# ILI9341 Displays

[**ElectroDragon (EDS) LCD Displays SPI SD-Card Touch Size 1.8 to 4.0 inches**](https://www.electrodragon.com/product/eds-tft-lcd-lcm-spi-interface-variable1-82-2/)

**2.4 inch Model 320x240:** 
Top and bottom and schematic:
<p align="left">
<img src="images/pic1.jpg" width="250" />  
<img src="images/pic3.jpg" width="250" /> 
<img src="images/schematic1.jpg" width="300" />  
<br>
The SDCard as configured on this model will not function when interfaced with a 3v3 MCU. To correct it, replace the 3 x 1k series resistors R1, R2, R3 with 0 ohm resistors or a solder bridge. Also connect SD-CS to a 10k resistor pullup to 3v3.

The display as supplied is also not in a Raspberry Pi GPIO compatible pin format. Details will be given below to connect it to a Raspberry Pi standard 40 pin GPIO connector.

Connect the display to Raspberry Pi (3B+ used):

|     LCD     | Pin  | RaspPi |  
|-------------|------|--------| 
|1 Vcc +5v    |	2 	 |  +5v   |  
|2 Gnd 	      |	6    |  Gnd   |  
|3 LCD-CS S   | 24	 | GPIO 8 | 
|4 RST Reset  |	13 	 | GPIO 27| 
|5 DC         | 15 	 | GPIO 22|  
|6 MOSI       |	19 	 | GPIO 10|  
|7 SCLK       | 23	 | GPIO 11| 
|8 LED        | 1	   | +3v3   |  
|9 MISO       |  	   |   NC   | 

swkim01 has compiled a waveshare ILI9340 driver for kernel 5.4 that can be used for the ILI9341 LCD displays here.
(1) git clone https://github.com/swkim01/waveshare-dtoverlays.git
(2) sudo cp waveshare-dtoverlays/waveshare32b.dtb /boot/overlays/waveshare32b-overlay.dtb
(3) sudo cp waveshare-dtoverlays/waveshare32b.dtb /boot/overlays/waveshare32b.dtb0

Now use the Zaryob github [**Zaryob LCD-show New Waveshare Type C LCD**](https://github.com/Zaryob/LCD-show), edit his local copy of LCD35C-show and comment out the sudo reboot at the end. Then proceed as shown below, but before the sudo reboot, edit /boot/config.txt and correct the line dtoverlay=waveshare35c:rotate=90 to dtoverlay=waveshare32b:rotate=270 (i.e add the overlay from swkim01).

(4) git clone https://github.com/Zaryob/LCD-show.git
(5) cd LCD-show/
(6) chmod +x LCD35C-show
(7) sudo ./LCD35C-show
(8) sudo reboot

The result is shown below:
<p align="left">
<img src="images/pic10.jpg" width="200" />  
<img src="images/piv11" width="200" />  
<img src="images/pic12" width="200" />  
<br>



 



