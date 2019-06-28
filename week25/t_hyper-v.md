## ARTS

### Tip

## Windows 10 下(开启)关闭 Hyper-V 服务

首先是关闭的情况：

![图片](https://uploader.shimo.im/f/jvZZysX7ZWcgnp9u.png!thumbnail)

在控制面板——程序和功能——开启或关闭Windows功能，把那个Hyper－V去掉。


* 以管理员身份运行命令提示符
*  执行命令 bcdedit /set hypervisorlaunchtype off 
* 重启，运行vm即可

如果想要恢复hyper启动， **bcdedit / set hypervisorlaunchtype auto**


