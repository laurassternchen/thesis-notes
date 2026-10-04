# ROS 2 Set up and workspace


cd /home/ws
colcon build --packages-select my_robot_sim --symlink-install
source install/setup.bash

1. Inside container
source /opt/ros/jazzy/setup.bash  
source /home/ws/install/setup.bash

2. Useful to verify
- ros2 pkg list | grep my_robot_sim  
You should get: my_robot_sim  
Also
- ros2 pkg prefix my_robot_sim  
should give 
- /home/ws/install/my_robot_sim

3. For custom map and world
ros2 launch my_robot_sim sim.launch.py - keepout experiment


ros2 launch my_robot_sim sim_speed.launch.py - speed limit in keepout zone

