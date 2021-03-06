![Darknet Logo](http://pjreddie.com/media/files/darknet-black-small.png)

# Yolo-Windows v2
# "You Only Look Once: Unified, Real-Time Object Detection (version 2)"
A yolo windows version (for object detection)

Contributtors: https://github.com/pjreddie/darknet/graphs/contributors

This repository is forked from Linux-version: https://github.com/pjreddie/darknet

More details: http://pjreddie.com/darknet/yolo/

##### Requires: 
* **MS Visual Studio 2015 (v140)**: https://www.microsoft.com/download/details.aspx?id=48146
* **CUDA 8.0 for Windows x64**: https://developer.nvidia.com/cuda-downloads
* **OpenCV 2.4.9**: https://sourceforge.net/projects/opencvlibrary/files/opencv-win/2.4.9/opencv-2.4.9.exe/download
  - To compile without OpenCV - remove define OPENCV from: Visual Studio->Project->Properties->C/C++->Preprocessor
  - To compile with different OpenCV version - change in file yolo.c each string look like **#pragma comment(lib, "opencv_core249.lib")** from 249 to required version.
  - With OpenCV will show image or video detection in window

##### Pre-trained models for different cfg-files can be downloaded from (smaller -> faster & lower quality):
* `yolo.cfg` (256 MB) - require 4 GB GPU-RAM: http://pjreddie.com/media/files/yolo-voc.weights
* `yolo-tiny.cfg` (60 MB) - require 1 GB GPU-RAM: http://pjreddie.com/media/files/tiny-yolo-voc.weights

Put it near compiled: darknet.exe

##### Examples of results:

[![Everything Is AWESOME](http://img.youtube.com/vi/VOC3huqHrss/0.jpg)](https://www.youtube.com/watch?v=VOC3huqHrss "Everything Is AWESOME")

Others: https://www.youtube.com/channel/UC7ev3hNVkx4DzZ3LO19oebg

### How to use:

##### Example of usage in cmd-files from `build\darknet\x64\`:

* `darknet_demo_voc.cmd` - initialization with 256 MB model yolo-voc.weights & yolo-voc.cfg and play your video file which you must rename to: test.mp4
* `darknet_net_cam_voc.cmd` - initialization with 256 MB model, play video from network video-camera mjpeg-stream (also from you phone)

How to use from command line with 256 MB model: `darknet.exe yolo demo yolo-voc.cfg yolo-voc.weights test.mp4 -i 0`

##### For using network video-camera mjpeg-stream with any Android smartphone:

1. Download for Android phone mjpeg-stream soft: IP Webcam / Smart WebCam


 Smart WebCam - preferably: https://play.google.com/store/apps/details?id=com.acontech.android.SmartWebCam
 IP Webcam: https://play.google.com/store/apps/details?id=com.pas.webcam

2. Connect your Android phone to computer by WiFi (through a WiFi-router) or USB
3. Start Smart WebCam on your phone
4. Replace the address below, on shown in the phone application (Smart WebCam) and launch:

```
darknet.exe yolo demo yolo-voc.cfg yolo-voc.weights http://192.168.0.80:8080/video?dummy=param.mjpg -i 0
```

##### How to use COCO instead of VOC:

* Get synset names from `build\darknet\x64\data\coco.names`: https://github.com/AlexeyAB/darknet/blob/master/build/darknet/x64/data/coco.names
* And change list `char *voc_names[] = ` to COCO-names in file `yolo.c`: https://github.com/AlexeyAB/darknet/blob/master/src/yolo.c#L30


### How to compile:

1. If you have CUDA 8.0, OpenCV 2.4.9 (C:\opencv_2.4.9) and MSVS 2015 then start MSVS, open `build\darknet\darknet.sln` and do the: Build -> Build darknet

2. If you have other version of CUDA (not 8.0) then open `build\darknet\darknet.vcxproj` by using Notepad, find 2 places with "CUDA 8.0" and change it to your CUDA-version, then do step 1

3. If you have other version of OpenCV 2.4.x (not 2.4.9) then you should change pathes after `\darknet.sln` is opened

  3.1 (right click on project) -> properties  -> C/C++ -> General -> Additional Include Directories
  
  3.2 (right click on project) -> properties  -> Linker -> General -> Additional Library Directories

4. If you have other version of OpenCV 3.x (not 2.4.x) then you should change many places in code by yourself.

### How to compile (custom):

Also, you can to create your own `darknet.sln` & `darknet.vcxproj`, this example for CUDA 8.0 and OpenCV 2.4.9

Then add to your created project:
- (right click on project) -> properties  -> C/C++ -> General -> Additional Include Directories, put here: 

`C:\opencv_2.4.9\opencv\build\include;..\..\3rdparty\include;%(AdditionalIncludeDirectories);$(CudaToolkitIncludeDir);$(cudnn)\include`
- right click on project -> Build dependecies -> Build Customizations -> set check on CUDA 8.0 or what version you have - for example as here: http://devblogs.nvidia.com/parallelforall/wp-content/uploads/2015/01/VS2013-R-5.jpg
- add to project all .c & .cu files from yolo-windows\src
-  (right click on project) -> properties  -> Linker -> General -> Additional Library Directories, put here: 

`C:\opencv_2.4.9\opencv\build\x64\vc12\lib;$(CUDA_PATH)lib\$(PlatformName);$(cudnn)\lib\x64;%(AdditionalLibraryDirectories)`
-  (right click on project) -> properties  -> Linker -> Input -> Additional dependecies, put here: 

`..\..\3rdparty\lib\x64\pthreadVC2.lib;cublas.lib;curand.lib;cudart.lib;cudnn.lib;%(AdditionalDependencies)`
- (right click on project) -> properties -> C/C++ -> Preprocessor -> Preprocessor Definitions

`OPENCV;_TIMESPEC_DEFINED;_CRT_SECURE_NO_WARNINGS;GPU;WIN32;NDEBUG;_CONSOLE;_LIB;%(PreprocessorDefinitions)`
- compile to .exe (X64 & Release) and put .dll`s near with .exe:

`pthreadVC2.dll, pthreadGC2.dll` from yolo-windows\3rdparty\dll\x64

`cusolver64_80.dll, curand64_80.dll, cudart64_80.dll, cublas64_80.dll` - 80 for CUDA 8.0 or your version, from C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v8.0\bin


