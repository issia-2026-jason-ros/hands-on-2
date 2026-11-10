# Hands-on activity

## Table of contents

- [Hands-on activity](#hands-on-activity)
- [1. Introduction](#1-introduction)
- [Exercises](#exercises)
  - [Part 1 - Using ROS to check sensors and actuators](#part-1---using-ros-to-check-sensors-and-actuators)
  - [Part 2 - Implementing a collision-avoidance vehicle](#part-2---implementing-a-collision-avoidance-vehicle)
    - [Preliminaries](#preliminaries)
      - [Getting the base source code](#getting-the-base-source-code)
      - [Running the agent](#running-the-agent)
    - [Exercise 2.1 - Testing the development environment](#exercise-21---testing-the-development-environment)
    - [Exercise 2.2 - Preventing collisions](#exercise-22---preventing-collisions)
    - [Exercise 2.3 - Implementing adaptive speed control](#exercise-23---implementing-adaptive-speed-control)
    - [Exercise 2.4 - Implementing obstacle avoidance](#exercise-24---implementing-obstacle-avoidance)

# 1. Introduction

This activity involves programming an agent to be embedded in a vehicle equipped with the following sensors and actuators:

##### Available actuators:
- Red led;
- Blue led;
- RGB led;
- Buzzer

##### Available sensors:
- Red led sensor;
- Blue led sensor;
- RGB led status;
- Buzzer status;
- Temperature sensor;
- Humidity sensor;
- Luminosity sensor;
- Potentiometer.
  
The sensors and actuators are accessible through ROS topics, as specified in the tables below.

#### Actuators Topics
| Topic name | Data type | Function | Possible parameter values |
|---|---|---|---|
| `/arduino/red_led/command` | `std_msgs/msg/Bool` | Controls the red led | `true`, `false` |
| `/arduino/blue_led/command` | `std_msgs/msg/Bool` | Controls the blue led | `true`, `false` |
| `/arduino/rgb_led/command` | `std_msgs/msg/String` | Controls the rgb led | `white`, `green`, `red`, `blue`, `off` |
| `/arduino/buzzer/command` | `std_msgs/msg/Bool` | Controls the buzzer | `true`, `false` |

#### Sensors Topics
| Topic name | Data type | Function | Possible parameter values |
|---|---|---|---|
| `/arduino/red_led/status` | `std_msgs/msg/Bool` | Reports the red led current state | `true`, `false` |
| `/arduino/blue_led/status` | `std_msgs/msg/Bool` | Reports the blue led current state | `true`, `false` |
| `/arduino/rgb_led/status` | `std_msgs/msg/String` | Reports the rgb led current state| `white`, `green`, `blue`, `red`, `off` |
| `/arduino/buzzer/status` | `std_msgs/msg/Bool` | Reports the buzzer's current state | `true`, `false` |
| `/arduino/temperature_sensor/` | `std_msgs/msg/Float32` | Current temperature | scale: `1.0`<br>offset: `0.0`<br>minimum: `0.0`<br>maximum: `1023.0` |
| `/arduino/humidity_sensor/` | `std_msgs/msg/Float32` | Current humidity level | scale: `1.0`<br>offset: `0.0`<br>minimum: `0.0`<br>maximum: `1023.0` |
| `/arduino/luminosity_sensor/` | `std_msgs/msg/Float32` | Current luminosity level | scale: `1.0`<br>offset: `0.0`<br>minimum: `0.0`<br>maximum: `1023.0` |
| `/arduino/potentiometer/` | `std_msgs/msg/Float32` | Current potentiometer level | scale: `1.0`<br>offset: `0.0`<br>minimum: `0.0`<br>maximum: `1023.0` |

# Exercises

## Part 1 - Using ROS to check sensors and actuators
To check whether the hardware is working properly and become familiar with the available ROS resources, use terminal commands to inspect sensor readings and actuator status, as well as to control the actuators.

First, connect to the vehicle's companion computer via SSH:
- Host name: `kit2.bot.chon.group`
- User name: root
- Password: root

In Unix-based systems, use `ssh root@kit2.bot.chon.group`.   

To inspect sensor readings and actuator status, use:
```
ros2 topic echo <TOPIC NAME>
```

To control the actuators, use:
```perception_action
ros2 topic pub -1 <TOPIC NAME> <TOPIC TYPE> "{data: <VALUE>}"
```

Notes: 
- for boolean topics, VALUE must be either `true` or `false`.
- for string topics, VALUE must be enclosed in single quotes.

## Part 2 - Implementing a collision-avoidance vehicle

###  Preliminaries

#### Getting the base source code
First of all, clone the repository from `https://github.com/issia-2026-jason-ros/hands-on-2`.

This repository contains a JaCaMo application with an agent whose goal is to `exec` around the environment. However, the agent does not have any plans to achieve this goal. In this exercise, you will implement the necessary elements for the vehicle to achieve this goal.

The vehicle companion computer has a copy of this repository in the folder `~/<VEHICLE NAME>`.

#### Running the agent
To run a new version of the agent, you must transfer the changes to the vehicle's companion computer by following these steps:
1. Commit and push the changes to the repository.
2. In the vehicle's companion computer:
   - access the application folder:  `cd ~/<VEHICLE NAME>`.
   - update the files: `git pull`.
   - run the application: `./gradlew run`.
   


### Exercise 2.1 - Testing the development environment
This exercise aims to get familiar with the development environment. 

In its initial version, the agent only prints the message `I do not know how to satisfy this goal.`.  Modify the agent so that it also prints `Hello world`. The expected output is:
```
[chonbot_agent] Hello world.
[chonbot_agent] I do not know how to satisfy this goal.
```
Modify the agent's code, push the changes to the repository, access the vehicle's companion computer via SSH, pull the changes, run the updated version of the agent, and observe the output.

### Exercise 2.2 - Preventing collisions
Make the vehicle move forward and stop when an obstacle is detected.

### Exercise 2.3 - Implementing adaptive speed control
Make the vehicle move forward and adjust its speed according to the distance to the detected obstacle:
- High speed when the obstacle is at a safe distance;
- Default speed when the obstacle is at a moderate distance;
- Low speed when the obstacle is nearby;
- Stop when the obstacle is at a critical distance.

In addition, briefly blink the brake light for a little while (e.g. for 500 milliseconds) whenever the vehicle reduces its speed.


### Exercise 2.4 - Implementing obstacle avoidance
Make the vehicle navigate the environment while avoiding obstacles. Define your own navigation strategy.
Observe the following requirements:
- Blink the brake light whenever the vehicle reduces its speed;
- Blink the corresponding turn signal whenever the vehicle turns left or right.

