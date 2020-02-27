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
LEDs is not connected to the circuit. But this is where we have had to improvise. Multiplexing requires one Ah line of conductors to travel north-south and the matching line to travel east-west. This would normally require a double-sided PC board, but since they are very expensive and ft difficult to solder, we have opted for the cheaper approach. 

![](https://github.com/SteveJustin1963/tec-8x8/blob/master/readme-img/wir.png)
 

==============

## Iterate, new hypotheses or predictions

https://github.com/SteveJustin1963/tec-8x8-mod




