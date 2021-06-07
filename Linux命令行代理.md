# Linux 命令行代理

## 下载v2ray

[下载地址](https://github.com/v2fly/v2ray-core/releases/tag/v4.31.0)

```shell
wget https://github.com/v2fly/v2ray-core/releases/download/v4.31.0/v2ray-linux-64.zip
```

解压

```shell
unzip v2ray-linux-64.zip
```

## 修改 `config.json` 配置文件

```shell
vim config.json
```

我的配置如下：

```json
{
  "log": {
    "access": "",
    "error": "",
    "loglevel": "warning"
  },
  "inbounds": [
    {
      "tag": "socks",
      "port": 1080,
      "listen": "127.0.0.1",
      "protocol": "socks",
      "sniffing": {
        "enabled": true,
        "destOverride": [
          "http",
          "tls"
        ]
      },
      "settings": {
        "auth": "noauth",
        "udp": true,
        "allowTransparent": false
      }
    }
  ],
  "outbounds": [
    {
      "tag": "proxy",
      "protocol": "vmess",
      "settings": {
        "vnext": [
          {
            "address": "xxx.xxx.xxx.xx",
            "port": 2020,
            "users": [
              {
                "id": "ed4690d9-ff09-4374-8aab-98a5305a1555",
                "alterId": 8,
                "email": "t@t.tt",
                "security": "auto"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "tcp"
      },
      "mux": {
        "enabled": false,
        "concurrency": -1
      }
    },
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {}
    },
    {
      "tag": "block",
      "protocol": "blackhole",
      "settings": {
        "response": {
          "type": "http"
        }
      }
    }
  ],
  "routing": {
    "domainStrategy": "IPIfNonMatch",
    "rules": [
      {
        "type": "field",
        "inboundTag": [
          "api"
        ],
        "outboundTag": "api"
      },
      {
        "type": "field",
        "outboundTag": "proxy",
        "domain": [
          "geosite:google"
        ]
      },
      {
        "type": "field",
        "outboundTag": "direct",
        "domain": [
          "domain:example-example.com",
          "domain:example-example2.com"
        ]
      },
      {
        "type": "field",
        "outboundTag": "block",
        "domain": [
          "geosite:category-ads-all"
        ]
      }
    ]
  }
}
```

## 后台启动v2ray

```shell
nohup ./v2ray &
```

可以通过当前目录下的 `nohup.out` 文件来查看输出信息

```shell
cat nohup.out
```

## 开启终端代理

```shell
export all_proxy=socks5://127.0.0.1:1080
```

查看ip

```shell
curl cip.cc
```

## 关闭终端代理

```shell
unset all_proxy
```

查看ip

```shell
curl cip.cc
```



