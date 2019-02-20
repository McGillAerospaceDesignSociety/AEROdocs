Ardupilot Development Quickstart
================================

This document provides an alternate set of instructions to setup a build
environment for the ArduPilot flight control system. In contrast to the
`Ardupilot’s official documentation`_, this document is condensed with
some low level information removed.

Setting up a build environment for Ardupilot consists of four primary
elements:

1. Setup Build Environment on Windows using Cygwin
2. Setup Git and obtain (*clone*) Arducopter source code
3. Build with waf and upload to autopilot hardware
4. Set up SITL simulation to test fly vehicles
5. Set up Eclipse IDE to streamline development (WIP)

Setup Build Environment
=======================

For official documentation for this step `click here`_

Install Cygwin
--------------

Cygwin is an environment to allow programs in Linux or Unix-like systems
to be compiled and ran natively on Windows. Installation steps are as
follows.

1. Go to the `Cygwin Install Page`_ and download the installer
   ``setup-x86_64.exe``

2. Open a command prompt and change directory to download location of
   ``setup-x86_64.exe``, then run the following command

   ::

      setup-x86_64.exe -P autoconf,automake,ccache,gcc-g++,git,libtool,make,gawk,libexpat-devel,libxml2-devel,libxslt-devel,python2-devel,python2-future,python2-libxml2,python2-pip,procps-ng,gdb,ddd,zip

   This command specifies the entire list of packages to be installed,
   bypassing the need to individually lookup each package in the *Select
   Packages* dialog.

3. Accept the all the prompts and default file locations. Click **Next**
   at the *Select Packages* dialog, then accept all remaining prompts
   and default options (including the additional dependencies)

4. Select Finish to start downloading and installing the packages[1]

Additional setup in Cygwin
--------------------------

Two final setup steps are done from the **Cygwin Terminal**. This is a
terminal application like Windows Command Prompt but admits Linux
commands like ``ls`` or ``grep``

1. Open **Cygwin64 Terminal** then quit it to create initialisation
   files for the user in the Cygwin home directory.

2. Open a new instance of **Cygwin64 Terminal**, run the following
   commands to install additional Python packages

   ::

      pip2 install empy
      pip2 install pyserial
      pip2 install pymavlink

Install the GCC compiler
------------------------

1. Download the GCC Compiler’s installer from the `Tools page`_ in
   Ardupilot firmware and run the installer

2. Accept the license and default install location

3. Accept the ssl certificate

4. Before finishing the installation, check the option to **Add path to
   environment variable**

.. warning::
   If step 4 is omitted, the path must be manually added to environment variables. This is not straightforward. Otherwise, the compiler will be unusable.


Setup Git and clone source code
===============================

The ArduPilot project uses **git** for source code management and **GitHub** for source code hosting. To download and build the source code, the developer must:

1.  Install a git client on your local computer. There are two options:

   a. Command line in the Cygwin terminal
   b. GUI client

2.  Download a copy of the source code to your local computer

Install Git Client
------------------

Option 1a, command line in the Cygwin terminal, will be available as long as the Cygwin installation is done correctly 

.. note::
   *git* is one of the many packages selected during installation

Option 1b, GUI client, requires separate installation of the client software, such as `GitHub Desktop`_
. GUI clients will not be discussed below, but their usage should be fairly intuitive.

Cloning source code
-------------------

Ardupilot source code is hosted on Github: `Ardupilot Repository`_

Clone refers to creating and downloading a local copy of the source code. This copy must be downloaded into the Cygwin file structure. The steps are as follows

1.  Open a new **Cygwin64 Terminal**
2.  Change directory to C:/cygwin64/home/user/, where user may be any account or username
3.  Run the command 

   ::

         git clone https://github.com/ArduPilot/ardupilot.git

4.  Change directory to the newly created C:/cygwin64/home/user/ardupilot
5.  Run the command 

   ::

         git submodule update --init --recursive

.. note::
   Submodules hold external code repositories that Ardupilot is dependent on, such as **ChibiOS**, the RTOS that Ardupilot uses, and **Waf**, the build system. 

.. warning::
   Forgetting to update submodules is a common source of build failure.

When all 5 steps are done, the source code may now be built with waf.

Building the Code
=================

For official documentation click `here3`_

Ardupilot uses the `Waf`_ build system. Waf is python based and has no dependency on additional software or libraries. It also does not rely on a code generator such as Makefiles. Waf is extensively documented in `The Waf Book`_

Preliminaries
-------------

When building Ardupilot, Waf must be called from the root directory of the source code. Following previous documentation, this directory will be
``C:\cygwin64\home\users\ardupilot``

Open a Cygwin64 terminal and enter this directory to proceed to the next steps.

Configure for hardware
----------------------

When building Ardupilot, a configure step is necessary to select the autopilot board. Run the following command when building for Pixracer,
for example.

   ::

       ./waf configure --board Pixracer

Commonly used boards
--------------------

To list all supported autopilots, run the following command

   ::

       ./waf list_boards

Following are some commonly used boards and their corresponding entry to the configure command. 

- ``sitl``: SITL simulator
- ``Pixhawk1``: `Original Pixhawk with 2Mb flash`_ 
- ``CubeBlack``: `Pixhawk 2.1 Cube`_
- ``mRoX21``: `AUAV/mRobotics X2.1`_
- ``Pixracer``: `mRobotics Pixracer`_
- ``Pixhawk4``: `Holybro Pixhawk 4`_
- ``omnibusf4pro``: OmnibusF4 v5 and lower (through holes)
- ``omnibusf4v4``: OmnibusF4 V6 (solder pads)
- ``OmnibusNanoV6``: Omnibus Nano (20mm mounting pattern)
- ``revo-mini``: OpenPilot Revolution Mini
- ``KakuteF4``: HolyBro Kakute F4
- ``KakuteF7``: HolyBro Kakute F7

