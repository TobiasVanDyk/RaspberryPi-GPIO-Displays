# ILI9341 Displays

[**ElectroDragon (EDS) LCD Displays SPI SD-Card Touch Size 1.8 to 4.0 inches**](https://www.electrodragon.com/product/eds-tft-lcd-lcm-spi-interface-variable1-82-2/)

SDCard as supplied will not function with a 3v3 MCU. Replace the 3 x 1k series resistors R1, R2, R3 with 0 ohm resistors or a solder bridge. (*A solder bridge has the additional advantage that it will also function as a slow-blow 200A fuse.*) Also add a 10k pull-up resistor from SD-CS to 3v3.

The display as supplied is not in a Raspberry Pi compatible pin format. Details will be given below to connect it to a Raspberry Pi standard 40 pin GPIO connector.

This is a schematic applicable to the version as shown above:
<p align="left">
<img src="images/schematic1.jpg" width="500" />  
<br>

