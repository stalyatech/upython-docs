Power control
=============

:meth:`sty.wfi` is used to reduce power consumption while waiting for an
event such as an interrupt.  You would use it in the following situation::

    while True:
        do_some_processing()
        sty.wfi()

Control the frequency using :meth:`sty.freq`::

    sty.freq(30000000) # set CPU frequency to 30MHz
