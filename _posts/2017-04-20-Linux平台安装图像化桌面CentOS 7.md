---
layout: post
title: "Linux平台安装图像化桌面CentOS 7"
date: 2017-04-20
description: "Linux平台安装图像化桌面CentOS 7"
tag: 博客
---



# Linux平台安装图像化桌面CentOS 7


 
###安装操作系统
本项目需要部署在Linux平台下。建议使用CentOS 7。
如果事前已经安装了操作系统，请跳过此章节。
本文假定读者具备Linux图形界面的使用能力。



1.	启动服务器，插入系统安装光盘或U盘，并在BIOS中设置以光盘或U盘启动（视具体场合下的安装介质而定）。

2.	启动后，进入如同下图的画面，利用键盘的方向键选择第一项“Install CentOS 7”，等待片刻后进入安装画面。

<img src="/images/centos图形化/1.png" height="500" width="600">

3.	进入CentOS7安装程序。第一个画面是选择要安装的系统的语言。接下来的截图选择的是简体中文（中国）。选择后，按【继续】进入下一步的安装概况画面。
<img src="/images/centos图形化/2.png" height="500" width="600">

4.	点击【日期与时间】，进入系统时间设置。

<img src="/images/centos图形化/3.png" height="500" width="600">

5.	确认系统时区、日期与时间是否正确。完成确认或修改后，点击【完成】按钮返回安装概况。

<img src="/images/centos图形化/4.png" height="500" width="600">
6.	回到安装概况画面后，点击【安装位置】，选择安装目的地。

<img src="/images/centos图形化/5.png" height="500" width="600">

7.	设置硬盘分区。一般而言，选择第一个本地磁盘，然后分区选项选择【自动配置分区】即可。如果有需要，也可以自定义分区大小等数据。完成配置后点击“完成”按钮即可。
<img src="/images/centos图形化/6.png" height="500" width="600">

8.	完成分区选择与配置后，回到安装概况画面，点击【软件选择】。
<img src="/images/centos图形化/6.png" height="500" width="600">


9.	在软件选择画面中，从左边栏找到【带GUI的服务器】，然后在右边栏找到【KDE】、【MariaDB数据库服务器】、【开发工具】，勾选这三个工具，然后点击“完成”。
<img src="/images/centos图形化/7.png" height="500" width="600">
<img src="/images/centos图形化/8.png" height="500" width="600">
<img src="/images/centos图形化/9.png" height="500" width="600">
10.	回到安装概况画面，点击右下角的【开始安装】按钮进入下一个环节。
<img src="/images/centos图形化/13.png" height="500" width="600">

11.	安装程序正在把文件复制到磁盘上。此时，用户可以配置root账号密码，以及添加更多用户账号。先点击【ROOT 密码】设置root密码。
<img src="/images/centos图形化/10.png" height="500" width="600">

12.	在ROOT密码画面中，设置root账户密码，然后点击“完成”。
<img src="/images/centos图形化/11.png" height="500" width="600">


13.	为了确保安全，一般建议系统管理员再设置另一个管理专用的账号，而不是直接使用root账号进行操作。点击【创建用户】可创建新的账户。记得勾选【将此用户做为管理员】选项。

<img src="/images/centos图形化/12.png" height="500" width="600">

14.	当系统文件复制、解压完毕后，安装程序会自动重启电脑，进入命令行界面。此时系统已安装完毕。

15.	启动系统后进入登录界面，系统默认登录界面是新创建的管理员账户，输入密码登录。

<img src="/images/centos图形化/14.png" height="500" width="600">


16.	如需用root账户登录，点击“未列出？”，输入root账户用户名、密码进行登录。

<img src="/images/centos图形化/15.png" height="500" width="600">

17.	登录后，系统界面。
<img src="/images/centos图形化/16.png" height="500" width="600">


 
###配置软件环境
在配置网络环境之前，请先通过ping命令，确认当前服务器是否可以访问互联网。
ping www.baidu.com


如果互联网已经接通，就可以执行下一步操作了。否则，请向网络管理相关人员寻求协助。

2.1安装JDK
在终端下运行以下命令安装或升级JDK 1.7.0：
sudo yum install java-1.7.0-openjdk



此时输入“y”，系统将自动完成安装。以下其他软件的安装方式也是类似，不再重复。

2.2安装Tomcat
在终端下按顺序运行以下命令，安装或升级Tomcat
sudo yum install tomcat
sudo yum install tomcat-webapps tomcat-admin-webapps

完成软件安装后，到以下路径，设置tomcat管理页面的账号密码：
/etc/tomcat/tomcat-users.xml



