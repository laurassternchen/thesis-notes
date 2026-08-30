



My sim.launch.py
       │
       ▼
nav2_bringup/tb3_simulation_launch.py
       │
       ├── Gazebo
       ├── TurtleBot3
       ├── robot_state_publisher
       ├── Nav2
       └── RViz
ros2 launch nav2_bringup tb3_simulation_launch.py headless:=False

The launch file orchestrated the whole simulation