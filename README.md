# Slice-IO-Hardware
Modular Slice IO Hardware

<img src="Resources/Images/IO Slice Mounted Photo.png" height="400px">

## Backplane

The backplane is broken in to two parts: The logic backplane and peripheral power backplane.
The backplane is interrupted by each IO card (input side goes into the card, and comparable output signals go out of the card) despite some signals being a common power plane or drop-bus.

<img src="Resources/Images/Backplane Diagram.png" height="600px">

Logic backplane signals:
* 5V "EBUS" (2A max) and 0V return
* Twisted pair set #1 for CAN signal
* Twisted pair set #2 reserved for future bus-adjacency detection and/or communication

The logic backplane is currently implemented by a 2x8 0.1" header with signals:
1. 5V
1. 0V
1. D1-A (CAN High)
1. D1-B (CAN Low)
1. 0V
1. D2-A (Reserved)
1. D2-B (Reserved)
1. 0V

Peripheral power backplane signals
* 24V (10A max) and 0V return
* Earth ground

The peripheral backplane is currently implemented by a 2x10 0.1" header with signals:
1. 0V
1. 0V
1. 0V
1. 0V
1. PE
1. PE
1. 24V
1. 24V
1. 24V
1. 24V

## IO Slice Board
This generic template board is appropriate for adapting an 8 or 16 point IO module to the common backplane.

Beyond the standard backplane (implemented as right angle male pin headers), The IO slice board layout features the following pins and signals:
* Upper IO Terminal Group
  * 4 x IO points (#1-#4) at 0.2" pitch, and a depth pitch of 0.1", 0.2", and 0.3" options. A larger size mounting hole is featured on the third row in from the edge.
  * 4 x IO points (#9-#12) interleaved with points #1-#4, at an effective pitch of 0.1" for all 8 points. Small mounting holes only are available for pitches of 0.1", 0.2", and 0.3: with no hole available in the third row in from the edge.
  * Flanked on each side by a set of holes at 0.1" pitch connected to earth ground for shield termination.
* Lower IO Terminal Group
  * 4 x IO points (#5-#8) at 0.2" pitch, and a depth pitch of 0.1", 0.2", and 0.3" options. A larger size mounting hole is featured on the third row in from the edge.
  * 4 x IO points (#13-#16) interleaved with points #5-#8, at an effective pitch of 0.1" for all 8 points. Small mounting holes only are available for pitches of 0.1", 0.2", and 0.3: with no hole available in the third row in from the edge.
  * Flanked on each side by a set of holes at 0.1" pitch connected to earth ground for shield termination.
* Process Data I2C bus lcoated between the two sets of IO Terminal Groups
  * 5V, 0V, SDA, SCL signals from top to bottom (designed to match QWIIC/STEMMA style connector)
  * This is the same I2C bus that contains process data reading chips and can be used for expansion to a secondary "dumb" expansion card or any othe rkind of cover.
* User Interface I2C bus located above the top Terminal Group
  * 0V, 5V, SCK, SDA signals from top to bottom (designed to match OLED panel board)
  * This I2C bus is separate from the process data bus and is meant only to control user interface components like screens and LEDs
* Optional Auxiliary Power Connection on the bottom of the board
  * 0V, PE, 24V from left to right
  * Meant to be used in place of drawing peripheral power from the backplane (relay modules)
  * Can be used with alternate voltages (48V, etc) for motion control
  * Must not be connected directly to the backplane peripheral power bus if the upstream peripheral power is also being used!


<img src="Resources/Images/IO Board Diagram.png" height="600">

## IO Slice Case
A 3D printed case for mounting the IO board. 
It supports the OLED screen on the front, provides openings for wire connections on the front and bottom, and provides an opening to the USB access port on top.
The back of the case has openings for the backplane connection headers.
The case also has alignment and locking features to enable slide-in locking installation on a backplane mount.
The case is printable without supports and is joined by M3 or #4 screws.

<img src="Resources/Images/IO Slice Case Front Render.png" height="400px">
<img src="Resources/Images/IO Slice Case Back Render.png" height="400px">


## IO Slice Mount
A 3D printed assembly for mounting an IO Slice onto a DIN rail based backplane.
The backplane clip part connects solidly to a standard DIN rail, and clips into locking features on the IO Slice Case.
The slice locator part interfaces with alignment features on the IO Slice Case, and mounts the backplane connector board.
The assembly is printable without supports and is joined by M3 or #4 screws.
Multiple slice mount assemblies located adjacent to eachother on a DIN rail will form an interconnected backplane bus.

<img src="Resources/Images/IO Slice Mount Left Render.png" height="400px">
<img src="Resources/Images/IO Slice Mount Right Render.png" height="400px">

## Raspberry Pi Compute Module Power Board
This board adapts the [waveshare dual ethernet Compute Module 5 carrier](https://www.waveshare.com/cm5-dual-eth-mini.htm) to the Slice IO backplane.
This board is also responsible from powering the Raspberry Pi, the EBUS, and Peripheral backplane.

Beyond the standard backplane (implemented as right angle male pin headers *downstream side only*), this board features the following pins and signals:
* Raspberry Pi Pin headers (abridged 26 pin version)[https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#gpio]
  * Powers the raspberry Pi through two 5V pins (requires stable, regualted power) and five 0V return pins
  * I2C breakout from pins 3 and 5 to run the user interface I2C bus
  * SPI breakout from pins 19/21/23 to run the CAN controller
  * Other potential uses of pins 1-26
* Logic power input located at the same spot as IO pins #1 and #2 (0.2" pitch)
  * One branch is regulated down to 5V/5A for the raspberry pi
  * One branch is regulated down to 5V/2A for the EBUS
* Peripheral power input located at the bottom of the board (same as IO card)
  * 0V, PE, 24V from left to right

<img src="Resources/Images/Pi Power Board Diagram.png" height="600">

## Raspberry Pi Compute Module Case
A 3D printed case for mounting the Pi Compute Module Power Board.
It supports the compute module carrier board, the compute module, and the passive heatsink inside the bottom half of the assembly.
A 25mm fan can be mounted to the outside of the case to provide active cooling, which circulates from the vent grille at the top of the case.
A 128x32 OLED screen is mounted in the typical position and driven by the Pi, with clearance for both the logic and peripheral power input terminals.
The back of the case has openings for the backplane connection headers.
The case has standard alignment and locking features for slide-in locking installation on a backplane mount.
The case is printable without supports and i joined by M3 or #4 screws.

<img src="Resources/Images/Pi Power Case Front Render.png" height="400px">
<img src="Resources/Images/Pi Power Case Back Render.png" height="400px">