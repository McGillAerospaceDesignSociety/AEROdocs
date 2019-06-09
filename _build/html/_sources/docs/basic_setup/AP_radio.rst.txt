Radio Control
-------------
	.. warning:: Connecting a radio control system to the autopilot is highly recommended. This ensures that the operator can safety regain control if an auto flight mode malfunctions

Connect the output of a radio receiver to the receiver port of the autopilot using a servo cable.  

Autopilot can read CPPM and SBUS directly. The instructions below are specifically for Spektrum and PWM receivers
-	Connect Spektrum satellite receivers to the SPKT/DSM port on Autopilot 1 or 2.1
-	Connect PWM receivers with an individual wire for each channel to a PPM encoder first, which converts the signal into CPPM for the Autopilot

	.. tip:: Sending RSSI (received signal strength indication) to the Autopilot allows monitoring of the radio signal strength and helps the pilot avoid outflying radio range

If the radio receiver has an RSSI output port, connect it to the SBUS port of the Autopilot. Configure the autopilot firmware to measure RSSI signal in percentage

.. figure:: Graphics/fig3.png
   :alt: image