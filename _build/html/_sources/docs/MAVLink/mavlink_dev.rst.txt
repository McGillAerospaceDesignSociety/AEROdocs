MAVLink Development
===================

The objective of MAVLink messaging is to send data to Ground Control Stations for inspection/visualization.

For demonstration purposes, the MAVLink message will be an analog voltage outputted by a rotary potentiometer connected to a Pixhawk's analog port. Hence, the new message definition is ``msg/ANALOG_POT.msg``.

MAVLink Code Submodule
----------------------
MAVLink is a submodule in ArduPilot firmware. Its location is ``C:\cygwin64\home\hells\ardupilot\modules\mavlink\`` if installation per previous documentation is done. Use ``git submodule update --init --recursive`` to update this submodule.

.. note::
  All paths in the following article are relative to the mavlink base directory described above.


Create custom mavlink message
-----------------------------

MAVLink communication is basd on messages. All messages are initially defined as an .xml file in ``\message_definitions``, then converted to C/C++ code. A custom message, for example ``ANALOG_POT``, may be created and formatted as follows. 

.. tip::
  It is recommended to put custom messages in a separate file, i.e. ``custom_messages.xml``, to prevent confusion with mainstream ArduPilot / PX4 messages

.. code:: xml

    <?xml version="1.0"?>
    <mavlink>
      <include>common.xml</include>
      <include>ardupilotmega.xml</include>
      <version>3</version>
      <messages>
        <message id="190" name="ANALOG_POT">
          <description>Analog port reading for potentiometer</description>
          <field type="float" name="VCCA">Raw ADC reading</field>
          <field type="float" name="POT_DEG" units="deg">Angle of potentiometer in degrees</field>
        </message>
      </messages>
    </mavlink>

XML Definitions
~~~~~~~~~~~~~~~

-  ``<message></message>`` : Encapsulates one message
-  ``id=“190”`` : Index number of the message. In general, id 150-240 is
   reserved for custom messages
-  ``<description></description>`` : Describes the message, and will be converted into comments during C/C++ code generation
-  ``<field></field>`` : Encodes one field of the message
-  ``type=“float”`` Defines data type of the field

Compiling XML to C/C++
~~~~~~~~~~~~~~~~~~~~~~

The message definition is compiled into C/C++ code by a python script ``mavgen.py``. This script is part of a set of python tools under **Pymavlink** located at ``\pymavlink\generator``. 

In order to run ``mavgen.py`` at the ``\message_definitions`` directory, add the following line to the end of ``.bashrc``. This adds the path to ``mavgen.py`` to Cygwin

::
  export PATH=$PATH:$HOME/ardupilot/modules/mavlink/pymavlink/generator


-  XML: .xml file of message definition
-  ``-o``: Specify output directory, generally set to be ``mavlink/include/mavlink/v2.0``. Code generation will create a new folder in this directory to contain the message headers.
-  ``--lang``: 

 C++11 for PX4 Firmware and QGroundControl mavlink library
-  Validate/Validate Units: Manually check your .xml definition, and omit validation to avoid getting spurious errors

Sending custom MAVLink messages
-------------------------------

Sending and receiving MAVLink messages is mostly handled in the various C/C++ files in ``src/modules/mavlink``.

A class for the message should be created in ``mavlink_messages.cpp``.
First, add the headers of the uORB and mavlink messages.
``c++ #include <uORB/topics/ANALOG_POT.h> #include <v2.0/custom_msgs/mavlink_msg_ANALOG_POT.h>``

Then create the class 

