# Traffic Vision
This app detects cars/buses in a live traffic at a phenomenal **50 frames/sec with HD resolution (1920x1080)** using deep learning network [Yolo-V2](https://pjreddie.com/darknet/yolov2). The model used in the app is optimized for inferencing performnce on AMD-GPUs using [MIVisionX toolkit](https://gpuopen-professionalcompute-libraries.github.io/MIVisionX/).

[![Traffic Vision Animation](media/traffic_viosion.gif)](https://youtu.be/YASOovwds_A)

## Features
1. Vehicle detection with bounding box
1. Vehicle direction ((upward, downward) detection 
1. Vehicle speed estimation
1. Vehicle type: bus/car.

## How to Run

### Use Model
<img src="media/speed_detection_user_interface.jpg" width=600>

### Demo

App starts the demo, if no other option is provided. Demo uses a video stored in the [media/](./media) dir.
```
% ./main.py
('Loaded', 'yoloOpenVX')
OK: loaded 22 kernels from libvx_nn.so
OK: OpenVX using GPU device#0 (gfx900) [OpenCL 1.2 ] [SvmCaps 0 1]
OK: annCreateInference: successful
Processed a total of  102 frames
OK: OpenCL buffer usage: 87771380, 46/46
%
```
Here is the [link to YouTube video](https://youtu.be/YASOovwds_A) detecting cars, bounding boxes, car speed, and confidence scores.
### Other Examples

**recorded video**
> 1. ./main.py --video <path-to-video>/vid.mp4
  
**traffic cam ip** 
> 2. ./main.py --cam_ip 'http://166.149.104.112:8082/snap.jpg'

## Installation

### Prerequisites 

1. GPU: Radeon Instinct or Vega Family of Products with [ROCm](https://rocm.github.io/ROCmInstall.html) and OpenCL development kit
1. [Install AMD's MIVisionX toolkit](https://gpuopen-professionalcompute-libraries.github.io/MIVisionX/) : AMD's MIVisionX toolkit is a comprehensive computer vision and machine intelligence libraries, utilities
1. [CMake](http://cmake.org/download/), [Caffe](http://caffe.berkeleyvision.org/installation.html)
1. [Google's Protobuf](https://github.com/google/protobuf)

### Steps

```
% git clone https://github.com/srohit0/trafficVision
```


**_1. Model Conversion_**

This steps downloads yolov2-tiny for voc dataset and converts to MIVision's openVX model. 
```
% cd trafficVision/model
% bash ./prepareModel.sh
```
More details on the pre-requisite (like [caffe](http://caffe.berkeleyvision.org/installation.html)) of the model conversion in the [models/](./models) dir.

**_2. MIVision Model Compilation_**

```
% cd trafficVision
% make
```

**_3. Test App_**

```
% cd trafficVision
% make test
```
It'll display detection all videos in media/ dir.

## Design
This section is a guide for developers, who would like to port vision and object detections models to AMD's Radeon GPUs from other frameworks including [tensorflow](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md), [caffe](http://caffe.berkeleyvision.org/model_zoo.html) or [pytorch](https://pytorch.org/).

### High Level Design
<img src="media/speed_detection_top_level_arch.jpg" width="600">

### Lower Level Modules
These lower level modules can be found as python modules (files) or packages (directories) in this repository.
<img src="media/speed_detection_modules.jpg" width="600">

## Development

### Model Conversion
Follow model conversion process similar to the one described below.
<img src="media/speed_detection_model_conversion.jpg" width=680>


### Infrastructure
Make sure you've infrastructure pre-requisites installed before you start porting neural network model for inferencing.
<img src="media/speed_detection_infrastructure.jpg" width=480>

## Developed and Tested on
1. Hardware
    1. AMD Ryzen Threadripper 1900X 8-Core Processor
    1. Accelerator = Radeon Instinct MI25 Accelerator 
1. Software
    1. Ubuntu 16.04 LTS OS
    1. Python 2.7
    1. MIVisionX 1.7.0
    1. AMD OpenVX 0.9.9
    1. GCC 5.4

## Credit
* MIVisionX Team

## References
1. [yoloV2 paper](https://arxiv.org/pdf/1612.08242.pdf)
1. [Tiny Yolo aka Darknet reference network](https://pjreddie.com/darknet/imagenet/#reference)
1. [MiVisionX Setup](https://github.com/kiritigowda/MIVisionX-setup)
1. [AMD OpenVX](https://gpuopen.com/compute-product/amd-openvx/)
1. [Optimization with OpenVX Graphs](http://openaccess.thecvf.com/content_cvpr_workshops_2014/W17/papers/Rainey_Addressing_System-Level_Optimization_2014_CVPR_paper.pdf)
1. [Measuring Traffic Speed With Deep Learning Object Detection](https://medium.com/datadriveninvestor/measuring-traffic-speed-with-deep-learning-object-detection-efc0bb9a3c57)
