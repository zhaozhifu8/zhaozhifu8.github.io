---
layout: post
title:  "树莓派安装系统之后的一些配置"
subtitle: 'post subtitle'
date:   2018-12-12 14:21:28
tags: 树莓派 Raspberry Pi
description: ''
color: 'rgb(154,133,255)'
cover: '../assets/blogImg/raspbian.webp'
---



![](../assets/blogImg/raspbian.webp)
	

树莓派Raspbian系统默认密码为raspberry，ubuntu mate默认密码为ubuntu

**（1）.**在sd卡上刷完了Ubuntu mate系统然后开机启动在命令行里输入sudo raspi-config然后输入密码进行初始配置。
	按上下键选择第五选项Advaned Options项 第一项是扩展sd卡的存储回车选ok就可以，在扩展sd卡的方法还有一个:


<!--more-->
```
pi@raspberrypi ~ $ df -h #查看当前磁盘大小，总大小只有2.9GB

Filesystem      Size  Used Avail Use% Mounted on

rootfs          2.9G  2.8G   15M 100% /

/dev/root       2.9G  2.8G   15M 100% /

devtmpfs        214M     0  214M   0% /dev

tmpfs            44M  244K   44M   1% /run

tmpfs           5.0M     0  5.0M   0% /run/lock

tmpfs            88M     0   88M   0% /run/shm

/dev/mmcblk0p1   56M   19M   37M  34% /boot

tmpfs            88M     0   88M   0% /tmp
```



```
pi@raspberrypi ~ $ cat /sys/block/mmcblk0/mmcblk0p2/start   # 查看第二分区的起始地址，后面会用到
122880

pi@raspberrypi ~ $ sudo fdisk /dev/mmcblk0   #使用fdisk操作磁盘

Command (m for help): d   #d，删除分区

Partition number (1-4): 2   # 2，删除第二分区

Command (m for help): n  #创建一个新分区

Partition type:

   p   primary (1 primary, 0 extended, 3 free)

   e   extended

Select (default p): p  #创建主分区

Partition number (1-4, default 2): 2  #分区2

First sector (2048-7744511, default 2048): 122880  #输入第一次得到的第二分区起始扇区

Last sector, +sectors or +size{K,M,G} (122880-7744511, default 7744511):  #最后一个sector，默认即可Enter
Using default value 7744511
```



```
Command (m for help): w   #将上面的操作写入分区表

The partition table has been altered!



Calling ioctl() to re-read partition table.



WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at

the next reboot or after you run partprobe(8) or kpartx(8)

Syncing disks.
```





```
pi@raspberrypi ~ $ sudo reboot  #设置完成需要重启，sudo reboot
```



**（2）.**设置root账户密码：
			在命令行里输入sudo passwd
			然后敲击回车 输入当前用户的密码。
			在回车就会出现Enter new UNIX password:输入新的root密码
			输入新的root密码之后还会让你重新输入一遍 Retype new UNIX password:重新输入密码
			passwd：passwd updated sucessfully
			然后在命令行里输入su root 回车和密码就可以进入root账户了

**（3）.**开启ssh服务也有两个方法先说简单的
	还是在raspi-config第三选项Interfacing Options里，然后开启ssh。
	之后在树莓派里查看ip地址 用putty这个软件进行远程连接。
**（4）.**安装vim编辑器与git

```
# sudo apt-get install vim

# sudo apt-get install git
```

**（5）.**安装google拼音

```
# sudo apt-get install fcitx fcitx-googlepinyin
```

**（6）.**更换软件源

1、把原来的http://ports.ubuntu.com/全部替换为http://mirrors.ustc.edu.cn/ubuntu-ports/(中科大)
使用命令：

```
# sudo vim /etc/apt/sources.list
```

```
deb https://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse

deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse

deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial-security main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse

deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse

deb https://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse

deb-src https://mirrors.ustc.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
```

2、然后保存退出

```
#sudo apt-get update
#sudo apt-get upgrade
```

进行更新

**（7）.**通过蓝牙连接音响播放音乐

打开蓝牙管理器然后查找设备，之后配对 完成之后右键连接到音频槽
使用vlc播放音乐，如果音响没有声音，在播放设备里选择音频设备，选择蓝牙音响设备就可以了。
2019年7月26日最后修改