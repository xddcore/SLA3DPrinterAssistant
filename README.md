# SLA3DPrinterAssistant:一个基于树莓派的光固化3D打印机助手，让你随时随地可以访问打印工作。
## 作者:xddcore(Chengsen Dong)|业余小玩具
## Email:1034029664@qq.com
## Github:www.github.com/xddcore
## Version:1.0
## Date:21/04/2022

# 项目简介
**SLA3DPrinterAssistant**致力于监测打印过程中可能出现的问题，以及远程接收操作者命令和回传打印机状态。

在正常的SLA 3D打印中，每次Z轴的抬升都会让模型接触面与离型膜发出独特频率的声音信号。这个声音信号标志着打印过程的正常进行。若声音信号消失，则证明可能出现模型脱落，打印面未能与模型融合等打印问题。此时，操作者应该尽快处理故障，避免引发严重的二次损坏。在过往的SLA 3D打印中，为了避免这个问题的出现。操作者往往需要值守在打印机附近，不断依据这个声音信号来判断打印机工作是否正常，这个过程往往费时费力。在本开源项目中，我们使用神经网络技术对声音信号进行实时监控。当出现声音信号消失的情况时，设备将会通过用户设定的提醒方式(不限于APP通知/邮件/短信)将故障信息发送至用户。用户便可以及时介入。

同时，针对于支持USB/以太网的SLA 3D打印机。本硬件会自动将打印机状态(不限于打印的速度，当前打印的文件名，预计打印时间等)传输至Web页面中。另外，如果用户需要视频流监控，可根据实际情况添加摄像头。同时，用户还可以通过Web页面对打印机发送打印文件及命令，远程操控打印机。最后，若您的打印机不支持通过USB/以太网等方式向外传输数据，则本设备会使用算法进行对当前打印进度的估测。

# 项目特性
1. 基于神经网络的打印状态监测。
2. Raspberry Pi(推荐Zero系列)实现，方便好买。
3. 让手中的3D打印机更方便易用。

# 演示视频及分析
1. 1.树莓派神经网络计算耗时较大，无法做到实时采样。可能导致错过有效声音信号（Good），或有效声音信号不完整。 |在用树莓派+WM8960采集数据集的时候，是实时采集的。
2. 数据集不够丰富以及用于训练的数据集不够多(标定不完了，太多了)。根据实验，神经网络计算和信号的能量，频率分布以及出现的时间(也就是frame index,(H Axis))都有关。
3. 不知道为啥stft的库，Fs = 16000，step = 512，用tf_IO出来的应该是frame = 30。然后树莓派上没有tf_IO,所以我用了另外一个库，出来的就是frame = 35.然后我直接[:30]，裁出了前30个frame。

# 关键Packet安装

1. Tensorflow For Raspi

https://github.com/lhelontra/tensorflow-on-arm

2. Tensorflow io For Raspi

```
# get a fresh start
$ sudo apt-get update
$ sudo apt-get upgrade
# install pip3
$ sudo apt-get install git python3-pip
Method 1
# download tensorflow io
$ git clone -b v0.23.1 --depth=1 --recursive https://github.com/tensorflow/io.git
$ cd io
$ python3 setup.py -q bdist_wheel --project tensorflow_io_gcs_filesystem
$ cd dist
$ sudo -H pip3 install tensorflow_io_gcs_filesystem-0.23.1-cp39-cp39-linux_aarch64.whl
$ cd ~
Method 2
# or download wheel
$ git clone https://github.com/Qengineering/Tensorflow-io.git
$ cd Tensorflow-io
$ sudo -H pip3 install tensorflow_io_gcs_filesystem-0.23.1-cp39-cp39-linux_aarch64.whl
$ cd ~
```

# 项目技术细节

## 1. 硬件实现

### 1.1 树莓派Zero 2W

### 1.2 WM8960（音频采集）

### 1.3 CSI摄像头（视频采集）

### 1.4 给设备打印个外壳QAQ

## 2. 软件实现

### 2.1 神经网络

#### 2.1.1 数据集

#### 2.1.2 神经网络

#### 2.1.3 网络部署

### 2.2 云平台

#### 2.2.1 打印机数据获取(针对于支持USB/以太网的打印机)

#### 2.2.2 Web搭建

#### 2.2.3 随时随地(内网穿透)

#### 2.2.4 异常提示

## 3. 结语

## 4. 致谢




