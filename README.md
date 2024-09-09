<div id="top"></div>

<h1 align="center">RoboGuard! Autonomous Security Guard</h1>

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://jacobsschool.ucsd.edu/">
    <img src="images\UCSDLogo_JSOE_BlueGold.png" alt="Logo" width="400" height="100">
  </a>
<h3>MAE148 Final Project</h3>
<p>
Team 2 SS02 2024
</p>

![image](images/robot_picture.png)
</div>




<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li><a href="#team-members">Team Members</a></li>
    <li><a href="#final-project">Final Project</a></li>
      <ul>
        <li><a href="#original-goals">Original Goals</a></li>
          <ul>
            <li><a href="#goals-we-met">Goals We Met</a></li>
            <li><a href="#our-hopes-and-dreams">Our Hopes and Dreams</a></li>
              <ul>
                <li><a href="#stretch-goal-1">Stretch Goal 1</a></li>
                <li><a href="#stretch-goal-2">Stretch Goal 2</a></li>
              </ul>
          </ul>
        <li><a href="#final-project-documentation">Final Project Documentation</a></li>
      </ul>
    <li><a href="#robot-design">Robot Design </a></li>
      <ul>
        <li><a href="#cad-parts">CAD Parts</a></li>
          <ul>
            <li><a href="#final-assembly">Final Assembly</a></li>
            <li><a href="#custom-designed-parts">Custom Designed Parts</a></li>
            <li><a href="#open-source-parts">Open Source Parts</a></li>
          </ul>
        <li><a href="#electronic-hardware">Electronic Hardware</a></li>
        <li><a href="#software">Software</a></li>
          <ul>
            <li><a href="#embedded-systems">Embedded Systems</a></li>
            <li><a href="#ros2">ROS2</a></li>
            <li><a href="#donkeycar-ai">DonkeyCar AI</a></li>
          </ul>
      </ul>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
    <li><a href="#authors">Authors</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>



<!-- TEAM MEMBERS -->
## Team Members

<div align="center">
    <img src="images\group_photo.png"
    <p align = "center">Matthew Huntley, Donovan Sanders, Conor Norris</p>
</div>

<h4>Team Member Major and Class </h4>
<ul>
  <li>Matt - Electrical Engineering, Computer System Design - Class of 2025</li>
  <li>Donovan - Computer Engineering - Class of 2023</li>
  <li>Conor - Mechanical Engineering, Ctrls & Robotics - Class of 2024</li>
</ul>

<!-- Final Project -->
## Final Project

Our project goal was to develop a prototype of an autonomous security guard that patrols around a chosen perimeter and detects any people in its path. Then it checks to see if the visitor's face is on the preauthorized guest list and takes action against them if not. We developed our own ROS2 packages as well as integrating the lane detection package that came with the Robocar framework, allowing our vehicle to perform multiple complex tasks simultaneously. 

<!-- Original Goals -->
### Original Goals
- Ride Request
  - When launching this node, the user will be prompted to define 4 variables
    - `first_name`
    - `last_name` 
    - `pickup_location` 
    - `dropoff_location` 
  - This "ride-request" node will then publish these details to a topic to be accessed by additional nodes to determine the robot's subsequent actions
- Custom User Interfaces
  - This package defines custom interfaces for the parameters entered by the user
  - Stores the user input data under predefined values for our nodes to access and compare, i.e. `identiifed_face` and `first_name` of ride request
- Face Recognition
  - Facial recognition and verification nodes that will be subscribed to the "Name" message given by the user and publish to a new topic
    - Upon arriving at the pickup point, this module will deploy facial recognition using open-source Python libraries (`face_recognition, cv2, dlib`) 
    - The service will initiate a live webcam stream through a mounted Oak-D Lite and attempt to identify the student
    - If the student's identity is correctly verified as the individual who requested the ride, the navigation to dropoff will be authorized
    - If the identified student does not match the name given in the ride request, the car will cancel pickup and return to base
- GPS navigation
  - A package dedicated to extracting the pickup and dropoff locations, which will be converted to their corresponding `.csv` path datasets and used in mapping the route and navigating the path
    - Subscribes to the `pickup` and `dropoff` location topics and matches the input to a saved path such as `ebu2-to-ebu1.csv`
    - Client/Action server node structure so the driving process happens one time as a service, unlike the publisher nodes
- LiDAR
  - A package for utilizing mounted LiDAR LD06 for object detection as a safety measurement for collision avoidance
    - This should launch as a submodule as part of the overall Robocar package that runs in the background for emergency stop capabilities
- 
   
<!-- End Results -->
### Goals We Met
- [`ride_request_publisher.py`](src/ride_request_pkg/ride_request_pkg/ride_request_publisher.py): ride request node
- [`user_input_interfaces`](src/user_input_interfaces/msg): custom interface definitions
  - [`RideRequest.msg`](src/user_input_interfaces/msg/RideRequest.msg)
  - [`RideMatch.msg`](src/user_input_interfaces/msg/RideMatch.msg)
