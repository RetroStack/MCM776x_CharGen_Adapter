# MCM667x Character Generator Adapter

This repository provides an adapter for the Motorola MCM6670 and related (MCM667x series) character generators for vintage computers. ROMs with various character sets are provided in a [separate repository](https://github.com/RetroStack/Character_Generator_ROMs).
The entire project is available under the MIT license.

![Adapter Variations](/Latest/MCM776x_Character_Generator_Adapter_Photo.png)

## Project Details

### Latest Files

In the "Latest" folder, you'll find the most up-to-date design files, including:

- Gerber files suitable for popular online PCB manufacturers like [PCBWay](/Latest/MCM776x_Character_Generator_Adapter_Gerber_PCBWay.zip) and [JLCPCB](/Latest/MCM776x_Character_Generator_Adapter_Gerber_JLCPCB.zip). Most manufacturers should be fine with either.
- A Bill of Materials (BOM) in both [CSV](/Latest/MCM776x_Character_Generator_Adapter_BOM.csv) (with sources) and [PDF](/Latest/MCM776x_Character_Generator_Adapter_BOM.pdf) formats.
- Layers exported in PDF format.
- The [full schematics](/Latest/MCM776x_Character_Generator_Adapter_Schematics.pdf) of the adapter.

### Implementation

The adapter has been implemented using KiCAD 7. The KiCAD project files are included in this repository.

![PCB Front](/Latest/MCM776x_Character_Generator_Adapter_3D_Front.png)
![PCB Back](/Latest/MCM776x_Character_Generator_Adapter_3D_Back.png)

### Assembly Instructions

![PCB](/Latest/MCM776x_Character_Generator_Adapter_Photo1.png)

1) Get a straight male pin header with pitch (2.54mm; standard) and break it to a 9-pin length. Do this twice. A machined header is recommended if the adapter will be inserted into a socket. Inserting a square pin in a socket might damage them.
2) Solder the pins in place into the central lengthwise holes. To align them correctly, you can use another socket or a breadboard. Stick the longer parts into the socket/breadboard and put the PCB on top. Solder first both edges on each. Recheck if they are flush to the header. If not, reheat the corner that needs correction and press down on PCB until flush. Then, solder all other pins.
3) Install the resistor network. One edge has a square around one pin. This should be where pin 1 will be inserted. Pin 1 is identified on the resistor network usually with a dot.
4) Write/burn one of the binaries for character sets onto a ROM.
5) (Optional) Solder a socket in place. Similarly to the headers mentioned above, only solder one pin on each side before soldering all to be able to make adjustments as needed. Adding a socket is not recommended for machines which do not have enough space for it (e.g., TRS-80 Model 1). For these, you need to skip this step and solder the ROM directly to the board.
6) Solder the ROM to the board or insert it into the socket.

![Partly Assembled](/Latest/MCM776x_Character_Generator_Adapter_Photo2.png)

The adapter provides various configurations. Depending on your ROM used, only some PINs can be configured. See the description on the board for more details.
For example, a 27256 ROM only uses 14 pins and therefore only 5 pins can be configured. The 6th will not have an affect.

NOTE: Keep in mind that the bits are active-low. That means if you want to put set a 1 on a specific address, you leave it unbridged. If you want a 0, then bridge it. This can be fixed by using a reversed/inverted ROM. See below for more details why that is.

NOTE: The board exposes the bits from right to left. If you prefer left to right (e.g. for DIP switches to match labels), then flip the address bits in the ROM or use the provided "f" versions of these.

Here are some examples:

#### No Configuration
7) Clip off extension board on perforated line.
8) Use solder bridges on the bottom to pre-select a character set. 

#### Jumper Caps
7) Add a 2 row pin straight header (2.54mm pitch) to the center two pins of the extension board. You need to leave one row on each side (on edge and on the side next to the ROM).
8) Add jumpers.

![Assembled](/Images/Image3.png)

### Right Angle Jumpers
NOTE: Due to space constraints, only 5 jumpers can be used.
7) Clip off extension board on perforated line.
8) Add a 2 row pin right-angle header (2.54mm pitch) to the bottom of the PCB, extending one end out over the board where the extension was.
8) Add jumpers.

![Assembled](/Images/Image2.png)

### DIP Switches
7) Add a DIP switch on the extension board. The DIP switch should cover all holes from front to back.

![Assembled](/Images/Image1.png)

