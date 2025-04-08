# 6502_Tony
*Description of development and pitfalls of 6502 computer project following Ben Eater*

This describes a project to build a computer similar to  an Apple I/II based on the 65c02 chip and following the videos of Ben Eater (https://eater.net/6502). My project developed past the original Ben version, this happening in a sequential process. While this was a great learning process it resulted in mistakes being made. These could all be corrected but could have been avoided had the process been thought through to conclusion at the beginning. 

**Credits**

- I have to credit Ben Eater, for the basic project and inspiration, Martin Mienczakowski for the TFT based game idea, and Rich Hintz for his great work on sound.
- Throughout the project I received fantastic debugging help from many at b/eater, too many to mention all. However I should mention the following regular contributors:
    - NormalLuser
    - kiss-my-what
    - The8BitEnthusiast
    - LiqvidNyquist
    - SomePeopleCallMeJJ
    - production-dave

**Project Scope**

What was added to Basic Ben was:  
- Installing the LCD display vertically for improved reading. This required the design of a PCB to hold the LCD and the pin connections.  
- Substituting a TFT display for the LCD. However the same pin connectors could be used to swap in the TFT display.  
- The TFT screen enabled the running of games. I used the code supplied by Martin Mienczakowski  (https://github.com/martinmienczakowski/6502TFTScreen), Catch Clemo. The code was impressive, but I  did manage with time to understand enough to be able to make some modifications.  
- With a game running the need for sound became obvious. I then developed a sound card, which went through several stages. I received a lot of support from Rich on programming the AY-3-8910. His old website contains most of his work (https://rehsd1.wixsite.com/my-site/blog). I also made modifications to how the code was set up as I understood the workings better with time.

   
**The Basic 6502**  

<img src="https://github.com/user-attachments/assets/6b0e4e2a-882f-4b98-b962-2dac7901bc73" alt="Alt-Text" width="300" height="200">

The image shows the starting point in breadboard form with the Arduino 2560 in place for de-bugging. I also used an Arduino Nano to provide the variable clock  instead of Ben´s clock as I had endless problems using this design in the 8 bit project and the Arduino allowed very accurate clock speeds. A bit of guilt about not sticking to the 1980´s technology was overcome by the fact that this was just a debugging and learning tool and played no part in the functioning of the final version. 
- A note on both devices: You need to look closely at how Ben wires the 2560 and the corresponding array in the Arduino sketch, otherwise you can spend hours trying to debug a non-existent problem. Also I found the Nano or Ben´s slow clock to not be very useful. It is too fast (even at low speeds) to see what is happening in the monitoring program, and too slow to to see the effect of a change to your ROM code.
- A comment on breadboards: I later used Ben´s recommended boards but no matter how good and expensive, they are not reliable pieces of equipment. They are necessary to see whether your design (or a copy of someone else´s) works or not but if it works a few times go as soon as possible to a PCB and don´t waste time trying to get repeatable results.
- Scrambled wiring (like mine) adds to the problem of debugging. Bad contact, wrong connection, faulty component? It also makes it difficult to extract and exchange suspect components.

**6502 PCB**

<img src="https://github.com/user-attachments/assets/adf35fd1-1185-452a-bac7-a89626010274" alt="Alt-Text" width="300" height="200">

I had limited experience with PCB´s before making this one. A few takeaways may be appropriate.
- This board is quite large measuring 320x220 mm. It is also 4 layers, this combination making it expensive. I attribute this to adding layers when running into problems connecting components (can´t cross in the same layer) and only later learned how to use via´s to avoid this problem. In addition I did not exploit the minimum distances allowed by the software (Kicad)  which would give a more compact boaard.
- I should have given more thought to the positioning of components. As an example, by the time I had reached the stage of ordering the PCB, it had become clear that I wanted  to replace the LCD with a TFT display to be able to run games as mentioned above. Looking at the hardware requirements I realised that I could use the same pin connector (bottom left) as I had already designed for the LCD display and just map the PCB for the TFT display accordingly. I therefore used the PCB layout already designed for the LCD. This was a mistake as the the TFT screen was then right in front of the pushbuttons used to control the game which is not ideal.
- There was enough space on the board to add connections for the sound card which had become desirable because of running the game. This can be seen on the right.

**Testing**

I designed and ordered the PCB´s for the LCD display and TFT display shortly after the main board and was soon able to test the basic 6502 as well as the TFT enhancement. A few comments:
- I was delighted that the PCB functioned as designed without the quirks experienced with the breadboard version. I struggled to get the RS-232 interface to work and eventually found that one of the capacitors forming part of the MAX 232 circuitry was grounded to an island. I was able to correct it underneath the board. The point of this discussion is to remind the reader to examine the PCB layout in KICAD caarefully for this. They are not easy to spot. I also struggled with the terminal programs, trying several. The one that worked most reliably for me (and easiest to understand) was TeraTerm.
- The pin connections for the displays worked but using pin connections proved to be inadequate from a stability point of view. I found a way to stabilise them but was not satified with this arrangement. I decided to go for edge connectors. I eventually found suitable ones from Samtec and as these are not run of the mill (single row, 2.54 pitch) it took several weeks before they were available. Not having had success removing multi-pin IC´s from a PCB I had to start all over, the extra boards resulting from the PCB manufacturer´s minimum order policy coming in handy.

**Sound**

I followed the very useful project by Rich to produce sound using the AY-3-8910 PSG. The documentation for this device is sketchy and hard to understand but Rich´s coding and videos helped me to understand how it worked. I exclude the use of noise which I never got a grip on. I connected the sound card prototype (breadboard) to the main board and finally got it to produce sound programmed on the EEPROM of the main board. I finally realised that it would not be possible to run sound at the same time as playing the game using the 6502  and EEPROM of the main board, unlike the early Commodore 64 and others. My understanding is that they used the time of VBlank in setting up each scene on the monitor to output sound and this screen refresh does not exist with TFT.
I then modified the setup so that the total project looked like this (not all components listed):
- Main Board
   - 6502M to run the game program
   - EEPROM to contain the game code
   - VIA1 to receive input from the pushbuttons, and trigger interrupts on 6502M
- Sound Card
   - 6502S to run the music program
   - EEPROM to contaain the music code
   - VIA2 to receive data from the game code which would be used to select the track to be played by 6502S
   - VIA3 to operate in handshake mode with VIA2. It would read the track information on VIA2 and trigger an interrupt on 6502S
   - VIA4 would output the code to the AY-3-8910 which would generate the sound     
