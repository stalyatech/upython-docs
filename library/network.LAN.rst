.. currentmodule:: network
.. _network.LAN:

class LAN -- control built-in ethernet interfaces
=================================================

This class provides a driver for Ethernet network processors.  Example usage::

    import network
    # enable interface and get IP address from gateway
    nic = network.LAN()
    nic.active(True)
    nic.ifconfig('dhcp')
    # now use sockets as usual

Constructors
------------
.. class:: LAN()

Create a LAN network interface object. 

Methods
-------

.. method:: LAN.active([is_active])

    Activate ("up") or deactivate ("down") network interface, if boolean
    argument is passed. Otherwise, query current state if no argument is
    provided. Most other methods require active interface.

.. method:: LAN.status([param])

    Return the current status of link.

    When called with no argument the return value describes the network link status.
    The possible statuses are defined as constants:

        * ``STAT_LINKDOWN`` -- no connection and no activity,
        * ``STAT_LINKUP`` -- connection successful,
        * ``STAT_LINKNOIP`` -- failed because no IP address get,
        * ``STAT_LINKJOIN`` -- failed due to other problems,

.. method:: LAN.ifconfig([(ip, subnet, gateway, dns)])

   Get/set IP-level network interface parameters: IP address, subnet mask,
   gateway and DNS server. When called with no arguments, this method returns
   a 4-tuple with the above information. To set the above values, pass a
   4-tuple with the required information.  For example::

    nic.ifconfig(('192.168.0.4', '255.255.255.0', '192.168.0.1', '8.8.8.8'))

.. method:: LAN.config('param')
            LAN.config(param=value, ...)

   Get or set general network interface parameters. These methods allow to work
   with additional parameters beyond standard IP configuration (as dealt with by
   `LAN.ifconfig()`). These include network-specific and hardware-specific
   parameters. For setting parameters, keyword argument syntax should be used,
   multiple parameters can be set at once. For querying, parameters name should
   be quoted as a string, and only one parameter can be queries at time::

    # Set Ethernet MAC address
    nic.config(mac=010203040506)
    # Query params one by one
    print(nic.config('mac'))
    print(nic.config('trace'))

   Following are commonly supported parameters (availability of a specific parameter
   depends on network technology type, driver, and :term:`MicroPython port`).

   =============  ===========
   Parameter      Description
   =============  ===========
   mac            MAC address (bytes)
   trace          Enable/Disable trace status (boolean)
   =============  ===========

Socket sample with ethernet NIC
-------------------------------

::

    import sty, utime
    import usocket
    import network

    nic = network.LAN()
    nic.active(True)
    nic.ifconfig('dhcp')

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
