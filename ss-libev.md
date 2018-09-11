# centos 7 yum install ss-libev
## add ss-libev repo
- wget -O /etc/yum.repos.d/librehat-shadowsocks-libev.repo https://copr.fedorainfracloud.org/coprs/librehat/shadowsocks/repo/epel-7/librehat-shadowsocks-epel-7.repo
## yum install ss-libev
- yum -y install shadowsocks-libev
## systemctl control shadowsocks-libev
- systemctl enable shadowsocks-libev-server.service
- systemctl start shadowsocks-libev-server.service
- systemctl status shadowsocks-libev-server.service
````
shadowsocks-libev.service - Shadowsocks-libev Default Server Service
   Loaded: loaded (/usr/lib/systemd/system/shadowsocks-libev.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2018-09-11 12:41:42 CST; 9h ago
     Docs: man:shadowsocks-libev(8)
 Main PID: 8266 (ss-server)
    Tasks: 1
   Memory: 2.2M
   CGroup: /system.slice/shadowsocks-libev.service
           └─8266 /usr/bin/ss-server -v -c /etc/shadowsocks-libev/config.json -u
````
- systemctl daemon-reload
## config shadowsocks-libev
- vim /etc/shadowsocks-libev/config.json
````
{
    "server":"REPLACE YOUR IP",
    "server_port":443,
    "local_port":1080,
    "password":"REPLACE YOUR PASS",
    "timeout":60,
    "method":"chacha20"
}
````
- how to bind ports below 1024 with non-root privilege
  - setcap cap_net_bind_service+ep /usr/bin/ss-server
