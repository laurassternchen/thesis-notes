# 2nd set-up (dynamic risk constrainer / "drc")

- Robot discovers zones itself while driving, instead of knowing them from the
  start (unlike Experiment 2/3). Keepout zones block movement, speed zones
  slow it down + limit acceleration.

## New nodes

- `zone_map_server`
  - standard `nav2_map_server` (same program as `map_server`/`keepout_mask_server`)
  - loads `zone_map.pgm`/`.yaml` (hidden ground truth, Nav2 itself never sees it)
  - remapped `/map` → `/zone_truth`

- `lifecycle_manager_zones`
  - standard `nav2_lifecycle_manager`
  - manages only `zone_map_server`

- `pose_adapter` (custom, `scripts/drc/pose_adapter.py`)
  - looks up `map → base_footprint` TF, republishes as `/pose_in`
  - ~20Hz
  - exists so the other two nodes don't care where the pose comes from

- `zone_detector` (custom, `scripts/drc/zone_detector.py`)
  - compares robot position (`/pose_in`) against `/zone_truth`
  - reveals zone cells only once robot is within `detection_range` (3m)
  - remembers revealed cells (doesn't forget once seen)
  - publishes:
    - `/detected_zones` (OccupancyGrid, only revealed cells)
    - `/zone_obstacles` (PointCloud2, keepout cells only, republished every
      tick so laser clearing doesn't erase them)

- `dynamic_risk_constrainer` (custom, `scripts/drc/dynamic_risk_constrainer.py`)
  - sits in the velocity command path: `cmd_vel_smoothed` → [this] → `cmd_vel_safe`
  - if robot is in a detected zone: clamps speed + rate-limits accel/decel
  - fail-safe: no pose/grid yet, or pose older than 0.5s → treat as "in zone"
  - also publishes `/constrainer/in_zone` (Bool)

## How they were added

- new files:
  - `scripts/drc/pose_adapter.py`, `zone_detector.py`, `dynamic_risk_constrainer.py`
  - `launch/drc/sim.launch.py`
  - `config/drc/nav2_params.yaml`
  - `maps/zone_map.pgm`, `zone_map.yaml`
- `CMakeLists.txt`: added `install(PROGRAMS ...)` for the 3 new scripts
  (same pattern as `zone_accel_limiter.py`)
- `package.xml`: added `rclpy`, `nav2_msgs`, `rcl_interfaces`, `sensor_msgs_py`,
  `tf2_ros`
- scripts made executable (`chmod +x`)
- launched via `ros2 launch my_robot_sim sim.launch.py` (only one file has
  this exact name now, old keepout one renamed to `sim_keepout.launch.py`)

## Changes to existing config (`config/drc/nav2_params.yaml`)

- `collision_monitor.cmd_vel_in_topic`: `cmd_vel_smoothed` → `cmd_vel_safe`
  (so the constrainer sits in between)
- both costmaps (`global_costmap`, `local_costmap`): added a `zones`
  obstacle source
  - `topic: /zone_obstacles`, `data_type: PointCloud2`
  - `clearing: False` (laser would otherwise clear these fake obstacles)
- `collision_monitor`: added its own `zones` source too, as a backstop

## Bugs found + fixed along the way

- `CMakeLists.txt`: closing `)` in wrong place, broke the `install()` call
- `launch/drc/sim.launch.py`: `params_file` pointed at a path that doesn't
  exist (`config/nav2_params_zones.yaml` instead of `config/drc/nav2_params.yaml`)
- scripts not executable yet (step 1 not finished)
- stale symlink from old `sim.launch.py` (pre-rename) collided with the new
  one, made `ros2 launch ... sim.launch.py` ambiguous
- `headless`/`use_rviz` were hardcoded in the launch file, not overridable
  → added as real launch arguments
- `cmd_vel_smoothed` type mismatch: `velocity_smoother` publishes `Twist`,
  constrainer was written for `TwistStamped` → set `STAMPED = False`

## Known issues, not fully solved yet

- the 3 custom nodes have died silently at least once mid-session, with no
  crash trace in their logs — robot goes fully unresponsive when this
  happens (no `cmd_vel_safe` publisher left), unrelated to robot location
  - check: `ros2 node list` should always show `pose_adapter`,
    `zone_detector`, `dynamic_risk_constrainer`
- AMCL pose can jump visibly in RViz after a goal is sent, if the initial
  pose estimate was imprecise — classic particle-filter re-convergence, not
  fully explained why it's more visible here than on the older launch files
- X11/RViz needs `xhost +local:` after every host reboot, otherwise GUI
  crashes with `Authorization required` / `could not connect to display`
