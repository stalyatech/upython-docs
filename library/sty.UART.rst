.. currentmodule:: sty
.. _sty.UART:

class UART -- duplex serial communication bus
=============================================

UART implements the standard UART/USART duplex serial communications protocol.  At
the physical level it consists of 2 lines: RX and TX.  The unit of communication
is a character (not to be confused with a string character) which can be 8 or 9
bits wide.

UART objects can be created and initialised using::

    from sty import UART

    uart = UART(1, 9600)                         # init with given baudrate
    uart.init(9600, bits=8, parity=None, stop=1) # init with given parameters

Bits can be 7, 8 or 9.  Parity can be None, 0 (even) or 1 (odd).  Stop can be 1 or 2.

*Note:* with parity=None, only 8 and 9 bits are supported.  With parity enabled,
only 7 and 8 bits are supported.

A UART object acts like a `stream` object and reading and writing is done
using the standard stream methods::

    uart.read(10)       # read 10 characters, returns a bytes object
    uart.read()         # read all available characters
    uart.readline()     # read a line
    uart.readinto(buf)  # read and store into the given buffer
    uart.write('abc')   # write the 3 characters

Individual characters can be read/written using::

    uart.readchar()     # read 1 character and returns it as an integer
    uart.writechar(42)  # write 1 character

To check if there is anything to be read, use::

    uart.any()          # returns the number of characters waiting


*Note:* The stream functions ``read``, ``write``, etc. are new in MicroPython v1.3.4.
Earlier versions use ``uart.send`` and ``uart.recv``.

Constructors
------------

.. class:: sty.UART(bus, ...)

   Construct a UART object on the given bus.
   For simpleRTK ``bus`` can be 'ZED1', 'ZED2', 'ZED3', 'GSM',
   'SRV', 'XBEE_LP' or 'XBEE_HP'.

     - ``ZED1`` is the on board high precision GPS1 bus.
     - ``ZED2`` is the on board high precision GPS2 bus.
     - ``ZED3`` is the on board high precision GPS3 bus.
     - ``GSM`` is the on board 4G LTE modem bus.
     - ``SRV`` is the external service bus.
     - ``XBEE_LP`` is the on board low power XBee module bus.
     - ``XBEE_HP`` is the on board high power XBee module bus.

   With no additional parameters, the UART object is created but not
   initialised (it has the settings from the last initialisation of
   the bus, if any).  If extra arguments are given, the bus is initialised.
   See ``init`` for parameters of initialisation.

Methods
-------

.. method:: UART.init(baudrate, bits=8, parity=None, stop=1, *, timeout=0, flow=0, timeout_char=0, rxbuf=0, callback=None, dma=True)

   Initialise the UART bus with the given parameters:

     - ``baudrate`` is the clock rate.
     - ``bits`` is the number of bits per character, 7, 8 or 9.
     - ``parity`` is the parity, ``None``, 0 (even) or 1 (odd).
     - ``stop`` is the number of stop bits, 1 or 2.
     - ``flow`` sets the flow control type. Can be 0, ``UART.RTS``, ``UART.CTS``
       or ``UART.RTS | UART.CTS``.
     - ``timeout`` is the timeout in milliseconds to wait for writing/reading the first character.
     - ``timeout_char`` is the timeout in milliseconds to wait between characters while writing or reading.
     - ``rxbuf`` is the character length of the read buffer (0 to disable).
     - ``callback`` is the function that will call on data received.
     - ``dma`` is the mode that will be use on transmit and receive process (False to disable).

   This method will raise an exception if the baudrate could not be set within
   5% of the desired value.  The minimum baudrate is dictated by the frequency
   of the bus that the UART is on; UART(1) and UART(6) are APB2, the rest are on
   APB1.  The default bus frequencies give a minimum baudrate of 1300 for
   UART(1) and UART(6) and 650 for the others.  Use :func:`sty.freq <sty.freq>`
   to reduce the bus frequencies to get lower baudrates.

   *Note:* with parity=None, only 8 and 9 bits are supported.  With parity enabled,
   only 7 and 8 bits are supported.

.. method:: UART.deinit()

   Turn off the UART bus.

.. method:: UART.any()

   Returns the number of bytes waiting (may be 0).

