## ARTS

### Tip

## 获取连接到服务器的IP地址命令

>netstat -an | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -n

**PS：建议大家保留这个命令；在Xshell终端里面，可以随时观察连接到服务器的IP情况。**


