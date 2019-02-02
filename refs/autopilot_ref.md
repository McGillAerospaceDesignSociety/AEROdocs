# Autopilot Hardware

Pixhawk series autopilots all run PX4 or ArduPilot firmware, but the hardware differ in interfaces and form factor. This section summarizes the available autopilot hardware and groups them into three categories.

## General purpose autopilots
These are full-feature autopilots with a full set of interfaces, i.e. 8+6 Main and Aux outputs, 5 serial ports, analog ports. They are suitable for a wide range of airframes and configurations. 

They can further be categorized by their intended applications.

### Industrial / Commercial
These are well tested and widely available at a reasonable price. They are optimized for easy deployment on industrial or professional UAVs.

* Pixhawk
* Pixhawk 2.1
* mRo X2.1

![Pixhawk I](/img/PixhawkI.png)

### Academic / Research
These are computationally powerful and feature-rich, and accordingly more expensive. They are good platforms for developing bleeding-edge capabilities

* Pixhawk 3 Pro
* Pixhawk 4

![Pixhawk IV](/img/PixhawkIV.png)


## Miniaturized Autopilots
These are small and lightweight autopilots with a reduced set of  interfaces. They are suitable for space-constrained configurations, e.g. microquads and racers.

* Pixhawk Mini
* Pixracer

![Pixracer](/img/Pixracer.png)

## Derivatives
These are derivatives of one of the above designs. They are less well tested, but retain the set of features and capabilities of their parent design.

* Dropix
* Pixhack
* MindPX

## Linux Based
These consist of a sensor / processor shield that interfaces with a Raspberry Pi single-board computer. They are very computationally powerful, and lends itself well to customized applications, APIs, or complicated tasks such as computer vision. 

![Navio](/img/Navio.png)

*Warning: Linux based autopilots are considerably more difficult to setup. Consider setting up a Linux based autopilot as configuring both a Raspberry Pi AND a traditional autopilot.*

Warning: Exotic designs are excluded from this list. It is the end-user’s responsibility to find them and verify their capabilities.