.. method:: UART.read([nbytes])

   Read characters.  If ``nbytes`` is specified then read at most that many bytes.
   If ``nbytes`` are available in the buffer, returns immediately, otherwise returns
   when sufficient characters arrive or the timeout elapses.

   If ``nbytes`` is not given then the method reads as much data as possible.  It
   returns after the timeout has elapsed.

   *Note:* for 9 bit characters each character takes two bytes, ``nbytes`` must
   be even, and the number of characters is ``nbytes/2``.

   Return value: a bytes object containing the bytes read in.  Returns ``None``
   on timeout.

.. method:: UART.readchar()

   Receive a single character on the bus.

   Return value: The character read, as an integer.  Returns -1 on timeout.

.. method:: UART.readinto(buf[, nbytes])

   Read bytes into the ``buf``.  If ``nbytes`` is specified then read at most
   that many bytes.  Otherwise, read at most ``len(buf)`` bytes.

   Return value: number of bytes read and stored into ``buf`` or ``None`` on
   timeout.

.. method:: UART.readline()

   Read a line, ending in a newline character. If such a line exists, return is
   immediate. If the timeout elapses, all available data is returned regardless
   of whether a newline exists.

   Return value: the line read or ``None`` on timeout if no data is available.

.. method:: UART.write(buf)

   Write the buffer of bytes to the bus.  If characters are 7 or 8 bits wide
   then each byte is one character.  If characters are 9 bits wide then two
   bytes are used for each character (little endian), and ``buf`` must contain
   an even number of bytes.

   Return value: number of bytes written. If a timeout occurs and no bytes
   were written returns ``None``.

.. method:: UART.writechar(char)

   Write a single character on the bus.  ``char`` is an integer to write.
   Return value: ``None``. See note below if CTS flow control is used.

.. method:: UART.sendbreak()

   Send a break condition on the bus.  This drives the bus low for a duration
   of 13 bits.
   Return value: ``None``.

.. method:: UART.send(buf)

   Write the buffer/string of bytes/char to the bus.  If characters are 7 or 8 bits wide
   then each byte is one character.  If characters are 9 bits wide then two
   bytes are used for each character (little endian), and ``buf`` must contain
   an even number of bytes. If DMA is enabled it uses the DMA engine to transfer data.

.. method:: UART.istxbusy()

   Returns the on going transfer status.

.. method:: UART.callback(fun)

   Register a function to be called when any data is received:

   - ``fun`` is the function to be called when the buffer becomes non empty.

   The callback function takes two arguments the first is the uart object it self the second is
   a byte array that contains the received data.

   Data receive sample with callback::

      from sty import UART

      def OnDataRecvFromZED1(uart, msg):
         print(msg)

      zed1 = UART('ZED1', 115200, rxbuf=0)
      zed1.callback(OnDataRecvFromZED1)

.. method:: UART.parser(type, rxbuf=256, rxcallback=None, frcallback=None)

   Initialise the UART bus framer/parser with the given parameters:

     - ``type`` is the protocol type. Can be ``UART.ParserNONE``, ``UART.ParserNMEA``,
       ``UART.ParserUBX`` or ``UART.ParserRTCM``.
     - ``rxbuf`` is the character length of the protocol buffer (0 to disable).
     - ``rxcallback`` is the function that will call on protocol message received.
     - ``frcallback`` is the function that will call on protocol message parsed.

.. method:: UART.process(type)

   Process queued protocol message. ``type`` is the protocol type. The method returns 
   the number of bytes message. If there is no valid message that belong to the type
   it will return with (-1).

.. method:: UART.parse_nmea(buf)

   Start parsing procedure of buffer that contains NMEA message. On success the function that
   has been registered with ``frcallback`` parameter will be called.

   Process NMEA messages on GPS::

      from sty import UART

      def OnNmeaMsg(uart, msg):
         uart.parse_nmea(msg)

      def OnNmeaParsed(type, items):
         print(type)
         print(items)

      zed1 = UART('ZED1', 115200, rxbuf=0, dma=False)
      zed1.parser(UART.ParserNMEA, rxbuf=256, rxcallback=OnNmeaMsg,  frcallback=OnNmeaParsed)

      while True:
         zed1.process(UART.ParserNMEA)

