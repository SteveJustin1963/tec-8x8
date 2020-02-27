# tec-8x8-TE-11-22

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
Before starting any of the construction, it is absolutely essential that you know which lead of the light emitting diode is the cathode. There is only one guaranteed way of determining this. You need a 3v to 6v battery and a 100 ohm or 220ohm resistor. Place the LED and resistor in a series circuit connected to the battery and check the degee of illumination. The cathode lead will be the one nearest the negative terminal of the battery. There are no other sure-fire methods of determining this as some LEDs have their long lead cut differently to the accepted practice. We have seen some LEDs with the outline (inside the LED) around the opposite way to the general rule. So, it can be quite confusing. You must test each LED or at least a sample from the batch. Next you must be certain which way they are to be inserted in the PC board. A mistake will take a very long time to rectify. The cathode lead is nearest to the row of transistors. When soldering the LEDs to the board, you must take special care to keep them all the same height and perpendicular to the board. The neatness of the dispay will depend entirely on how well you position the LEDs. At first you may think one lead of the
LEDs is not connected to the circuit. But this is where we have had to improvise. Multiplexing requires one Ah line of conductors to travel north-south and the matching line to travel east-west. This would normally require a double-sided PC board, but since they are very expensive and ft difficult to solder, we have opted for the cheaper approach. cont...

![](https://github.com/SteveJustin1963/tec-8x8/blob/master/readme-img/wir.png)

## DIP HEADER
A DIP HEADER is a plug which has thin pins similar to the pins on an IC, on the underside. On the top are cupshape (or 'Y' shape) terminals to which you can make a solder connection. Leads can be soldered to the terminals to create a low-cost adaptor. It is suggested that heat-shrink tubing be placed over each lead before soldering to the Dip Plug. When all the leads are attached, the sleeving is slid over each terminal so that the conductor is strengthened. This will prevent fine wiskers of wire shorting from one pin to the other and creating havoc.

## MATRIX PINS & SOCKETS
These are the cheapest and best way of connecting a single line to a printed circuit board. 

cont...
Solder the 64 LEDs into position as well as the 8 transistors and their 1 k base resistors. The east-west conductors are created with tinned copper wire running along the ends of the LED leads and this connects to the collector of the driver transitor via the PC circuit. The only lead which has to be cut short is the anode lead, to prevent it touching the tinned copper wire.  

![](https://github.com/SteveJustin1963/tec-8x8/blob/master/led-side.png) 

Diagram showing how the 'COMMON' line is created on the underside of the board. Both leads of each LED are soldered to the PC board. But only the ANODE lead is cut short. The CATHODE leads are either joined with a length of tinned copper wire which runs below the board, or each lead is bent over and soldered to the next lead to produce a rigid conductor which runs at rightangles to the copper tracks on the board.

The lower part of the board contains a BUS from which the 8 lines for each chip are taken. The code letter P on this BUS stands for positive and the N stands for negative. The other lines are numbered 0 to 7 and this coincides with the data lines on the latches. The two output latches can be: 74LS347, or 74LS377 or 74LS273 or a combination of any two. The information on the overlay shows which jumper link must be included for the type of latch you choose. Eighteen jumper links connect between the data bus and the latches to complete the assembly. The only wiring left is the connecting wires between the DIP HEADER plug and the PC board. This plug is designed to fit into the expansion port socket on the TEC-1. The 10 lines from the bus on the display board connect to the DIP plug and the two spare lines connect to the chip select outputs near the 74LS138 (near the keyboard encoder). These lines are for ports 3 and 4. Solder two matrix pins to these output holes and use a matrix-pin connector soldered to the hook-up wire to connect to these pins.  

The 8x8 display is now ready for testing and we will give 3 simple programs to test the operation of the LEDs. This will check their illumination, their OFF response and the correct wiring of the data lines and chip select lines. To check the brightness of the LEDs, insert this program at 800:

3E FF
D3 03
3E FF
D3 04
76
Reset
Go

Replace any dull LEDs. 

To make sure all the LEDs are extinguished when they are not being accessed, change the program above to:  

3E 00
D3 03
3E 00
D3 04
76 

To check the data and chip select lines, insert this program: 

3E 04
D3 03
3E 02
D3 04
76
Reset
GO 

![](https://github.com/SteveJustin1963/tec-8x8/blob/master/led-side.png) 


## Iterate, new hypotheses or predictions

https://github.com/SteveJustin1963/tec-8x8-mod




