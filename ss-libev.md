# centos 7 yum install ss-libev
## 1. add ss-libev repo
- wget -O /etc/yum.repos.d/librehat-shadowsocks-libev.repo https://copr.fedorainfracloud.org/coprs/librehat/shadowsocks/repo/epel-7/librehat-shadowsocks-epel-7.repo
````
cat /etc/yum.repos.d/librehat-shadowsocks-libev.repo
[librehat-shadowsocks]
name=Copr repo for shadowsocks owned by librehat
baseurl=https://copr-be.cloud.fedoraproject.org/results/librehat/shadowsocks/epel-7-$basearch/
type=rpm-md
skip_if_unavailable=True
gpgcheck=1
gpgkey=https://copr-be.cloud.fedoraproject.org/results/librehat/shadowsocks/pubkey.gpg
repo_gpgcheck=0
enabled=1
enabled_metadata=1

````
## 2.  yum install ss-libev
- yum -y install shadowsocks-libev
## 3. systemctl control shadowsocks-libev
- systemctl enable shadowsocks-libev-server.service
````
cat /usr/lib/systemd/system/shadowsocks-libev.service
[Unit]
Description=Shadowsocks-libev Default Server Service
Documentation=man:shadowsocks-libev(8)
After=network.target network-online.target

[Service]
Type=simple
EnvironmentFile=/etc/sysconfig/shadowsocks-libev
User=nobody
Group=nobody
LimitNOFILE=32768
ExecStart=/usr/bin/ss-server -v -c "$CONFFILE" $DAEMON_ARGS
CapabilityBoundingSet=CAP_NET_BIND_SERVICE

[Install]
WantedBy=multi-user.target

````
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
## 4. config shadowsocks-libev
- edit  /etc/shadowsocks-libev/config.json as below
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
