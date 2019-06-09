Sensors
=======
A flight control system uses sensors to obtain information on vehicle state in order to perform its primary functions of stabilization and/or navigation. The vehicle states include: 

-	Attitude
-	Body rotation rates
-	Position
-	Heading
-	Altitude
-	Speed / Airspeed

The flight control system minimally requires an inertial measurement unit (IMU) to for stable flight. Secondly, a compass and a barometer is required to enable some assisted modes. Lastly, a GPS is required to enable fully autonomous modes.

IMU
---
The inertial measurement unit (IMU) consists of a gyroscope and an accelerometer. It is integral in every autopilot board. 

It measures the vehicle’s angular rates and attitude, and enables the autopilot to perform basic flight stabilization.

Compass
-------
The compass is an electronic magnetometer which measures the vehicle’s heading relative to the earth. It enables the autopilot to control vehicle heading in flight.

The compass is sensitive to electromagnetic interference (EMI). It should be mounted far away from electrical wiring.
 
	.. warning:: Proper calibration of the compass is critical for the UAV to hold a steady course in flight. Failure to do so can result in 

		1. Multicopters may begin circling uncontrollably when attempting to hold position
		2. All vehicle types may deviate from commanded course and fly away

Barometer
---------
The barometer is an absolute pressure sensor which measures the vehicle's altitude above ground. It enables the autopilot to control the altitude of the vehicle in flight


GPS
The GPS enables the autopilot to sense the vehicle’s geographic location. It is necessary for vehicle navigation in automatic flight modes. 
