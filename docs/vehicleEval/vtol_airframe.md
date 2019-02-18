# VTOL airframe

VTOL aircraft usually combine a multicopter configuration
with wings to achieve Vertical-Takeoff-Or-Landing while retaining the
efficiency of fixed-wings.

## Tailsitter 

The tailsitter mounts two tractor motors on a flying
wing. In fixed-wing mode, they fly like a simple twin-engine plane. In
multicopter mode, the flying wing is oriented vertically, such that the
motors thrust up for lift, and the wing ailerons redirect slipstream air
for lateral control.

\<img src="/Graphics/VTOLDuoRotorTailSitter.svg" width="100"\>

  * Mechanically simplest way to achieve Vertical-Takeoff-Or-Landing.
  * Primary drawback is unwieldiness: They take off nose vertically up
    but flies level, making payload mounting very complicated

## Tiltrotor 

The tiltrotor mounts two motors that can tilt up, plus
one tail motor to provide stabilization during the motor-tilting
process. Short wings provide lift during forward-flight.

\<img src="/Graphics/VTOLTiltRotor.svg" width="100"\>

  * Efficient and versatile VTOL configuration that combines many
    valuable features of multicopters and planes
  * Primary drawback is unreliability: The tiltrotor mechanism is
    complex, and the VTOL transition, during which the motor tilts, is
    hard to control

## Quadplane 

The quadplane mounts four motors in a Quadcopter X
configurations to a fixed wing plane, which also has a motor for
propulsion

\<img src="/Graphics/VTOLPlane.svg" width="100"\>

  * Most reliable VTOL configuration that uses no moving actuators or
    changing vehicle orientation in flight.
  * Primary drawback is inefficiency: quadcopter motors are useless in
    fixed-wing mode, and plane motor is useless in multicopter mode