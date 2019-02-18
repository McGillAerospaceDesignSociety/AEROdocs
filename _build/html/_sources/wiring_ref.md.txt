## Mount and Orient Autopilot

Position the autopilot as close to the vehicle’s center of gravity as possible. Ideally, the autopilot should be mounted in the standard orientation, indicated by the heading mark arrow visible on top and pointing towards the front of the vehicle.

To mount the pixhawk in a non-standard orientation, configure the pixhawk firmware to recognize the new orientation. This adjusts sensors readings to realign with the vehicle.

The pixhawk should be mounted on vibration absorbing material, which isolates the sensitive sensors in the Pixhawk from vibrations. Soft foam such as 3M Pixhawk Damping Foams, gel pads, and mounting tables with rubber damping balls are effective.


## Powering the autopilot


Connect the output port of a power module to the power port of the Pixhawk with a 6-wire cable.

A power module supplies stable and filtered power at 5.4V for the pixhawk, measures battery voltage and/or current drawn from the battery.

The power module can supply power to the autopilot, radio receiver, and some low-power devices, but not servos and other devices connected to the output rail.

If servos are used, the pixhawk servo rail should be connected to voltage regulator (alias BEC, battery eliminator circuit) that provides power at 5V to drive the servos. Connect the BEC output to a power pin (+) and ground pin (-) on the rail.

## Radio Control

__Warning: Connecting a radio control system to the pixhawk is highly recommended. This ensures that the operator can safety regain control if an auto flight mode malfunctions__

Connect the output of a radio receiver to the receiver port of the pixhawk using a servo cable.

Pixhawk can read CPPM and SBUS directly. The instructions below are specifically for Spektrum and PWM receivers

* Connect Spektrum satellite receivers to the SPKT/DSM port on Pixhawk 1 or 2.1
* Connect PWM receivers with an individual wire for each channel to a PPM encoder first, which converts the signal into CPPM for the Pixhawk

*Sending RSSI (received signal strength indication) to the Pixhawk allows monitoring of the radio signal strength and helps prevent outflying radio range by accident* 

If the radio receiver has an RSSI output port, connect it to the SBUS port of the Pixhawk. Configure the pixhawk firmware to measure RSSI signal in percentage

## Telemetry

Telemetry is used to monitor vehicle status in flight and send commands to the vehicle, i.e. guide the vehicle to a position on a map, or upload a mission.

Connect the output of a telemetry radio to the telem 1 port of the pixhawk using a 6-wire cable.

When using a high power radio modem, such as the RFD900 modem, only connect the 4 signal wires to the Pixhawk. Connect the power and ground wires to a separate BEC.