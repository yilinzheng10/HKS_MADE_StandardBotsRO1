Architecture:
*Standard Bots RO1*
RO1 receives:
JointTrajectory or REST motion command or Modbus registers

|

*ROS2*
ROS2 converts Python data into ROS messages
Stream points
Cameras

|

*COMPAS + Python*
Produces joint angles: Frame → IK solution → Joint values

|

*Grasshopper/Rhino*
Geometry (curves, frames, meshes, targets), Toolpath curves, Waypoints, Custom parameters (speed, accel, zone, tool id)

**********************************
Standard bot
https://github.com/standardbots/ros2-realtime-api
https://standardbots.notion.site/Standard-Bots-RO1-URDF-Files-6eb8e9d6a42440baa779e4f0e6c80d79 

https://compas.dev/#/gettingstarted

grasshopper interop:
https://github.com/compas-dev/tutorials/blob/main/fastapi/README.md


References:
https://forum.vention.io/t/understanding-movej-vs-movel-in-robot-programming/462

*************************************
ip address
command line: ipconfig

dependencies
1. conda
2. python

Installation (anaconda prompt)
1. install: 
conda create -n compas-dev -c conda-forge c

2. activate environment: 
conda activate compas-dev

3. Start the Python interpreter: 
python

4. Import the core compas package and print the installed version: 
>>> import compas
>>> compas.__version__
'2.0.0'