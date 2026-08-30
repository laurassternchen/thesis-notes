# Start your custom Gazebo world
ros2 launch nav2_bringup tb3_simulation_launch.py \
    headless:=False \
    world:=/home/ws/install/my_robot_sim/share/my_robot_sim/worlds/world.sdf \
    map:=/home/ws/install/my_robot_sim/share/my_robot_sim/maps/map.yaml \
    x_pose:=0.0 \
    y_pose:=0.0 \
    yaw:=0.0  

It should start:
Your world
    ↓
Gazebo
    ↓
TurtleBot3 at (0, 0)
    ↓
Laser / odometry / TF
    ↓
Nav2
    ↓
Your map.pgm
    ↓
RViz