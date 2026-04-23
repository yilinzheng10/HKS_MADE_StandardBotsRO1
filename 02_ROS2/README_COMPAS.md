grasshopper interop:
https://github.com/compas-dev/tutorials/blob/main/fastapi/README.md

References:
https://forum.vention.io/t/understanding-movej-vs-movel-in-robot-programming/462

*************************************
ip address
command line: ipconfig

network interface name
command line: Get-NetAdapter | Select-Object Name, Status, InterfaceDescription
**************************************
ROS2 Bridge over LAN
1. ping IP address of the robot before testing the robot

*************************************
dependencies for Compass
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