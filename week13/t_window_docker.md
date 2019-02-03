## ARTS

### Tip

## win10系统docker环境安装

**适用版本：64位Windows 10 Pro、Enterprise或者Education版本(Build 10586以上版本，需要安装1511 November更新)**


Docker for Windows Installer.exe  下载


**安装过程一直鼠标单击 "下一步" 即可**


**1、安装完成后，双击桌面上图标：**

![图片](https://uploader.shimo.im/f/hvZVKp69RBUxUIWC.png!thumbnail)

**2、在桌面左下角右键单击docker(类似帆船)的图标选择 "settings"，出现如下界面;**

![图片](https://uploader.shimo.im/f/EoG82YJUN9AE36k4.png!thumbnail)

**3、勾选一个共享盘符 （本教程以F盘符为共享盘为例）**

**备注：如果你使用的win10系统不是administrator账户，此时为弹出一个验证账户的对话框，要求输入你PC机器的账户的登陆密码，输入确定即可**

**4、设置docker仓库镜像地址，如下：**

![图片](https://uploader.shimo.im/f/NmiaQkZZaFsNm2YV.png!thumbnail)

**5、事先准备好的docker运行环境的压缩包放置在docker先前设定的共享的盘符内！dockerproject包文件**


**然后解压，如图所示：**

![图片](https://uploader.shimo.im/f/oAzlNwU8aAw5ZNPq.png!thumbnail)

**6、运行cmd,，进入dos界面，切换到共享盘符内，进入dockerproject内，运行dockertag.bat **

![图片](https://uploader.shimo.im/f/sZqcAaDjXRwqQC4Z.png!thumbnail)

备注：以上操作为拉取docker镜像

7、检查docker镜像，并启动docker服务

![图片](https://uploader.shimo.im/f/1yVEMCi5ljojBzlX.png!thumbnail)

**备注：**

**docker服务启动： docker-compose up -d**

**docker 服务停止： docker-compose down**

**查看nginx服务报错信息： docker logs patpat-nginx   依次类推**

8、项目部署：

前提条件：**需要在本机上事先安装好git 客户端工具，并且有拉取远程仓库项目的权限**

进入patpat-php容器:  docker exec -it  patpat-php /bin/bash




