# DragonLink

DragonLink is a 433MHz UHF RC system

## Quick Start Checklist

1. Connect the DragonLink Transmitter to the RC transmitter. Signal, Power and Ground must all be connected 
2. 

## Configuration Menu
The Bind/Menu button on top of the DragonLink provides access to important configuration options listed below.

| Option | LED Indicator | Beeper Indicator |
|---------------------|---------------|------------------|
| Range Test | Green | 1 beep |
| Bind Mode | Blue | 2 beeps |
| Servo Test | Yellow | 3 beeps |
| Change Tx ID | Red | 4 beeps |
| Exit USB Telem mode | White | 5 beeps |

Press the Menu button while powering on the transmitter, then keep the button pressed to move down these options. Release the button upon seeing/hearing the associated LED/beeper indicator to select an option.

### Range Test Mode
Tip: Conducting a range test helps diagnosing faults, interference and misconfiguration that can potentially cause a crash

Range test mode greatly reduces RF transmitting power to simulate the effects of extreme range. 

Procedure:
1. Select Range Test Mode using the Menu Button
2. Orient the transmitter with the antenna vertical
3. Carry the model aircraft away from the transmitter, keeping the aircraft oriented as if under normal flight conditions
4. Monitor the LED state on the transmitter, which indicates a loss of signal by turning Off.

    a. LED On - Excellent Signal

    b. LED Flickering - Acceptable Signal

    c. LED Off for 50% of the time - Poor signal, range limit reached

5. Upon reaching the range limit, measure distance between transmitter and model aircraft
6. In normal conditions, maximum range achievable in Range Test mode is approximately 10m (30ft). If less than 5m (15ft) range can be achieved, check setup and do not fly.
7. Press the menu button to exit Range Test Mode

Troubleshooting: Range tests should be conducted on-location, with a clear line-of-sight and with environmental conditions matching those of the planned flight.

### Bind Mode
Bind Mode allows binding the transmitter to a receiver without a computer running the GUI configurator.

Procedure:
1. Select Bind Mode using the Menu Button
2. Power on the receiver
3. Successful binding is indicated by flashing Blue and Green LEDs on the receiver
4. Power cycle both transmitter and receiver after successful binding
 
### Servo Test Mode
Servo Test Mode sweeps all channels between maximum and minimum signal values.

Warning: Disconnect motors and remove propellers before conducting Servo Test!

Procedure:
1. Select Servo Test Mode using the Menu Button
2. Check servo travel of each servo
3. Press the menu button to exit Servo Test Mode

### Change Transmitter ID
Changing the Transmitter ID allows multiple DragonLink transmitters with different IDs to be used at the same time.

Procedure:
1. Select Change Transmitter ID using the Menu Button
2. Successful ID change is indicated by flashing red LED on the transmitter
3. Power cycle the transmitter after changing transmitter ID

### Exit USB Telemetry Mode
Exiting USB Telemetry Mode sets the USB port to function as interface to GUI configurator instead of to Ground Control Station

Procedure:
1. Select Exit USB Telemetry Mode using the Menu Button
2. Successful USB function change is indicated by flashing white LED on the transmitter and the power up tone playing
3. Power cycle the transmitter after exiting USB telemetry mode
