---
title: 快速配置ca证书远程访问docker
tags: [ca证书, docker]
categories: 技术博客
date: 2023-11-04 1:51:00
---

# 快速配置ca证书远程访问docker

## 一键创建证书脚本

#### **docker-ca-shell.sh**

```shell
#!/bin/sh
ip=你的IP
password=你的密码
dir=/root/docker/cert # 证书生成位置
validity_period=10    # 证书有效期10年

# 将此shell脚本在安装docker的机器上执行，作用是生成docker远程连接加密证书
if [ ! -d "$dir" ]; then
  echo ""
  echo "$dir , not dir , will create"
  echo ""
  mkdir -p $dir
else
  echo ""
  echo "$dir , dir exist , will delete and create"
  echo ""
  rm -rf $dir
  mkdir -p $dir
fi

cd $dir || exit
# 创建根证书RSA私钥
openssl genrsa -aes256 -passout pass:"$password" -out ca-key.pem 4096
# 创建CA证书
openssl req -new -x509 -days $validity_period -key ca-key.pem -passin pass:"$password" -sha256 -out ca.pem -subj "/C=NL/ST=./L=./O=./CN=$ip"
# 创建服务端私钥
openssl genrsa -out server-key.pem 4096
# 创建服务端签名请求证书文件
openssl req -subj "/CN=$ip" -sha256 -new -key server-key.pem -out server.csr

echo subjectAltName = IP:$ip,IP:0.0.0.0 >>extfile.cnf

echo extendedKeyUsage = serverAuth >>extfile.cnf
# 创建签名生效的服务端证书文件
openssl x509 -req -days $validity_period -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem -passin "pass:$password" -CAcreateserial -out server-cert.pem -extfile extfile.cnf
# 创建客户端私钥
openssl genrsa -out key.pem 4096
# 创建客户端签名请求证书文件
openssl req -subj '/CN=client' -new -key key.pem -out client.csr

echo extendedKeyUsage = clientAuth >>extfile.cnf

echo extendedKeyUsage = clientAuth >extfile-client.cnf
# 创建签名生效的客户端证书文件
openssl x509 -req -days $validity_period -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem -passin "pass:$password" -CAcreateserial -out cert.pem -extfile extfile-client.cnf
# 删除多余文件
rm -f -v client.csr server.csr extfile.cnf extfile-client.cnf

chmod -v 0400 ca-key.pem key.pem server-key.pem

chmod -v 0444 ca.pem server-cert.pem cert.pem
```

#### 给予权限

运行前请给脚本文件`777`权限

```shell
chmod 777 docker-ca-shell.sh
```

#### 编辑docker.service配置文件

```
vim /usr/lib/systemd/system/docker.service
```

找到`ExecStart` = 开头的一行代码，将其替换为如下内容：

```shell
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd --tlsverify --tlscacert=/证书地址/ca.pem --tlscert=/证书地址/server-cert.pem --tlskey=/证书地址/server-key.pem -H tcp://0.0.0.0:2375 -H unix:///var/run/docker.sock
```

#### 刷新Docker

```shell
systemctl daemon-reload && systemctl restart docker
```

## 测试连接方法

#### 服务器本机测试(先`CD`进入证书文件夹)：

```shell
docker --tlsverify --tlscacert=ca.pem --tlscert=cert.pem --tlskey=key.pem -H tcp://你的ip:2375 version
```

如果能看到下面格式的内容，即为成功：

![服务器本机测试成功图片](https://s2.loli.net/2023/11/04/5xRfEAiU7yL3zn1.png)

#### 个人终端测试(先`CD`进入证书文件夹)：

```shell
curl https://你的ip:2375/info --cert cert.pem --key key.pem --cacert ca.pem
```

> 测试前要先将服务器的密钥复制到个人终端上

如果返回一段json格式的数据，即为成功：

![个人终端测试成功图片](https://s2.loli.net/2023/11/04/pGKnP4hlbzEmvBL.png)

## 参考资料

[1]: https://blog.csdn.net/qq_60750453/article/details/128730618	"Linux开启Docker远程访问并设置安全访问(证书密钥)，附一份小白一键设置脚本哦！"

