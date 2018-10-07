# Object Detection Research Based on TX2
Lately our lab get a new device:TX2.My job is to do some research around implementing Object Detecton Algorithms on it. 

TX2, whose whole name is Nvidia Jetson TX2, is a powerful embedded system(SoC).It has 6 cpu cores and a gpu with 256 cuda cores.More details can be found [here](https://www.nvidia.com/zh-cn/autonomous-machines/embedded-systems-dev-kits-modules/).
Its computing power may not be able to beat GTX 1070ti but is enough for some actual applications.

Object Detection is a category of Computer Vision which aims at recognizing objects in images and simultaneously reporting object\`s position and bounding box.

Here is an example.<br>
![image](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1233252412,2871820223&fm=26&gp=0.jpg)<br>
And another example.
![image](http://mmbiz.qpic.cn/mmbiz_png/iaTa8ut6HiawDhWYblXp7Uqo1KKTNzCzzRITWA48CsUGcnVUiayPmfGW00KF7ia6nXPguAYLVpicTYZ3EMOusgT5Y5w/640?wx_fmt=png&wxfrom=5&wx_lazy=1)<br>
It is easy to understand what the term 'detection' means after seeing  this.It means the combination of **Classification** and **Localization**.

Current Object Detection technique can be subdivided into two  classes: one-stage methods and two-stage methods.The two-stage methods divide the problem into two steps:1 .Find appropriate area. 2. Use classifiers to recognize what\`s in the area.Howeverr, one-stage methods simply use a unified CNN network to predict both objects\` class and location.Usually speaking, one-stage methods run faster with a little accuracy loss.Classic one-stage model includes YOLO(you only look once),SSD(single shot detection),retina-net and so on.These models and their variation are better candidates because they are much more faster than two-stage methods.
After some researches, I found some promising one-stage methods.
Before we go on, let\`s figure out some important conception.
## Metrics - what does mAP mean? 
Prepare yourself, click into this link for an answer - http://tarangshah.com/blog/2018-01-27/what-is-map-understanding-the-statistic-of-choice-for-comparing-object-detection-models/
## DataSets
[COCO](http://cocodataset.org/) and [VOC](http://host.robots.ox.ac.uk:8080/leaderboard/main_bootstrap.php) are usually used in training and evaulating models.
## State of art


#### Normal models(time tested on Titan X)

|Models | AP| AP.5 |Backbone| time|
|----|----|----|----|----|
|[YOLOv3-608](https://arxiv.org/abs/1804.02767)|33.0 |57.9 |darknet53   |51|
|[YOLOV3-320](https://arxiv.org/abs/1804.02767)|nan  |51.5 |darknet53   |22|
|[SSD300](https://arxiv.org/abs/1512.02325)    |23.2 |41.2 |VGG-16      |17|
|[SSD512](https://arxiv.org/abs/1512.02325)    |26.8 |46.5 |VGG-16      |45|
|[DSSD513](https://arxiv.org/abs/1701.06659)   |33.2 |53.3 |Residual-101|182|
|[RFB Net512](https://arxiv.org/abs/1711.07767) |33.8|54.2 |VGG-16      |30|
|[RetinaNet](https://arxiv.org/pdf/1708.02002.pdf)|40.8|61.1|ResNeXt-101-FPN|200+|

#### Light models(time tested on TX2)

|Models | VOC mAP | COCO mAP |Params|forward time|
|----|----|----|----|-----|
|[RFB_Mobile](https://arxiv.org/abs/1711.07767)       |73     |nan     |7.4M    |30    |  
|[YOLOV3-tiny-512](https://arxiv.org/abs/1804.02767)  |62     |nan     |nan     |46    |
|[Pelee](https://arxiv.org/pdf/1804.06882.pdf)        |70.9   |22.4    |5.98M   |60    | 
|[Tiny-DSOD](https://arxiv.org/abs/1807.11013)        |77     |23.2    |0.95M   |110   |  


It\`s very hard to list all models and their perfomance but a little easier to give some typical models.But you can click [here](http://cocodataset.org/#detection-leaderboard) to see leaderboards of object detection on coco datasets.

Remeber, not all of these models are implemented and tested under a same circustance(though I personally hope there will be an official testing organiztion).This means speed can be different if you run on your own machine.



