# Frp

## 使用frp做内网穿透需要哪些准备？

一台本地可以连接网络的电脑

一个有公网IP的云服务器(VPS)

## VPS相关

- 因为frp的原理是利用服务端（所准备的VPS）进行转发，因而VPS的速度直接决定了之后连接的质量，请根据自己的需要选择相应主机配置。

## 服务端设置

SSH连接到VPS之后运行如下命令查看处理器架构，根据架构下载不同版本的frp

```bash
arch
```

查看结果，如果是“X86_64“即可选择”amd64”

![image-20230203180723157](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302031807229.png)

我们只需要关注如下几个文件

- frps
- frps.ini
- frpc
- frpc.ini

前两个文件（s结尾代表server）分别是服务端程序和服务端配置文件，后两个文件（c结尾代表client）分别是客户端程序和客户端配置文件。

因为我们正在配置服务端，可以删除客户端的两个文件

```bash
rm frpc
rm frpc.ini
```

然后修改frps.ini文件

```bash
[common]
bind_port = 7000
dashboard_port = 7500	# 仪表盘，端口
token = 12345678
dashboard_user = admin
dashboard_pwd = admin
vhost_http_port = 80	# 反向代理 HTTP 主机
vhost_https_port = 443	# 反向代理 HTTP 主机
```

- “bind_port”表示用于客户端和服务端连接的端口，这个端口号我们之后在配置客户端的时候要用到。
- “dashboard_port”是服务端仪表板的端口，若使用7500端口，在配置完成服务启动后可以通过浏览器访问 x.x.x.x:7500 （其中x.x.x.x为VPS的IP）查看frp服务运行信息。
- “token”是用于客户端和服务端连接的口令，请自行设置并记录，稍后会用到。
- “dashboard_user”和“dashboard_pwd”表示打开仪表板页面登录的用户名和密码，自行设置即可。
- “vhost_http_port”和“vhost_https_port”用于反向代理HTTP主机时使用。

之后我们就可以运行frps的服务端了

```bash
./frps -c frps.ini
```

![image-20230203182121374](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302031821426.png)

查看防火墙状态

```java
firewall-cmd --state
```

查看防火墙开放的端口

```bash
firewall-cmd --zone=public --list-ports
```

![image-20230203193614680](https://cdn.jsdelivr.net/gh/RonnieLee24/PicGo_Pictures@master/imgs/DB/202302031936720.png)

查看监听的端口

```bash
netstat -lnpt
```

检查端口被哪个进程占用

```bash
netstat -lnpt |grep 5672
```

下载客户端

只需保留frpc.exe和frpc.ini即可

https://www.freesion.com/article/77951079713/

## VPN

```bash
rpm -ivh
```

配置 pptp

```bash
cd /etc/pptpd.conf
cd /etc/ppp/options.pptpd
```

设置使用 pptp 的用户名和密码

```bash
vim /etc/ppp/chap-secrets
# 例如：kuro pptpd 123456*
```

设置最大传输单元MTU

```bash
```



修改内核设置，使其支持转发

```bash
vim /etc/sysctl.conf
```

```bash
  1 vm.swappiness = 0
  2 kernel.sysrq = 1
  3
  4 net.ipv4.neigh.default.gc_stale_time = 120
  5
  6 # see details in https://help.aliyun.com/knowledge_detail/39428.html
  7 net.ipv4.conf.all.rp_filter = 0
  8 net.ipv4.conf.default.rp_filter = 0
  9 net.ipv4.conf.default.arp_announce = 2
 10 net.ipv4.conf.lo.arp_announce = 2
 11 net.ipv4.conf.all.arp_announce = 2
 12
 13 # see details in https://help.aliyun.com/knowledge_detail/41334.html
 14 net.ipv4.tcp_max_tw_buckets = 5000
 15 net.ipv4.tcp_syncookies = 1
 16 net.ipv4.tcp_max_syn_backlog = 1024
 17 net.ipv4.tcp_synack_retries = 2
 18
 19 # 新增的配置
 20 net.ipv4.ip_forward = 1
~
~
```

添加防火墙规则

```bash
iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -j MASQUERADE
```

添加NAT转发规则

```bash
# *为云服务器的公网IP地址，命令如下：
iptables -t nat -A POSTROUTING -s 192.168.0.0/255.255.255.0 -j SNAT --to-source 8.129.52.105
```

保存设置命令：

```bash
service iptables save
```

## 配置PPTP服务

重启PPTP服务，命令：

```bash
systemctl restart pptpd
```

重启iptables，命令：

```bash
systemctl start iptables
```

设置pptpd和iptables自启动，命令：

```bash
chkconfig --add pptpd
chkconfig pptpd on
chkconfig --add iptables
chkconfig iptables on
```

至此PPTP VPN服务端已安装结束。下面开始配置PPTP客户端：