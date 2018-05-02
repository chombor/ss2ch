# TryHard: CentOS | Debian

> Небольшая ремарка — SS на кентосе7 у меня так и не поднялись *(не пытайте кентось6 - systemd в зависимостях)* если найдется смекалистый анон который **СМОЖЕТ** — маякни братуха.

## Debian

1. подняли vps'ку
2. зарутились в ssh
3. выкачиваем сс из стречовых репок:

`printf "deb http://deb.debian.org/debian stretch-backports main" > /etc/apt/sources.list.d/stretch-backports.list && apt update -y && apt -t stretch-backports install shadowsocks-libev -y`

> vim /etc/shadowsocks-libev/config.json

        {
            "server":["[::0]","0.0.0.0"],
            "server_port":443,
            "local_address":"127.0.0.1",
            "local_port":1080,
            "password":" $PASSWORD$ ",
            "timeout":30,
            "method":"chacha20-ietf-poly1305",
            "fast_open":true,
            "reuse_port": true,
            "plugin":"obfs-server",
            "plugin_opts":"obfs=tls",
            "mode": "tcp_and_udp"
        }

> качаем тулы клоним и запускаем скриптулю

`apt-get install --no-install-recommends build-essential autoconf libtool libssl-dev libpcre3-dev libev-dev asciidoc xmlto automake git -y && git clone https://github.com/shadowsocks/simple-obfs.git && cd simple-obfs && git submodule update --init --recursive && ./autogen.sh && ./configure && make && make install`

> подымаем сервис

`setcap cap_net_bind_service>ep /usr/local/bin/obfs-server && systemctl enable shadowsocks-libev.service && systemctl restart shadowsocks-libev && systemctl status shadowsocks-libev`

> серверная часть готова!

## CenOS 7

1. подняли vps'ку
2. зарутились в ssh
3. вигетим репку эпеля, инсталим эфель и сам ss-libev:

`yum install epel-releas git wget -y && wget --force-directories -O /etc/yum.repos.d/librehat-shadowsocks-epel-7.repo https://copr.fedorainfracloud.org/coprs/librehat/shadowsocks/repo/epel-7/librehat-shadowsocks-epel-7.repo && yum update -y && yum install shadowsocks-libev -y`

> vi /etc/shadowsocks-libev/config.json

        {
            "server":["[::0]","0.0.0.0"],
            "server_port":443,
            "local_address":"127.0.0.1",
            "local_port":1080,
            "password":" $PASSWORD$ ",
            "timeout":30,
            "method":"chacha20-ietf-poly1305",
            "fast_open":true,
            "reuse_port": true,
            "plugin":"obfs-server",
            "plugin_opts":"obfs=tls",
            "mode": "tcp_and_udp"
        }

> качаем тулы клоним и запускаем скриптулю

`yum install gcc autoconf libtool automake make zlib-devel openssl-devel asciidoc xmlto gettext pcre-devel c-ares-devel libev-devel libsodium-devel mbedtls-devel -y && git clone https://github.com/shadowsocks/simple-obfs.git && cd simple-obfs && git submodule update --init --recursive && ./autogen.sh && ./configure && make && make install
`

> подымаем сервис

`setcap cap_net_bind_service>ep /usr/local/bin/obfs-server && systemctl enable shadowsocks-libev.service && systemctl restart shadowsocks-libev && systemctl status shadowsocks-libev`

> серверная часть готова!