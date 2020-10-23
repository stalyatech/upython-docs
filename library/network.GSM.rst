.. currentmodule:: network
.. _network.GSM:

class GSM -- control built-in GSM interfaces
==============================================

This class provides a driver for GSM network processors.  Example usage::

    import network
    # enable GSM interface and connect to service provider
    pwr = Pin('PWR_GSM', Pin.OUT_OD)
    pon = Pin('M2M_PON', Pin.OUT_OD)
    rst = Pin('M2M_RST', Pin.OUT_OD)
    mon = Pin('M2M_MON', Pin.IN)
    nic = network.GSM(UART('GSM', 115200, flow=UART.RTS|UART.CTS, rxbuf=1024, dma=False), 
                      pwr_pin=pwr, 
                      pon_pin=pon, 
                      rst_pin=rst, 
                      mon_pin=mon)
    nic.connect()
    # Wait till connection
    while not nic.isconnected():
        pass
    # now use sockets as usual

Constructors
------------
.. class:: GSM(uart_object)

Create a GSM network interface object. 

Methods
-------

.. method:: GSM.active([is_active])

    Activate ("up") or deactivate ("down") network interface, if boolean
    argument is passed. Otherwise, query current state if no argument is
    provided. Most other methods require active interface.

.. method:: GSM.connect()

    Activate ("up") network interface, connect to the service provider.

.. method:: GSM.disconnect()

    Deactivate ("down") network interface, disconnect from the service provider.

.. method:: GSM.imei()

    Get modem IMEI number as a string.

.. method:: GSM.imsi()

    Get SIM card IMSI number as a string.

.. method:: GSM.isconnected()

    Returns ``True`` if connected to a service provider. Returns ``False`` otherwise.

.. method:: GSM.ifconfig()

   Get IP-level network interface parameters: IP address, subnet mask,
   gateway and DNS server. This method returns a 4-tuple with the above information. 

.. method:: GSM.config('param')
            GSM.config(param=value, ...)

   Get or set general network interface parameters. These methods allow to work
   with additional parameters beyond standard IP configuration (as dealt with by
   `GSM.ifconfig()`). These include network-specific and hardware-specific
   parameters. For setting parameters, keyword argument syntax should be used,
   multiple parameters can be set at once. For querying, parameters name should
   be quoted as a string, and only one parameter can be queries at time::

    # Query params one by one
    print(nic.config('imei'))
    print(nic.config('imsi'))

   Following are commonly supported parameters (availability of a specific parameter
   depends on network technology type, driver, and :term:`MicroPython port`).

   =============  ===========
   Parameter      Description
   =============  ===========
   imei           GSM IMEI number (string)
   imsi           GSM IMSI number (string)
   qos            GSM quality of service value (tuple)
   =============  ===========

Socket sample with GSM NIC
--------------------------

::

    from sty import UART
    import sty, network

    pwr = Pin('PWR_GSM', Pin.OUT_OD)
    pon = Pin('M2M_PON', Pin.OUT_OD)
    rst = Pin('M2M_RST', Pin.OUT_OD)
    mon = Pin('M2M_MON', Pin.IN)
    nic = network.GSM(UART('GSM', 115200, flow=UART.RTS|UART.CTS, rxbuf=1024, dma=False), pwr_pin=pwr, pon_pin=pon, rst_pin=rst, mon_pin=mon)

    nic.connect()
    while not nic.isconnected():
        pass

    ifconfig = nic.ifconfig()
    print('GSM connection done: %s' % ifconfig[0])
    print('IMEI Number: %s' % nic.imei())
    print('IMSI Number: %s' % nic.imsi())
    qos = nic.qos()
    print('Signal Quality: %d,%d' % (qos[0],qos[1]))

    addr = usocket.getaddrinfo('micropython.org', 80)[0][-1]
    print('Host: %s:%d' % (addr[0], addr[1]))
    sock = usocket.socket(usocket.AF_INET, usocket.SOCK_STREAM)
    sock.connect(addr)
    sock.send(b'GET / HTTP/1.1\r\nHost: ardusimple.com\r\n\r\n')

    sock.settimeout(10.0)
    try:
        data = sock.recv(1000)
        print(data)
    except:
        print('Timeout!\r\n')

    sock.close()

SMS Send and receive sample
---------------------------

::

    from sty import UART
    import sty, network

    def OnSmsReceived(msg):
        print(msg)

    pwr = Pin('PWR_GSM', Pin.OUT_OD)
    pon = Pin('M2M_PON', Pin.OUT_OD)
    rst = Pin('M2M_RST', Pin.OUT_OD)
    mon = Pin('M2M_MON', Pin.IN)
    nic = network.GSM(UART('GSM', 115200, flow=UART.RTS|UART.CTS, rxbuf=1024, dma=False), pwr_pin=pwr, pon_pin=pon, rst_pin=rst, mon_pin=mon)
    sms = nic.SMS(OnSmsReceived)

    nic.connect()
    while not nic.isconnected():
        pass

    ifconfig = nic.ifconfig()
    print('GSM connection done: %s' % ifconfig[0])
    print('IMEI Number: %s' % nic.imei())
    print('IMSI Number: %s' % nic.imsi())
    qos = nic.qos()
    print('Signal Quality: %d,%d' % (qos[0],qos[1]))

    sms.send('0555XXXXXXX', 'Message from simpleRTK')
    while sms.isbusy():
        pass

    if sms.iserror():
        print('SMS send error!\r\n')
    else:
        print('SMS has been sent\r\n')
    print('Now we are waiting for any message...\r\n')

    while True:
        pass
