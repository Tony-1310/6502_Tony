# 6502_Tony
Description of development and pitfalls of 6502 computer

This describes a project to build a computer similar to  an Apple I/II based on the 65c02 chip and following the videos of Ben Eater (https://eater.net/6502). My project developed past the original Ben version, this happening in a sequential process. While this was a great learning process it resulted in mistakes being made. These could all be corrected but could have been avoided had the process been thought through to conclusion at the beginning.  

What was added to Basic Ben was:  
- Installing the LCD display vertically for improved reading. This required the design of a PCB to hold the LCD and the pin connections.  
- Substituting a TFT display for the LCD. However the same pin connectors could be used to swap in the LCD display.  
- The TFT screen enabled the running of games. I used the code supplied by Martin Mienczakowski  (https://github.com/martinmienczakowski/6502TFTScreen), Catch Clemo. The code was way beyond my capabilities, but I  did manage to understand enough to be able to make some modifications to the layout.  
- With a game running the need for sound became obvious. I then developed a sound card, which went through several stages. I received a lot of support from Rich on programming the AY-3-8910. His old website     contains most of his work (https://rehsd1.wixsite.com/my-site/blog).  
   
**The Basic 6502**    
