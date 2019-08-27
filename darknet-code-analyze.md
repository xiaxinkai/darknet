# darkent 源代码解析
参考代码路径：<https://github.com/xiaxinkai/darknet>  
说明：
- <https://github.com/xiaxinkai/darknet>是在<https://github.com/pjreddie/darknet>的基础上增加了1个demo内容所需的配置文件和图片。
- demo内容是：通过网络搜集NBA球员库里(KuLi)和杜兰特(DuLanTe)的图片进行训练，然后再用训练完成的weights参数来测试其他的图片。
- demo的详细内容和执行方法请参考：<https://zhuanlan.zhihu.com/p/45852709> 和 <https://github.com/xiaxinkai/darknet>
- 结合这个demo来说明darknet的原因是：这个demo只有40张标注好的图片训练样本，样本数量比较小，便于我们理解整个darknet训练和测试的原理。

## 全局观概述
- <https://github.com/pjreddie/darknet>中C语言实现的代码只是1个深度学习框架。
- YOLOv3算法是darknet通过读取配置文件cfg/yolov3.cfg来实现的。
- cfg/yolov3.cfg主要定义了YOLOv3的卷积神经网络的网络结构。
- darknet通过读取不同的配置文件，除了实现YOLOv3算法，还可以实现其他的算法，例如rnn(循环神经网络)，Tiny YOLOv3等。
- 如果想要使用自己的算法，创建自己的cfg文件，并在cfg文件内定义自己的网络结构即可。

## 以下将结合YOLOv3算法的实现来讲解darknet代码。
主要分为训练和测试两部分来讲解。

### [机能] 1. 训练
命令行示例：  
`./darknet detector train cfg/KD.data cfg/yolov3-KD.cfg_train darknet53.conv.74`

- 入口函数: examples/darknet.c line:400  
`int main(int argc, char **argv)`  

- examples/darknet.c line:431  
`run_detector(argc, argv);`

- examples/detector.c line:431  
`else if(0==strcmp(argv[2], "train")) train_detector(datacfg, cfg, weights, gpus, ngpus, clear);`

- examples/detector.c line line:26 读取cfg文件，创建神经网络  
`nets[i] = load_network(cfgfile, weightfile, clear);`


### [机能] 2. 测试
命令行示例：  
`./darknet detector test cfg/KD.data cfg/yolov3-KD.cfg_test backup/yolov3-KD_900.weights data/KD/val_images/41.jpg -thresh 0.5`
