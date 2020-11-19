# YOLO for ROS on Jetson

## Modify 
<config/ros.yaml> 4 line 
topic: /camera/image_raw

<launch/darknet_ros.launch> 12/13 line 
default="$(find darknet_ros)/config/ros.yaml"/>
default="$(find darknet_ros)/config/yolov2-tiny.yaml"

<src/YoloObjectDetector.cpp> 158 line
imageSubscriber_ = imageTransport_.subscribe(cameraTopicName, cameraQueueSize, &YoloObjectDetector::cameraCallback, this);
objectPublisher_ = nodeHandle_.advertise<std_msgs::Int8>(objectDetectorTopicName, objectDetectorQueueSize, objectDetectorLatch);
boundingBoxesPublisher_ = nodeHandle_.advertise<darknet_ros_msgs::BoundingBoxes>( boundingBoxesTopicName, boundingBoxesQueueSize, boundingBoxesLatch);
detectionImagePublisher_ = nodeHandle_.advertise<sensor_msgs::Image>(detectionImageTopicName, detectionImageQueueSize, detectionImageLatch);


## Build 

cd ~/catkin_ws/src

git clone https://github.com/katebrighteyes/darknet_ros

cd ~/catkin_ws/src/darknet_ros/darknet_ros

cd ~/catkin_ws/

catkin_make


## Test 

roscore

rosrun usb_cam usb_cam_node /usb_cam/image_raw:=/camera/image_raw

roslaunch darknet_ros darknet_ros.launch

rosrun rqt_graph rqt_graph


# USB CAM ROS

## Build

ls -ltr /dev/video*

cd ~/catkin_ws/src

git clone https://github.com/bosch-ros-pkg/usb_cam

cd ~/catkin_ws

sudo apt-get install ros-melodic-camera-info-manager libavcodec-dev libswscale-dev -y

catkin_make

source ~/catkin_ws/devel/setup.bash

rospack find usb_cam

sudo apt-get install ros-melodic-image-view

source ~/catkin_ws/devel/setup.bash


## Test 

roslaunch usb_cam usb_cam-test.launch

rosrun rqt_graph rqt_graph


## Change Vide Source 

<usb_cam/launch/usb_cam-test.launch> line 3
usb_cam/nodes/usb_cam_node.cpp:92: node_.param("video_device", video_device_name_, std::string("/dev/video0")); --> video1

<usb_cam/nodes/usb_cam_node.cpp> line 92
node_.param("video_device", video_device_name_, std::string("/dev/video0")); --> video1
