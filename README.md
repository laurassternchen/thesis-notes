# First set-up and running default nav2 map and gazebo world
- installation docker.io. 
    - For more information: https://docs.ros.org/en/foxy/How-To-Guides/Setup-ROS-2-with-VSCode-and-Docker-Container.html
- installation ROS2
- installation nav2
- installation gazebo
--> https://docs.nav2.org/getting_started/index.html#installation


Starting docker
Try starting like this: 
1. docker start 3f0944698fa6
  
- docker run <id> creates a new container with <id>

Enter container: docker exec -it 3f0944698fa6 bash
    Container id: 3f0944698fa6
    Find out container id: <command>
Now you're inside the container

2. Run inside container: source /opt/ros/jazzy/setup.bash
- jazzy is the name of the ros_distro
- Find out name: echo $ROS_DISTRO

3. ros2 launch <package_name> <launch_file>.launch.py
ros2 launch nav2_bringup tb3_simulation_launch.py headless:=False
    https://docs.nav2.org/getting_started/index.html#installation

