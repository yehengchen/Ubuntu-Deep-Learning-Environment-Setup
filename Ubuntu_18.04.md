 # Ubuntu 18.04 Deep Learning Environment Setup
 
*tensorflow-gpu & Nvidia GPU & Cuda & Cudnn 环境配置*

<div align="left">
  <img src="https://github.com/yehengchen/Ubuntu-16.04-Deep-Learning-Environment-Setup/blob/master/img/cuda_gpu_version.png" width="600">

***
## Ubuntu 18.04 配置版本
* #### Ubuntu Kernel Version 5.0.0-29
* #### GeForce GTX 1080 Ti / RTX 2080 Ti
* #### NVIDIA 430.14
* #### cuda 10.0
* #### cuDNN v7.6.0
* #### tensorflow-gpu 1.13.1
* #### numpy 1.14.5
* #### python 3.6
***

## __0. 更新软件源 & 确认配置版本 - Update and Upgrade__
    
    $ sudo apt-get update
    $ sudo apt-get upgrade
    
## 1. 卸载所有原驱动

    $ sudo apt-get purge nvidia*

## 2. 禁用nouveau

##### • 新建-blacklist-nouveau.conf 输⼊指令:

    $ sudo vi /etc/modprobe.d/blacklist-nouveau.conf

##### • 往 blacklist-nouveau.conf 文件中写⼊：
    
    blacklist nouveau
    options nouveau modeset=0

*禁 Ubuntu 自带开源驱动nouveau，写入后保存重启*
    
    $ sudo reboot 

##### • 重启后在终端执行行命令:(Terminal) 
    
    $ lsmod | grep nouveau

##### 查看nouveau模块是否被加载，若`无输出`，则执行下一步
***
## 3. 下载并安装 Cuda 和 Nvidia 驱动
##### Download Installer for Linux Ubuntu 18.04 x86_64

##### `GTX 1080 / RTX 2080 - Download CUDA: CUDA Toolkit 10.0 (Sept 2018)` [[CUDA Link]](https://developer.nvidia.com/cuda-toolkit-archive)  

<div align="left">
  <img src="https://github.com/yehengchen/Ubuntu-16.04-Deep-Learning-Environment-Setup/blob/master/img/ubuntu_18.04_cuda.png" width="800">

	
    Ctrl + Alt + F1-( Enter virtual consoles )进入tty1命令行界面安装CUDA
    Ctrl + Alt + F7-( Return back to GUI )回到桌面系统界面

#### 禁⽤X服务 - Kill your current X server session by typing

    $ sudo service lightdm stop

### 安装 CUDA 和 NVIDIA 驱动
*__注意：确定下载的 cuda 版本后执行指令__*
	
    $ sudo chmod 777 cuda_10.0.130_410.48_linux.run
    $ sudo sh cuda_10.0.130_410.48_linux.run --no-opengl-libs
    ...
    
   __*进入 CUDA 安装中同意安装 Nvidia 驱动*__
>     [accept] #同意安装
>     [y]      #安装Driver，将自动安装CUDA版本相匹配的Nvidia驱动
>     [y]      #安装CUDA Toolkit install
>     <Enter>  #安装到默认目录
>     [y]      #创建安装目录的软链接
>     [n]      #不复制Samples，因为在安装目录下有/samples

__*安装完成后会显示 CUDA 和 Nvidia 驱动成功安装*__
##### A. vim 打开.bashrc 在末行加⼊以下命令

    export PATH="/usr/local/cuda/bin:$PATH"
    export LD_LIBRARY_PATH="/usr/local/cuda/lib64:$LD_LIBRARY_PATH"
    
##### 执行指令更新 .bashrc 文件 - Reload .bashrc with 
    
    $ source .bashrc

##### B. Nvidia 驱动安装完成

    $ nvidia-smi

##### C. 安装及路径测试，CUDA安装完成

    $ nvcc -V
    
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190609205106404.png)

##### 编译并测试设备 deviceQuery: 
    
    $ cd /usr/local/cuda-9.0/samples/1_Utilities/deviceQuery
    $ sudo make
    $ ./deviceQuery

##### 编译并测试带宽 bandwidthTest:
    
    $ cd ../bandwidthTest
    $ sudo make
    $ ./bandwidthTest

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190406155104326.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODEwNzI3MQ==,size_16,color_FFFFFF,t_70)
##### 如果两个测试的结果都是 `Result = PASS CUDA` 安装成功 
***
## 4. 安装 cuDNN
#### `GTX 1080 - cuDNN v7.1.4 (May 16, 2018), for CUDA 9.0` [[cuDNN Link]](https://developer.nvidia.com/rdp/cudnn-archive)
#### `RTX 2080 - cuDNN v7.5.0 (May 16, 2018), for CUDA 10.0` [[cuDNN Link]](https://developer.nvidia.com/rdp/cudnn-archive)
__*RTX 2080 Ti 请按指定版本安装 cuDNN 以及更改以下相对应的安装文件名默认执行 gtx 1080 Ti*__

