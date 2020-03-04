---
title: Centos7安装Shadowsocks
date: 2020-03-04 14:10:02
tags:
  - Shadowsocks
---

### 安装pip
```bash
curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
python get-pip.py
```
### 安装Shadowsocks
```bash
pip install --upgrade pip
pip install shadowsocks
```
安装完成后，创建配置文件/etc/shadowsocks.json，内容如下
```bash
{
"server":"0.0.0.0",
"local_address": "127.0.0.1",
"local_port":1080,
"port_password":{
	"8381": "D77b73E578",
	"8382": "53AFf96aEf",
	"8383": "6E18a11eA2",
	"8384": "OTU0OWQ2Nz"
},
"timeout":300,
"method":"aes-256-cfb",
"fast_open": false
}
```
### 配置shadowsocks服务启动
新建
```bash
vi /etc/systemd/system/shadowsocks.service
```
编辑
```bash
[Unit]
Description=Shadowsocks

[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/ssserver -c /etc/shadowsocks.json

[Install]
WantedBy=multi-user.target
```
启动 shadowsocks 服务
```bash
systemctl enable shadowsocks
systemctl start shadowsocks
```

查看启动状态
```bash
systemctl status shadowsocks
```

### 防火墙开放端口
```bash
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --reload
```
如果防火墙FirewallD is not running
```
systemctl start firewalld #开启防火墙
systemctl status firewalld #查看状态
```