- [`face_rec_pkg`](src/face_rec_pkg/face_rec_pkg): face recognition package
  - [`face_publisher.py`](src/face_rec_pkg/face_rec_pkg/face_publisher.py): face recognition node for publishing identified name and video stream
  - [`verification_service.py`](src/face_rec_pkg/face_rec_pkg/verification_service.py): identity verification node
  - GPS Navigation Training: DonkeyCar framework

See [`README`](src/README.md) section in our `src` directory for breakdown of how our packages run together

See [`README`](docker/README.md) section in our `docker` directory for breakdown of how to run the Docker container for our program with all dependencies built into the image

### Our Hopes and Dreams
#### Stretch Goal 1
- Following people when detected
  - Didn't have enough time to implement 
  - Unfortunately we didn't have enough time to ROS-ify the Donkey GPS framework to run them from within our ROS/Robocar modules

#### Stretch Goal 2
- LiDAR
  - If our car is driving autonomously with GPS only, we would definitely activate the LiDAR to incorporate an emergency stop
  - Object detection for collision avoidance on while driving on the pretrained GPS paths

### Final Project Documentation

* [Final Project Proposal](https://docs.google.com/presentation/d/16qUQfX1MIP-nKk3mGd-FVhuW9kBISMMb51jPM2vEShc/edit?usp=sharing)
* [Update Presentation](https://docs.google.com/presentation/d/1XAe-p0AtVESGOgnL7_4Y8I-bLYzD0H-fxcK1hfjJbZo/edit?usp=sharing)
* [Final Presentation](https://docs.google.com/presentation/d/14qgWDKYyoA284vqveSCneuvcSrip2JctvzqzZFwIgrg/edit?usp=sharing)

<!-- Early Quarter -->
## Robot Design

### CAD Parts
#### Final Assembly
<img src="images/full_assembly.png" width="700" height="500" />

#### Custom Designed Parts
| Part | CAD Model |
|------|--------------|
| OAKD Camera Mount | <img src="images/oakd_camera_mount.png" width="300" height="300" /> 
| OAKD Articulating Arm | <img src="images/articulating_oakd_arm.png" width="300" height="400" /> 
| GPS Lidar Mount | <img src="images/gps_lidar_mount.png" width="300" height="300" /> 
| Nerf Gun Mount | <img src="images/nerf_gun_mount.png" width="300" height="300" />
| Perforated Plate | <img src="images/perforated_plate.png" width="300" height="300" /> 



#### Open Source Parts
| Part | CAD Model | Source |
|------|--------|-----------|
| Jetson Nano Case | <img src="https://github.com/kiers-neely/ucsd-mae-148-team-4/assets/161119406/6770d099-0e2e-4f8d-8072-991f1b72971f" width="400" height="300" /> | [Printables](https://www.printables.com/model/1420-nvidia-jetson-nano-case-nanomesh-mini/files) |



### Electronic Hardware
Below is a circuit diagram of the electronic hardware setup for the car.

<img src="https://github.com/kiers-neely/ucsd-mae-148-team-4/assets/161119406/6f7501ee-382a-4590-9c0a-f8ce738efec3" width="800" height="400" />


### Software
#### Embedded Systems
To program the Jetson Nano, we accessed the Jetson Nano through remote SSH connection to an embedded Linux system onboard and ran a docker container with all the necessary dependencies to run our packages. This allowed us to eliminate any incompatibility issues and to maximize resource efficiency on the Jetson. We used a variation of virtualization softwares including VMWare and WSL2 to build, test and launch our programs. 

#### ROS2
The base image pulled from Docker Hub for our project development contained the UCSD Robocar module ran on a Linux OS (Ubuntu 20.04). The Robocar module, consisting of several submodules using ROS/ROS2, was originally developed by Dominic Nightingale, a UC San Diego graduate student. His framework was built for use with a wide variety of sensors and actuation methods on scale autonomous vehicles, providing the ability to easily control a car-like robot while enabling the robot to simultaneously perform autonomous tasks.

#### DonkeyCar AI
For our early quarter course deliverables we used DonkeyCar to train a car in driving autonomous laps around a track in a simulated environment. We used Deep Learning to record visual data of driving on a simulated track and trained the car with the data to then race on a remote server. This helped us to prepare for training our physical car on an outdoor track with computer vision.

<!-- Authors -->
## Authors
  - [mahuntley](https://github.com/)  

<!-- Badges -->
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

<!-- ACKNOWLEDGMENTS -->
## Acknowledgments
*Thanks to Eric, Alex, and Professor Silberman*

<!-- CONTACT -->
## Contact

* Matt | mhuntley@ucsd.edu
* Conor | cpnorris@ucsd.edu 
* Donovan | dsanders@ucsd.edu


