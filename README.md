# Ubuntu 16.04 / 18.04 Deep Learning Environment Setup

* #### Ubuntu 18.04 - GTX 1080 / RTX 2080 CUDA 和 NVIDIA 驱动同时安装 [[中文文档]](https://github.com/yehengchen/Ubuntu-16.04-Deep-Learning-Environment-Setup/blob/master/Ubuntu_18.04.md)

* #### Ubuntu 16.04 - GTX 1080 / RTX 2080 CUDA 和 NVIDIA 驱动同时安装 [[中文文档]](https://github.com/yehengchen/Ubuntu-16.04-Deep-Learning-Environment-Setup/blob/master/Ubuntu_16.04_CN.md)

* #### Ubuntu 16.04 - GTX 1080 / RTX 2080 CUDA 和 NVIDIA 驱动单独安装 [[中文文档]](https://github.com/yehengchen/Ubuntu-16.04-Deep-Learning-Environment-Setup/blob/master/README.md)

*tensorflow-gpu & Nvidia GPU & Cuda & Cudnn 环境配置*

<div align="left">
  <img src="https://github.com/yehengchen/Ubuntu-16.04-Deep-Learning-Environment-Setup/blob/master/img/cuda_gpu_version.png" width="600">

***
## Ubuntu 16.04 配置版本
* #### Ubuntu Kernel Version 4.10.09
* #### GeForce GTX 1080 Ti
* #### NVIDIA 390.87
* #### cuda 9.0
* #### cuDNN v7.1.4
* #### tensorflow-gpu 1.10.0
* #### python 3.5
***
* #### Ubuntu Kernel Version 4.15.0
* #### GeForce RTX 2080 Ti
* #### NVIDIA 410.48
* #### cuda 10.0
* #### cuDNN v7.5.0
* #### tensorflow-gpu 1.13.1
* #### python 3.5
***

## __0. 更新软件源 & 确认配置版本 - Update and Upgrade__
    
    $ sudo apt-get update
    $ sudo apt-get upgrade

#### (Kernel Version 4.10.09)
#### Install Version: NVIDIA-Linux-x86_64-390.87.run
#### Download Nvidia Drivers: [[Nvidia Link]](https://www.geforce.com/drivers/results/137276)



***
## __1. 卸载所有原驱动 - uninstall all original drivers for safety__

    $ sudo apt-get purge nvidia*
***

## __2. 禁用nouveau - Disabling Nouvea__

#### • 新建-blacklist-nouveau.conf 输⼊指令:(Create a file)

    $ sudo vim /etc/modprobe.d/blacklist-nouveau.conf

#### • 往文件中写⼊-input :(with the following contents)
    blacklist nouveau
    options nouveau modeset=0

#### 禁 Ubuntu自带开源驱动nouveau，写入后 After above 重启系统 - Disabling Nouveau
    
    $ sudo reboot 

#### 在终端执行行命令:(Terminal) 
    
    $ lsmod | grep nouveau

#### 查看nouveau模块是否被加载，若无输出，则执行下一步 - If no output go to the next step.

***
## __3. 安装Nvidia驱动 - Install the Drivers__

    Ctrl + Alt + F1-( Enter virtual consoles )进入tty1命令行界面 
    Ctrl + Alt + F7-( Return back to GUI )回到桌面系统界面

#### 禁⽤X服务 - Kill your current X server session by typing

    $ sudo service lightdm stop

#### 给驱动run文件赋予执行权限(根据版本执行 - Check your NVIDIA version)

    $ sudo chmod a+x NVIDIA-Linux-x86_64-390.87.run
    $ sudo ./NVIDIA-Linux-x86_64-390.87.run -no-opengl-files -no-x-check -no-nouveau-check

#### [Tips] (登录界面循环问题 - Login loop issue)
* -no-opengl-files 只安装驱动文件，不安装OpenGL文件 (no install OpenGL file)
* -no-x-check 安装驱动时不检查X服务器 (no check X server)
* -no-nouveau-check 安装驱动时不检查nouveau模块 （no check nouveau module）
  
#### 安装驱动时 (Downloading)
    
    “Would you like to run the nvidia-xconfig utility to automatically update your X configuration file...”
    Choose No.
    After above: $sudo reboot
***
## __4. Nvidia驱动安装完成 - Check Driver was successfully installed__

    $ nvidia-smi
    
<img src="https://github.com/chenyeheng/Ubuntu_16.04_Deep_Learning_Environment_Setup/blob/master/img/gpu_setting.png" width="60%" height="60%">

***

## __5. 安装CUDA - CUDA Toolkit 9.0 Downloads__
#### Download Installer for Linux Ubuntu 16.04 x86_64
#### Download CUDA: cuda_9.0.176_384.81_linux.run [[CUDA Link]](https://developer.nvidia.com/cuda-toolkit-archive)

<div align="left">
  <img src="https://github.com/chenyeheng/Ubuntu_16.04_Deep_Learning_Environment_Setup/blob/master/img/cuda9.0.png" width="400">
</div>

