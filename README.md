# Computer Pointer Controller

## Introduction
Computer Pointer Controller app is used to control the movement of mouse pointer by the direction of eyes and also estimated pose of head. This app takes video as input(video file or camera) and then estimate the gaze of the user's eyes and change the mouse pointer position accordingly. 

## Project Set Up and Installation
#### Install Intel® Distribution of OpenVINO™ toolkit

Refer to this [guide](https://docs.openvinotoolkit.org/latest/) for installing OpenVINO.

#### Initialize the OpenVINO environment

- For Linux, open terminal
```
source /opt/intel/openvino/bin/setupvars.sh
```
- For Windows, open command prompt as Admin
```
cd C:\Program Files (x86)\IntelSWTools\openvino\bin\
```
```
setupvars.bat
```


#### Install pre-trained models

After successfully installing OpenVINO toolkit, we need to install the models required for our project. In this project we require 4 models:-

- [Face Detection Model](https://docs.openvinotoolkit.org/latest/_models_intel_face_detection_adas_binary_0001_description_face_detection_adas_binary_0001.html)
- [Face Landmarks Detection Model](https://docs.openvinotoolkit.org/latest/_models_intel_landmarks_regression_retail_0009_description_landmarks_regression_retail_0009.html)
- [Head Pose Estimation Model](https://docs.openvinotoolkit.org/latest/_models_intel_head_pose_estimation_adas_0001_description_head_pose_estimation_adas_0001.html)
- [Gaze Estimation](https://docs.openvinotoolkit.org/latest/_models_intel_gaze_estimation_adas_0002_description_gaze_estimation_adas_0002.html)

#### Pipeline

You will have to coordinate the flow of data from the input, and then amongst the different models and finally to the mouse controller. The flow of data will look like this:

![Pipeline](./images/pipeline.png)

#### Downloading Models

**For Linux**

- face-detection-adas-binary-0001
```
sudo /opt/intel/openvino/deployment_tools/tools/model_downloader/downloader.py --name face-detection-adas-binary-0001
```
- landmarks-regression-retail-0009
```
sudo /opt/intel/openvino/deployment_tools/tools/model_downloader/downloader.py --name landmarks-regression-retail-0009
```
- head-pose-estimation-adas-0001
```
sudo /opt/intel/openvino/deployment_tools/tools/model_downloader/downloader.py --name head-pose-estimation-adas-0001
```
- gaze-estimation-adas-0002
```
sudo /opt/intel/openvino/deployment_tools/tools/model_downloader/downloader.py --name gaze-estimation-adas-0002
```

**For Windows**

- face-detection-adas-binary-0001
```
python "C:/Program Files (x86)/IntelSWTools/openvino/deployment_tools/tools/model_downloader/downloader.py" --name "face-detection-adas-binary-0001"
```
- landmarks-regression-retail-0009
```
python "C:/Program Files (x86)/IntelSWTools/openvin/deployment_tools/tools/model_downloader/downloader.py" --name "landmarks-regression-retail-0009"
```
- head-pose-estimation-adas-0001
```
python "C:/Program Files (x86)/IntelSWTools/openvino/deployment_tools/tools/model_downloader/downloader.py" --name "head-pose-estimation-adas-0001"
```
- gaze-estimation-adas-0002
```
python "C:/Program Files (x86)/IntelSWTools/openvino/deployment_tools/tools/model_downloader/downloader.py" --name "gaze-estimation-adas-0002"
```
#### Clone the Repository

```
git clone https://github.com/suryaprabhakar414/Computer-Pointer-Controller-OpenVINO.git
```
#### Create a Virtual Environment

**For Windows**
```
cd Computer-Pointer-Controller-OpenVINO.git
```
```
virtualenv Virtual_Env
```

**For Linux**
```
cd Computer-Pointer-Controller-OpenVINO
```
```
python -m venv Virtual_Env
```

#### Install the requirements
```
pip install -r requirements.txt
```

#### Activate the Virtual Environment

**For Windows**
```
Virtual_Env\Scripts\activate
```
**For Linux**
```
source Virtual_Env/bin/activate
```

## Running the Application

- CPU
```
python <path to main.py> -f <path to face detection directory> -fl <path to landmarks regression retail directory> -hp <path to head pose estimation directory> -g <path to gaze estimation directory> -i <path to input video> -d CPU
```
- GPU
```
python <path to main.py> -f <path to face detection directory> -fl <path to landmarks regression retail directory> -hp <path to head pose estimation directory> -g <path to gaze estimation directory> -i <path to input video> -d GPU
```
- FPGA
```
python <path to main.py> -f <path to face detection directory> -fl <path to landmarks regression retail directory> -hp <path to head pose estimation directory> -g <path to gaze estimation directory> -i <path to input video> -d HETERO:FPGA,CPU
```
- VPU (NCS2)
```
python <path to main.py> -f <path to face detection directory> -fl <path to landmarks regression retail directory> -hp <path to head pose estimation directory> -g <path to gaze estimation directory> -i <path to input video> -d MYRIAD 
```
## Demo

```
python3 main.py -f "/opt/intel/openvino_2020.2.120/deployment_tools/tools/model_downloader/intel/face-detection-adas-binary-0001/FP32-INT1/" -fl "/opt/intel/openvino_2020.2.120/deployment_tools/tools/model_downloader/intel/landmarks-regression-retail-0009/FP32/" -hp "/opt/intel/openvino_2020.2.120/deployment_tools/tools/model_downloader/intel/head-pose-estimation-adas-0001/FP32/" -g "/opt/intel/openvino_2020.2.120/deployment_tools/tools/model_downloader/intel/gaze-estimation-adas-0002/FP32/" -i  "/home/vegeta/Documents/Python/Project/bin/demo.mp4" -d CPU -flags "fd fld hp ge"
```
![Visualization](./images/Visualization.png)




## Documentation

### Command Line Arguments

-  -h, --help    show this help message and exit
-  -i(required): Specify Path to video file or enter cam for webcam
-  -f(required): Specify Path to folder(.xml and .bin) of Face Detection model.
-  -fl(required): Specify Path to folder(.xml and .bin) of Facial Landmark Detection model.
                
-  -hp(required): Specify Path to folder(.xml and .bin) of Head Pose Estimation model.
                
-  -g(required): Specify Path to folder(.xml and .bin) of Gaze Estimation model.
                
-  -flags(optional):  Specify the flags from fd, fld, hp, ge like --flags fd hp fld (Seperate each flag by space) for see the visualization of different model outputs of each frame, fd for Face Detection, fld for Facial Landmark Detection hp for Head Pose Estimation, ge for Gaze Estimation.

-  -prob(optional): Probability threshold for model to detect the face accurately from the video frame.

-  -d(optional): Specify the target device to infer on: CPU, GPU, FPGA or MYRIAD is acceptable. Sample will look for a suitable plugin for device specified (CPU by default).
-  -l(optional): MKLDNN (CPU)-targeted custom layers. Absolute path to a shared library with the kernels impl.

-  -it(optional): Inference type, sync for Synchronous or async for Asychronous

### Directory Structure

![Directory_Structure](./images/Tree.png)

src folder contains the following files:- 
- [face_detection.py](./src/face_detection.py):- 
  - Performs preprocessing and inference on the input frame.  
  - Returns the face coordinates and the face image.
- [face_landmarks_detection.py](./src/face_landmarks_detection.py)
  - Performs preprocessing and inference on the face image.
  - Returns the left eye image, right eye image and eye coordinates.
- [gaze_estimation.py](./src/gaze_estimation.py)
  - Takes the left eye image, right eye image, head pose angles as input.
  - Performs preprocessing ans inference.
  - Returns the mouse coordinates and gaze vector.
- [head_pose_estimation.py](./src/head_pose_estimation.py)
  - Performs preprocessing and inference on the face image.
  - Returns head pose angles(yaw, pitch, roll).
- [input_feeder.py](.src/input_feeder.py)
  - Contains InputFeeder class which initialize VideoCapture as per the user argument.
  - Returns the frames one by one.
- [main.py](./src/main.py)
  - Contains the main script to run the application.
- [mouse_controller.py](.src/mouse_controller.py)
  - Contains MouseController class which take x, y coordinates value, speed, precisions and according these values it moves the mouse pointer by using pyautogui library.
  
  


## Benchmarks

### Synchronous Inference

#### FP32


| Model | Type | Load Time | Inference Time |
|------|---|---| --- |
| face-detection-adas-binary-0001 |FP32-INT1| 280.731ms | 34.636ms|
| landmarks-regression-retail-0009 |FP32|135.403ms |  1.722ms| 
| head-pose-estimation-adas-0001 |FP32 |233.368ms | 3.510ms |
| gaze-estimation-adas-0002 |FP32| 139.832ms | 4.696ms |

Total Model Load Time: 790.420ms
Total Inference Time: 28.839s 
FPS:2


#### FP16-INT8


| Model | Type | Load Time | Inference Time |
|------|---|---| --- |
| face-detection-adas-binary-0001 |FP32-INT1| 300.426ms | 33.491ms|
| landmarks-regression-retail-0009 |FP16-INT8|132.802ms |  1.700ms| 
| head-pose-estimation-adas-0001 |FP16-INT8 |177.418ms| 3.450ms |
| gaze-estimation-adas-0002 |FP16-INT8| 231.592ms |4.364ms|


Total Model Load Time: 843.230ms
Total Inference Time: 26.750s
FPS:2

#### FP16

| Model | Type | Load Time | Inference Time |
|------|---|---| --- |
| face-detection-adas-binary-0001 |FP32-INT1|202.636ms | 30.853ms|
| landmarks-regression-retail-0009 |FP16|182.331ms | 1.618ms| 
| head-pose-estimation-adas-0001 |FP16 |409.070ms | 2.689ms |
| gaze-estimation-adas-0002 |FP16| 495.434ms| 3.333ms |

Total Model Load Time: 1290.419ms
Total Inference Time: 27.472s
FPS:2

### Asynchronous Inference

#### FP32
| Model | Type | Load Time | Inference Time |
|------|---|---| --- |
| face-detection-adas-binary-0001 |FP32-INT1|216.242ms | 20.996ms|
| landmarks-regression-retail-0009 |FP32|114.711ms|  0.946ms| 
| head-pose-estimation-adas-0001 |FP32 |201.458ms | 2.716ms |
| gaze-estimation-adas-0002 |FP32| 212.321ms | 2.941ms|

Total Model Load Time: 746.801ms
Total Inference Time: 25.853s 
FPS:2


#### FP16-INT8


| Model | Type | Load Time | Inference Time |
|------|---|---| --- |
| face-detection-adas-binary-0001 |FP32-INT1| 300.426ms | 33.491ms|
| landmarks-regression-retail-0009 |FP16-INT8|132.802ms |  1.700ms| 
| head-pose-estimation-adas-0001 |FP16-INT8 |177.418ms| 3.450ms |
| gaze-estimation-adas-0002 |FP16-INT8| 231.592ms |4.364ms|


Total Model Load Time: 843.230ms
Total Inference Time: 26.750s
FPS:2

#### FP16

| Model | Type | Load Time | Inference Time |
|------|---|---| --- |
| face-detection-adas-binary-0001 |FP32-INT1|202.636ms | 30.853ms|
| landmarks-regression-retail-0009 |FP16|182.331ms | 1.618ms| 
| head-pose-estimation-adas-0001 |FP16 |409.070ms | 2.689ms |
| gaze-estimation-adas-0002 |FP16| 495.434ms| 3.333ms |

Total Model Load Time: 1290.419ms
Total Inference Time: 27.472s
FPS:2





## Results
*TODO:* Discuss the benchmark results and explain why you are getting the results you are getting. For instance, explain why there is difference in inference time for FP32, FP16 and INT8 models.

## Stand Out Suggestions
This is where you can provide information about the stand out suggestions that you have attempted.

### Async Inference
If you have used Async Inference in your code, benchmark the results and explain its effects on power and performance of your project.

### Edge Cases
There will be certain situations that will break your inference flow. For instance, lighting changes or multiple people in the frame. Explain some of the edge cases you encountered in your project and how you solved them to make your project more robust.
