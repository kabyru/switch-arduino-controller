# Nintendo Switch Automated Controller (via Arduino micro-controller)

### A means to automate Nintendo Switch controller input to cover repetitive tasks, such as Pokemon egg hatching, minigame grinding, etc.

## What is this?
This repository contains code files that allow for the easy implementation of automated repetitive Switch controller input, which can prove to be useful for certain tasks within games or for capturing game footage. This is possible by flashing a micro-controller (like an Arduino) to act as a [HORI Pokken Tournament Pro Pad Limited Edition Controller for Nintendo Wii U](https://www.amazon.com/Pokken-Tournament-Limited-Controller-Nintendo-u/dp/B019QB4SL0?SubscriptionId=AKIAILSHYYTFIVPWUY6Q&tag=duckduckgo-ffab-20&linkCode=xm2&camp=2025&creative=165953&creativeASIN=B019QB4SL0), which can be used on the Switch as a Pro Controller.

A diagram of the controller is shown below:
![](https://i.imgur.com/9bfJWKZ.png)![](https://i.imgur.com/eo5tIKw.png)**Figures 1 and 2**: Diagram of the Pokken Tournament Pro Pad which the Arduino will imitate.

## How does it work?
A micro-controller is able to imitate a Pokken Tournament Pro Pad through the use of *USB descriptors*, which provide the necessary information to properly imitate an I/O stream between the device and the Switch. From there, C code is used to establish an I/O stream between the micro-controller and Switch, where then scripted button-strokes can be sent to the Switch. **Establishing an I/O Stream a solved problem thanks to Dean Camera's LUFA library and shinyquagsire24's Switch-Fightstick repo. I am beyond amazed and impressed with their work, and this project would not exist without their effort.**

Once an I/O Stream is established between the micro-controller and the Switch, an array of controller input is continuously iterated through in an infinite loop. Commands are scripted using the format:
### {COMMAND,DURATION}
...where COMMAND can be any of the following:

| Avaliable Commands        | Function
| ------------- |:-------------:|
| UP		| Sets the left stick to the up position.
| DOWN		| Sets the left stick to the down position.
| LEFT		| Sets the left stick to the left position.
| RIGHT		| Sets the left stick to the right position.
| X		| Presses and holds the X button for the duration specified.
| Y		| Presses and holds the Y button for the duration specified.
| A		| Presses and holds the A button for the duration specified.
| B		| Presses and holds the B button for the duration specified.
| L		| Presses and holds the L bumper for the duration specified.
| R		| Presses and holds the R bumper for the duration specified.
| ZL		| Presses and holds the ZL trigger for the duration specified.
| ZR		| Presses and holds the ZR trigger for the duration specified.
| PLUS		| Presses and holds the + button for the duration specified.
| MINUS		| Presses and holds the - button for the duration specified.
| HOME		| Presses and holds the HOME button for the duration specified.
| CAPTURE	| Presses and holds the CAPTURE button for the duration specified.
| LCLICK	| Presses and holds the left stick in for the duration specified.
| RCLICK	| Presses and holds the right stick in for the duration specified.
| BUMPERS	| Presses and holds both L & R bumpers for the duration specified.
| NOTHING	| Does nothing, essentially a time delay. Useful between commands.

...and DURATION is the number of *frames* of which the command is held. **Typically, the Switch runs at 30 frames per second, so a good rule of thumb is to assume a duration of 30 lasts one second.**

Automated commands can be added in the **Joystick.c** file, where commands added below line 63 and within the specified code block will be perpetually ran as long as the micro-controller is connected to the Switch.

## How do I upload my code to my micro-controller? Which one should I use?
The LUFA library, which is used to establish the connection between the micro-controller and the Switch, is compatible with a multitude of micro-controllers, and has been tested to work with Arduino UNO R3 (and its clones, Elegoo, etc.), Arduino Mini, and the Teensy++ 2.0 board. I personally developed this project using an Arduino R3, and can vouch that it and its clones should run this code as expected without issue.

To upload this code to the micro-controller, it must first be built using **make**. On Windows, Make can be installed using [Chocolatey](https://chocolatey.org/) or through [GNU's SourceForge repo](http://gnuwin32.sourceforge.net/packages/make.htm). I recommend going through Chocolately, as its install will also make the proper changes to your environment variable PATH that will allow you to call make from the command line. You will also need to install [Atmel's 8-bit AVR toolchain](https://www.microchip.com/mplab/avr-support/avr-and-arm-toolchains-c-compilers) and add its installation directory to PATH. You will also need to install [MinGW's default packages](https://osdn.net/projects/mingw/releases/) and add its bin directory to PATH. Once each of these have been installed, the project can be compiled into a HEX file using make. To do this, open up a PowerShell (or Command Prompt) window, change to project directory, and enter the command **make**. 

NOTE: Depending on your development board, you may need to tweak the **MCU** value in the makefile to properly match which MCU is used in your development board. This project uses atmega16u2 by default, which is the MCU for the Arduino UNO R3 family of boards. When using a Teensy or other kind of board, this value will need to be modified. 

Once this completes, the newly generated HEX file can be then flashed onto the micro-controller. These steps will vary by development board. If using a Teensy, [follow the instructions here, and use the Teensy Loader](https://www.pjrc.com/teensy/loader.html) to flash the HEX file onto the board. If using an Arduino, first set the board in DFU mode, [which can be easily done by following these instructions](https://www.arduino.cc/en/Hacking/DFUProgramming8U2). Once in DFU mode, use the [Atmel FLIP application](https://www.microchip.com/developmenttools/ProductDetails/flip) to flash the HEX file onto the Arduino. Once this completes, you should notice Windows trying to find the drivers for a "POKKEN TOURNAMENT" device. This signifies that the flashing process was a success. Another sign of a successful flash is if the RX and TX LEDs on the Arduino are solidly illuminating. 

The micro-controller is now ready for use as a controller for the Switch! Simply connect the micro-controller to the Switch through the USB ports (or into the USB-C port through an adapter), and will begin the scripted commands momentarily after.

## In Conclusion...
This project would not be possible without the groundwork laid out by the Switch hacking community on GitHub, especially [shinyquagsire23](https://github.com/shinyquagsire23/Switch-Fightstick) and [bertrandom](https://github.com/bertrandom/snowball-thrower). Their work has provided the means for how to properly communicate with the Switch via USB, as well as a way to communicate automated inputs through iterative lists. If you have any questions or concerns, please feel free to create an issue on this repo page.




> Written with [StackEdit](https://stackedit.io/).
