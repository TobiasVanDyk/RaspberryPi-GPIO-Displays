# ILI9341 Displays

[**ElectroDragon (EDS) LCD Displays SPI SD-Card Touch Size 1.8 to 4.0 inches**](https://www.electrodragon.com/product/eds-tft-lcd-lcm-spi-interface-variable1-82-2/)

**2.4 inch Model 320x240:** 
Top and bottom and schematic:
<p align="left">
<img src="images/pic1.jpg" width="250" />  
<img src="images/pic2.jpg" width="250" /> 
<img src="images/schematic1.jpg" width="300" />  
<br>
The SDCard as configured on this model will not function when interfaced with a 3v3 MCU. To correct it, replace the 3 x 1k series resistors R1, R2, R3 with 0 ohm resistors or a solder bridge. Also connect SD-CS to a 10k resistor pullup to 3v3.

The display as supplied is also not in a Raspberry Pi GPIO compatible pin format. Details will be given below to connect it to a Raspberry Pi standard 40 pin GPIO connector.



