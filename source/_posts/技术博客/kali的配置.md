---
title: kali的配置
tags: [kali]
categories: 信息安全
date: 2023-11-07 21:59:00
---

# kali的配置

## 用户配置

```
sudo -i
```



```
sudo su root
#进入root用户
sudo passwd root
#如果不知道密码请使用此命令修改
```



## 更换apt的镜像源

#### 编辑APT源

```
vim /etc/apt/sources.list
```

#### 添加如下APT源地址

```shell
#中科大
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
deb-src http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
#阿里云
deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
deb-src http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
```

#### 更新APT 

```shell
# 更新APT的资源列表
apt-get update
# 清除已安装软件的安装抱
apt-get clean
```

## 网络配置

#### 编辑配置文件

```
vim /etc/network/interfaces
```

**如果是虚拟机启动，需要将网络连接方式设置为桥接方式**



#### 修改内容

```
auto eth0 
iface eth0 inet static 
address 192.168.136.199（IP地址）
netmask 255.255.255.0 （子网掩码）
gateway 192.168.136.254（网关）
```

#### 设置DNS

```
vi /etc/resolv.conf
```

```
nameserver 8.8.8.8 
```

#### 重启网络生效

```shell
service networking restart
```

#### 测试成功

```
ping baidu.com
ifconfig
```

## SSH登入

```
vim /etc/ssh/sshd_config
```

```
#PermitRootLogin prohibit-password 取消注释并prohibit-password改为yes
 
#PubkeyAuthentication yes 取消注释
```

```
systemctl restart ssh
```

```
update-rc.d ssh enable
```



## kali汉化

```
dpkg-reconfigure locales
```

**选中en_US.UTF-8 UTF-8和zh_CN.UTF-8 UTF-8(注意:按下空格键选中,选好后按下TAB键退出编码格式选项,跳到OK选项)**

在终端键入`reboot`, 重启Kali



## 下载中文输入法

```
apt install fcitx -y
```

```
apt-get install fcitx-googlepinyin -y
```

**重启**

重启后可以看到右上角多了一个小键盘的图标，点击后选择“配置”。

在弹出的页面内可以看到多出了“Google拼音”汉语输入法。

如果没有，则只需要点击左下角的“+”，然后搜索“google”，然后添加即可。



## 配置vpn

[在 Linux 中使用 Clash](https://blog.iswiftai.com/posts/clash-linux/)

## 相关链接

[1]: https://blog.csdn.net/weixin_44971640/article/details/127387950	"Kali Linux基础配置 超详细图解"

[2]: https://blog.csdn.net/weixin_62808713/article/details/130373096	"Kali 安装中文输入法（超详细）"

[3]: https://blog.iswiftai.com/posts/clash-linux/	"在 Linux 中使用 Clash"

