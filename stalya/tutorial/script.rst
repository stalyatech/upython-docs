Running your first script
=========================

Let's jump right in and get a Python script running on the stalya boards. 
After all, that's what it's all about!

Connecting your stalya boards
-----------------------------

Connect your stalya board to your PC (Windows, Mac or Linux) with a USB Type-C cable.
There is only one way that the cable will connect, so you can't get it wrong.

.. image:: img/sbc_usb_micro.jpg

When the stalya board is connected to your PC it will power on and enter the start up
process (the boot process).  The blue LED should light up for half a second or
less, and when it turns off it means the boot process has completed.

Opening the stalya boards USB drive
-----------------------------------

Your PC should now recognise the stalya board. It depends on the type of PC you
have as to what happens next:

  - **Windows**: Your stalya board will appear as a removable USB flash drive.
    Windows may automatically pop-up a window, or you may need to go there
    using Explorer.

    Windows will also see that the stalya board has a serial device, and it will
    try to automatically configure this device.  If it does, cancel the process.
    We will get the serial device working in the next tutorial.

  - **Mac**: Your stalya board will appear on the desktop as a removable disc.
    It will probably be called ``STYFLASH``.  Click on it to open the stalya folder.

  - **Linux**: Your stalya board will appear as a removable medium.  On Ubuntu
    it will mount automatically and pop-up a window with the stalya folder.
    On other Linux distributions, the stalya board may be mounted automatically,
    or you may need to do it manually.  At a terminal command line, type ``lsblk``
    to see a list of connected drives, and then ``mount /dev/sdb1`` (replace ``sdb1``
    with the appropriate device).  You may need to be root to do this.

Okay, so you should now have the stalya board connected as a USB flash drive, and
a window (or command line) should be showing the files on the stalya drive.

The drive you are looking at is known as ``/flash`` by the stalya board, and should contain
the following 4 files:

* `boot.py <http://micropython.org/resources/fresh-pyboard/boot.py>`_ -- the various configuration options for the stalya.
    It is executed when the stalya board boots up.

* `main.py <http://micropython.org/resources/fresh-pyboard/main.py>`_ -- the Python program to be run.
    It is executed after ``boot.py``.

* `README.txt <http://micropython.org/resources/fresh-pyboard/README.txt>`_ -- basic information about getting started with the stalya.
    This provides pointers for new users and can be safely deleted.

* `pybcdc.inf <http://micropython.org/resources/fresh-pyboard/pybcdc.inf>`_ -- the Windows driver file to configure the serial USB device.
    More about this in the next tutorial.

Editing ``main.py``
-------------------

Now we are going to write our Python program, so open the ``main.py``
file in a text editor.  On Windows you can use notepad, or any other editor.
On Mac and Linux, use your favourite text editor.  With the file open you will
see it contains 1 line::

    # main.py -- put your code here!

This line starts with a # character, which means that it is a *comment*.  Such
lines will not do anything, and are there for you to write notes about your
program.

Let's add 2 lines to this ``main.py`` file, to make it look like this::

    # main.py -- put your code here!
    import sty
    sty.LED(3).on()

The first line we wrote says that we want to use the ``sty`` module.
This module contains all the functions and classes to control the features
of the stalya board.

The second line that we wrote turns the blue LED on: it first gets the ``LED``
class from the ``sty`` module, creates LED number 4 (the blue LED), and then
turns it on.

Resetting the stalya boards
---------------------------

To run this little script, you need to first save and close the ``main.py`` file,
and then eject (or unmount) the stalya board USB drive.  Do this like you would a
normal USB flash drive.

When the drive is safely ejected/unmounted you can get to the fun part:
press the RST switch on the stalya board to reset and run your script. 
The RST switch is the small black button just below the USB connector on the board,
on the right edge.

When you press RST the green LED will flash quickly, and then the blue
LED should turn on and stay on.

Congratulations!  You have written and run your very first MicroPython
program!
