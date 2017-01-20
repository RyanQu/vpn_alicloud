# vpn_alicloud
use alicloud to build a vpn server in China

##Brief
Besides the fantastic part of studying abroad, there are really suck moments just as I found some apps like Netease Mucis and Xiami Music can't be used off the mainland China.

When I'm in UESTC, I tried to use VPN cross the Great Wall to use Google, now I need to cross the Great Wall back to China, LOL.

##Start
Connect to my alicloud server by ssh and `su root`:

```
➜  ~ ssh username@ip_address

Last login: Thu Jan 19 11:55:13 2017 from 100.35.109.189

Welcome to aliyun Elastic Compute Service!

[username@iZ280zdkms7Z ~]$ su root

[root@iZ280zdkms7Z ~]#
```

Install `ppp` & `pptp`:

```
[root@iZ280zdkms7Z ~]# yum install ppp
[root@iZ280zdkms7Z ~]# yum install pptp
```

Config `pptp`:

```
[root@iZ280zdkms7Z ~]# vim /etc/pptpd.conf
```
```
localip 192.168.0.1
remoteip 192.168.0.234-238, 192.168.0.245
```

```
[root@iZ280zdkms7Z ~]# vim /etc/ppp/options.pptpd
```
```
ms-dns 114.114.114.114
ms-dns 8.8.8.8
```
```
[root@iZ280zdkms7Z ~]# vim /etc/ppp/chap-secrets
```
```
# Secrets for authentication using CHAP
# client server secret IP addresses
username pptpd password *
```
```
[root@iZ280zdkms7Z etc]# vim /etc/sysctl.conf
```
```
...
net.ipv4.ip_forward = 1
#net.ipv4.tcp_syncookies = 1
...
```
```
[root@iZ280zdkms7Z etc]# sysctl -p
```
```
[root@iZ280zdkms7Z etc]# iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -o eth1 -jMASQUERADE
```
```
[root@iZ280zdkms7Z ~]# systemctl start pptpd.service
[root@iZ280zdkms7Z ~]# systemctl enable pptpd.service
Created symlink from /etc/systemd/system/multi-user.target.wants/pptpd.service to /usr/lib/systemd/system/pptpd.service.
```

