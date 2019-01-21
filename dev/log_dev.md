# Logging and analyzing flight logs

Logging is the process of collecting flight data or autopilot system data in a log file while the vehicle was airborne. This is the same as what a black box / flight recorder does on an airliner.

Flight logs are useful for assessing vehicle performance or tuning PIDs. More importantly, logs allow developers to diagnose a specific issue.

## Log Collection

Logging automatically starts when the vehicle arms, and stopped when it disarms. A new log file is created for each arming session on the SD card. The log file is a binary data file with extension __.ulg__

The uORB topics to be logged can be manually defined using a file on the SD card. Create a file `etc/logging/logger_topics.txt` (Also reate the `etc/logging` directory for first time users) and list the required uORB topics in it. Use the format as follows
```
<topic_name>, <interval>
```

Specify logging rate for a topic by specify the *interval between two logged messages* in milliseconds, in `<interval>`.

If custom uORB topics have been created, specifying it in `logger_topics.txt` is the best way to ensure the custom flight data is logged. Otherwise, firmware files have to be changed. See __Deep Dive__ below for details

## Log Analysis

PX4’s default log analysis tool is Flight Review, an online utility. It has a set of tools to visualize the flight path, and does a good job recreating the flight experience.

![Airframe Screen](/img/3D_view.png)

Flight Review graphs the following flight data

* Attitude Estimates
	* Barometric Altitude, GPS Altitude, Fused Altitude Estimation, Thrust
* Roll / Pitch / Yaw angle
	* Estimate, Setpoint
* Roll / Pitch / Yaw angular rate
	* Estimate, Setpoint
* Local Position Estimation
	* X, Y, Z (coordinates)
* Velocity Estimation
	* X, Y, Z (direction)
* Manual Control Input
	* Channels 1-8
* Actuator Control Command
	* Roll, Pitch, Yaw, Thrust
* Actuator Control Output
	* Motor 1,2,3,4
* Raw acceleration, i.e. Acclerometer measurements
* Raw angular speed, i.e. Gyro measurements
* Raw magnetic field strength, i.e. Compass measurements)
* *And some advanced graphs*

### Log parsing with *pyulog*
pyulog is a python package that can parse ulog files and convert or display them. It enables third-party scripts or utilities, such as MATLAB to process ulog files.

Install pyulog with 
```
pip install pyulog
```

Detailed instructions on using pyulog can be found on its github page. The following are the commands in the package

* Display information of file: `ulog_info`
* Extract messaged from file: `ulog_messages`
* Extracting parameters from file: `ulog_params`
* Convert ulog to csv: `ulog2csv`
* Convert ulog to kml: `ulog2kml`

## Deep Dive

The PX4 firmware has two modules for logging flight data. 

* __sdlog2__: This module is obsolete and it writes logs to .px4log files. Location: `src/modules/sdlog2`
* __logger__: This module is modern and it writes logs to .ulog files. Location: `src/modules/logger`

__sdlog2__ hard codes log messages and their data types into the module’s source code.
On the other hand, __logger__ collects flight data on a per topic basis, and automatically labels and declares log messages. 

One important consequence of this change is that while __sdlog2__ must collect all flight data at the same rate, __Logger__ can collect flight data from different uORB topics at different rates. For example, IMU data which is sampled at 32kHz may be logged at a much higher rate than GPS position data, which is sampled at 10Hz.

### Logger code
To add, remove or customize log messages in source code, only `logger.cpp` needs to edited in one location as shown below

``` c++
void Logger::add_default_topics()
{
  // Note: try to avoid setting the interval where possible
  add_topic("actuator_controls_0", 100);
  /*...*/
  add_topic("debug", 100);	// This is a custom topic
  /*...*/
}
```