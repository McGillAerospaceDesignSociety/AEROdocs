Powering the autopilot
----------------------

.. warning::
	Double check the rated voltage and current input or output for all devices!

Connect the output port of a power module to the power port of the Autopilot with a 6-wire cable.

.. figure:: Graphics/fig2.png
   :alt: image

.. note::
	A power module supplies stable and filtered power at 5.4V for the autopilot, and measures battery voltage and current drawn from the battery.

The power module supplies power to the autopilot, radio receiver, and some electronic devices, but not servos and other devices connected to the output rail. 

If servos are used, the autopilot servo rail should be connected to a BEC, i.e. step-down voltage regulator, that provides power at 5V to drive the servos. Connect the BEC output to a power pin (+) and ground pin (-) on the rail.

Additional high power devices, such as the RFD900 modem, can also be connected to the servo rail.

