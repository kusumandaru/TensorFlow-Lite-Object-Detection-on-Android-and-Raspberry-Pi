# Dashcam with object detection on raspberry pi powered by tensor flow lite
These guide combine from detection object using tensor flow lite https://github.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi
and save frame into video on https://www.pyimagesearch.com/2016/02/22/writing-to-video-with-opencv/

A guide showing how to train TensorFlow Lite object detection models and run them on Android, the Raspberry Pi, and more!

TensorFlow Lite (TFLite) models run much faster than regular TensorFlow models on the Raspberry Pi. [Dashcam with Object Detection sample using tensorflow lite on Raspberry Pi 4](https://www.youtube.com/watch?v=4XVpRhM-IeMI).

## Introduction
TensorFlow Lite is an open source deep learning framework for on-device inference. It is also an optimized framework for deploying lightweight deep learning models on resource-constrained edge devices. TensorFlow Lite models have faster inference time and require less processing power, so they can be used to obtain faster performance in realtime applications.

You can get various model on https://www.tensorflow.org/lite/models.

This repository also contains Python code for running the newly converted TensorFlow Lite model to perform detection on images, videos, or webcam feeds and also additional create dashcam from your raspberry.

### A Note on Versions
I used TensorFlow v1.13 while creating this guide, because TF v1.13 is a stable version that has great support from Anaconda. I will periodically update the guide to make sure it works with newer versions of TensorFlow. 

The TensorFlow team is always hard at work releasing updated versions of TensorFlow. I recommend picking one version and sticking with it for all your TensorFlow projects. Every part of this guide should work with newer or older versions, but you may need to use different versions of the tools needed to run or build TensorFlow (CUDA, cuDNN, bazel, etc). Google has provided a list of build configurations for [Linux](https://www.tensorflow.org/install/source#linux), [macOS](https://www.tensorflow.org/install/source#macos), and [Windows](https://www.tensorflow.org/install/source_windows#tested_build_configurations) that show which tool versions were used to build and run each version of TensorFlow.


#### Run the TensorFlow Lite model!
I wrote three Python scripts to run the TensorFlow Lite object detection model and save directly on avi format video: TFLite_detection_image.py, TFLite_detection_video.py, and [TFLite_detection_dashcampy](https://github.com/kusumandaru/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi/blob/master/TFLite_detection_dashcam.py). The scripts are based off the example given in the [TensorFlow Lite examples GitHub repository by Edje Electronics](https://github.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi).

We’ll download the Python scripts directly from this repository. First, install wget for Anaconda by issuing:

```
conda install -c menpo wget
```

Once it's installed, download the scripts by issuing:

```
wget https://raw.githubusercontent.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi/master/TFLite_detection_image.py --no-check-certificate
wget https://raw.githubusercontent.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi/master/TFLite_detection_video.py --no-check-certificate
wget https://raw.githubusercontent.com/EdjeElectronics/TensorFlow-Lite-Object-Detection-on-Android-and-Raspberry-Pi/master/TFLite_detection_webcam.py --no-check-certificate
```

The following instructions show how to run the webcam, video, and image scripts. These instructions assume your .tflite model file and labelmap.txt file are in the “TFLite_model” folder in your \object_detection directory as per the instructions given in this guide.

If you’d like try using the sample TFLite object detection model provided by Google, simply download it [here](https://storage.googleapis.com/download.tensorflow.org/models/tflite/coco_ssd_mobilenet_v1_1.0_quant_2018_06_29.zip) and unzip it into the \object_detection folder. Then, use `--modeldir=coco_ssd_mobilenet_v1_1.0_quant_2018_06_29` rather than `--modeldir=TFLite_model` when running the script. 

For more information on options that can be used while running the scripts, use the `-h` option when calling the script. For example:

```
python TFLite_detection_image.py -h
```

##### Dashcam
Make sure you have a USB webcam plugged into your computer. If you’re on a laptop with a built-in camera, you don’t need to plug in a USB webcam. 

From the \object_detection directory, issue: 

```
python TFLite_detection_dashcam.py --modeldir=TFLite_model 
```

After a few moments of initializing, a window will appear showing the webcam feed. Detected objects will have bounding boxes and labels displayed on them in real time.

## Frequently Asked Questions and Common Errors

#### How do I check which TensorFlow version I used to train my detection model?
Here’s how you can check the version of TensorFlow you used for training.  

1. Open a new Anaconda Prompt window and issue `activate tensorflow1` (or whichever environment name you used)  
2. Open a python shell by issuing `python`  
3. Within the Python shell, import TensorFlow by issuing `import tensorflow as tf`  
4. Check the TensorFlow version by issuing `tf.__version__` . It will respond with the version of TensorFlow. This is the version that you used for training. 

#### Bazel configuration session for building GPU-enabled TensorFlow

#### Building TensorFlow from source
In case you run into error `error C2100: illegal indirection` during TensorFlow compilation, simply edit the file `tensorflow-build\tensorflow\tensorflow\core\framework\op_kernel.h`, go to line 405, and change `reference operator*() { return (*list_)[i_]; }` to `reference operator*() const { return (*list_)[i_]; }`. Credits go to: https://github.com/tensorflow/tensorflow/issues/15925#issuecomment-499569928
