.. currentmodule:: Imu
.. _Imu.Mahony:

class Mahony -- control built-in Mahony algorithm based AHRS filter
===================================================================

This class provides a filter interface for accelerometer/gyroscope data. 

Constructors
------------
.. class:: Mahony(kp=1.5, ki=0.01)

   Create a Mahony filter object with the given parameters: 

     - ``kp`` is the Kp value of filter PI controller.
     - ``ki`` is the Ki value of filter PI controller.

Methods
-------

.. method:: Mahony.update(vals)

    Apply the one iteration Mahony filter for ``vals``. Return value is quaternions. 

   - ``vals`` is the raw values that comes from the IMU sensor.


Mahony Filter Usage 
-------------------

::

    import sty

    # Create the IMU sensor class
    # it takes some times to configure it
    imu = sty.Imu()

    # Create the filter class
    ahrs = imu.Mahony(kp=1.5, ki=0.01)

    while True:
        print(ahrs.update(imu.read()))
