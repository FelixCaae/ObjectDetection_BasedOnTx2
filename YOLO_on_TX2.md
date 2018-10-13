# YOLO: Install and run on TX2
-----
YOLO is a unified convolutional neural network which is fast and both accurate in Object Detection tasks.It\`s latest version is YOLOV3.
See it`s official site [here](https://pjreddie.com/darknet/yolo/)
To run YOLO on TX2, there are some simple steps to follow.<br>

0. #### Preinstall <br>
    JetPack 3.1 or later is required.Or you should at least installed cuda.<br>
    *If you have done this before you can jump this step*<br>
        
        sudo nvpmodel -m 0
        sudo ~\jetson_clocks.sh
    
1. #### Clone the repo
    
        git clone https://github.com/pjreddie/darknet
        cd darknet

2. #### Modify makefile<br>
    Firstly open the file.You can use gedit also
  
        vim Makefile
    Or
    
        gedit Makefile        
    Use # to comment original settings and add new lines aside.Or you can just 
  
        cp Makefile Makefile.bak
    
    before you do anything. 
    Modify first several lines like this
    
        GPU = 1
        CUDNN = 1
        OPENCV = 1
        ARCH = -gencode arch=compute_62,code=compute_62
    
    If you want to test it without GPU、CUDNN、OPENCV just leave those lines alone.By the way, OPENCV is used to
    show result immediately after the command.Otherwise you may need to manually open the prediction.jpg to see.
    
3. #### Make

        make -j6
        
    This will take serveral minutes.
    You are almost successed!

4. #### Now get weights
    From official site 
        
        wget https://pjreddie.com/media/files/yolov3.weights
    
    Or from [Baiduyun Driver](https://pan.baidu.com/s/1ZxJgjWrq7RCaGif8NvVrJg)(Fast in China)


Now all your preparation work are **done**.Good job!Here are some instructions to let your YOLO work for you.

1. #### Image Detection

        ./darknet detect cfg/yolov3.cfg yolov3.weights data/dog.jpg
    You should see something like this.
```
  layer     filters    size              input                output
      0 conv     32  3 x 3 / 1   416 x 416 x   3   ->   416 x 416 x  32  0.299 BFLOPs
      1 conv     64  3 x 3 / 2   416 x 416 x  32   ->   208 x 208 x  64  1.595 BFLOPs
      .......
    105 conv    255  1 x 1 / 1    52 x  52 x 256   ->    52 x  52 x 255  0.353 BFLOPs
    106 detection
  truth_thresh: Using default '1.000000'
  Loading weights from yolov3.weights...Done!
  data/dog.jpg: Predicted in 0.029329 seconds.
  dog: 99%
  truck: 93%
  bicycle: 99%
```
    If you have used OPENCV = 1 ,then you should see the pictrue.
    The time consume should between 0.1 and 0.4 if you have used GPU = 1.

2. #### Video Detection
    
        ./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights video.mp4
    *By now I have only used .mp4 format video file.*
    It should run at 2-4 fps if you have used GPU = 1.
3. #### Onboard Camera 
    
        ./darknet detector demo cfg/coco.data cfg/yolo.cfg yolo.weights "nvcamerasrc ! video/x-raw(memory:NVMM), width=(int)1280, height=(int)720,format=(string)I420, framerate=(fraction)30/1 ! nvvidconv flip-method=0 ! video/x-raw, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink"
4. #### Plug-in Camera
    
        I haven`t tested it really.So leave it a blank.

Hope everything is OK for you!

### Further reading
1. #### How to use tiny models<br>
  
  
    It`s simple.Replace all yolov3 with yolov3-tiny in those instructions.
2. #### It`s so slow!<br>
  
  
    There is a good idea to improve this situaiton: modify input size.
        
        vim cfg/yolov3.cfg
    Set width and height smaller.Try 256, 320, 384 and so on. But this will decrease it`s accuracy of course.My experience 
    is 256 is still a good choice.But the smaller ones would be too bad. 

3. #### How to train and evaluate models?


    There is a good repo by [AlexyAB](https://github.com/AlexeyAB/darknet).He can help you in this topic a lot.
   
    
  