Build and upload
----------------

Run the following command to build ArduCopter

   ::

       ./waf copter

Similarly, ``./waf plane`` builds ArduPlane and ``./waf rover`` builds ArduRover.

Build commands have a ``--upload`` option for uploading the binary to an autopilot. Run the following command

   ::

       ./waf copter --upload

Linux-based boards require additional steps before building. These steps will not be discussed here.

Setup SITL simulator
====================

For official documentation click `here2`_

SITL (Software-in-the-loop) simulation allows Ardupilot flight code to control a computer modeled vehicle in a simulated world. The pilot can
interact with this vehicle as if it is a real vehicle, using Mission Planner, MAVProxy, or a radio controller/gamepad.

Install MAVProxy
----------------

MAVProxy is a minimal Ground Control utility that uses a command line interface to interact with a vehicle. It is commonly used for testing
and developing ArduPilot.

The installer for MAVProxy can be downloaded
`here <http://firmware.ardupilot.org/Tools/MAVProxy/>`__

Configure paths in Cygwin
-------------------------

The Ardupilot SITL simulator is run with the command ``sim_vehicle.py``. This is a python script that is located in a subdirectory within
ardupilot. To run this script at the root directory, follow the steps below

1. Navigate to ``\home`` in the Cygwin file system and open ``.bashrc`` (Usually ``C:\cygwin\home\user\.bashrc.``)

2. Add the following line to the end of ``.bashrc``. This adds the path to the ``sim_vehicle.py`` to Cygwin.

   ::

       export PATH=$PATH:$HOME/ardupilot/Tools/autotest

3. Exit and restart the instance of Cygwin64 terminal to make the change effective

Install additional Python packages
----------------------------------

::

       python -m ensurepip --user
       python -m pip install --user future
       python -m pip install --user lxml
       python -m pip install --user uavcan

Build Ardupilot SITL
--------------------

Build the ardupilot firmware as previously mentioned with configuration to ``sitl``. Use the following commands.

::

       ./waf configure --board sitl
       ./waf copter

Where copter may be a different product group for which the simulation is run, e.g. \ ``plane``

Install FlightGear simulator
----------------------------

The FlightGear Flight Simulator provides a 3D simulation of the vehicle and its surroundings to allow for visualization of vehicle attitude and
its movement through the environment. Go to the `FlightGear Install Page`_, download the installer and run it.

.. tip::
   As FlightGear is installing, it is highly recommended to accept any prompt to install scenery automatically

Running SITL
------------

1. Start **Cygwin64 Terminal** and change directory to ``/ardupilot/ArduCopter``, or ``/ardupilot/ArduPlane``, depending on vehicle to be simulated

2. Call ``sim_vehicle.py`` using the following command

   ::

       sim_vehicle.py --map --console

3. SITL and MAVProxy will start. MAVProxy displays three windows:

-  MAVProxy command line interface
-  Console that displays vehicle status and messages
-  2D map that shows vehicle position and can be used (via right-click)
   to control vehicle movement and missions.

4. Start Mission Planner, which interfaces with the SITL simulator via UDP at port 14550 or 14551. To ascertain the UDP port that SITL is outputting data at, use the following command

   ::

       GUIDED> output
       GUIDED> 2 outputs
       0: 127.0.0.1:14550
       1: 127.0.0.1:14551

5. In Mission Planner, use the UDP option with the correct port specified to connect to the vehicle. Once connected, interact with the vehicle as
if it is an actual UAV connected over radio telemetry.

6. To enable control via radio controller, plug in a radio controller that can function like a joystick, e.g. Taranis X9D, and make appropriate configurations in Mission Planner.

Low level vehicle command and control via MAVProxy directly is out of scope of this document. Consult `here`_ for details.

.. _here: http://ardupilot.org/dev/docs/copter-sitl-mavproxy-tutorial.html

.. _here2: http://ardupilot.org/dev/docs/sitl-native-on-windows.html

.. _here3: https://github.com/ArduPilot/ardupilot/blob/master/BUILD.md
.. _FlightGear Install Page: http://www.flightgear.org/download/
.. _Ardupilot repository: https://github.com/ArduPilot/ardupilot
.. _Github Desktop: https://desktop.github.com/
.. _Waf: https://waf.io/
.. _The Waf Book: https://waf.io/book/
.. _Original Pixhawk with 2Mb flash: https://store.mrobotics.io/Genuine-PixHawk-Flight-Controller-p/mro-pixhawk1-minkit-mr.htm
.. _Pixhawk 2.1 Cube: http://www.proficnc.com/content/13-pixhawk2
.. _AUAV/mRobotics X2.1: https://store.mrobotics.io/mRo-X2-1-Rev-2-p/mro-x2.1rv2-mr.htm
.. _mRobotics Pixracer: https://store.mrobotics.io/mRo-PixRacer-R15-Official-p/auav-pxrcr-r15-mr.htm
.. _Holybro Pixhawk 4: https://shop.holybro.com/pixhawk-4_p1089.html
.. _click here: http://ardupilot.org/dev/docs/building-setup-windows-cygwin.html

.. _Cygwin Install Page: www.cygwin.com/install.html
.. _Tools page: firmware.ardupilot.org/Tools/STM32-tools
.. _Ardupilot’s official documentation: http://ardupilot.org/dev/index.html


