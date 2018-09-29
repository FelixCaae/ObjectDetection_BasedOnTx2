# Object Detection Research Based on TX2
Object Detection is a category of Computer Science which aims at recognizing objects in images and simultaneously reporting object`s position and bounding box.
Here is an example.
![image][https://pjreddie.com/media/image/Screen_Shot_2018-03-24_at_10.53.04_PM.png]
And another example.
![image][https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=1233252412,2871820223&fm=26&gp=0.jpg]
Current Object Detection technique can be subdivided into two  classes: one-stage methods and two-stage methods.Usually speaking, one-stage methods run faster and their models are simpler.Classic one-stage model includes yolo,ssd,retina-net and so on.These models and their variation are considered to be good for embeded system like TX2.

TX2 ,whose whole whole name is Nvidia Jetson TX2 , is a powerful embedded system(Soc).It has 6 cpu cores and a gpu with 256 cuda cores.Its computing power may not be able to beat GTX 1070ti but is enough for some applications. To deeply dig out its potential in object detection, we firstly need to find a best baseline model.  