### DIP Switches on extension board
NOTE: Due to space constraints, only 5 DIP switches can be used.
7) Clip off extension board on perforated line.
8) Solder up to 6 wires (e.g. ribbon cable) to the bottom of the PCB. Use up to 5 wires for each address line, which is the row close to the center of the board (next to the revision number). To identify which address line is which, see the label just below the ROM on the top of the adapter PCB. Solder at least one wire to any of the holes closer to the edge of the board. These are ground pins and therefore any could be used.
9) Solder all wires to the extension board. Use the central two rows of holes. Solder it on from the bottom as DIP switches will be added to the top. Solder the address lines closest to the perforated line where the extension board was attached to the adapter PCB. Solder the single ground wire to and other hole of the second row of holes further away from the perforated edge.
10) Add a DIP switch on the extension board. The DIP switch should cover all holes from front to back. You may need to trim the soldered pins of the cable to solder the DIP switches as close to flush as possible.

![Assembled](/Images/Image4.png)

### Jumper caps on extension board
NOTE: Due to space constraints, only 5 jumpers can be used.
7) Clip off extension board on perforated line.
8) Solder up to 6 wires (e.g. ribbon cable) to the bottom of the PCB. Use up to 5 wires for each address line, which is the row close to the center of the board (next to the revision number). To identify which address line is which, see the label just below the ROM on the top of the adapter PCB. Solder at least one wire to any of the holes closer to the edge of the board. These are ground pins and therefore any could be used.
9) Add a 2 row pin straight header (2.54mm pitch) to the center two pins of the extension board. You need to leave one row on each side (on edge and on the side next to the ROM).
10) Solder all wires to the extension board. Use the row closer to the perforated edge to solder the address lines. Solder the single ground wire to and other hole at the bottom edge furthest away from the perforated edge. You can solder it from the top or bottom.
11) Add jumpers.

### Why is the configuration active low?
Some ROMs, especially the EEPROMs have an active-low WRITE pin that needs to be HIGH (or 1) in normal (read) use. To avoid putting ROMs in an incorrect mode (write mode instead of read mode), all additional pins are pulled high by default through pull-up resistors.

### Why are address lines from right to left?
Having them in this orientation was the simplest without adding complexity to the design. Additionally, the addressing depends on what type of configuration you use. Binary numbers are usually written from right to left (as implemented) and this works best fro jumpers, for example. However, DIP switches uses labels from left to right.
Overall, one direction had to be chosen, and I went with the simplest. This can be fixed by flipping the address bits. ROMs having this fix already marked with an "f".

### Bill of Materials (BOM)

Below is a list of materials needed to assemble a complete system. Please note that the links and prices (scroll to the right to see them; as of March 3rd, 2024) will not be updated in the future and should only be used as a reference for locating the correct items.

Note: Links and alternatives are provided to assist you in finding the necessary components. I cannot guarantee the complete accuracy or reliability of all these links and alternatives. Please check it for yourself!

|PCB REFERENCE|QTY|VALUE|COMMENT|EACH|TOTAL|SOURCE LINK|
|-|-|-|-|-|-|-|
|RN1|1|4.7k Ohm|6 resistor network with 7-pins, bussed resistors|0.5|0.5|[Mouser](https://www.mouser.com/ProductDetail/652-4607X-1LF-4.7K)|
|U1|2||Source Link is for a rather expensive version of a machined straight male pin header with 40-pins which can be broken into smaller pieces to server for both. There are cheaper ones, but the link is used for future reference.|1.0|1.0|[Mouser](https://www.amazon.com/ZYAMY-2-54mm-Breakable-Straight-Connector/dp/B0778KCHHR/)|
|U2|1|2x64-2x512|Examples are 23128, 23256, 2764, 27128, 27256, 27512, 2864, 28256 and all other variations such as 27C256 or AT28C64B|5.5|5.5|[Mouser](https://www.mouser.com/ProductDetail/556-AT28C64B15PU)|
|J1|1|-|Optional. Could be used for right-angle jumpers or attaching ribbon cable to move switch towards more accessible place.||||
|J2|1|-|Optional. Adding the ability to switch character sets.||||

### RetroStack Libraries

To work with this KiCAD project, you'll need the RetroStack libraries for KiCAD. Please [follow this link](https://www.github.com/RetroStack/KiCAD-Libraries) to access and install these libraries.

## Support this Project

RetroStack is passionate about exploring and preserving the legacy of older computer systems. My work involves creating detailed documentation and videos to share the knowledge. I am also dedicated to reviving these classic systems by reimplementing them and offering replacement parts at no cost. If you're keen on supporting this unique project, I invite you to visit my [Patreon page](https://www.patreon.com/RetroStack). Your support would be immensely valuable and greatly appreciated!

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

