# x-ui

xray panel with multi-protocol multi-user support

# Features

- System Status Monitoring
- Support multi-user multi-protocol, web page visual operation
- Supported Protocols：vmess、vless、trojan、shadowsocks、dokodemo-door、socks、http
- Support for configuring more transport configurations
- Traffic Statistics, Limit Traffic, Limit Expiration Date
- Customizable xray configuration templates
- Support https access panel (self-provided domain name + ssl certificate)
- One-click SSL certificate application and automatic renewal are supported
- For more advanced configuration items, see the panel

# Install & Upgrade

```
bash <(curl -Ls https://raw.githubusercontent.com/philve/x-ui/master/install.sh)
```

## Manual install & upgrade

1. First download the latest compressed package from https://github.com/vaxilu/x-ui/releases, generally choose the `amd64` architecture
2. Then upload the compressed package to the `/root/` directory of the server, and use the `root` user to log in to the server

> If your server cpu architecture is not `amd64`, replace `amd64` in the command with another architecture

```
cd /root/
rm x-ui/ /usr/local/x-ui/ /usr/bin/x-ui -rf
tar zxvf x-ui-linux-amd64.tar.gz
chmod +x x-ui/x-ui x-ui/bin/xray-linux-* x-ui/x-ui.sh
cp x-ui/x-ui.sh /usr/bin/x-ui
cp -f x-ui/x-ui.service /etc/systemd/system/
mv x-ui/ /usr/local/
systemctl daemon-reload
systemctl enable x-ui
systemctl restart x-ui
```

## Install using docker

> This docker tutorial and docker image are provided by [Chasing66](https://github.com/Chasing66)

1. install docker

```shell
curl -fsSL https://get.docker.com | sh
```

2. install x-ui

```shell
mkdir x-ui && cd x-ui
docker run -itd --network=host \
    -v $PWD/db/:/etc/x-ui/ \
    -v $PWD/cert/:/root/cert/ \
    --name x-ui --restart=unless-stopped \
    enwaiax/x-ui:latest
```

> Build your own image

```shell
docker build -t x-ui .
```

## SSL certificate application

> This function and tutorial are provided by [FranzKafkaYu](https://github.com/FranzKafkaYu)

The script has a built-in SSL certificate application function. To use this script to apply for a certificate, the following conditions must be met:

- Cloudflare registered email
- Cloudflare Global API Key
- The domain name should be resolved to the current server through cloudflare

How to get the Cloudflare Global API Key:
    ![](media/bda84fbc2ede834deaba1c173a932223.png)
    ![](media/d13ffd6a73f938d1037d0708e31433bf.png)

When using, just enter `domain name`, `email`, `API KEY`, as shown in the diagram below：
        ![](media/2022-04-04_141259.png)

Notes:

- The script uses the DNS API for certificate requests
- By default, Let'sEncrypt is used as the CA party
- Certificate installation directory is /root/cert directory
- The certificates applied for by this script are all domain name certificates

## Telegram robot use (under development, temporarily unavailable)

> This function and tutorial are provided by [FranzKafkaYu](https://github.com/FranzKafkaYu)

X-UI supports daily traffic notification, panel login reminder and other functions through Telegram robot. To use Telegram robot, you need to apply by yourself
For specific application tutorials, please refer to [blog link](https://coderfan.net/how-to-use-telegram-bot-to-alarm-you-when-someone-login-into-your-vps.html)

Instructions for use: Set robot-related parameters in the background of the panel, including

- Tg Robot Token
- Tg Robot ChatId
- Tg Robot Cycle Run Time，Use crontab syntax 

Reference syntax：
- 30 * * * * * //Notification will be made at the 30s of each point
- @hourly      //Hourly notification
- @daily       //Daily notification (00:00 at the morning)
- @every 8h    //Notification every 8 hours  

TG notification content：
- Node traffic usage
- Panel login reminder
- Node expiration reminder
- Traffic warning reminder  

More features are planned...

## Recommendation system

- CentOS 7+
- Ubuntu 16+
- Debian 8+

# Common problem

## Migrating from v2-ui

First install the latest version of x-ui on the server where v2-ui is installed，Then use the following command to migrate, `all inbound account data` of the native v2-ui will be migrated to x-ui，`Panel settings and username passwords are not migrated`

> After the migration is successful, please `close v2-ui` and `restart x-ui`, otherwise the inbounds of v2-ui and the inbounds of x-ui will cause a `port conflict`

```
x-ui v2-ui
```

## issue is closed

Various little white problems see high blood pressure

## Stargazers over time

[![Stargazers over time](https://starchart.cc/vaxilu/x-ui.svg)](https://starchart.cc/vaxilu/x-ui)
