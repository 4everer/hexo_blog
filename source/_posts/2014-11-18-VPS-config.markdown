---
layout: post
title:  "VPS Configuration"
tags: [VPS, linux]
categories: resource
---

# vps configuration
(vpn, fw, ssh port, user)

## ssh port change and no root ssh login

`nano /etc/ssh/sshd_config`

find port 22, then add `port <port you want>`

save and restart ssh service with `service ssh restart`

after testing new port you can comment out port 22

___

## vpn ufw
refer [link](http://www.zhihu.com/question/20113381)

connect use **putty** via ssh in windows

``` bash
sudo apt-get update
sudo apt-get dist-upgrade
```

1. 安装pptpd（VPN服务器）和ufw（防火墙
`sudo apt-get install pptpd ufw`

2. 修改ufw规则
```
sudo ufw allow 22
sudo ufw allow 1723
sudo ufw enable
```

这里请注意，如果你有其他服务运行，例如有网站也运行在这个VPS中，记得同样开启相应的端口，例如网站使用的端口80或者8080，记得加入一下两
```
sudo ufw allow 80
sudo ufw allow 8080
sudo ufw disable && sudo ufw enable
```

3. 编辑pptpd选项
`sudo nano /etc/ppp/pptpd-options`

输入此行命令后会打开一个文档编辑，找到以下三行并注解with“#”
```
refuse-pap
refuse-chap
refuse-mschap
```
之后在最后的地方添加如下信息
```
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```

4. 编辑IP信息以及客户端IP地址范围
`sudo nano /etc/pptpd.conf`

在最后添加：
```
localip 128.199.171.10  #your vps ip
remoteip 10.99.99.100-199
```

5. 添加VPN用户登录信息，就是你登陆VPN的时候使用的用户名密码
`sudo nano /etc/ppp/chap-secrets`

会打开一个编辑文本，然后按照以下模式添加一个用户账号
`[Username] [Service] [Password] [Allowed IP Address]`

例如,添加一个 用户名为 fuckGfw，密码为fuckGfwDaddy的vpn账号，可以创建多个，一行一个用户。
`fuckGfw pptpd fuckGfwDaddy *`

同样，输入完成后按ctrl+x键（退出编辑器）会询问是否保存，点击Y，回车。

6. 重启pptpd
`sudo /etc/init.d/pptpd restart`

7. 编辑系统设置
`sudo nano /etc/sysctl.conf`
找到`#net.ipv4.ip_forward=1`
去掉前面的#，改完后为`net.ipv4.ip_forward=1`

同样，输入完成后按ctrl+x键（退出编辑器）会询问是否保存，点击Y，回车。

8. 重新加载系统设置
`sudo sysctl -p`

9. 修改ufw防火墙设置
`sudo nano /etc/default/ufw`

将
`DEFAULT_FORWARD_POLICY = "DROP"`
改为
`DEFAULT_FORWARD_POLICY = "ACCEPT"`

同样，输入完成后按ctrl+x键（退出编辑器）会询问是否保存，点击Y，回车。

10. 继续修改ufw防火墙设置
`sudo nano /etc/ufw/before.rules`

会打开文本编辑，在文本最开始处复制添加如下内容，不要做任何修改，

``` bash
# NAT table rules
*nat

:POSTROUTING ACCEPT [0:0]
# Allow forward traffic to eth0
-A POSTROUTING -s 10.99.99.0/24 -o eth0 -j MASQUERADE

# Process the NAT table rules
COMMIT
```

11. 重启防火墙
`sudo ufw disable && sudo ufw enable`

恭喜你！！你的VPN服务器架设成功了！！你现在可以随便(fan)浪(qiang)了 =）。

___

**更新：这里可能有的朋友会看到一条Error信息：**
`>ERROR: problem running ufw-init`

如果出现这个情况，在你确保没输入错误的命令时，请输入一下命令：
`ufw --force enable`
