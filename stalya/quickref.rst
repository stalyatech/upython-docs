.. _stalya_quickref:

Quick reference for the stalya
===============================

The below pinout is for PYBv1.1.  You can also view pinouts for
other versions of the stalya:
`PYBv1.0 <http://micropython.org/resources/pybv10-pinout.jpg>`__
or `PYBLITEv1.0-AC <http://micropython.org/resources/pyblitev10ac-pinout.jpg>`__
or `PYBLITEv1.0 <http://micropython.org/resources/pyblitev10-pinout.jpg>`__.

.. only:: not latex

   .. image:: http://micropython.org/resources/pybv11-pinout.jpg
      :alt: PYBv1.1 pinout
      :width: 700px

.. only:: latex

   .. image:: http://micropython.org/resources/pybv11-pinout-800px.jpg
      :alt: PYBv1.1 pinout

Below is a quick reference for the stalya.  If it is your first time working with
this board please consider reading the following sections first:

.. toctree::
   :maxdepth: 1

   general.rst
   tutorial/index.rst

General board control
---------------------

See :mod:`sty`. ::

    import sty

    sty.repl_uart(sty.UART(1, 9600)) # duplicate REPL on UART(1)
    sty.wfi() # pause CPU, waiting for interrupt
    sty.freq() # get CPU and bus frequencies
    sty.freq(60000000) # set CPU freq to 60MHz
    sty.stop() # stop CPU, waiting for external interrupt

Delay and timing
----------------

Use the :mod:`time <utime>` module::

    import time

    time.sleep(1)           # sleep for 1 second
    time.sleep_ms(500)      # sleep for 500 milliseconds
    time.sleep_us(10)       # sleep for 10 microseconds
    start = time.ticks_ms() # get value of millisecond counter
    delta = time.ticks_diff(time.ticks_ms(), start) # compute time difference

Internal LEDs
-------------

See :ref:`sty.LED <sty.LED>`. ::

    from sty import LED

    led = LED(1)
    led.toggle()
    led.on()
    led.off()

    # LEDs 3 and 4 support PWM intensity (0-255)
    LED(4).intensity()    # get intensity
    LED(4).intensity(128) # set intensity to half

Pins and GPIO
-------------

See :ref:`sty.Pin <sty.Pin>`. ::

    from sty import Pin

    p_out = Pin('QP1', Pin.OUT_PP)
    p_out.high()
    p_out.low()

    p_in = Pin('DIN1', Pin.IN, Pin.PULL_UP)
    p_in.value() # get value, 0 or 1

Servo control
-------------

See :ref:`sty.Servo <sty.Servo>`. ::

    from sty import Servo

    s1 = Servo(1) # servo on position 1 (X1, VIN, GND)
    s1.angle(45) # move to 45 degrees
    s1.angle(-60, 1500) # move to -60 degrees in 1500ms
    s1.speed(50) # for continuous rotation servos

External interrupts
-------------------

See :ref:`sty.ExtInt <sty.ExtInt>`. ::

    from sty import Pin, ExtInt

    callback = lambda e: print("intr")
    ext = ExtInt(Pin('DIN2'), ExtInt.IRQ_RISING, Pin.PULL_NONE, callback)

Timers
------

See :ref:`sty.Timer <sty.Timer>`. ::

    from sty import Timer

    tim = Timer(1, freq=1000)
    tim.counter() # get counter value
    tim.freq(0.5) # 0.5 Hz
    tim.callback(lambda t: sty.LED(1).toggle())

RTC (real time clock)
---------------------

See :ref:`sty.RTC <sty.RTC>` ::

    from sty import RTC

    rtc = RTC()
    rtc.datetime((2017, 8, 23, 1, 12, 48, 0, 0)) # set a specific date and time
    rtc.datetime() # get date and time

PWM (pulse width modulation)
----------------------------

See :ref:`sty.Pin <sty.Pin>` and :ref:`sty.Timer <sty.Timer>`. ::

    from sty import Pin, Timer

    p = Pin('QP1') # QP1 has TIM4, CH1
    tim = Timer(4, freq=1000)
    ch = tim.channel(1, Timer.PWM, pin=p)
    ch.pulse_width_percent(50)

ADC (analog to digital conversion)
----------------------------------

See :ref:`sty.Pin <sty.Pin>` and :ref:`sty.ADC <sty.ADC>`. ::

    from sty import Pin, ADC

    adc = ADC(Pin('VAIN1'))
    adc.read() # read value, 0-4095

UART (serial bus)
-----------------

See :ref:`sty.UART <sty.UART>`. ::

    from sty import UART

    uart = UART(1, 9600)
    uart.write('hello')
    uart.read(5) # read up to 5 bytes

I2C bus
-------

Hardware I2C is available on the X and Y halves of the stalya via ``I2C('X')``
and ``I2C('Y')``.  Alternatively pass in the integer identifier of the peripheral,
eg ``I2C(1)``.  Software I2C is also available by explicitly specifying the
``scl`` and ``sda`` pins instead of the bus name.  For more details see
:ref:`machine.I2C <machine.I2C>`. ::

    from machine import I2C

    i2c = I2C('X', freq=400000)                 # create hardware I2c object
    i2c = I2C(scl='X1', sda='X2', freq=100000)  # create software I2C object

    i2c.scan()                          # returns list of slave addresses
    i2c.writeto(0x42, 'hello')          # write 5 bytes to slave with address 0x42
    i2c.readfrom(0x42, 5)               # read 5 bytes from slave

    i2c.readfrom_mem(0x42, 0x10, 2)     # read 2 bytes from slave 0x42, slave memory 0x10
    i2c.writeto_mem(0x42, 0x10, 'xy')   # write 2 bytes to slave 0x42, slave memory 0x10

Note: for legacy I2C support see :ref:`sty.I2C <sty.I2C>`.

CAN bus (controller area network)
---------------------------------

See :ref:`sty.CAN <sty.CAN>`. ::

    from sty import CAN

    can = CAN(1, CAN.LOOPBACK)
    can.setfilter(0, CAN.LIST16, 0, (123, 124, 125, 126))
    can.send('message!', 123)   # send a message with id 123
    can.recv(0)                 # receive message on FIFO 0

Internal accelerometer/gyroscope
--------------------------------

See :ref:`sty.Imu <sty.Imu>`. ::

    from sty import Imu

    imu = Imu()
    print(imu.ax(), imu.ay(), imu.az(), imu.gx(), imu.gy(), imu.gz())
