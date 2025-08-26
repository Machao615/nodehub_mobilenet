# MobileNetV2

[中文文档](README_cn.md) | **English**

## Introduction

The MobileNetV2 image classification algorithm example uses images as input, leverages BPU for algorithm inference, and publishes algorithm messages containing object categories.

MobileNetV2 is a Caffe model trained using the [ImageNet data](http://www.image-net.org/) dataset. Model source: https://github.com/shicai/MobileNet-Caffe.
It supports 1000 types of targets including people, animals, fruits, vehicles, etc. For specific supported categories, see the RDK board file `/opt/tros/${TROS_DISTRO}/lib/dnn_node_example/config/imagenet.list` (with TogatherROS.Bot installed).

Code repository: https://github.com/D-Robotics/hobot_dnn

Application scenarios: MobileNetV2 can predict the category of a given image, enabling digit recognition, object recognition, and other functions. It is mainly used in text recognition, image retrieval, and other fields.

Food type recognition case: https://github.com/frotms/Chinese-and-Western-Food-Classification

## Important Notice

⚠️ **Source Code Information**:
- The C++ compilation source code required for this project comes from: https://github.com/WuChao-2024/ROS2_YOLO_RDK_TROS
- Everything is based on the above source address. This repository only provides usage instructions and documentation
- Specific updates are mainly based on the source address and RDK official manual: https://developer.d-robotics.cc/rdk_doc/rdk_s/Robot_development/boxs/classification/mobilenetv2

💡 **Recommended Usage**:
- **Recommended**: Use pre-compiled packages installed directly via apt, no manual compilation required
- **Only compile source code when custom development is needed**

## ROS2 Package Installation and Compilation

### Method 1: APT Installation (Recommended)

For most users, it is recommended to directly use apt to install pre-compiled packages:

```bash
# Update software sources
sudo apt update

# Install tros.b related packages
sudo apt install tros-dnn-node-example

# Ready to use directly after installation, no compilation required
```

### Method 2: Source Code Compilation (Only for Custom Development)

⚠️ **Note**: Source code compilation is only needed when custom development, source code modification, or debugging is required.

#### Get Source Code

```bash
# Create workspace
mkdir -p ~/tros_ws/src
cd ~/tros_ws/src

# Clone source repository
git clone https://github.com/WuChao-2024/ROS2_YOLO_RDK_TROS.git
```

#### Compilation Environment Setup

```bash
# Install compilation dependencies
sudo apt install python3-colcon-common-extensions

# Configure tros.b environment
source /opt/tros/setup.bash
# Or for Humble version
# source /opt/tros/humble/setup.bash
```

#### Compile Package

```bash
cd ~/tros_ws

# Compile specific package
colcon build --packages-select dnn_node_example

# Set environment variables after compilation
source install/setup.bash
```

#### Verify Compilation Results

```bash
# Check if package compiled successfully
ros2 pkg list | grep dnn_node_example

# View available launch files
ros2 pkg executables dnn_node_example
```

## Supported Platforms

| Platform | Runtime | Example Features |
| -------- | ------- | ---------------- |
| RDK X3, RDK X3 Module | Ubuntu 20.04 (Foxy), Ubuntu 22.04 (Humble) | · Start MIPI/USB camera and display inference rendering results via web<br/>· Use local playback, rendering results saved locally |
| RDK X5, RDK X5 Module | Ubuntu 22.04 (Humble) | · Start MIPI/USB camera and display inference rendering results via web<br/>· Use local playback, rendering results saved locally |
| RDK Ultra | Ubuntu 20.04 (Foxy) | · Start MIPI/USB camera and display inference rendering results via web<br/>· Use local playback, rendering results saved locally |
| RDK S100 | Ubuntu 22.04 (Humble) | · Start MIPI/USB camera and display inference rendering results via web<br/>· Use local playback, rendering results saved locally |
| X86 | Ubuntu 20.04 (Foxy) | · Use local playback, rendering results saved locally |

## Prerequisites

### RDK Platform

1. RDK has been flashed with Ubuntu 20.04/Ubuntu 22.04 system image.

2. RDK has successfully installed tros.b.

3. RDK has installed MIPI or USB camera. Without a camera, experience the algorithm effects through local JPEG/PNG format images or MP4, H.264, and H.265 video playback.

4. Confirm that the PC can access RDK through the network.

### X86 Platform

1. X86 environment has been configured with Ubuntu 20.04 system image.

2. X86 environment system has successfully installed tros.b.

## Usage Instructions

### RDK Platform

The MobileNetV2 image classification subscribes to images published by the sensor package, publishes algorithm messages after inference, and uses the websocket package to render and display the published images and corresponding algorithm results on the PC browser.

#### Using MIPI Camera to Publish Images

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

```bash
# Configure tros.b environment
source /opt/tros/setup.bash
```

</TabItem>

<TabItem value="humble" label="Humble">

```bash
# Configure tros.b environment
source /opt/tros/humble/setup.bash
```

</TabItem>

</Tabs>

```shell
# Configure MIPI camera
export CAM_TYPE=mipi

# Launch file
ros2 launch dnn_node_example dnn_node_example.launch.py dnn_example_config_file:=config/mobilenetv2workconfig.json dnn_example_image_width:=480 dnn_example_image_height:=272
```

#### Using USB Camera to Publish Images

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

```bash
# Configure tros.b environment
source /opt/tros/setup.bash
```

</TabItem>

<TabItem value="humble" label="Humble">

```bash
# Configure tros.b environment
source /opt/tros/humble/setup.bash
```

</TabItem>

</Tabs>

```shell
# Configure USB camera
export CAM_TYPE=usb

# Launch file
ros2 launch dnn_node_example dnn_node_example.launch.py dnn_example_config_file:=config/mobilenetv2workconfig.json dnn_example_image_width:=480 dnn_example_image_height:=272
```

#### Using Local Image Playback

The MobileNetV2 image classification algorithm example uses local JPEG/PNG format image playback, and stores the algorithm result-rendered images locally in the running path after inference.

<Tabs groupId="tros-distro">
<TabItem value="foxy" label="Foxy">

```bash
# Configure tros.b environment
source /opt/tros/setup.bash
```

</TabItem>

<TabItem value="humble" label="Humble">

```bash
# Configure tros.b environment
source /opt/tros/humble/setup.bash
```

</TabItem>

</Tabs>

```shell
# Launch file
ros2 launch dnn_node_example dnn_node_example_feedback.launch.py dnn_example_config_file:=config/mobilenetv2workconfig.json dnn_example_image:=config/target_class.jpg
```

### X86 Platform

#### Using Local Image Playback

The MobileNetV2 image classification algorithm example uses local JPEG/PNG format image playback, and stores the algorithm result-rendered images locally in the running path after inference.

```bash
# Configure tros.b environment
source /opt/tros/setup.bash

# Launch file
ros2 launch dnn_node_example dnn_node_example_feedback.launch.py dnn_example_config_file:=config/mobilenetv2workconfig.json dnn_example_image:=config/target_class.jpg
```

## Result Analysis

### Using Camera to Publish Images

The following information is output in the running terminal:

```shell
[example-3] [WARN] [1655095481.707875587] [example]: Create ai msg publisher with topic_name: hobot_dnn_detection
[example-3] [WARN] [1655095481.707983957] [example]: Create img hbmem_subscription with topic_name: /hbmem_img
[example-3] [WARN] [1655095482.985732162] [img_sub]: Sub img fps 31.07
[example-3] [WARN] [1655095482.992031931] [example]: Smart fps 31.31
[example-3] [WARN] [1655095484.018818843] [img_sub]: Sub img fps 30.04
[example-3] [WARN] [1655095484.025123362] [example]: Smart fps 30.04
[example-3] [WARN] [1655095485.051988567] [img_sub]: Sub img fps 30.01
[example-3] [WARN] [1655095486.057854228] [example]: Smart fps 30.07
```

The output log shows that the topic for publishing algorithm inference results is `hobot_dnn_detection`, the topic for subscribing to images is `/hbmem_img`, and the subscribed images and algorithm inference output frame rate is about 30fps.

Enter http://IP:8000 in the PC browser to view the image and algorithm rendering effects (IP is the IP address of RDK):

![render_web](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/03_boxs/classification/image/mobilenetv2/mobilenetv2_render_web.jpeg)

### Using Local Image Playback

The following information is output in the running terminal:

```shell
[example-1] [INFO] [1654767648.897132079] [example]: The model input width is 224 and height is 224
[example-1] [INFO] [1654767648.897180241] [example]: Dnn node feed with local image: config/target_class.jpg
[example-1] [INFO] [1654767648.935638968] [example]: task_num: 2
[example-1] [INFO] [1654767648.946566665] [example]: Output from image_name: config/target_class.jpg, frame_id: feedback, stamp: 0.0
[example-1] [INFO] [1654767648.946671029] [ClassificationPostProcess]: outputs size: 1
[example-1] [INFO] [1654767648.946718774] [ClassificationPostProcess]: out cls size: 1
[example-1] [INFO] [1654767648.946773602] [ClassificationPostProcess]: class type:window-shade, score:0.776356
[example-1] [INFO] [1654767648.947251721] [ImageUtils]: target size: 1
[example-1] [INFO] [1654767648.947342212] [ImageUtils]: target type: window-shade, rois.size: 1
[example-1] [INFO] [1654767648.947381666] [ImageUtils]: roi.type: , x_offset: 112 y_offset: 112 width: 0 height: 0
[example-1] [WARN] [1654767648.947563731] [ImageUtils]: Draw result to file: render_feedback_0_0.jpeg
```

The output log shows that the algorithm uses the input image config/target_class.jpg to infer the image classification result as window-shade with a confidence of 0.776356 (the algorithm only outputs the classification result with the highest confidence). The stored rendered image file name is render_feedback_0_0.jpeg. Rendered image effect:

![render_feedback](https://rdk-doc.oss-cn-beijing.aliyuncs.com/doc/img/05_Robot_development/03_boxs/classification/image/mobilenetv2/mobilenetv2_render_feedback.jpeg)

## FAQ

### Q: Do I need to compile source code?
A: **Not needed in most cases**. It is recommended to directly use `sudo apt install tros-dnn-node-example` to install pre-compiled packages. Source code compilation is only needed when custom development or algorithm modification is required.

### Q: How to get the latest source code?
A: Please visit the source repository: https://github.com/WuChao-2024/ROS2_YOLO_RDK_TROS

### Q: How to solve problems?
A: 
1. First check the RDK official documentation: https://developer.d-robotics.cc/rdk_doc/rdk_s/Robot_development/boxs/classification/mobilenetv2
2. Check the Issues page of the source repository
3. Ensure that the tros.b environment is properly installed

## Reference Documentation

- [RDK Official MobileNetV2 Documentation](https://developer.d-robotics.cc/rdk_doc/rdk_s/Robot_development/boxs/classification/mobilenetv2)
- [Source Repository](https://github.com/WuChao-2024/ROS2_YOLO_RDK_TROS)
- [Horizon RDK User Manual](https://developer.d-robotics.cc/)
- [TROS.B User Manual](https://developer.d-robotics.cc/rdk_doc/rdk_s/installation/tros_installation)

## Copyright Notice

This project is developed based on the D-Robotics RDK platform and TROS.B framework, following the corresponding open source licenses. For specific source code copyright, please refer to the LICENSE file in the source repository.
