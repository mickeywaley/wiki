* 前言
以前也碰到很多次这种问题了，大多数情况是不管他然后等维护的时候重启解决。这次来扒一下原因和解决方案。

* 问题描述
Proxmox集群部分节点出现问号 功能却全部正常：

<img src="https://raw.githubusercontent.com/mickeywaley/wiki/refs/heads/main/PC%E5%A3%81%E7%BA%B8/1.jpg"  />   测试代码


这次出现这个问题是在添加完NFS存储之后，因此估计是NFS那块出现了问题。 查看PVE cluster信息全部正常，都是online，这就很尴尬了…

* 问题原因
决定pve上状态的服务是pvestat daemon，也就是pvestatd，因此查看它的运行状况，果然有问题！是NFS这边挂载的问题，虽然整个服务是online的，但是NFS卡住了。

root@JS-3003:~# service pvestatd status
● pvestatd.service - PVE Status Daemon
 Loaded: loaded (/lib/systemd/system/pvestatd.service; enabled; vendor preset:
 Active: active (running) since Sun 2020-06-28 03:40:20 CST; 6 days ago
 Process: 2132 ExecStart=/usr/bin/pvestatd start (code=exited, status=0/SUCCESS
 Main PID: 2200 (pvestatd)
 Tasks: 1 (limit: 7372)
 Memory: 105.8M
    CPU: 43min 18.643s
 CGroup: /system.slice/pvestatd.service
         └─2200 pvestatd
Jul 02 23:16:47 JS-3003 pvestatd[2200]: storage 'NFS-2001' is not online
Jul 02 23:16:57 JS-3003 pvestatd[2200]: storage 'NFS-2001' is not online
Jul 02 23:17:07 JS-3003 pvestatd[2200]: storage 'NFS-2001' is not online
Jul 02 23:17:17 JS-3003 pvestatd[2200]: storage 'NFS-2001' is not online
Jul 02 23:17:27 JS-3003 pvestatd[2200]: storage 'NFS-2001' is not online
Jul 02 23:17:37 JS-3003 pvestatd[2200]: storage 'NFS-2001' is not online
Jul 02 23:17:47 JS-3003 pvestatd[2200]: storage 'NFS-2001' is not online
Jul 02 23:17:57 JS-3003 pvestatd[2200]: storage 'NFS-2001' is not online
* 解决方案
重启pvestatd即可：

root@JS-3003:~# service pvestatd restart
root@JS-3003:~# service pvestatd status
 ● pvestatd.service - PVE Status Daemon
 Loaded: loaded (/lib/systemd/system/pvestatd.service; enabled; vendor preset:
 Active: active (running) since Sat 2020-07-04 16:50:30 CST; 2s ago
 Process: 13782 ExecStop=/usr/bin/pvestatd stop (code=exited, status=0/SUCCESS)
 Process: 13796 ExecStart=/usr/bin/pvestatd start (code=exited, status=0/SUCCES
 Main PID: 13813 (pvestatd)
 Tasks: 1 (limit: 7372)
 Memory: 67.5M
    CPU: 540ms
 CGroup: /system.slice/pvestatd.service
         └─13813 pvestatd
Jul 04 16:50:29 JS-3003 systemd[1]: Starting PVE Status Daemon...
Jul 04 16:50:30 JS-3003 pvestatd[13813]: starting server
Jul 04 16:50:30 JS-3003 systemd[1]: Started PVE Status Daemon.
* 简单总结
直接在pve的shell执行

service pvestatd restart

和

service pvestatd status

刷新网页，搞定
