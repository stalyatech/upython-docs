.. currentmodule:: Imu
.. _Imu.Madgwick:

class Madgwick -- control built-in Madgwick algorithm based AHRS filter
=======================================================================

This class provides a filter interface for accelerometer/gyroscope data. 

Constructors
------------
.. class:: Madgwick(beta=0.2)

   Create a Madgwick filter object with the given parameters: 

     - ``beta`` is the gain value of filter.

Methods
-------

.. method:: Madgwick.update(vals)

    Apply the one iteration Madgwick filter for ``vals``. Return value is quaternions. 

   - ``vals`` is the raw values that comes from the IMU sensor.
 

Madgwick Filter Usage 
---------------------

::

    import sty

    # Create the IMU sensor class
    # it takes some times to configure it
    imu = sty.Imu()

    # Create the filter class
    ahrs = imu.Madgwick(beta=0.2)

    while True:
        print(ahrs.update(imu.read()))
