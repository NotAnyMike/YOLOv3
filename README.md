# YOLO v3 implementation in YOLO

![Example](example.gif)

This is an implementation of [YOLOv3](https://pjreddie.com/media/files/papers/YOLOv3.pdf) using Pytorch and pretrained weights using Pytorch and pretrained weights based on [this repo](https://github.com/ayooshkathuria/pytorch-yolo-v3)

## TODO

* remove this readme
* add instructions to run it out of the box
* add videos

### Instructions 

0. Clone this repo (``git clone this``).
1. (optional) Create a conda environment
1. Install all the requirements to run it by running
``conda install numpy pandas matplolib opencv``
2. Install pytorch depending on your system, the code does not require CUDA nor CudNN to run.
3. Download the weights from [here](https://pjreddie.com/media/files/yolov3.weights) or if in ubuntu run
``wget https://pjreddie.com/media/files/yolov3.weights``
and move them to the root directory of the repo.
4. Run it (see section **RUNNING CODE**)
3. If there is an error with cv2, install opencv with ``conda install opencv``

## Running Code

There are two different ways to run the code, on images (specifing the path) or in video, I am implementing code to run in realtime with video input from simularion (using AirSim) and to run it using web cam.

### Running on images

Run the code

```
python detect.py --images [image file] --det [output directory]
```

An example running in multiple images

```
python detect.py --images imgs --det det 
```
or in a single image

```
python detect.py --images imgs/giraffe.jpg --det det
```


### Running on video

The output of this script will be the video with each frame modified showing the dectected objects, without GPU the video will look pretty slow. You should also run the file, video_demo.py with --video flag specifying the video file. The video file should be in .avi or .mp4 format

```
python video_demo.py --video [video file]
```

ans example would be:

```
python video_demo.py --video test.mp4
```

Tweakable settings can be seen with -h flag. 


### Running on Simulation

Comming soon...

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

Comming soon...

### For help 

You can run
```
python detect.py -h
```

### Speed Accuracy Tradeoff
You can change the resolutions of the input image by the `--reso` flag. The default value is 416. Whatever value you chose, rememeber **it should be a multiple of 32 and greater than 32**. Weird things will happen if you don't. You've been warned. 

```
python detect.py --images imgs --det det --reso 320
```

---

## Requirements
1. Python 3.5++
2. OpenCV
3. PyTorch 0.4++

Using PyTorch 0.3 will break the detector.

### Speeding up Video Inference

To speed video inference, you can try using the video_demo_half.py file instead which does all the inference with 16-bit half 
precision floats instead of 32-bit float. I haven't seen big improvements, but I attribute that to having an older card 
(Tesla K80, Kepler arch). If you have one of cards with fast float16 support, try it out, and if possible, benchmark it. 


