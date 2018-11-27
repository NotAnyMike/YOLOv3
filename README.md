# YOLO v3 implementation in YOLO

This is an implementation of [YOLOv3](https://pjreddie.com/media/files/papers/YOLOv3.pdf) using Pytorch and pretrained weights using Pytorch and pretrained weights

## TODO

* remove this readme
* add instructions to run it out of the box
* add videos

### Instructions 

0. Clone this repo (``git clone this``).
1. Install all the requirements to run it by running
`` SOME CODE I DON'T HAVE YET``
2. Install pytorch depending on your system, the code does not require CUDA nor CudNN to run.
3. Download the weights from [here](https://pjreddie.com/media/files/yolov3.weights) or if in ubuntu run
``wget https://pjreddie.com/media/files/yolov3.weights``
and move them to the root directory of the repo.
4. Run it (see section **RUNNING CODE**)

## Running Code

There are two different ways to run the code, on images (specifing the path) or in video, I am implementing code to run in realtime with video input from simularion (using AirSim) and to run it using web cam.

### Running on images

Run the code

``python detect.py --images [image file] --det [output directory]

An example would be:

``python detect.py --images imgs/giraffe.jpg --det det``


### Running on video

The output of this script will be the video with each frame modified showing the dectected objects, without GPU the video will look pretty slow. To run it, run this code:

``python video.py --video [video file]``

ans example would be:

``python video.py --video test.mp4``.


### Running on Simulation

Comming soon...

### Running on web cam

Comming soon...

---

# A PyTorch implementation of a YOLO v3 Object Detector

This repository contains code for a object detector based on [YOLOv3: An Incremental Improvement](https://pjreddie.com/media/files/papers/YOLOv3.pdf), implementedin PyTorch. The code is based on the official code of [YOLO v3](https://github.com/pjreddie/darknet), as well as a PyTorch 
port of the original code, by [marvis](https://github.com/marvis/pytorch-yolo2). One of the goals of this code is to improve
upon the original port by removing redundant parts of the code (The official code is basically a fully blown deep learning 
library, and includes stuff like sequence models, which are not used in YOLO). I've also tried to keep the code minimal, and 
document it as well as I can. 

### Tutorial for building this detector from scratch
If you want to understand how to implement this detector by yourself from scratch, then you can go through this very detailed 5-part tutorial series I wrote on Paperspace. Perfect for someone who wants to move from beginner to intermediate pytorch skills. 

[Implement YOLO v3 from scratch](https://blog.paperspace.com/how-to-implement-a-yolo-object-detector-in-pytorch/)

As of now, the code only contains the detection module, but you should expect the training module soon. :) 

## Requirements
1. Python 3.5
2. OpenCV
3. PyTorch 0.4

Using PyTorch 0.3 will break the detector.



## Detection Example

![Detection Example](https://i.imgur.com/m2jwneng.png)
## Running the detector

### On single or multiple images

Clone, and `cd` into the repo directory. The first thing you need to do is to get the weights file
This time around, for v3, authors has supplied a weightsfile only for COCO [here](https://pjreddie.com/media/files/yolov3.weights), and place 

the weights file into your repo directory. Or, you could just type (if you're on Linux)

```
wget https://pjreddie.com/media/files/yolov3.weights 
python detect.py --images imgs --det det 
```


`--images` flag defines the directory to load images from, or a single image file (it will figure it out), and `--det` is the directory
to save images to. Other setting such as batch size (using `--bs` flag) , object threshold confidence can be tweaked with flags that can be looked up with. 

```
python detect.py -h
```

### Speed Accuracy Tradeoff
You can change the resolutions of the input image by the `--reso` flag. The default value is 416. Whatever value you chose, rememeber **it should be a multiple of 32 and greater than 32**. Weird things will happen if you don't. You've been warned. 

```
python detect.py --images imgs --det det --reso 320
```

### On Video
For this, you should run the file, video_demo.py with --video flag specifying the video file. The video file should be in .avi format
since openCV only accepts OpenCV as the input format. 

```
python video_demo.py --video video.avi
```

Tweakable settings can be seen with -h flag. 

### Speeding up Video Inference

To speed video inference, you can try using the video_demo_half.py file instead which does all the inference with 16-bit half 
precision floats instead of 32-bit float. I haven't seen big improvements, but I attribute that to having an older card 
(Tesla K80, Kepler arch). If you have one of cards with fast float16 support, try it out, and if possible, benchmark it. 

### On a Camera
Same as video module, but you don't have to specify the video file since feed will be taken from your camera. To be precise, 
feed will be taken from what the OpenCV, recognises as camera 0. The default image resolution is 160 here, though you can change it with `reso` flag.

```
python cam_demo.py
```
You can easily tweak the code to use different weightsfiles, available at [yolo website](https://pjreddie.com/darknet/yolo/)

NOTE: The scales features has been disabled for better refactoring.
### Detection across different scales
YOLO v3 makes detections across different scales, each of which deputise in detecting objects of different sizes depending upon whether they capture coarse features, fine grained features or something between. You can experiment with these scales by the `--scales` flag. 

```
python detect.py --scales 1,3
```


