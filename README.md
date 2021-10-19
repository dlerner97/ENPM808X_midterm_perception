# ACME_perception_proposal
[![Build Status](https://app.travis-ci.com/dlerner97/ACME_perception_proposal.svg?branch=position_estimator_test)](https://app.travis-ci.com/dlerner97/ACME_perception_proposal)
[![Coverage Status](https://coveralls.io/repos/github/dlerner97/ACME_perception_proposal/badge.svg?branch=main)](https://coveralls.io/github/dlerner97/ACME_perception_proposal?branch=main)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

Please note that we are having issues setting up Travis-CI. The code does in fact build on our local machines despite the Travis build status.
### Authors

- Dani Lerner (dlerner97)
- Diane Ngo (dngo13)

### Information

In this proposal, we will develop a human obstacle detection prototype for mobile robots.
This repo contains coursework for the ENPM808X Midterm project. 

### Midterm Phase 1 Deliverables

- The most recent activity diagram and UML can be found in the [visual reps](/visual_reps) folder while the original UML can be found in the [Original UML](/original_UML) folder.

### Required Libraries and Software

  1. **OpenCV**. Please install the OpenCV package using the following link: [OpenCV Installation](https://docs.opencv.org/3.4.15/d7/d9f/tutorial_linux_install.html)
  2. **Eigen**. This library should already be installed on a standard linux system.
  3. **math**. This library should already be installed on a standard linux system.
  4. **C++ std libs**. This library is certainly installed on a standard linux system.
  5. **CMake**. This software should already be installed on a standard linux system.

### Running the code
To run the code, open a terminal and follow these steps:

1. Clone the repo.

  FOR HTTPS CLONES:

```bash
git clone https://github.com/dlerner97/ACME_perception_proposal.git
```

  FOR SSH CLONES:

```bash
git clone git@github.com:dlerner97/ACME_perception_proposal.git
```

2. Build the repo.

```bash
cd <absolute path>/ACME_perception_proposal
mkdir build && cd build
cmake ..
make
```

3. To run the main script,

```bash
./app/shell-app
```

4. To run the tests,

```bash
./test/cpp-test
```

### Agile Iterative Process (AIP)

Please see this link to find our [AIP implementation document](https://docs.google.com/spreadsheets/d/1gBVo8C_xLlcH5OcivV810-puy338YpDrUkYAftgQPH4/edit?usp=sharing).

### Visual Representations of Code

The following Activity Diagram and UML's are jpg files and will not work well with zoom. See the [visual representation pdf](/visual_reps/activity_and_UML.drawio.pdf) for a higher quality image.

#### Activity Diagram

![Original activity diagram of the perception stack.](/visual_reps/activity_diagram.jpg?raw=true "Activity Diagram")

#### UML

![Original UML of the perception stack.](/visual_reps/UML.jpg?raw=true "UML")

### Input Robot Parameters

Since our vision system is hardware agnostic, one must simply edit the [robot_params](/robot_params/robot_params.txt) text file to input their respective robot parameters. The robot parameters include: 
- Partial transform (in x, y, z, pitch) between the robot's center of mass (or any other desired model center). 
- Average height of humans in order to estimate the z-location of a detected human. [Source](https://www.worlddata.info/average-bodyheight.php).
- The probability threshold for classification. Any image with a probability threshold less than this value will be considered noise and subsequently discarded. If the probability is larger than the threshold we will consider there to be a human in sight.
- Image height and width. This parameter depends on the specific detection algorithm we will be using. 
- Low and high alert thresholds. The robot should have different reactions to humans at different distances. If the distance < high_alert, the robot should stop. If the the high_alert < distance < low_alert, the robot should plan new path and if the human is further than the low alert, we should ignore them until they come within the given distances.
- Camera matrix parameters.
    
Many of these parameters are arbitrary since we do not have a physical robot to test. However, some are carefully selected to provide a realistic system. For example, while the distances between the camera and robot center are selected at random, the pitch is 90 deg. This is because robot "x" generally corresponds to a camera's "z." Additionally, the proposal specifies that the camera is "front-facing" and therefore, we prevent any roll or yaw from occuring. We believe that this is realistic in the real world as well since cameras are nearly always horizontal and front facing. However, the pitch can still be selected in case the user would like to change this parameter. If changing, please remember that the value must be 90 + *desired pitch* to account for the change in coordinate system.

#### Visual of Camera Position

![Camera Position visual](/visual_reps/Physical_Robot.jpg?raw=true "Camera Position")

### Position Estimation Methods
In this section, we will briefly describe our world frame position estimation methods. We will start with a diagram and equations we used to derive the equations and will then solve one of the cases we used in our position estimation unit tests. Note that the actual code in the PositionEstimation class uses matrix multiplication to solve the camera to robot transform.

The pinhole model of a camera can be seen below and this simplification gives us the following derivations.

![Pinhole Model](/visual_reps/position_estimation-CDE.drawio.png?raw=true "Pinhole Model") 
---
<img src="/visual_reps/LaTex_Imgs/deriving_camera_z_eq0.png" width="200">
<img src="/visual_reps/LaTex_Imgs/deriving_camera_z_eq.png" width="600">
<img src="/visual_reps/LaTex_Imgs/deriving_camera_xy.png" width="800">
---

We will now use these derivation to solve one of the unit test scenarios.

![Unit Test Scenario](/visual_reps/position_estimation-scenario.drawio.png?raw=true "Unit Test Scenario")
---
$$z_{camera}=\frac{l_{obj}*f*px\_density}{detection.height}=\frac{2*.05*1000}{100}=1\ m$$
<img src="/visual_reps/LaTex_Imgs/z_camera.png" width="400">
<img src="/visual_reps/LaTex_Imgs/x_camera.png" width="600">
<img src="/visual_reps/LaTex_Imgs/y_robot.png" width="600">
<img src="/visual_reps/LaTex_Imgs/x_robot.png" width="300">
<img src="/visual_reps/LaTex_Imgs/z_robot.png" width="300">
---
