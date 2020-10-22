.. currentmodule:: sty
.. _sty.ExtInt:

class ExtInt -- configure I/O pins to interrupt on external events
==================================================================

There are a total of 22 interrupt lines. 16 of these can come from GPIO pins
and the remaining 6 are from internal sources.

For lines 0 through 15, a given line can map to the corresponding line from an
arbitrary port. So line 0 can map to Px0 where x is A, B, C, ... and
line 1 can map to Px1 where x is A, B, C, ... ::

    def callback(line):
        print("line =", line)

Note: ExtInt will automatically configure the gpio line as an input. ::

    extint = sty.ExtInt(pin, sty.ExtInt.IRQ_FALLING, sty.Pin.PULL_UP, callback)

Now every time a falling edge is seen on the X1 pin, the callback will be
called. Caution: mechanical pushbuttons have "bounce" and pushing or
releasing a switch will often generate multiple edges.
See: http://www.eng.utah.edu/~cs5780/debouncing.pdf for a detailed
explanation, along with various techniques for debouncing.

Trying to register 2 callbacks onto the same pin will throw an exception.

If pin is passed as an integer, then it is assumed to map to one of the
internal interrupt sources, and must be in the range 16 through 22.

All other pin objects go through the pin mapper to come up with one of the
gpio pins. ::

    extint = sty.ExtInt(pin, mode, pull, callback)

Valid modes are sty.ExtInt.IRQ_RISING, sty.ExtInt.IRQ_FALLING,
sty.ExtInt.IRQ_RISING_FALLING, sty.ExtInt.EVT_RISING,
sty.ExtInt.EVT_FALLING, and sty.ExtInt.EVT_RISING_FALLING.

Only the IRQ_xxx modes have been tested. The EVT_xxx modes have
something to do with sleep mode and the WFE instruction.

Valid pull values are sty.Pin.PULL_UP, sty.Pin.PULL_DOWN, sty.Pin.PULL_NONE.

There is also a C API, so that drivers which require EXTI interrupt lines
can also use this code. See extint.h for the available functions and
usrsw.h for an example of using this.


Constructors
------------

.. class:: sty.ExtInt(pin, mode, pull, callback)

   Create an ExtInt object:

     - ``pin`` is the pin on which to enable the interrupt (can be a pin object or any valid pin name).
     - ``mode`` can be one of:
       - ``ExtInt.IRQ_RISING`` - trigger on a rising edge;
       - ``ExtInt.IRQ_FALLING`` - trigger on a falling edge;
       - ``ExtInt.IRQ_RISING_FALLING`` - trigger on a rising or falling edge.
     - ``pull`` can be one of:
       - ``sty.Pin.PULL_NONE`` - no pull up or down resistors;
       - ``sty.Pin.PULL_UP`` - enable the pull-up resistor;
       - ``sty.Pin.PULL_DOWN`` - enable the pull-down resistor.
     - ``callback`` is the function to call when the interrupt triggers.  The
       callback function must accept exactly 1 argument, which is the line that
       triggered the interrupt.


Class methods
-------------

.. classmethod:: ExtInt.regs()

   Dump the values of the EXTI registers.


Methods
-------

.. method:: ExtInt.disable()

   Disable the interrupt associated with the ExtInt object.
   This could be useful for debouncing.

.. method:: ExtInt.enable()

   Enable a disabled interrupt.

.. method:: ExtInt.line()

   Return the line number that the pin is mapped to.

.. method:: ExtInt.swint()

   Trigger the callback from software.


Constants
---------

.. data:: ExtInt.IRQ_FALLING

   interrupt on a falling edge

.. data:: ExtInt.IRQ_RISING

   interrupt on a rising edge

.. data:: ExtInt.IRQ_RISING_FALLING

   interrupt on a rising or falling edge
