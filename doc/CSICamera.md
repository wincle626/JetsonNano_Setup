# Document the CSI camera usage on Jetson Nano

## 1. [NVIDIA API](https://developer.nvidia.com/embedded/learn/tutorials/first-picture-csi-usb-camera)

a. In order to check that the CSI camera is working, you can run the following command, which will start capture and preview display it on the screen.

`nvgstcapture-1.0`

This example command will rotate the image 180 degrees (vertical flip)

`nvgstcapture-1.0 --orientation 2`

Runtime command line option of `nvgstcapture-1.0`

Press 'j' to Capture one image.  Press 'q' to exit. 

Press '1' to Start recording video. Press '0' to Stop recording video

Automated command line option

`nvgstcapture-1.0 --automate --capture-auto`

NOTE: Use “nvgstcapture-1.0 --help” to refer supported command line options

## 2. [Gstreamer API](https://developer.ridgerun.com/wiki/index.php/Jetson_Nano/Gstreamer/Example_Pipelines/Capture_Display)

Using `gst-launch-1.0` command to call the gstreamer API, the format is:

`gst-launch-1.0 [OPTIONS] PIPELINE-DESCRIPTION`

For example: 

`gst-launch-1.0 nvarguscamerasrc sensor_id=0 ! 'video/x-raw(memory:NVMM),width=1920, height=1080, framerate=30/1' ! nvvidconv flip-method=0 ! 'video/x-raw,width=960, height=540' ! nvvidconv ! nvegltransform ! nveglglessink -e`

