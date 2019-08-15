# Use Darknet YOLOv3 to train my own dataset KD

1. make

2. modify cfg/KD.data:  
modify train path and valid path to your absolute path.  
for example:  
train  = /home/xiaxinkai/YOLOv3/darknet/data/KD/train.txt  
valid  = /home/xiaxinkai/YOLOv3/darknet/data/KD/val.txt  

3. wget https://pjreddie.com/media/files/darknet53.conv.74

4. ./darknet detector train cfg/KD.data cfg/yolov3-KD.cfg_train darknet53.conv.74 2>&1 |tee 201908.log

5. ./darknet detector test cfg/KD.data cfg/yolov3-KD.cfg_test backup/yolov3-KD*.weights data/KD/val_images/41.jpg -thresh 0.5

reference site:
https://zhuanlan.zhihu.com/p/45852709