.. method:: UART.parse_ubx(buf)

   Start parsing procedure of buffer that contains UBLOX message. On success the function that
   has been registered with ``frcallback`` parameter will be called.

   Process UBX messages on GPS::

      from sty import UART

      def OnUbloxMsg(uart, msg):
         uart.parse_ubx(msg)

      def OnUbloxParsed(type, items):
         print(type)
         print(items)

      zed1 = UART('ZED1', 115200, rxbuf=0, dma=False)
      zed1.parser(UART.ParserUBX, rxbuf=256, rxcallback=OnUbloxMsg, frcallback=OnUbloxParsed)

      while True:
         zed1.process(UART.ParserUBX)

.. method:: UART.connect(obj1=None, obj2=None, obj3=None, obj4=None)

   Redirects the selected UART buses to the caller UART bus:

     - ``obj1`` is the 1st uart object.
     - ``obj2`` is the 2nd uart object.
     - ``obj3`` is the 3rd uart object.
     - ``obj4`` is the 4th uart object.
   
   Redirect the XBee low power module messages to the all GPS::

      from sty import UART

      zed1 = UART('ZED1', 115200, rxbuf=0, dma=True)     # GPS1 bus
      zed2 = UART('ZED2', 115200, rxbuf=0, dma=True)     # GPS2 bus
      zed3 = UART('ZED3', 115200, rxbuf=0, dma=True)     # GPS3 bus
      xblp = UART('XBEE_LP', 115200, rxbuf=0, dma=False) # XBee low power bus
      xblp.connect(obj1=zed1, obj2=zed2, obj3=zed3)      # Redirect all data from xbee lp to all zeds

Constants
---------

.. data:: UART.RTS
          UART.CTS

   to select the flow control type.

.. data:: UART.IRQ_RXIDLE

   to select the receive interrupt type.

.. data:: UART.ParserNONE
          UART.ParserNMEA
          UART.ParserUBX
          UART.ParserRTCM

   to select the special protocol parser type.

Flow Control
------------

On simpleRTK-SBC V1.0 ``UART(7)`` support RTS/CTS hardware flow control
using the following pins:

   - ``UART(7)`` is on: ``(TX, RX, nRTS, nCTS) = (GSM_TXD, GSM_RXD, GSM_RTS, GSM_CTS) = (PE8, PE7, PE9, PE10)``

In the following paragraphs the term "target" refers to the device connected to
the UART.

When the UART's ``init()`` method is called with ``flow`` set to one or both of
``UART.RTS`` and ``UART.CTS`` the relevant flow control pins are configured.
``nRTS`` is an active low output, ``nCTS`` is an active low input with pullup
enabled. To achieve flow control the simpleRTK's ``nCTS`` signal should be connected
to the target's ``nRTS`` and the simpleRTK's ``nRTS`` to the target's ``nCTS``.

CTS: target controls simpleRTK transmitter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If CTS flow control is enabled the write behaviour is as follows:

If the simpleRTK's ``UART.write(buf)`` method is called, transmission will stall for
any periods when ``nCTS`` is ``False``. This will result in a timeout if the entire
buffer was not transmitted in the timeout period. The method returns the number of
bytes written, enabling the user to write the remainder of the data if required. In
the event of a timeout, a character will remain in the UART pending ``nCTS``. The
number of bytes composing this character will be included in the return value.

If ``UART.writechar()`` is called when ``nCTS`` is ``False`` the method will time
out unless the target asserts ``nCTS`` in time. If it times out ``OSError 116``
will be raised. The character will be transmitted as soon as the target asserts ``nCTS``.

RTS: simpleRTK controls target's transmitter
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If RTS flow control is enabled, behaviour is as follows:

If buffered input is used (``read_buf_len`` > 0), incoming characters are buffered.
If the buffer becomes full, the next character to arrive will cause ``nRTS`` to go
``False``: the target should cease transmission. ``nRTS`` will go ``True`` when
characters are read from the buffer.

Note that the ``any()`` method returns the number of bytes in the buffer. Assume a
buffer length of ``N`` bytes. If the buffer becomes full, and another character arrives,
``nRTS`` will be set False, and ``any()`` will return the count ``N``. When
characters are read the additional character will be placed in the buffer and will
be included in the result of a subsequent ``any()`` call.

If buffered input is not used (``read_buf_len`` == 0) the arrival of a character will
cause ``nRTS`` to go ``False`` until the character is read.
