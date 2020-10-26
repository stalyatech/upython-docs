.. currentmodule:: sty
.. _sty.Imu:

class Imu -- Inertial Measurement Unit
======================================

Imu is an object that controls the accelerometer, gyroscope and AHRS filters.  

Example usage::

   imu = sty.Imu()
   for i in range(10):
      print(imu.ax(), imu.ay(), imu.az(), imu.gx(), imu.gy(), imu.gz())

ax, ay, az values are between -12.0G and +12.0G. 
gx, gy, gz values are between -2000dps and +2000dps. 


Constructors
------------

.. class:: sty.Imu()

   Create and return an IMU object.

Methods
-------

.. method:: Imu.stat()

   Get the sensor initialize status. If it is true the sensor is found.
   Otherwise something is wrong. 

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

   Update the all accelerometer and gyroscope values. Return with ax, ay, az, gx, gy, gz and dt.

.. method:: Imu.read_accel(reg)

   Read the accelerometer register value.

     - ``reg`` is the register address.

.. method:: Imu.write_accel(reg, val)

   Write the accelerometer register value.

     - ``reg`` is the register address.
     - ``val`` is the register value.

.. method:: Imu.read_gyro(reg)

   Read the gyroscope register value.

     - ``reg`` is the register address.

.. method:: Imu.write_gyro(reg, val)

   Write the gyroscope register value.

     - ``reg`` is the register address.
     - ``val`` is the register value.

Specific filter class implementations
=====================================

The following concrete classes implement the AHRS filter interface and
provide a way to filter raw IMU data of various kinds.

.. toctree::
   :maxdepth: 1

   Imu.DCM.rst
   Imu.Mahony.rst
   Imu.Madgwick.rst
