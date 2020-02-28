# tec-8x8-TE-11

## tec-8x8 Matrix, 64 LED 2D Display

![](https://github.com/SteveJustin1963/tec-8x8/blob/master/readme-img/cct.png)

This is our first "add-on" for the TEC1. It is an array of 64 LEDs arranged in a matrix of 8 LEDs by 8 LEDs. Actually it has almost the same number of LEDs as the display on the TEC but in this design the LEDs are arranged in ROWS to create a very interesting display. The whole concept of the 8 x 8 matrix is to produce the equivalent of a WINDOW ON A VIDEO SCREEN. Each LED represents one pixel and this will enable us to produce characters, letters and movement equal to 8 pixels by 8 pixels. This may be only a small fraction of the area of video screen but it is the best place to start. If you can produce effects and movement on a small scale, a full-size VDU screen is only an enlargement of our 'window'. If you have seen the advertising signs composed of thousands of LEDs or globes on which moving letters and characters are displayed, you will be interested to know the same effect can be produced with this project. Modules of the 8x8 display can be placed side-by-side to create a long display. The PC board is designed to be cut so that the pattern runs as a continuous display. At this stage it is not out intention to promote the extended display as it requires a slightly different driving circuit. To achieve a readable brightness with more than 8 columns of LEDs, it is necessary to introduce blocks of columns which are latched or even latching for each column. This will enable each LED to be turned on to full brightness and produce a bright display. 

## PARTS LIST
* 8 1k 'watt
* 8 BC 547 transistors 
* 2 - 74LS273 or
* 2 - 74LS374 or
* 2 - 74LS377
* 64 - 5mm red LEDs
* 2 matrix pins
* 2 matrix connectors
* 30 cm hook-up wire, 12 colours.
* 30cm tinned copper wire
* 15cm - Heat-shrink tubing
* 1 - 24 pin DIP HEADER
* 1 - 8x8 DISPLAY PC BOARD


![](https://github.com/SteveJustin1963/tec-8x8/blob/master/readme-img/lay.png)

In our design, the LEDs are multiplexed and this means they are being turned on for one-eighth of the time during one cycle. The result is a dull display but one which can be read under normal lighting conditions. We are presenting this project slightly ahead of time so that it will be ready when needed. There are a number of MACHINE CODE instructions which can only be investigated on a display format and this project is a necessary part to understanding the Z80. The greatest visual impact for this type of display revolves around programmed lighting and effects such as COCA-COLA signs and some of the background effects at discos and TV shows. Most of the dazzling effects behind singers and dancers on TV have some form of micro-processor controlled lighting. The effects that can be produced are limitless. We have chosen LEDs in our design for cheapness and simplicity but they could quite easily be replaced with miniature 6v or 12v globes. The only extra components would be the addition of one extra transistor in each line as an emitter follower. This will enable the extra current to be supplied to the globes. Our 8x8 display is most effective when flashing blocks of LEDs (or globes) and having them jump from one position to another. In addition, bands of LEDs can be made to pass across or up the screen . These effects are very effective and very simple to produce. But first you must understand the Machine Code instructions involved and how to include them in a program. This will be our endevour in the latter part of the course in this issue and the time is right to prepare the display project so that it can be plugged into the TEC-1 when the time comes.  Believe me, you will be most impressed with the results.

## CONSTRUCTION
Before starting any of the construction, it is absolutely essential that you know which lead of the light emitting diode is the cathode. There is only one guaranteed way of determining this. You need a 3v to 6v battery and a 100 ohm or 220 ohm resistor. Place the LED and resistor in a series circuit connected to the battery and check the degree of illumination. The cathode lead will be the one nearest the negative terminal of the battery. There are no other sure-fire methods of determining this as some LEDs have their long lead cut differently to the accepted practice. We have seen some LEDs with the outline (inside the LED) around the opposite way to the general rule. So, it can be quite confusing. You must test each LED or at least a sample from the batch. Next you must be certain which way they are to be inserted in the PC board. A mistake will take a very long time to rectify. The cathode lead is nearest to the row of transistors. When soldering the LEDs to the board, you must take special care to keep them all the same height and perpendicular to the board. The neatness of the display will depend entirely on how well you position the LEDs. At first you may think one lead of the
LEDs is not connected to the circuit. But this is where we have had to improvise. Multiplexing requires one Ah line of conductors to travel north-south and the matching line to travel east-west. This would normally require a double-sided PC board, but since they are very expensive and ft difficult to solder, we have opted for the cheaper approach. cont...

![](https://github.com/SteveJustin1963/tec-8x8/blob/master/readme-img/wir.png)

## DIP HEADER
A DIP HEADER is a plug which has thin pins similar to the pins on an IC, on the underside. On the top are cup shape (or 'Y' shape) terminals to which you can make a solder connection. Leads can be soldered to the terminals to create a low-cost adapter. It is suggested that heat-shrink tubing be placed over each lead before soldering to the Dip Plug. When all the leads are attached, the sleeve is slid over each terminal so that the conductor is strengthened. This will prevent fine whiskers of wire shorting from one pin to the other and creating havoc.

## MATRIX PINS & SOCKETS
These are the cheapest and best way of connecting a single line to a printed circuit board. 

cont...
Solder the 64 LEDs into position as well as the 8 transistors and their 1 k base resistors. The east-west conductors are created with tinned copper wire running along the ends of the LED leads and this connects to the collector of the driver transistor via the PC circuit. The only lead which has to be cut short is the anode lead, to prevent it touching the tinned copper wire.  

![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/led-side.png) 

Diagram showing how the 'COMMON' line is created on the underside of the board. Both leads of each LED are soldered to the PC board. But only the ANODE lead is cut short. The CATHODE leads are either joined with a length of tinned copper wire which runs below the board, or each lead is bent over and soldered to the next lead to produce a rigid conductor which runs at right angles to the copper tracks on the board.

The lower part of the board contains a BUS from which the 8 lines for each chip are taken. The code letter P on this BUS stands for positive and the N stands for negative. The other lines are numbered 0 to 7 and this coincides with the data lines on the latches. The two output latches can be: 74LS347, or 74LS377 or 74LS273 or a combination of any two. The information on the overlay shows which jumper link must be included for the type of latch you choose. Eighteen jumper links connect between the data bus and the latches to complete the assembly. The only wiring left is the connecting wires between the DIP HEADER plug and the PC board. This plug is designed to fit into the expansion port socket on the TEC-1. The 10 lines from the bus on the display board connect to the DIP plug and the two spare lines connect to the chip select outputs near the 74LS138 (near the keyboard encoder). These lines are for ports 3 and 4. Solder two matrix pins to these output holes and use a matrix-pin connector soldered to the hook-up wire to connect to these pins.  

The 8x8 display is now ready for testing and we will give 3 simple programs to test the operation of the LEDs. This will check their illumination, their OFF response and the correct wiring of the data lines and chip select lines. To check the brightness of the LEDs, insert this program at 800:

`3E FF`

`D3 03`

`3E FF`

`D3 04`

`76`

Reset
Go

Replace any dull LEDs. 

To make sure all the LEDs are extinguished when they are not being accessed, change the program above to:  

`3E 00`

`D3 03`

`3E 00`

`D3 04`

`76 `

To check the data and chip select lines, insert this program: 

`3E 04`

`D3 03`

`3E 02`

`D3 04`

`76`

Reset
GO 

![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/one.png) 

The LED which will illuminate is shown in the diagram. If any other LED illuminates, check the select lines. Now go back to experimenting with the display on the TEC-1. When you get to p.30, you will be able to use the 64 LED display to create some startling effects.

![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/pcb.png) 

The possibilities and effects on a MATRIX layout are infinite. We will allocate the next few pages to showing some interesting visual effects. Firstly we will show how each of the LEDs is accessed. As with any matrixing system, each location has a set of co-ordinates. If we compare our display with the x and y axes in geometry, we find the axis has the lower output port number and the y-axis the higher number. The output ports allocated to this display are 3 and 4 and this is determined by the chip access lines on the main board. Each line from the 74LS138 has a particular number and we have selected lines 3 and 4.  On the display board, each of the LEDs has a particular co-ordinate value which must be in the form of a Hex number. Each successive row or column has a hex number which is DOUBLE the previous number. The following diagram shows this: The lowest priority LED has the value 01, 01 and the highest LED 80, 80. The value of each LED between these limits is also given, as well as the value for 4 individual LEDs, as a guide. Placing these hex values into a simple program will illuminate any particular LED on the screen. Here is the general program: 

![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/gen.png)

##  IF THE 8x8 MATRIX DOESN'T WORK 
We described the construction of the 8x8 matrix and presented 3 short programs to test the LEDs in the display. Hopefully you will have put the project together by now and will be ready to explore its capabilities. The main difference between this project and the display on the TEC-1 is not so much the number of LEDs, but the way in which they are arranged. We have created a regular matrix of 8 LEDs by 8 LEDs and this produces a screen very similar to a window on a video display. The most common fault will be one or two of the LEDs failing to illuminate when the whole screen is accessed. If this is the case, or if one is dull, the fault will be a damaged LED. LEDs  are temperature sensitive. and excess heat when soldering will damage them. On the other hand, it may be a poor quality LED in the batch. If any of the LEDs are particularly dull, they should be replaced at this stage to produce a good display. Here are some of the possible faults and their remedies: If a row or column fails to light, the fault will be in one of the output lines of a latch or one of the driver transistors. Make sure it is not a dry joint or a missing link and then check the orientation of the transistors and the LEDs. If a row and column is failing to illuminate, the fault will lie in a shorted LED at the intersection. Remove the LED and turn on the remainder of the screen. If the remainder of the LEDs come on, the fault is a short. The only other fault we have seen is one row glowing brighter than the rest. This can be due to one of the transistors shorting between collector and emitter. A short to base may cause the row to be extinguished. If all these suggestions fail to locate the fault, turn the TEC-1 off and reprogram the set of instructions. Check to see that you have loaded FF into both port 3 and port 4. Check both ends of the connecting leads and make sure they are connected correctly to the pins on the dip plug. Since the expansion port socket is effectively in parallel with the other memory chips, it is very unlikely the the PC tracks will have shorts between them. This means you should look mainly on the display board itself. 

## Code

![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/p4p3.png)

Diag 1: The ports and their Hex values.

If we take a particular case and load the co-ordinates 04, 02 into the program: 

`3E 04`

`D3 03`

`3E 02`

`D3 04`

`76 `

As you type the program, this is what you should be saying: Load the accumulator with 4, output it to port 3. Load the accumulator with 2 and output it to port 4. Halt.

## Problems:
Illuminate 3 of the other LEDs by inserting the following data into the program: 

`1: 04,40`

`2: 20,08`

`3: 80,80.`

## TWO OR MORE LEDs

More than one LED can be illuminated in any row or column by adding the Hex value of each LED. We will start with the simplest case but absolutely any LEDs in any row or column can be illuminated.  

![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/diag2.png)

In diagram 2, two LEDs are shown illuminated. These have co-ordinates 01,01 and 01,02. To turn on both of these LEDs we add the bottom Hex numbers. The result is 03. Place this value into the program at address 801.

![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/diag3.png)

Diagram 3 shows five LEDs illuminated. Add the Hex numbers together and insert it into the program and see if you are correct. Did you get 1 F? 

![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/diag4.png)

The fourth diagram shows ALL the LEDs on the bottom row illuminated. What value must be placed in the program at 801 to access these LEDs? 

The answer if FF. This is obtained by adding 01, 02, 04, 08   10, 20, 40, 80. this gives: 0F + F0 = FF

## Problem:
Load the program with a hex value which will illuminate the four LEDs in the centre of the bottom row: 

![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/diag5.png)

Firstly look up which values are allocated to each LED then add these values. Place this into the program and observe the result. You will be correct with the value 3C. The program for accessing the LEDs  in the 8X8 Marix is identical to that for the display on the 
TEC-1. The only difference is in appearance. A regular array makes the effect more dramatic and the overall possibilities are much
greater.

![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/diag6.png)

To turn on the four centre LEDs we must insert the value 08 + 10 into the program for both outputs.
## Problem:
What value must be inserted into the program to illuminate the four corner LEDs? 

![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/diag7.png)

It is now your turn to illuminate a LED. Select a LED on the matrix and mark it with a pen. Determine its coordinates and put them into the program. Execute the program and see it the marked LED comes on. Try two more of these routines and confirm the program by illuminating the LED. Now illuminate two or three LEDs in any row or column by adding the relevant Hex values together and observe the LEDs on the display. With this simple program it is not possible to illuminate any combination of LEDs on the whole screen because we are using the outputs in the static mode. To illustrate this, try to illuminate one column and one row at the same time. You know the Hex value for a complete row is FF. Place this into the program and see what happens. The result is a completely-filled screen. The closest effect to producing an intersecting row and column is a non-illuminated row and column produced by inserting a value such as EF into the program.  

## PROBLEMS:
Demonstrate your understanding of addressing the matrix display by solving the following:
1. Illuminate the whole screen.
2. Illuminate the whole screen except for the outer row and column of LEDs.
3. Illuminate the four centre LEDs as well as the next row and column on each side.
4. Illuminate any quarter of the display.
5. Leave the two centre rows and columns non-illuminated.
6. Place FF in port 3 and 00 in port 4. What appears on the screen? Why? 

## MAKING A FLASHING LED
We know the general formula for turning on a LED on the matrix: 

`3E (data) ; Single Byte`

`D3 03`

`3E (data)`

`D3 04`

`76`
  
 ![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/flashing-led.png)

To FLASH the LOWEST priority LED we insert data into the program as follows: 

`LD A,01     800 3E 01`

`OUT (3),A   802 D3 03`

`LD A,01     804 3E 01`

`OUT (4),A   806 D3 04`

`CALL DELAY  808 CD 00 0A`

`LD A,00     80B 3E 00`

`OUT (3),A   80D D3 03`

`LD A,00     80F 3E 00`

`OUT (4),A   811 D3 04`

`CALL DELAY  813 CD 00 0A`

`JP 0800     816 C3 00 08`

DELAY ROUTINE AT 0A00:

`11 FF 06`

`1B`

`7B`

`B2`

`C2 03 0A`

`C9 `

Press RESET, GO and the lowest LED will blink ON and OFF. The program is basically loading data into ports 3 and 4 then calling the delay so that the information will be displayed on the screen for a short period of time. The output latches are then loaded with 00 data which will produce a non-illuminated display and the delay routine is called. This produces the 'OFF' period. The program is cycled in an endless loop to produce the flashing. With this program it is easy to flash any number of LEDs or even the whole screen.  

## TO BLINK THE WHOLE SCREEN 
To blink the whole screen, change the data at addresses 801 and 805 to FF. This has the effect of filling the screen for one delay period and then non-illuminating the screen for one delay period. To alternately blink the left-hand side of the screen and then the right-hand side: Insert the following data: 

at address:

`801 insert FF`

`805 insert OF`

`80C insert FF`

`810 insert FO`

You can make the flash move in the up/down motion by programming: 

`801 insert 0F`

`805 insert FF`

`80C insert F0`

`810 insert FF`

An overlap can be created by inserting the following data: 

`801 insert 1F `

`805 insert FF`

`80C insert F8`

`810 insert FF`

You will notice the two centre rows remain ON for the whole period of time as shown by this table: 

![](https://github.com/SteveJustin1963/tec-8x8-TE-11-22/blob/master/readme-img/top-bot.png)

An interlocking effect can be created by programming the following: 

`801 insert AA

805 insert FF

80C insert 55

810 insert FF `

To make a block of 4 LEDs jump diagonally and back again, the following information is inserted into the program: 

`change 801 to 0F

change 805 to 0F

change 80C to F0

change 810 to F0 `

You can experiment with the length of the delay to produce a faster or slower flash rate.

For a slow flash insert: `11 FF 0A`

For a medium flash insert: `11 FF 08`

For a fast flash insert: `11 FF 06`

## TO RUN A SINGLE LED ACROSS THE DISPLAY 

This program will run a single LED across the bottom of the display, from left to right and HALT. 

`LD A,01       800 3E 01

OUT (4),A     802 D3 04

LD C,08       804 0E 08

LD A,01       806 3E 01

OUT (3),A     808 D3 03

LD BA         809 47

CALL DELAY    80A CD 00 0C

LD A,B        80D 78

RLC           80E CB 07

DEC C         810 0D

JP NZ LOOP    812 C2 08 08

HALT          815 76`

To regulate the speed at which the LED crosses the display, we need a delay routine. (Exactly the same as the previous delay routine.) 

Delay routine at 0C00:

`11 FF 06
1B
7B
B2
C2 03 0C
C9`

For a full column to move across the screen, change the data at 801 to FF. To create a REPEAT, change the Halt at 

`815 to C3 00 08`

To make a single LED run around the perimeter of the display, we must create a program for each of the four sides. The program above is suitable for the first side and three more  

![](8x8start)

programs are needed. At location 815 we remove the HALT function (or the return function) and add the following: Press RESET,ADdress 0815, +. Now continue: 