<div align="left">
  <img src="https://github.com/chenyeheng/Ubuntu_16.04_Deep_Learning_Environment_Setup/blob/master/img/cuda9.0_1.png" width="400"><br><br>
</div>

### Install CUDA

    $ sudo ./cuda_9.0.176_384.81_linux.run --no-opengl-libs
    ...
>     [accept] #同意安装
>     [n]      #安装Driver，将自动安装CUDA版本相匹配的Nvidia驱动, - No install Driver
>     [y]      #安装CUDA Toolkit install
>     <Enter>  #安装到默认目录
>     [y]      #创建安装目录的软链接
>     [n]      #不复制Samples，因为在安装目录下有/samples

##### A. vim 打开.bashrc 在末行加⼊以下命令 - Add following lines to .bashrc
    
    export PATH="/usr/local/cuda/bin:$PATH"
    export LD_LIBRARY_PATH="/usr/local/cuda/lib64:$LD_LIBRARY_PATH"

##### 执行指令更新 .bashrc 文件 - Reload .bashrc with 
    
    $ source .bashrc

##### B. 安装及路径测试，查看CUDA版本 CUDA Sample Testing:

    $ nvcc -V

#### 编译并测试设备 deviceQuery: 
    
    $ cd /usr/local/cuda-9.0/samples/1_Utilities/deviceQuery
    $ sudo make
    $ ./deviceQuery

#### 编译并测试带宽 bandwidthTest:
    
    $ cd ../bandwidthTest
    $ sudo make
    $ ./bandwidthTest
    
#### 如果两个测试的结果都是 *Result = PASS CUDA* 安装成功 - Install successed 
     
     $ cd /usr/local/cuda-9.0/samples/1_Utilities/deviceQuery
     $ sudo make
     $ ./deviceQuery
     $ cd ../bandwidthTest
     $ sudo make
     $ ./bandwidthTest
*** 
## 6. 安装cuDNN - Download cuDNN 
#### Download Version: cuDNN v7.1.4 (May 16, 2018), for CUDA 9.0 [[cuDNN Link]](https://developer.nvidia.com/rdp/cudnn-archive)
![cudnn](https://github.com/chenyeheng/Ubuntu_16.04_Deep_Learning_Environment_Setup/blob/master/img/cudnn.png)
##### 解压后的 cudnn-9.0-linux-x64-v7.1.tgz ⽂文件cuda，执行以下指令安装:-Install cudnn

    $ tar -zxvf cudnn-9.0-linux-x64-v7.1.tgz
    $ cd cuda $ sudo cp lib64/lib* /usr/local/cuda/lib64/
    $ sudo cp include/cudnn.h /usr/local/cuda/include/

##### 然后更新网络连接:-Updata Network Connection

    $ cd /usr/local/cuda/lib64/ 
    $ sudo chmod +r libcudnn.so.7.1.4
    $ sudo ln -sf libcudnn.so.7.1.4 libcudnn.so.7
    $ sudo ln -sf libcudnn.so.7 libcudnn.so 
    $ sudo ldconfig
***
## __7. 安装Tensorflow-gpu - Install Tensorflow-gpu__
*注意这里 tensorflow-gpu 的版本要低于 1.10 否则 cuda 会报错误*

    $ sudo pip3 uninstall tensorflow
    $ pip3 install --user tensorflow-gpu==1.10.0


<img src="https://github.com/chenyeheng/Ubuntu_16.04_Deep_Learning_Environment_Setup/blob/master/img/tensorflow-gpu.png" width="60%" height="60%">

***

## 8. 测试 - Testing

<img src="https://github.com/chenyeheng/Ubuntu_16.04_Deep_Learning_Environment_Setup/blob/master/img/tensorflow.png" width="60%" height="60%">

***

## 9. 常见配置问题 - Issues

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

#### 0. 确认 Kernel 版本是否高于 4.10
	$ uname -a
	
	#目前使用版本为 4.15
	Linux CAI 4.15.0-50-generic #54~16.04.1-Ubuntu SMP Wed May 8 15:55:19 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux

*__若版本高于 4.10 必须升级， 降级方法如下__*
	
	wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.15.7/linux-headers-4.15.7-041507_4.15.7-041507.201802280530_all.deb

	wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.15.7/linux-headers-4.15.7-041507-generic_4.15.7-041507.201802280530_amd64.deb

	wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.15.7/linux-image-4.15.7-041507-generic_4.15.7-041507.201802280530_amd64.deb

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
    
## 国内 pip 速度问题换源 （阿里云pip源）

	cd ~
	mkdir .pip
	sudo vim .pip/pip.conf

写入：
	
	[global]
	index-url = http://mirrors.aliyun.com/pypi/simple/
	[install]
	trusted-host = mirrors.aliyun.com


<div align="left">	
  <img src="https://github.com/yehengchen/Ubuntu-16.04-Deep-Learning-Environment-Setup/blob/master/img/cuda_9.0_10.0_kernel_version.png" width="630">

# References
[CUDA Toolkit Documentation v9.0](https://docs.nvidia.com/cuda/archive/9.0/)

[CUDA Toolkit Documentation v10.0](https://docs.nvidia.com/cuda/archive/10.0/)

