.. currentmodule:: machine
.. _machine.SDCard:

class SDCard -- secure digital memory card
==========================================

SD cards are one of the most common small form factor removable storage media.
SD cards come in a variety of sizes and physical form factors. MMC cards are
similar removable storage devices while eMMC devices are electrically similar
storage devices designed to be embedded into other systems. All three form
share a common protocol for communication with their host system and high-level
support looks the same for them all. As such in MicroPython they are implemented
in a single class called :class:`machine.SDCard` .

Both SD and MMC interfaces support being accessed with a variety of bus widths.
When being accessed with a 1-bit wide interface they can be accessed using the
SPI protocol. Different MicroPython hardware platforms support different widths
and pin configurations but for most platforms there is a standard configuration
for any given hardware. In general constructing an ``SDCard`` object with without
passing any parameters will initialise the interface to the default card slot
for the current hardware. The arguments listed below represent the common
arguments that might need to be set in order to use either a non-standard slot
or a non-standard pin assignment. The exact subset of arguments supported will
vary from platform to platform.

.. class:: SDCard(slot=1, width=1, cd=None, wp=None, sck=None, miso=None, mosi=None, cs=None, freq=20000000)

    This class provides access to SD or MMC storage cards using either
    a dedicated SD/MMC interface hardware or through an SPI channel.
    The class implements the block protocol defined by :class:`uos.AbstractBlockDev`.
    This allows the mounting of an SD card to be as simple as::

      uos.mount(machine.SDCard(), "/sd")

    The constructor takes the following parameters:

     - *slot* selects which of the available interfaces to use. Leaving this
       unset will select the default interface.

     - *width* selects the bus width for the SD/MMC interface.

     - *cd* can be used to specify a card-detect pin.

     - *wp* can be used to specify a write-protect pin.

     - *sck* can be used to specify an SPI clock pin.

     - *miso* can be used to specify an SPI miso pin.

     - *mosi* can be used to specify an SPI mosi pin.

     - *cs* can be used to specify an SPI chip select pin.
     
     - *freq* selects the SD/MMC interface frequency in Hz (only supported on the ESP32).

Implementation-specific details
-------------------------------

Different implementations of the ``SDCard`` class on different hardware support
varying subsets of the options above.

simpleRTK
`````````

The simpleRTK-SBC has just one slot. No arguments are necessary or supported.

