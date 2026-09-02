# First Setup: Running the Default Nav2 Map and Gazebo World

## Installation

- Install `docker.io`
  - More info: https://docs.ros.org/en/foxy/How-To-Guides/Setup-ROS-2-with-VSCode-and-Docker-Container.html
- Install ROS2
- Install Nav2
- Install Gazebo

See: https://docs.nav2.org/getting_started/index.html#installation

## Starting Docker

1. Start the container:

   ```
   docker start 3f0944698fa6
   ```

   - `docker run <id>` creates a new container with `<id>`

2. Enter the container:

   ```
   docker exec -it 3f0944698fa6 bash
   ```

   - Container id: `3f0944698fa6`
   - Find the container id: `docker ps`

   You're now inside the container.

3. Source the ROS setup script:

   ```
   source /opt/ros/jazzy/setup.bash
   ```

   - `jazzy` is the ROS distro name
   - Find the distro name: `echo $ROS_DISTRO`

4. Launch the simulation:

   ```
   ros2 launch nav2_bringup tb3_simulation_launch.py headless:=False
   ```

   See: https://docs.nav2.org/getting_started/index.html#installation
