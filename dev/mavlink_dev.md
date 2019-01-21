# MAVLink Development
The objective of MAVLink messaging is to send data to QGroundControl GCS for inspection/visualization.

Internally, flight data is handled as uORB messages. In our case, this message is `pot_angle` as defined in `msg/pot_angle.msg`.

## Create custom mavlink message
For MAVLink communication, a corresponding MAVLink message `pot_angle` should be created. This message is defined in an .xml file in `mavlink/include/mavlink/v2.0/message_definitions` and then converted to C/C++ code.

_MAVLink is a submodule in PX4 firmware! Use_ `git submodule update --init --recursive` _to fetch it._

_Unfortunately, this means the edited MAVLink library and included headers is not pushed to the Firmware repository. Please follow the direction below to generate MAVLink headers_

`custom_msgs.xml` is the .xml message definition for `pot_angle` shown below

```xml
<?xml version="1.0"?>
<mavlink>
  <include>common.xml</include>
  <include>ardupilotmega.xml</include>
  <version>3</version>
  <messages>
    <message id="190" name="POT_ANGLE">
      <description>Potentiometer readings</description>
      <field type="uint64_t" name="error_count">Number of errors detected by driver</field>
      <field type="float" name="pot_ADCraw">Raw ADC reading</field>
      <field type="float" name="pot_angle_deg_raw" units="deg">unfiltered angle</field>
      <field type="float" name="pot_angle_deg" units="deg">filtered angle in degrees</field>
      <field type="float" name="pot_angle_rad" units="rad">filtered angle in radians</field>
      <field type="float" name="pot_rate_deg_raw" units="deg">unfiltered angle rate in degrees</field>
      <field type="float" name="pot_rate_deg" units="deg"> filtered rate in degrees</field>
      <field type="float" name="pot_rate_rad" units="rad"> filtered rate in radians</field>
      <field type="float" name="pot_dt"></field>
    </message>
  </messages>
</mavlink>
```
### XML Definitions
* `<message></message>` : Encapsulates one message
* `id=“190”` : Index number of the message. In general, id 150-240 is reserved for custom messages 
* `<description></description>` : Describes the message, and will be converted into comments during C/C++ code generation
* `<field></field>` : Encodes one field of the message
* `type=“float”` Defines data type of the field

