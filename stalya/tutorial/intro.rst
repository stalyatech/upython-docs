Introduction to the stalya boards
=================================

To get the most out of your stalya boards, there are a few basic things to
understand about how it works.

Caring for your stalya boards
-----------------------------

Because the stalya boards does not have a housing it needs a bit of care:

  - Be gentle when plugging/unplugging the USB cable.  Whilst the USB connector
    is soldered through the board and is relatively strong, if it breaks off
    it can be very difficult to fix.

  - Static electricity can shock the components on the stalya and destroy them.
    If you experience a lot of static electricity in your area (eg dry and cold
    climates), take extra care not to shock the stalya.  If your stalya boards 
    came in a black plastic box, then this box is the best way to store and carry 
    the stalya boards as it is an anti-static box (it is made of a conductive plastic, 
    with conductive foam inside).

As long as you take care of the hardware, you should be okay.  It's almost
impossible to break the software on the stalya boards, so feel free to play 
around with writing code as much as you like.  If the filesystem gets corrupt, 
see below on how to reset it.  In the worst case you might need to reflash the
MicroPython software, but that can be done over USB.

Layout of the stalya boards
---------------------------

The Type-C USB connector is on the top middle, the micro SD card slot on
the top left of the board.  There are 3 LEDs between the SD slot and
USB connector.  The colours are blue. 

Plugging in and powering on
---------------------------

The stalya boards can be powered via USB.  Connect it to your PC via a micro USB
cable.  There is only one way that the cable will fit.  Once connected,
the blue LED on the board should flash quickly.

Powering by an external power source
------------------------------------

The stalya can be powered by a battery or other external power source.

**Be sure to connect the positive lead of the power supply to VIN, and
ground to GND.  There is a polarity protection on the stalya boards.**

**The input voltage must be between 6V and 28V.**
