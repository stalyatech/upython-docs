.. currentmodule:: sty
.. _sty.Imu:

class Imu -- accelerometer/gyroscope control
============================================

Imu is an object that controls the accelerometer and gyroscope.  Example usage::

    imu = sty.Imu()
    for i in range(10):
        print(imu.ax(), imu.ay(), imu.az(), imu.gx(), imu.gy(), imu.gz())

Raw values are between -32 and 31.


Constructors
------------

.. class:: sty.Imu()

   Create and return an IMU object.

Methods
-------

.. method:: Imu.filtered_xyz()

   Get a 3-tuple of filtered x, y and z values.

   Implementation note: this method is currently implemented as taking the
   sum of 4 samples, sampled from the 3 previous calls to this function along
   with the sample from the current call.  Returned values are therefore 4
   times the size of what they would be from the raw x(), y() and z() calls.

.. method:: Imu.ax()

   Get the accelerometer x-axis value.

.. method:: Imu.ay()

   Get the accelerometer y-axis value.

.. method:: Imu.az()

   Get the accelerometer z-axis value.

.. method:: Imu.gx()

   Get the gyroscope x-axis value.

.. method:: Imu.gy()

   Get the gyroscope y-axis value.

.. method:: Imu.gz()

   Get the gyroscope z-axis value.

.. method:: Imu.read()

   Update the all accelerometer and gyroscope values. There is no return value.

.. method:: Imu.filter()

   Calculate the quaternions using accelerometer and gyroscope values.
