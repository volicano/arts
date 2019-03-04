## ARTS

### Tip

## win查看端口占用情况

1、win+R打开输入cmd回车，打开cmd窗口，如图

![图片](https://uploader.shimo.im/f/ui3yviCMNP4q4u2l.png!thumbnail)

2，netstat -ano列出所有端口的情况，找到被占用的端口（如果知道是哪个端口被占用了，可以省略这一步），如图

![图片](https://uploader.shimo.im/f/jZB4uxYuE8sjEU6m.png!thumbnail)

3、输入命令netstat -aon|findstr "3306"   找对应的PID

![图片](https://uploader.shimo.im/f/U1iWJGQiQswN9xHK.png!thumbnail)

4、输入命令tasklist|findstr "9904" 查找具体的占用进程

![图片](https://uploader.shimo.im/f/RYTWcCmMWoAqJmGw.png!thumbnail)

6、结束进程，使用：taskkill /f /t /im vpnkit.exe

这样，占用的3306端口就被释放了。


