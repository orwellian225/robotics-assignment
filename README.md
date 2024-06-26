# COMS4045A Robotics Assignment

## Members

* Lisa Godwin
* Brendan Griffiths
* Nihal Ranchod
* Zach Schwark


## Project Overview

[Assignment PDF](./assignment2024.pdf)

## Preprocessing

Once a map is generated, it needs to be feed through a pre-processor to generate a navigation graph

```python
python scripts/process_map.py <map_image_filepath> <map_yaml_filepath> <output_directory> <number_of_vertices> <number_of_neighbours>
```

## Controlling SurvBot

SurvBot has a dedicated controller script.

Use the following command in a new terminal window:
```bash
cd robotics_assignment_ws/robotics-assignment
python scripts/survbot_controller.py
```

## Running

### Download

```bash
wget https://lamp.ms.wits.ac.za/robotics/robot_assignment_ws.tar.gz
tar zxvf robot_assignment_ws.tar.gz
cd robot_assignment_ws
rm -rf build

git clone https://github.com/orwellian225/robotics-assignment.git

catkin_make
```

### Creating a Map

#### Terminal Window 1

```bash
cd robot_assignment_ws
source devel/setup.bash

./startWorld
```

#### Terminal Window 2

```bash
source devel/setup.bash
roslaunch turtlebot_gazebo gmapping_demo.launch
```

#### Termnial Window 3
```bash
source devel/setup.bash
catkin_make turtlebot_interactions
roslaunch turtlebot_rviz_launchers view_navigation.launch
```

#### Terminal Window 4
```bash
source devel/setup.bash
roslaunch kobuki_keyop keyop.launch
```

#### Terminal Window 5
Saving a map -> make a maps directory
```bash
mkdir maps
cd maps
rosrun map_server map_saver -f <map_name>
```

### Running SurvBot

#### Terminal Window 1

```bash
cd robot_assignment_ws
source devel/setup.bash

./startWorld
```

#### Terminal Window 2

```bash
cd robot_assignment_ws
source devel/setup.bash

python robotics-assignment/SurvBot.py
```

#### Terminal Window >2

Startup of window

```bash
cd robot_assignment_ws
source devel/setup.bash
```

Setting navigation target

```bash
# NOTE: The coordinates cannot be negative here (pain)
rostopic pub survbot/state/navigate/goal geometry_msgs/Vector3 '0.0' '0.0' '0.0'
```

Changing Surv State

```bash
rostopic pub survbot/state std_msgs/String "NAVIGATE" # Move Surv towards the 
rostopic pub survbot/state std_msgs/String "IDLE" # Stop Surv from moving
rostopic pub survbot/state std_msgs/String "PATROL" # Move Surv along a cycle in the graph
```