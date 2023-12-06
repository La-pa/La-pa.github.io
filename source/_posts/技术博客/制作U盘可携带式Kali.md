---
title: 制作U盘可携带式Kali
tags: [kali, U盘]
categories: 信息安全
date: 2023-11-07 21:59:00
---

# 制作U盘可携带式Kali

## 1.准备材料

### 1.1 硬件

1.32GB以上，USB 3.0及以上U盘一块；
2.电脑一台；

### 1.2 软件

1.[balenaEtcher](https://www.balena.io/etcher/)
2.[分区助手](https://www.disktool.cn/)
3.VMWare Workstation（已安装Kali，用于配置持久化分区）
4.[Kali USB版镜像](https://www.kali.org/get-kali/#kali-live)

## 烧录

## 启动

## 持久化



```
fdisk -l

```

```
mkfs.ext4 -L persistence /dev/sdb5

```

```
mkdir -p /mnt/usb
mount /dev/sdb5 /mnt/usb
echo "/ union" > /mnt/usb/persistence.conf
umount /mnt/usb

```



## 下载镜像

![image-20231107145917896](https://s2.loli.net/2023/11/07/faCTRNw8G7iXmlq.png)

## 参考链接

[1]: https://zhuanlan.zhihu.com/p/129348579	"将Kali Linux部署在U盘上，并实现U盘启动（汇总纠错）"
[2]: https://blog.csdn.net/qq_59032809/article/details/127189546	"如何将kali系统写入U盘，并在U盘中启动"
[3]: https://blog.csdn.net/BlackBtuWhite/article/details/129645845	"制作U盘可携带式KaliLinux"
[4]: https://blog.csdn.net/qq_25426559/article/details/130190594	"【KALI】自制U盘版KALI（即插即用具有可持久化功能）"