### Compiling XML to C/C++
The message definition is compiled into C/C++ code for use by the autopilot by a python script `mavgenerate.py`. This script is part of the [Mavlink Repository](https://github.com/mavlink/mavlink). Clone the repo and run `python mavgenerate.py` to bring up a GUI for configuring code generation.

![](https://api.ning.com/files/UNdbGkqHoSpJdWrdaMY6xWZObJANncpmH32UDK8R*BaXZbqZwgtPzm93MiOAJMj0ZJ7Gl0*NtjFXJn0bygusiIXQ1V2r644q/Mavlinkgenerator.png)

* XML: .xml file of message definition
* Out: Output directory, generally set to be `mavlink/include/mavlink/v2.0`. Code generation will create a new folder in this directory to contain the message headers. 
* Language: Use C++11 for PX4 Firmware and QGroundControl mavlink libraty
* Validate/Validate Units: Manually check your .xml definition, and omit validation to avoid getting spurious errors

## Sending custom MAVLink messages
Sending and receiving MAVLink messages is mostly handled in the various C/C++ files in `src/modules/mavlink`. 

A class for the message should be created in `mavlink_messages.cpp`. First, add the headers of the uORB and mavlink messages.
```c++
#include <uORB/topics/pot_angle.h>
#include <v2.0/custom_msgs/mavlink_msg_pot_angle.h>
```

Then create the class
```c++
class MavlinkStreamPotAngle : public MavlinkStream
{
public:
    const char *get_name() const
    {
        return MavlinkStreamPotAngle::get_name_static();
    }
    static const char *get_name_static()
    {
        return "POT_ANGLE";
    }
    static uint16_t get_id_static()
    {
        return MAVLINK_MSG_ID_POT_ANGLE;
    }
	uint16_t get_id()
	{
		return get_id_static();
	}
    static MavlinkStream *new_instance(Mavlink *mavlink)
    {
        return new MavlinkStreamPotAngle(mavlink);
    }
    unsigned get_size()
    {
        return MAVLINK_MSG_ID_POT_ANGLE_LEN + MAVLINK_NUM_NON_PAYLOAD_BYTES;
    }

private:
    MavlinkOrbSubscription *_sub;
    uint64_t _pot_time;

    /* do not allow top copying this class */
    MavlinkStreamPotAngle(MavlinkStreamPotAngle &);
    MavlinkStreamPotAngle& operator = (const MavlinkStreamPotAngle &);

protected:
    explicit MavlinkStreamPotAngle(Mavlink *mavlink) : MavlinkStream(mavlink),
        _sub(_mavlink->add_orb_subscription(ORB_ID(pot_angle))),  // make sure you enter the name of your uORB topic here
        _pot_time(0)
    {}

    bool send(const hrt_abstime t)
    {
        pot_angle_s _pot_angle;

        if (_sub->update(&_pot_time, &_pot_angle)) {
            mavlink_pot_angle_t msg = {};  //make sure mavlink_pot_angle_t is the definition of your custom MAVLink message

            msg.error_count = _pot_angle.error_count;
            msg.pot_ADCraw = _pot_angle.pot_ADCraw;
            msg.pot_angle_deg_raw  = _pot_angle.pot_angle_deg_raw;
            msg.pot_angle_deg = _pot_angle.pot_angle_deg;
            msg.pot_angle_rad = _pot_angle.pot_angle_rad;
            msg.pot_rate_deg_raw = _pot_angle.pot_rate_deg_raw;
            msg.pot_rate_deg = _pot_angle.pot_rate_deg;
            msg.pot_rate_rad = _pot_angle.pot_rate_rad;
            msg.pot_dt = _pot_angle.pot_dt;

			mavlink_msg_pot_angle_send_struct(_mavlink->get_channel(), &msg);

			return true;
        }

        return false;
    }
};
```

Finally append the stream class to the `streams_list` at the bottom of the code.
```c++
static const StreamListItem streams_list[] = {

StreamListItem(&MavlinkStreamPotAngle::new_instance, &MavlinkStreamPotAngle::get_name_static, 
&MavlinkStreamPotAngle::get_id_static),

}
```

## Enabling the MAVLink stream
To enable the MAVLink stream, edit `mavlink_main.cpp` and add relevant messages to `configure_streams_to_default`. 

First include the MAVLink message header in `mavlink_main.h`
```c++
#include <v2.0/custom_msgs/mavlink_msg_pot_angle.h>
```
Then append the configure function to each case matching a MAVLink mode in which the MAVLink stream should be enabled
```c++
case MAVLINK_MODE_NORMAL:

	configure_stream_local("POT_ANGLE", 10.0f);

	break;
```

Alternately, add a line to the startup script `extras.txt` on the SD card to enable the streaming.
> mavlink stream -r 50 -s POT_ANGLE -d /dev/ttyACM0

Parameters are `-r`, `-s` and `-d`, which configures the data rate, MAVLink message, and serial port of the stream.

## Reading the MAVLink Stream
To read the MAVLink Stream, QGroundControl should be rebuilt with the custom MAVLink message headers. This simply entails repeating the steps in the above section **Creating custom MAVLink message** in `qgroundcontrol/libs/mavlink/include/mavlink/v2.0`

_MAVLink is a submodule in QGroundControl firmware too_

The composition of the .xml message definition becomes important here. 
```c++
  <include>common.xml</include>
  <include>ardupilotmega.xml</include>
```
Both `common.xml` and `ardupilotmega.xml` definitions must be included. The latter is a superset of all MAVLink messages and has an include line for `common.xml` within it. However, C++ code generation omits nested includes. Hence, both .xml files must be included, and omission of either will break the build.

### Configuring QGroundControl build
Add a file `user_config.pri` in the QGroundControl base directory and enter the following line.

```qml
MAVLINK_CONF = custom_msgs
```

This will tell QGroundControl to build the customized MAVLink library. 