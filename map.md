# Create a pgm file for gazebo map

1. isntall pyhton3
    sudo apt update
    sudo apt install python3-full

2. Create virtual environment 
    python3 -m venv ~/map_env

    activate it:
    source ~/map_env/bin/activate

3. Install Pillow - Python imaging library which works with many different file formats
    pip install Pillow

Check if it has worked:  
        python -c "from PIL import Image; print('Pillow works')"  
    Should give you:  
        Pillow works

4. File set-up

Info pgm - portable graymap  
Eine PGM-Datei (Portable Graymap) ist ein einfaches, unkomprimiertes Bildformat für Graustufenbilder. Jedes Pixel speichert einen Grauwert von Schwarz bis Weiß. Das Format ist leicht zu lesen und wird oft in der Bildverarbeitung oder der Robotik (zum Beispiel für Karten in der Navigation) genutzt  
    ~/map/
    ├── generate_map.py
    ├── map.pgm
    ├── map.yaml
    ├── (slow_zone.pgm)
    └── (slow_zone.yaml)
    - map.pgm only contains code for empty map (no obstacles, zones etc)

4.1. 
- Open python file - nano generate_map.py
- Paste code

4.2. Acivate virtual environment - source ~/map_env/bin/activate

4.3. Run code - python generate_map.py

5. Generate yaml file 
Info yaml - Ain't markup language leicht lesbares Format, um Daten strukturiert zu speichern. Es wird sehr oft für Einstellungen und Konfigurationen von Programmen genutzt, weil es für Menschen einfach zu verstehen ist.


## Loading map into nav2 
1. Start docker and ros2 &rarr; README.md until step 2

1.2. map directory currently on Ubunutu host, not inside docker container  
-  Copy map directory into container: docker cp ~/map <docker id>:/<copy destination>/
- docker cp ~/map 3f0944698fa6:/home/

2. ros2 launch nav2_bringup tb3_simulation_launch.py \
    headless:=False \
    map:=/home/map/map.yaml

    3f0944698fa6

## Project set-up in container and on laptop

Host                              Container

~/ros2/src/my_robot_sim/   →      /home/ws/src/my_robot_sim/

### Host
    ~/ros2/src/my_robot_sim/
    │
    ├── maps/
    │   ├── map.pgm
    │   └── map.yaml
    │
    └── worlds/
        └── my_room.sdf

### Container
    And because of your Docker mount, these will appear inside the container as:  
    /home/ws/src/my_robot_sim/
    ├── maps/
    │   ├── map.pgm
    │   └── map.yaml
    └── worlds/
        └── my_room.sdf  
These files are persistent. They're actually stored on your Ubuntu host, so restarting or recreating the container won't delete them.


                         y = +10 m
                             ↑
             400 px          │
       ┌─────────────────────┐
       │                     │
       │                     │
       │                     │
       │        MAP          │
       │      20 × 20 m      │
       │                     │
       │                     │
       │                     │
       └─────────────────────┘
x = -10 m                    x = +10 m
       │                     │
       └──────────┬──────────┘
                  ↓
              y = -10 m

