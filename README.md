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


os_jetson_start
USB CAM ROS
Melodic
ls -ltr /dev/video*

cd catkin_ws/src

git clone https://github.com/bosch-ros-pkg/usb_cam

if your usb camera is /dev/video1 go to 54 line of this page.

cd ..

sudo apt-get install ros-melodic-camera-info-manager libavcodec-dev libswscale-dev -y

catkin_make

source ~/catkin_ws/devel/setup.bash

rospack find usb_cam

sudo apt-get install ros-melodic-image-view

source ~/catkin_ws/devel/setup.bash

[1]

$ roslaunch usb_cam usb_cam-test.launch

[2]

$ rosrun rqt_graph rqt_graph

if your usb camera is /dev/video1
nvidia@nvidia:~$ ls -ltr /dev/video*

crw-rw----+ 1 root video 81, 0 9월 18 23:46 /dev/video0

crw-rw----+ 1 root video 81, 3 9월 19 02:41 /dev/video1

nvidia@nvidia:~$ cd catkin_ws/src

nvidia@nvidia:~/catkin_ws/src$ grep -rn video0 *

usb_cam/launch/usb_cam-test.launch:3:

usb_cam/nodes/usb_cam_node.cpp:92: node_.param("video_device", video_device_name_, std::string("/dev/video0"));

** YOU HAVE TO MODIFY THOSE 2 LINES TO "/dev/video1" !!!!

Kinetic
ls -ltr /dev/video*

cd catkin_ws/src

git clone https://github.com/bosch-ros-pkg/usb_cam

sudo apt-get install ros-indigo-camera-info-manager

source ~/catkin_ws/devel/setup.bash

rospack find usb_cam

sudo apt-get install ros-kinetic-image-view

roscore

roslaunch usb_cam usb_cam-test.launch

rosrun rqt_graph rqt_graph
