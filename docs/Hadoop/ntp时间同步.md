# ntp服务器

## bios时间同步

让系统时间与bios时间同步

` hwclock -w`

## 安装ntp

```bash
#检查包
yum list | grep ntp
# 安装 ntp,ntpdate
yum -y install ntp
#开启自启
systemctl enable ntpd
#验证
systemctl is-enabled ntpd
```

##  编辑配置文件

`vi \etc\ntp.conf`

```bash
# 设置ntp服务器使用ip区段
restrict 192.168.87.2 mask 255.255.255.0 nomodify notrap
#注释掉原来的sever, 新增4个
server 0.asia.pool.ntp.org iburst
server 1.asia.pool.ntp.org iburst
server 2.asia.pool.ntp.org iburst
server 3.asia.pool.ntp.org iburst

#指明互联网和局域网中作为NTP服务器的IP
server 192.168.87.101

#当服务器与公用的时间服务器失去联系时以本地时间为客户端提供时间服务
server 127.127.1.0
fudge 127.127.1.0
stratum 10
```

## 查看

```bash
#启动
systemctl start ntpd

#重启后10分钟左右才会时间同步
systemctl restart ntpd

#查看进程
ps -ef | grep ntp
#执行同步
hwclock -w
#查看ntp状态
ntpstat
```


# ntp客户端

设置与服务器相同



## 编辑配置文件

清空/etc/ntp.conf

```bash
restrict 127.0.0.1
restrict ::1
restrict 192.168.87.2 mask 255.255.255.0 nomodify notrap
server 192.168.87.101
Fudge 192.168.87.101 stratum 10
```

# ntpdate

ntpdate会发生时间跃迁,会影响应用运行,而**ntp是缓慢改变时间**

```bash
ntpdate master
```

## 设置定时器

` crontab -e`

```bash
10 23 * * * (/usr/sbin/ntpdate -u 192.168.10.200 && /sbin/hwclock -w) & >> /var/log/ntpdate.log
```

## 错误

`the NTP socket is in use, exiting`

```bash
#关闭ntpd
service ntpd stop
```

