# Elegoo Smart Robot Car Kit V4.0

This repository contains the code, tutorials, and necessary resources for the **ELEGOO Smart Robot Car Kit V4.0**.

The ELEGOO Smart Robot Car V4.0 with camera is an educational kit for beginners to get hands-on experience about Arduino programming, electronics assembling, and robotics knowledge. 

## Repository Structure

The project is organized into the following main directories:

### 1. `01_ReadMeFirst`
Important preliminary information and setup instructions before you start working with the robot.

### 2. `02_Manual_and_Main_Code_and_App`
Contains the core files to operate the robot out-of-the-box.
- **`01_User_Manual`**: The complete instruction manual for assembling and using the robot.
- **`02_Main_Program___(Arduino_UNO)`**: The main controller firmware for the Arduino UNO board.
- **`03_APP`**: Mobile application installers/source for controlling the robot.
- **`04_Code_of_Carmer_(ESP32)`**: Firmware for the ESP32 camera module.
- **`05_3D_Model`**: 3D models for the robot chassis and components.

### 3. `03_Tutorial_and_Code`
Step-by-step tutorials and the corresponding code examples to help you learn and customize your robot.
- `01_SmartRobotCarV4.0_Preparation`: Basic setup and sensor testing.
- `02_SmartRobotCarV4.0_Move`: Controlling the motors to move the robot.
- `03_SmartRobotCarV4.0_Tracking`: Line tracking functionality.
- `04_SmartRobotCarV4.0_ServoControl`: Controlling the ultrasonic sensor servo motor.
- `05_SmartRobotCarV4.0_Obstacle`: Obstacle avoidance logic using the ultrasonic sensor.
- `06_SmartRobotCarV4.0_Follow`: Following mode implementation.
- `07_SmartRobotCarV4.0_Others`: Additional examples and functionalities.
- `08_SmartRobotCarV4.0_DIY_and_Program_on_APP`: Instructions for custom programming via the mobile app.

### 4. `04_Related_Chip_Information`
Datasheets and detailed technical information regarding the specific chips and modules used in this kit (e.g., motor drivers, microcontrollers, sensors).

## Getting Started

1. Check the `01_ReadMeFirst` directory for initial guidelines.
2. Follow the `01_User_Manual` inside the `02_Manual_and_Main_Code_and_App` folder to assemble your robot car.
3. Upload the Main Program to your Arduino UNO or follow along with the tutorials in `03_Tutorial_and_Code` to build up your own firmware step-by-step.
4. Install the mobile app to control the car via Bluetooth/Wi-Fi and see the camera stream!

## IR Remote Control Mapping

The infrared remote control is mapped to the following functions on the Smart Robot Car:

### Movement Controls
| Remote Button | Action |
| :--- | :--- |
| **Up Arrow** | Move Forward |
| **Down Arrow** | Move Backward |
| **Left Arrow** | Turn Left |
| **Right Arrow** | Turn Right |
| **OK Button** | Standby Mode (Stops all movement) |

### Operating Modes
| Remote Button | Action |
| :--- | :--- |
| **1** | Enter **Trace-Based Mode** (Line Tracking) |
| **2** | Enter **Obstacle Avoidance Mode** |
| **3** | Enter **Follow Mode** |

### Trace-Based Mode Adjustments 
*(These buttons only work when the car is currently in Trace-Based Mode)*
| Remote Button | Action |
| :--- | :--- |
| **4** | Increase the tracking threshold by 10 (Up to a maximum of 600) |
| **5** | Reset the tracking threshold to its default value (250) |
| **6** | Decrease the tracking threshold by 10 (Down to a minimum of 30) |

### Speed Controls
| Remote Button | Action |
| :--- | :--- |
| **7** | Increase the "Rocker Car Speed" by 5 (Up to a maximum of 255) |
| **8** | Reset the "Rocker Car Speed" to 250 |
| **9** | Decrease the "Rocker Car Speed" by 5 (Down to a minimum of 50) |

## Notes

- This repository represents the V4.0 version of the Smart Robot Car. Make sure your hardware matches this version to avoid incompatibilities.
- The main code relies on specific libraries included within the respective sketch directories.
