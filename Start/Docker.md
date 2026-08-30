# Start Docker

1. Start docker desktop
- <command>

2. Find your existing container
- docker ps -a

3. Start container
- docker start <container-name>
- docker start 3f0944698fa6

4. Enter container
- docker exec -it 3f0944698fa6 bash  

Now you are in the ROS 2 Docker container