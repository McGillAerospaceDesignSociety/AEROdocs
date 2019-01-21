# Sensors

A flight control system uses sensors to obtain information on vehicle state in order to perform its primary functions of stabilization and/or navigation. The vehicle states include:

* Attitude
* Body rotation rates
* Position
* Heading
* Altitude
* Speed / Airspeed

The flight control system minimally requires an inertial measurement unit (IMU) to for stable flight. Secondly, a compass and a barometer is required to enable some assisted modes. Lastly, a GPS is required to enable fully autonomous modes.

## Core Sensors

### IMU
The IMU consists of a gyroscope and an accelerometer. This is the most important sensor integral in every autopilot. It measures the vehicle’s angular rates and attitude, and enables the autopilot to perform basic flight stabilization.


![MPU6000: Commonly used IMU chip known for ruggedness](/img/sensors/MPU.png)

### Compass
The compass / magnetometer measures the vehicle’s heading relative to the earth. It enables the autopilot to control vehicle heading during autonomous navigation.


![HMC5883L: Very reliable and widely-used compass chip, but recently deprecated](/img/sensors/HMC.png)

__Warning: The compass is sensitive to electromagnetic interference (EMI). It must be calibrated carefully to avoid poor performance in auto-modes!__

### Barometer
The barometer measures the vehicle's elevation. It enables the autopilot to control vehicle altitude during altitude-hold mode or autonomous navigation.


![MS5611: Notable high-precision barometer chip](/img/sensors/MS.png)

__Warning: The barometer is sensitive to pressure change unrelated to elevation change (wind gusts) and changing of lighting conditions. It must be shielded by foam to prevent vehicle climbing and falling unpredictably.__

## Advanced/External Sensors

### GPS 
The GPS enables the autopilot to sense the vehicle’s geographic location, i.e. position estimation. It is necessary for holding a fixed vehicle position and navigation in automatic flight modes.


![mRo M8N GPS](/img/sensors/GPS.png)

*Additional information: Main components in a GPS module include the ceramic antenna, the GPS chip, and a compass chip which serves as an external compass*

__Warning: It is strongly advised to mount the GPS module above the UAV on a tall mast. This isolates the GPS chip and the compass chip from electromagnetic interference (EMI) and improves navigation performance__

### Airspeed Sensor (Pitot tube)
Airspeed sensor measures the speed of a fixed-wing aircraft relative to surrounding airflow, as opposed to speed measured relative to the ground. It is necessary for all fixed-wing aircraft to maintain lift (and avoid stalling)


![Airspeed sensor and pitot tube](/img/sensors/AIRSPD.png)

## Optional Sensors
### Rangefinder
Sonar / LIDAR rangefinders provide distance measurements. It enables very precise altitude control (error in the order of cm), terrain-following, or collision avoidance.

![LIDAR-lite](/img/sensors/LIDAR.png)

*Sonar rangefinders have very limited range, using LIDAR is recommended.*

### Optical Flow
Optical flow uses a downward facing camera for position recognition. It enables position estimation without using GPS, or in a GPS-deprived environment, i.e. indoors / poor weather.


![PX4FLow](/img/sensors/OpFlow.png)

*Optical flow sensors must be used together with a rangefinder.*

### RTK GPS
RTK GPS relies on a 'Base GPS' on the ground to send real-time position correction to a 'Rover GPS' in the air to realize very precise position estimation (error in the order of 10cms)

![RTK_GPS](/img/sensors/RTKGPS.png)