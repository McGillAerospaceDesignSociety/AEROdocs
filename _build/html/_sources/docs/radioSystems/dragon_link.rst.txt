DragonLink
==========

DragonLink is a 433MHz UHF RC system

Quick Start Checklist
---------------------

- Connect the DragonLink Transmitter to the RC transmitter. Signal,
   Power and Ground must all be connected
- 

Configuration Menu
------------------

The Bind/Menu button on top of the DragonLink provides access to
important configuration options listed below.

+-----------------------+-----------------+--------------------+
| Option                | LED Indicator   | Beeper Indicator   |
+=======================+=================+====================+
| Range Test            | Green           | 1 beep             |
+-----------------------+-----------------+--------------------+
| Bind Mode             | Blue            | 2 beeps            |
+-----------------------+-----------------+--------------------+
| Servo Test            | Yellow          | 3 beeps            |
+-----------------------+-----------------+--------------------+
| Change Tx ID          | Red             | 4 beeps            |
+-----------------------+-----------------+--------------------+
| Exit USB Telem mode   | White           | 5 beeps            |
+-----------------------+-----------------+--------------------+

Press the Menu button while powering on the transmitter, then keep the
button pressed to move down these options. Release the button upon
seeing/hearing the associated LED/beeper indicator to select an option.

Range Test Mode
~~~~~~~~~~~~~~~

.. tip::
   Range test is useful for diagnosing faults, interference and misconfiguration that can potentially cause a crash

Greatly reduces RF transmitting power to simulate the effects of extreme range

Procedure:

- Select Range Test Mode using the Menu Button
- Orient the transmitter with the antenna vertical
- Carry the model aircraft away from the transmitter, keeping the aircraft oriented as if under normal flight conditions
- Monitor the LED state on the transmitter, which indicates a loss of signal by turning Off.

   - LED On - Excellent Signal
   - LED Flickering - Acceptable Signal
   - LED Off for 50% of the time - Poor signal, range limit reached

- Upon reaching the range limit, measure distance between transmitter and model aircraft
- In normal conditions, maximum range achievable in Range Test mode is approximately 10m (30ft). If less than 5m (15ft) range can be achieved, check setup and do not fly.
- Press the menu button to exit Range Test Mode

.. attention:: 
   1. Range tests should be conducted on-location, with a clear line-of-sight and with environmental conditions matching those of the planned flight

Bind Mode
~~~~~~~~~

Binds a receiver to the transmitter without connecting to a computer running the GUI configurator.

Procedure:

- Select Bind Mode using the Menu Button
- Power on the receiver
- Successful binding is indicated by flashing Blue and Green LEDs on the receiver
- Power cycle both transmitter and receiver after successful binding

Servo Test Mode
~~~~~~~~~~~~~~~

Sweeps all channels between maximum and minimum signal values.

.. warning::
   Make sure only servos but not motors are powered when conducting servo test

Procedure:

- Select Servo Test Mode using the Menu Button
- Check servo travel of each servo
- Press the menu button to exit Servo Test Mode

Change Transmitter ID
~~~~~~~~~~~~~~~~~~~~~

Allows multiple DragonLink transmitters with different IDs to operate at the same time.

Procedure:

- Select Change Transmitter ID using the Menu Button
- Successful ID change is indicated by flashing red LED on the transmitter
- Power cycle the transmitter after changing transmitter ID

Exit USB Telemetry Mode
~~~~~~~~~~~~~~~~~~~~~~~

Sets the USB port to interface with GUI configurator instead of outputting telemetry data to Ground Control Station

Procedure:

- Select Exit USB Telemetry Mode using the Menu Button
- Successful USB function change is indicated by flashing white LED on the transmitter and the power up tone playing
- Power cycle the transmitter after exiting USB telemetry mode