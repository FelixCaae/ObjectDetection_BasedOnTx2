# Object Detection Research Based on TX2
Lately our lab get a new device:TX2.My job is to do some research around implementing Object Detecton Algorithms on it. 

TX2, whose whole name is Nvidia Jetson TX2, is a powerful embedded system(SoC).It has 6 cpu cores and a gpu with 256 cuda cores.More details can be found [here](https://www.nvidia.com/zh-cn/autonomous-machines/embedded-systems-dev-kits-modules/).
Its computing power may not be able to beat GTX 1070ti but is enough for some actual applications.

Object Detection is a category of Computer Vision which aims at recognizing objects in images and simultaneously reporting object`s position and bounding box.

Here is an example.<br>
![image](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1233252412,2871820223&fm=26&gp=0.jpg)<br>
And another example.
![image](http://mmbiz.qpic.cn/mmbiz_png/iaTa8ut6HiawDhWYblXp7Uqo1KKTNzCzzRITWA48CsUGcnVUiayPmfGW00KF7ia6nXPguAYLVpicTYZ3EMOusgT5Y5w/640?wx_fmt=png&wxfrom=5&wx_lazy=1)<br>
It is easy to understand what the term 'detection' means after seeing  this.It means the combination of **Classification** and **Localization**.

Current Object Detection technique can be subdivided into two  classes: one-stage methods and two-stage methods.The two-stage methods divide the problem into two steps:1 .Find appropriate area. 2. Use classifiers to recognize what\`s in the area.Howeverr, one-stage methods simply use a unified CNN network to predict both objects\` class and location.Usually speaking, one-stage methods run faster with a little accuracy loss.Classic one-stage model includes YOLO(you only look once),SSD(single shot detection),retina-net and so on.These models and their variation are better candidates because they are much more faster than two-stage methods.
After some researches, I found some promising one-stage methods.
Before we go on, let`s figure out some important conception.
## Metrics - what does mAP mean? 
Prepare yourself, click into this link for an answer - http://tarangshah.com/blog/2018-01-27/what-is-map-understanding-the-statistic-of-choice-for-comparing-object-detection-models/
## DataSets
[COCO](http://cocodataset.org/) and [VOC](http://host.robots.ox.ac.uk:8080/leaderboard/main_bootstrap.php) are usually used in training and evaulating models.
## State of art
It`s very hard to list all models and their metrics and a little easier to give some typical models.But if we want to compare these models` ability, we have to make sure that some variables like machines they run on are same.Frameworks also influence, so it`s better to implement them all on a single framework with all condition same.But I think to make things easier, use their reported numbers can be a good start.

#Non light model
|Models | AP| AP.5 |Backbone| time|
|----|----|----|----|----|
|[YOLOv3-608](https://arxiv.org/abs/1804.02767)|33.0 |57.9 |darknet53   |51|
|[YOLOV3-320](https://arxiv.org/abs/1804.02767)|nan  |51.5 |darknet53   |22|
|[SSD300](https://arxiv.org/abs/1512.02325)    |23.2 |41.2 |VGG-16      |17|
|[SSD512](https://arxiv.org/abs/1512.02325)    |26.8 |46.5 |VGG-16      |45|
|[DSSD513](https://arxiv.org/abs/1701.06659)   |33.2| 53.3 |Residual-101|182|
|Tiny-DSOD|40.4||nan|
|[RFB Net512](https://arxiv.org/abs/1711.07767) |33.8|54.2 |VGG-16      |30|
|[RetinaNet](https://arxiv.org/pdf/1708.02002.pdf)|40.8|61.1|ResNeXt-101-FPN|200+|
#Light model
|Models | AP| AP.5 |Backbone| time|
|----|----|----|----|----|
|[YOLOV3-tiny](https://arxiv.org/abs/1804.02767)|nan |33.1 |nan         |5|
|
![image](https://pjreddie.com/media/image/map50blue.png)
*This result come from pjreddie.com*

Then you can clone the newest yolov3 repo、modify Makefile and make.Details can be found in this [blog](https://jkjung-avt.github.io/yolov3/)
Assuming everything went OK, you should be able to  run test on your TX2 board!
But I found that yolov3 is really slow on TX2.The first time I run it, I only get 2\~3 fps(or 0.3\~0.5 sec).I have run nvpmodel and jetson_clocks already.So we decided to check something else.
The best thing I found is that you can set the model\`s input size in yolov3.cfg file.The input size can vary from 144 to 608.(The size must be 32 x ratio).I didn\`t test bigger or smaller size.As to our research I guess that\`s enought.Too small make model useless while too big is not bearably slow.
So let\`s try to decrease the default size(416) and see whether we can find a good trade-off.
I do this test by using yolov3 model(.cfg) and voc data.Sadly I found that there is no yolov3-voc weights available, the official yolov3.weights is trained on coco data.So I trained my own yolov3-voc weights(10400 iterations , used yolov3.weights as pretrained model).
And use this weights to calculate mAP.
Here is the result I have(Size vary from 160-384).

![image](https://github.com/FelixCaae/ObjectDetection_BasedOnTx2/blob/master/fps_mAP.png)

So the result is interesting, the fourth point and fifth point has nearly same perfomance but totally different speed.There is a gap between two group of points.And so I think the fifth point(size 256) is a good choice to speed up yolov3`s perfomance.
