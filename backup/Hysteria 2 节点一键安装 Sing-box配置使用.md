Hysteria 2 是一个比较新的协议，类似于v2ray，但可能比v2ray节点速度更快，支持全平台使用。
视频教程：▶ https://youtu.be/EbB7BVe81Fc

# 搭建步骤：
## 一、准备一个VPS服务器
VPS服务器注册：https://my.racknerd.com/aff.php?aff=11886

## 二、下载搭建工具
FinalShell下载（Windows版）：[点击下载>>](https://kjfx.lanzoui.com/iqm6Uosbzha)
备用下载（Windows MacOS Linux）：[点击下载>>](http://www.hostbuf.com/t/988.html)

## 三、更新 VPS 系统
```
apt update -y
apt install curl pseudo -y
```

## 四、Hysteria 2 一键搭建代码
`wget -N --no-check-certificate https://raw.githubusercontent.com/flame1ce/hysteria2-install/main/hysteria2-install-main/hy2/hysteria.sh && bash hysteria.sh`

## 五、Hysteria 服务相关命令
```
systemctl start hysteria-server.service    # 启动 hysteria 服务
systemctl enable hysteria-server.service   # 设置 hysteria 服务 开机自启动
systemctl restart hysteria-server.service  # 重启 hysteria 服务
systemctl stop hysteria-server.service     # 停止 hysteria 服务
systemctl status hysteria-server.service   # 查看 hysteria 服务 状态
```

## 六、Hysteria客户端配置
Windows 建议使用 V2rayN [点击下载>>](https://github.com/2dust/v2rayN/releases/latest)
IOS/MAC 建议使用 sing-box 或 Shadowrocket
需要使用美区 AppleID 登录 App Store 下载，如果没有美区ID，可以点击免费注册一个 [美区AppleID>>](https://github.com/kjfx/AppleID)
安卓手机 建议使用 sing-box
可以在 Google Play 商店下载，或者[官方下载>](https://github.com/SagerNet/sing-box/releases/latest)>
请下载.apk文件，建议下载v8a.apk

## 七、sing-box 配置文件
```
{
  "dns": {
    "servers": [
      {
        "tag": "cf",
        "address": "https://1.1.1.1/dns-query"
      },
      {
        "tag": "local",
        "address": "223.5.5.5",
        "detour": "direct"
      },
      {
        "tag": "block",
        "address": "rcode://success"
      }
    ],
    "rules": [
      {
        "geosite": "category-ads-all",
        "server": "block",
        "disable_cache": true
      },
      {
        "outbound": "any",
        "server": "local"
      },
      {
        "geosite": "cn",
        "server": "local"
      }
    ],
    "strategy": "ipv4_only"
  },
  "inbounds": [
    {
      "type": "tun",
      "inet4_address": "172.19.0.1/30",
      "auto_route": true,
      "strict_route": false,
      "sniff": true
    }
  ],
  "outbounds": [
    {
      "type": "hysteria2",
      "tag": "proxy",
      "server": "IP",             // VPS IP
      "server_port": 443,         // 端口
      "up_mbps": 30,              //上传速率，按本地速度填写
      "down_mbps": 100,           //下载速率，按本地速度填写
      "password": "88888888",     //hysteria2 服务密码
      "tls": {
        "enabled": true,
        "server_name": "www.bing.com",
        "insecure": true              
      }
    },
    {
      "type": "direct",
      "tag": "direct"
    },
    {
      "type": "block",
      "tag": "block"
    },
    {
      "type": "dns",
      "tag": "dns-out"
    }
  ],
  "route": {
    "rules": [
      {
        "protocol": "dns",
        "outbound": "dns-out"
      },
      {
        "geosite": "cn",
        "geoip": [
          "private",
          "cn"
        ],
        "outbound": "direct"
      },
      {
        "geosite": "category-ads-all",
        "outbound": "block"
      }
    ],
    "auto_detect_interface": true
  }
}
```

## 八、常见问题
节点如果不能正常使用，请放行端口，请将以下命令复制到搭建工具，再点回车
`iptables -I INPUT -p tcp --dport 443 -j ACCEPT`
请将 443 改为你节点的端口

# 如果不能使用，请重新搭建一次
