Telemetry Configuration
-----------------------
To enable MAVLink telemetry communication, both DragonLink Hardware and Software must be setup

Hardware Interfaces
~~~~~~~~~~~~~~~~~~~

Connect the Pixhawk Telem1 or Telem2 port to the DragonLink receiver's UEXP port as shown in diagrams below. 

.. image:: ./Graphics/6pos_GH.png
   :scale: 50%
   :alt: For Advanced Receivers Only
.. image:: ./Graphics/5pos_GH.png
   :scale: 50%
   :alt: For other Receivers

When using the Advanced Receiver with Flow Control, stock Pixhawk telemetry cables can be used. Otherwise, a custom cable must be created with the Flow Control wires removed. In all cases, remove the power wire to prevent power supplies fighting each other.

Receiver Configuration
~~~~~~~~~~~~~~~~~~~~~~

Adjust settings in the GUI configurator as described below, and always save settings after each change

**Radio Modem**: Set *Input/output buad rate* to 38400 (57600 when using Flow Control) and click *Save Baud*

.. Note::
   Turn on Mavlink Decoding to enable flight data display on RC transmitter screen. Otherwise keep disabled

**Receiver Outputs**: Set the outputs for the 5-pin UEXP port as follows

+----------------+-------------+
| Setting        | Description |
+----------------+-------------+
| UEXP con pin 3 | Serial In   |
+----------------+-------------+
| UEXP con pin 4 | Serial Out  |
+----------------+-------------+

When using Flow Control, set the outputs for the 6-pin UEXP port as follows

+----------------+-------------+
| Setting        | Description |
+----------------+-------------+
| UEXP con pin 2 | Serial In   |
+----------------+-------------+
| UEXP con pin 3 | Serial Out  |
+----------------+-------------+
| UEXP con pin 4 | CTS         |
+----------------+-------------+
| UEXP con pin 5 | RTS         |
+----------------+-------------+

Transmitter Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~

Adjust settings in the GUI configurator as described below, and always save settings after each change
**RF Settings**: Select 9X mode

**External Connections**
- Set baudrate to 57600
- Select **Radio Modem** for Bluetooth or USB depending on preferred connection method to Mission Planner.

.. Warning::
   Once the USB function is set to Radio Modem, the transmitter will not connect to the GUI configurator until the GUI config USB function is re-enabled again using the Menu button on the transmitter

- Save settings and re-bind transmitter to receiver

Set ArduPilot Parameters
~~~~~~~~~~~~~~~~~~~~~~~~
**Serial Port**: SERIAL1, SR1 and SER1 specifies that the Telem1 hardware port is being configured. If Telem2 is used, change SERIAL2 and so on. 

+------------------+-------+-----------------------+
| Parameter        | Value | Description           |
+------------------+-------+-----------------------+
| SERIAL1_PROTOCOL | 1/2   | Mavlink 1 or 2        |
+------------------+-------+-----------------------+
| SERIAL1_BAUD     | 38    | 38400 Bauds           |
+------------------+-------+-----------------------+
| BRD_SER1_RTSCTS  | 0     | Disable Flow Control  |
+------------------+-------+-----------------------+

.. Note::
   Set **SERIAL1_BAUD** to 57 and **BRD_SER1_RTSCTS** to 1 when using Flow Control

**Data streaming Rate**

+------------------+-------+
| Parameter        | Value |
+------------------+-------+
| SR1_EXT_STAT     | 1     |
+------------------+-------+
| SR1_EXTRA1       | 5     |
+------------------+-------+
| SR1_EXTRA2       | 5     |
+------------------+-------+
| SR1_EXTRA3       | 1     |
+------------------+-------+
| SR1_PARAMS       | 10    |
+------------------+-------+
| SR1_POSITION     | 3     |
+------------------+-------+
| SR1_RAW_CTRL     | 1     |
+------------------+-------+
| SR1_RAW_SENS     | 1     |
+------------------+-------+
| SR1_RC_CHAN      | 1     |
+------------------+-------+