.. code:: c++

  class MavlinkStreamAnalogPot : public MavlinkStream
  {
  public:
      const char *get_name() const
      {
          return MavlinkStreamAnalogPot::get_name_static();
      }
      static const char *get_name_static()
      {
          return "ANALOG_POT";
      }
      static uint16_t get_id_static()
      {
          return MAVLINK_MSG_ID_ANALOG_POT;
      }
    uint16_t get_id()
    {
      return get_id_static();
    }
      static MavlinkStream *new_instance(Mavlink *mavlink)
      {
          return new MavlinkStreamAnalogPot(mavlink);
      }
      unsigned get_size()
      {
          return MAVLINK_MSG_ID_ANALOG_POT_LEN + MAVLINK_NUM_NON_PAYLOAD_BYTES;
      }

  private:
      MavlinkOrbSubscription *_sub;
      uint64_t _analog_time;

      /* do not allow top copying this class */
      MavlinkStreamAnalogPot(MavlinkStreamAnalogPot &);
      MavlinkStreamAnalogPot& operator = (const MavlinkStreamAnalogPot &);

  protected:
      explicit MavlinkStreamAnalogPot(Mavlink *mavlink) : MavlinkStream(mavlink),
          _sub(_mavlink->add_orb_subscription(ORB_ID(ANALOG_POT))),  // make sure you enter the name of your uORB topic here
          _pot_time(0)
      {}

      bool send(const hrt_abstime t)
      {
          ANALOG_POT_s _ANALOG_POT;

          if (_sub->update(&_pot_time, &_ANALOG_POT)) {
              mavlink_ANALOG_POT_t msg = {};  //make sure mavlink_ANALOG_POT_t is the definition of your custom MAVLink message

              msg.error_count = _ANALOG_POT.error_count;
              msg.pot_VCCA = _ANALOG_POT.pot_VCCA; 
              msg.ANALOG_POT_deg = _ANALOG_POT.angle_deg;
              msg.ANALOG_POT_rad = _ANALOG_POT.angle_rad;

        mavlink_msg_ANALOG_POT_send_struct(_mavlink->get_channel(), &msg);

        return true;
          }

          return false;
      }

  };


Finally append the stream class to the ``streams_list`` at the bottom of
the code. 

.. code:: c++

  static const StreamListItem streams_list[] = {

    StreamListItem(&MavlinkStreamAnalogPot::new_instance, &MavlinkStreamAnalogPot::get_name_static, 
    &MavlinkStreamAnalogPot::get_id_static),

  }

Enabling the MAVLink stream
---------------------------

To enable the MAVLink stream, edit ``mavlink_main.cpp`` and add relevant messages to ``configure_streams_to_default``.

First include the MAVLink message header in ``mavlink_main.h`` 

.. code:: c++

  #include <v2.0/custom_msgs/mavlink_msg_analog_pot.h>

Then append the configure function to each case matching a MAVLink mode in which the
MAVLink stream should be enabled 

.. code:: c++

  case MAVLINK_MODE_NORMAL:

    configure_stream_local("ANALOG_POT", 10.0f);

    break;

Alternately, add a line to the startup script ``extras.txt`` on the SD card to enable the streaming. > mavlink stream -r 50 -s POT\_ANGLE -d
/dev/ttyACM0

Parameters are ``-r``, ``-s`` and ``-d``, which configures the data rate, MAVLink message, and serial port of the stream.

Reading the MAVLink Stream
--------------------------

To read the MAVLink Stream, QGroundControl should be rebuilt with the custom MAVLink message headers. This simply entails repeating the steps
in the above section **Creating custom MAVLink message** in ``qgroundcontrol/libs/mavlink/include/mavlink/v2.0``

*MAVLink is a submodule in QGroundControl firmware too*

The composition of the .xml message definition becomes important here.
``c++   <include>common.xml</include>   <include>ardupilotmega.xml</include>``
Both ``common.xml`` and ``ardupilotmega.xml`` definitions must be
included. The latter is a superset of all MAVLink messages and has an
include line for ``common.xml`` within it. However, C++ code generation
omits nested includes. Hence, both .xml files must be included, and
omission of either will break the build.

Configuring QGroundControl build
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Add a file ``user_config.pri`` in the QGroundControl base directory and
enter the following line.

.. code:: qml

    MAVLINK_CONF = custom_msgs

This will tell QGroundControl to build the customized MAVLink library.
