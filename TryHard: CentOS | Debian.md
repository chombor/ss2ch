# TryHard: CentOS | Debian

> Небольшая ремарка — SS на центосе у меня так и не поднялись, если найдется смекалистый анон который СМОЖЕТ — маякни братуха.

## Debian

+ подняли vps'ку
+ зарутились в ssh
+ пастим в терминал следующее:

`printf "deb http://deb.debian.org/debian stretch-backports main" > /etc/apt/sources.list.d/stretch-backports.list && apt update -y && apt -t stretch-backports install shadowsocks-libev -y`

+ вимаемся в /etc/shadowsocks-libev/config.json

    `{`
        `"server":["[::0]","0.0.0.0"],`
        `"server_port":443,`
        `"local_address":"127.0.0.1",`
        `"local_port":1080,`
        `"password":" $PASSWORD$ ",`
        `"timeout":30,`
        `"method":"chacha20-ietf-poly1305",`
        `"fast_open":true,`
        `"reuse_port": true,`
        `"plugin":"obfs-server",`
        `"plugin_opts":"obfs=tls",`
        `"mode": "tcp_and_udp"`
    `}`

+ качаем тулы, клоним и запускаем скриптулю

`apt-get install --no-install-recommends build-essential autoconf libtool libssl-dev libpcre3-dev libev-dev asciidoc xmlto automake git -y && git clone https://github.com/shadowsocks/simple-obfs.git && cd simple-obfs && git submodule update --init --recursive && ./autogen.sh && ./configure && make && make install`

+ подымаем непосредсвенно сам сервис

`setcap cap_net_bind_service+ep /usr/local/bin/obfs-server && systemctl enable shadowsocks-libev.service && systemctl restart shadowsocks-libev && systemctl status shadowsocks-libev`

+ серверная часть готова!