TBS Crossfire
=============

TBS Crossfire is a 433MHz UHF RC system. Compared to other UHF systems, its primary advantage is its support for the CRSF protocol, which offers a very low latency control link with integral telemetry data downlink.

TBS Crossfire has support for bidirectional telemetry, including MAVLink telemetry for ArduPilot, but this functionality is limited by the low downlink transmit power on Receivers.

Basic Setup
-----------

Binding
~~~~~~~

Binding identifies one specific receiver to a transmitter. Procedure:

1. Place the Crossfire Transmitter in Bind Mode by navigating to the Configuration Menu and selecting **Bind**
2. Power the Crossfire receiver. If the receiver has previously been bound, press the small **Binding** pushbutton on the receiver
3. Successful binding is indicated by slowly blinking status LED on the Transmitter and *Binding complete* message on the LED screen
4. Power cycle both transmitter and receiver after successful binding

Configuring Receiver Interfaces
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The signal output pins of the Crossfire Receiver can be configured to serve different purposes. Procedure:

1. Power on both the transmitter and receiver
2. Navigate to the configuration menu. If the transmitter is communicating with the receiver, an option labelled by the receiver type, i.e. **Rx Diversity** or **Rx Micro** will be available. 
3. **RX Type/Output Map**: Pick a channel and cycle through the available output options.

+---------------+---------------------------------------------------------------------------+
| Option        | Definition                                                                |
+---------------+---------------------------------------------------------------------------+
| PWM1-12       | PWM signal pin for one corresponding channel                              |
+---------------+---------------------------------------------------------------------------+
| PPM           | PPM signal pin carrying all channels in one stream                        |
+---------------+---------------------------------------------------------------------------+
| SBus          | SBus RC protocol signal pin                                               |
+---------------+---------------------------------------------------------------------------+
| RSSI          | RSSI signal pin, encoding signal strength as a PWM signal value           |
+---------------+---------------------------------------------------------------------------+
| LQ            | Link Quality pin, encoded signal reception quality as a 0-300% value      |
+---------------+---------------------------------------------------------------------------+
| Serial Tx/Rx  | Serial pins for passing raw serial data                                   |
+---------------+---------------------------------------------------------------------------+
| Mavlink Tx/Rx | Serial pins for passing MAVLink telemetry data from autopilots            |
+---------------+---------------------------------------------------------------------------+
| CRSF Tx/Rx    | Serial pins for the bidirectional CRSF RC protocol                        |
+---------------+---------------------------------------------------------------------------+

.. warning::
	Some hardware pins on some receivers cannot be configured to serve all functions listed above. 


Setting Transmit Power
~~~~~~~~~~~~~~~~~~~~~~

The Crossfire transmitter may operate at 6 different transmit power levels. This can be configured via the OLED configurator. Default power output is 100mW which will provide approximately 10 km of range in ideal conditions. Power levels exceeding 500mW may be enabled only if an external battery is connected