##### 解压后的 cudnn-9.0-linux-x64-v7.1.tgz ⽂文件cuda，执行以下指令安装:

    $ tar -zxvf cudnn-9.0-linux-x64-v7.1.tgz
    $ cd cuda $ sudo cp lib64/lib* /usr/local/cuda/lib64/
    $ sudo cp include/cudnn.h /usr/local/cuda/include/

##### 然后更新网络连接:

    $ cd /usr/local/cuda/lib64/ $ sudo chmod +r libcudnn.so.7.1.4
    $ sudo ln -sf libcudnn.so.7.1.4 libcudnn.so.7
    $ sudo ln -sf libcudnn.so.7 libcudnn.so $ sudo ldconfig
### __5. 安装Tensorflow-gpu_

__GTX 1080 / RTX 2080__

     $ pip install --user tensorflow-gpu==1.13.0
     
![tensorflow-gpu](https://img-blog.csdnimg.cn/20190406155752303.png)

## 6. 测试 - Testing
![tensorflow-gpu](https://img-blog.csdnimg.cn/20190406155821717.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl8zODEwNzI3MQ==,size_16,color_FFFFFF,t_70)
***
## 7. 常见配置问题 - Issues

### 登⼊界⾯死循环问题: Login loop issue
#### 1. 进入文本界面: CTRL+ALT+F1
#### 2. Uninstall any previous drivers: - 删除 nvidia 相关文件
    
    $ sudo apt-get remove nvidia-*
    $ sudo apt-get autoremove       

#### 3. Uninstall the drivers from the .run file: - 卸载 nvidia 驱动

    $ sudo nvidia-uninstall

#### 4. 此时，重启 - Reboot -> login normally
#### 5. 驱动重新安装 - Driver reinstall

***

### NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running issue

__GTX 1080 Ti__

#### 0. 确认 Kernel 版本是否高于 4.10
	$ uname -a
	
	#目前使用版本为 4.15
	Linux CAI 4.15.0-50-generic #54~16.04.1-Ubuntu SMP Wed May 8 15:55:19 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux

*__若版本高于 4.10 必须降级， 下载 4.10 内核方法如下__*
	
  	wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10/linux-headers-4.10.0-041000_4.10.0-041000.201702191831_all.deb
  	wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10/linux-headers-4.10.0-041000-generic_4.10.0-041000.201702191831_amd64.deb
	wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v4.10/linux-image-4.10.0-041000-generic_4.10.0-041000.201702191831_amd64.deb

	sudo dpkg -i *.deb
*升级完成后 nvidia-smi 出现 GPU 使用狀況栏可不用重新安装 Driver， 若未出现可按步骤重新安装 Driver*

#### 1. 确认是否插入显卡
	$ lspci | grep 'VGA'
	
	#找到卡后，显示显卡讯息
	01:00.0 VGA compatible controller: NVIDIA Corporation Device 1b06 (rev a1)

#### 2. 确认 security boot 是否为disable的状态
	a. 开机后, 进入Bios 设定画面(若是Acer的电脑, 按Del 或是F2 即可进入Bios)
	b. 改成disable 后, 重新开机

***
### 调整屏幕分辨率: Display resolution issue

### METHOD1:
#### 1.添加 /etc/X11/xorg.conf 文件，将此模式保存为默认分辨率。 
    
    $ sudo gedit /etc/X11/xorg.conf

#### 2.粘贴以下内容: - copy below -> paste to xorg.conf
    
    Section "Monitor"
    Identifier "Configured Monitor"
    Modeline "1920x1080_60.00" 173.00 1920 2048 2248 2576 1080
    1083 1088 1120 -hsync +vsync
    Option "PreferredMode" "1920x1080_60.00"
    EndSection
    Section "Screen"
    Identifier "Default Screen"
    Monitor "Configured Monitor"
    Device "Configured Video Device"
    EndSection
    Section "Device"
    Identifier "Configured Video Device"
    EndSection

### METHOD2: 
#### 1.生成指定分辨率 （one-off）

	$ cvt 1920 1080
	# 1920x1080 59.96 Hz (CVT 2.07M9) hsync: 67.16 kHz; pclk: 173.00 MHz
	Modeline "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync

#### 2.使用xrandr创建new mode （make newmode）
	$ sudo xrandr --newmode "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
#### 3.添加newmode,终端输入xrand查看显示器名称 (add mode)
	$ sudo xrand --addmode [THE NAME OF YOUR DISPLAY] "1920x1080_60.00"
#### 4.将分辨率应用到输出设备 （output display）
	$ sudo xrand --output [THE NAME OF YOUR DISPLAY] --mode "1920x1080_60.00"
***
## Uninstall - 卸载

### 卸载 nvidia - uninstall nvidia
    $ sudo apt-get purge nvidia*

### 卸载 Cuda - uninstall Cuda:
    
    $ cd /usr/local/cuda/bin 
    $ sudo ./uninstall_cuda_7.5.pl
    
### 卸载 Cudnn - uninstall Cudnn:

    $ sudo rm -rf /usr/local/cuda/include/cudnn.h
    $ sudo rm -rf /usr/local/cuda/lib64/libcudnn
    


