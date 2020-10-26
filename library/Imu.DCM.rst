.. currentmodule:: Imu
.. _Imu.DCM:

class DCM -- control built-in DCM algorithm based AHRS filter
=============================================================

This class provides a filter interface for accelerometer/gyroscope data. 

Constructors
------------
.. class:: DCM(yawfix=True, kp_rollpitch=0.2, ki_rollpitch=0.00005, kp_yaw=1.2, ki_yaw=0.00005)

   Create a DCM filter object with the given parameters: 

     - ``yawfix`` is the parameter to enable yaw correction filter.
     - ``kp_rollpitch`` is the Kp value of roll/pitch PI controller.
     - ``ki_rollpitch`` is the Ki value of roll/pitch PI controller.
     - ``kp_yaw`` is the Kp value of yaw PI controller.
     - ``ki_yaw`` is the Ki value of yaw PI controller.

Methods
-------

.. method:: DCM.update(vals, yaw)

    Apply the one iteration DCM filter for ``vals``. Return value is roll, pitch
    and yaw angles in radians.

   - ``vals`` is the raw values that comes from the IMU sensor.
   - ``yaw`` is the heading that will be filtered.


DCM Filter Usage 
----------------

::

    import sty

    def printRollPitchYaw(rpy, dt):
        roll  = rpy[0] * 57.29577951
        pitch = rpy[1] * 57.29577951
        yaw   = rpy[2] * 57.29577951
        print("DEL:%6.3f #RPY:%2.0f,%2.0f,%2.0f" % (dt, roll, pitch, yaw))

    # Create the IMU sensor class
    # it takes some times to configure it
    imu = sty.Imu()

    # Create the filter class
    ahrs = imu.DCM(yawfix=True, kp_rollpitch=0.2, ki_rollpitch=0.00005, kp_yaw=1.2, ki_yaw=0.00005)

    while True:
        # Read the unfiltered values 
        # ax, ay, az, gx, gy, gz, dt
        vals = imu.read()
        # Update the filtered values (DCM algorithm)
        printRollPitchYaw(ahrs.update(vals, 0), vals[sty.Imu.DT])
