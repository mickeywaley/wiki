Proxmox（PVE）连接UPS实现自动关机(基于APCUPSD）

环境 Proxmox 6.4-13

安装apcupsd

#PVE 6基于debian10，ssh root登录PVE或者使用web shell

apt install apcupsd -y

编辑apcupsd配置文件/etc/apcupsd/apcupsd.conf

有以下几个地方要改

#UPSNAME改成

UPSNAME XXX（随便起个名字）

#设置为30表示，切换到ups电源30S后开始关闭虚拟机，然后关闭宿主机，0为不启用

TIMEOUT 30

#每隔15s输出ups状态到日志中

STATTIME 15

#开启日志，日志文件为/var/log/apcupsd.status

LOGSTATS on

一些参数说明

#线缆类型为usb

UPSCABLE usb

#usb接口，自动识别

UPSTYPE usb

DEVICE

#断电6s后才识别为正在使用电池，防止短时间断电导致错误+1

ONBATTERYDELAY 6

#电池电量低于5%时关闭主机

BATTERYLEVEL 5

#预计电量剩余时间小于3分钟时关闭主机

MINUTES 3

关于apcupsd的命令

#启动apcupsd

systemctl start apcupsd

#查看apcupsd进程状态

systemctl status apcupsd

#开机启动

systemctl enable apcupsd

#重启apcupsd，更改配置文件后使用

systemctl restart apcupsd

#查看ups状态

apcaccess

现在可以断电试试看了

参考资料

https://wiki.debian.org/apcupsd
