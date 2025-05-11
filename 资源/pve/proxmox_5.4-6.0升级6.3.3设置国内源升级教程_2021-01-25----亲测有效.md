Proxmox 5.4、 6.0升级6.3-3设置国内源升级教程（2021-01-25—–亲测有效）


<img src="https://raw.githubusercontent.com/mickeywaley/wiki/refs/heads/main/%E8%B5%84%E6%BA%90/pve/Proxmox%205.4-6.0%E5%8D%87%E7%BA%A76.3-3-01.png"  />

<img src="https://raw.githubusercontent.com/mickeywaley/wiki/refs/heads/main/%E8%B5%84%E6%BA%90/pve/Proxmox%205.4-6.0%E5%8D%87%E7%BA%A76.3-3-02.png"  />

<img src="https://raw.githubusercontent.com/mickeywaley/wiki/refs/heads/main/%E8%B5%84%E6%BA%90/pve/Proxmox%205.4-6.0%E5%8D%87%E7%BA%A76.3-3-03.png"  />




1、网上看了很多教程 折腾了好几天天，全部升级失败，终于在2021-1-25下午找到了

ps：拷贝过来的

最后一步执行之后 会有很多提示 都是确认信息。

务必用ssh连接升级，不要在PVE下用shell

删除企业源-复制到终端回车

 rm -rf /etc/apt/sources.list.d/pve-enterprise.list
 
下载秘钥-复制到终端回车

 wget http://mirrors.ustc.edu.cn/proxmox/debian/proxmox-ve-release-6.x.gpg -O /etc/apt/trusted.gpg.d/proxmox-ve-release-6.x.gpg
 
添加国内源–复制到终端回车：

 echo "deb http://mirrors.ustc.edu.cn/proxmox/debian/pve buster pve-no-subscription" >/etc/apt/sources.list.d/pve-install-repo.list
 
这里要注意下，很多教程会设置成错误 deb （忽略这里）http://mirrors.ustc.edu.cn/proxmox/debian/pve strech pve-no-subscription

造成无法升级为pve6.2strech–debian 9–pve 5.xbuster–debian 10–pve 6.x

建议同时使用国内 debian 源，修改 /etc/apt/sources.list 文件，

添加如下内容-复制到终端回车：

 echo "deb http://mirrors.ustc.edu.cn/debian buster main contribdeb http://mirrors.ustc.edu.cn/debian buster-updates main contribdeb 
 
 http://mirrors.ustc.edu.cn/debian-security buster/updates main contrib" > /etc/apt/sources.listpve_ceph

设置国内源原配置文件：

 /etc/apt/sources.list.d/ceph.listecho "deb http://mirrors.ustc.edu.cn/proxmox/debian/ceph-nautilus buster main" > /etc/apt/sources.list.d/ceph.list

最后执行

 apt update
 
 apt dist-upgrade
 
会有询问是否同意 Y/N .输入Y 回车即可

这就升级成功了 生产环境也可以直接升级
