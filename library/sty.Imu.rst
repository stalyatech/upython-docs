.. currentmodule:: sty
.. _sty.Imu:

class Imu -- Inertial Measurement Unit
======================================

Imu is an object that controls the accelerometer, gyroscope and quaternions.  

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

   Update the all accelerometer and gyroscope values. There is no return value.

.. method:: Imu.filter(dt)

   Calculate the quaternions using accelerometer and gyroscope values. Returns
   q0, q1, q2 and q3.

     - ``dt`` is the sampling time.

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

Example roll and pitch calculation::

   import sty, math

   def calcRollPitch(q, dt):
      gx = 2 * (q[1]*q[3] - q[0]*q[2])
      gy = 2 * (q[0]*q[1] + q[2]*q[3])
      gz = q[0]*q[0] - q[1]*q[1] - q[2]*q[2] + q[3]*q[3]
      roll  = math.atan(gy / math.sqrt(gx*gx + gz*gz)) * 57.29577951
      pitch = math.atan(gx / math.sqrt(gy*gy + gz*gz)) * 57.29577951
      print("DT:%6.3f #RP:%2.0f,%2.0f" % (dt, roll, pitch))

   imu = sty.Imu()
   while True:
      dt = imu.read()
      calcRollPitch(imu.filter(dt), dt)
