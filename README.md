# Mobile Robotics — Assignment 2: Bug 2 Path Planning

Implementation of the **Bug 2** reactive path-planning algorithm for TurtleBot3 Burger in ROS 2 Humble and Gazebo Classic (`a2_worlds/bug2.py`).

## Student Information
- **Name:** Bagas Aryo Dananjoyo
- **NRP:** 5024241031 (`1031 mod 5 = 1`)
- **Initial Lateral Offset (`Y0`):** `-0.10` m (`0.10 * 1 - 0.20`)

## Five Run Configurations

All five experiments were executed with `MODE = 'bug2'` and `Y0 = -0.10` starting at `(0.00, -0.10)` toward the goal at `(10.00, 0.00)`:

| Run (`RUN_NAME`) | World | `Y0` | `MODE` | `RUN_NAME` | `HAND` | `DELTA` | Description |
| :--- | :--- | :---: | :---: | :--- | :---: | :---: | :--- |
| **Run 1** | `bug_1_circles` | `-0.10` | `'bug2'` | `'w1'` | `+1` | `0.25` | Baseline left-hand wall following on circular obstacles |
| **Run 2** | `bug_2_boxes` | `-0.10` | `'bug2'` | `'w2'` | `+1` | `0.25` | Baseline left-hand wall following on rectangular box obstacles |
| **Run 3** | `bug_3_concave` | `-0.10` | `'bug2'` | `'w3'` | `+1` | `0.25` | Baseline left-hand wall following on concave U-shaped obstacle |
| **Run 4** | `bug_1_circles` | `-0.10` | `'bug2'` | `'w1_naive'` | `+1` | `-1e6` | Naive run with progress test disabled (`DELTA = -1e6`) |
| **Run 5** | `bug_3_concave` | `-0.10` | `'bug2'` | `'w3_right'` | `-1` | `0.25` | Right-hand wall following (`HAND = -1`) on concave obstacle |

## Build and Run Instructions

From a clean workspace (`~/mobile_robotics_ws`):

```bash
source /opt/ros/humble/setup.bash
export TURTLEBOT3_MODEL=burger
export GAZEBO_MODEL_PATH=$GAZEBO_MODEL_PATH:/opt/ros/humble/share/turtlebot3_gazebo/models

cd ~/mobile_robotics_ws
colcon build --packages-select a2_worlds
source install/setup.bash

# Terminal 1: Launch Gazebo simulation
ros2 launch a2_worlds bug_world.launch.py world:=bug_1_circles y_pose:=-0.10 spawn_delay:=25

# Terminal 2: Run Bug 2 controller
source install/setup.bash
ros2 run a2_worlds bug2
```bash
cat << 'EOF' > ~/a2_submission/SOURCES.md
# Sources & Acknowledgments

## Academic References
1. Lumelsky, V. J., & Stepanov, A. A. (1987). *Path-planning strategies for a point mobile automaton moving amidst unknown obstacles of arbitrary shape*. Algorithmica, 2(1–4), 403–430. (Foundational definition of the Bug 2 algorithm, m-line construction, hit/leave point latching, and worst-case perimeter bound analysis).
2. McGuire, K. N., de Croon, G. C. H. E., & Tuyls, K. (2019). *A comparative study of bug algorithms for robot navigation*. Robotics and Autonomous Systems, 121, 103261 (arXiv:1808.05050). (Comparative taxonomy of reactive sensor-based Bug variants, practical LIDAR noise/standoff considerations, and concave obstacle traversal behavior).

## Software, Frameworks & Simulation Assets
3. **Course Assignment Package (`a2_worlds`):** Provided Mobile Robotics Assignment 2 starter package containing Gazebo Classic worlds (`bug_1_circles`, `bug_2_boxes`, `bug_3_concave`), launch files, RViz2 marker plumbing, and CSV logging utilities (`bug2.py` skeleton).
4. **ROS 2 Humble Hawksbill & Gazebo Classic:** Open Robotics documentation for `rclpy`, `sensor_msgs/msg/LaserScan` (REP-117 finite range filtering), `nav_msgs/msg/Odometry`, `geometry_msgs/msg/Twist`, and `visualization_msgs/msg/MarkerArray`.
5. **TurtleBot3 Burger Simulation Model:** ROBOTIS TurtleBot3 Gazebo package (`turtlebot3_burger` kinematic dimensions and 360-degree LIDAR configuration).

## AI Assistance Disclosure
- **Tool Used:** Google Gemini
- **Scope of Assistance:** Assisted in structuring the four `TODO` logic blocks in `a2_worlds/bug2.py` (proportional m-line heading control, hit-point latching, progress-filtered leave condition, and P-controlled contour following), debugging Docker/ROS 2 environment commands, and drafting documentation (`README.md`, `SOURCES.md`, and report structure). All simulation runs, parameter tuning, and verification were executed and validated directly by the student.
