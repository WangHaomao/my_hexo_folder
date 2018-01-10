---
title: Ubuntu 与 Mac os 终端代理
date: 2018-01-07 15:41:44
tags: [Experence, Linux]
---

主要使用SS(shadowsocks)做代理，在Ubuntu和大多数Linux中可以安装SS-qt图形化界面使用，当然使用 Python 脚本的 SS也是比较方便的; 在Mac os中，SS也可以在互联网上下载。使用其他方式对系统代理后，以下终端代理方法仍适用

注：SS的安装此处不提及，请访问其他相关页面

## Ubuntu
在Ubuntu中，推荐使用 proxychains
安装比较简单，同时速度也不错，配置也相对简单，以下是安装使用过程：

proxychains 仓库地址： https://github.com/rofl0r/proxychains-ng
详细配置与安装上述网站中有提及，如下
#### 1、下载安装
```bash
$ git clone https://github.com/rofl0r/proxychains-ng.git
$ cd proxychains-ng
#以下命令需要GCC环境，仓库地址中有提及
#needs a working C compiler, preferably gcc
$ sudo make install 
$ sudo make install-config (installs proxychains.conf)

```
#### 2、完善配置文件
```bash
$ sudo vim /etc/proxychains.conf
#找到[ProxyList] 后，添加代理信息，SS使用socks5，如果用http代理可使用http
socks5  127.0.0.1 1080 
```
#### 3、测试与使用
可以使用wget 测试 google.com 页面
proxychains 的用法是在需要使用的命令前加 proxychains4 ，例：

```bash
$ proxychains4 wget google.com
$ sudo proxychains4 [your shell]
```
## Mac os


