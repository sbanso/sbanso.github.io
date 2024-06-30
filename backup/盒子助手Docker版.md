
🤔 这是什么？
该项目可以让你使用电脑、NAS等一切能运行docker的设备变成盒子的ADB安装助手。让你的盒子用起来更加得心应手。


💡 特色功能
💻 支持一键修改安卓原生电视盒子/TV的NTP服务器地址

💻 支持SSH连接 且容器内ADB服务均已准备就绪,无需额外安装

🔑 支持安装装机必备app 尤其是文件管理器和三方市场、图标等

🌏 支持一键批量安装主机上指定目录的全部apk

🐋 支持Docker compose和 docker cli一键部署

📕 支持为国行Sony电视安装时下流行的流媒体应用

❓ 兼容`ARMv7/ARM64/x86_64 双平台设备

❓ 其他功能和特点会持续迭代

MacOS(Apple芯片/Intel芯片)✅

Windows 10/11 ✅

Linux发行版 ✅

NAS系统（群晖、威联通等）✅

软路由iStoreOS/OpenWrt ✅

❗️更新日志
https://github.com/wukongdaily/tvhelper-docker/releases

## 🚀 快速上手

### 1. 安装Docker和Docker compose
Docker安装教程：https://docs.docker.com/engine/install/

Docker compose安装教程：https://docs.docker.com/compose/install/

个人普通电脑安装教程：https://docs.docker.com/get-docker/

docker镜像主页：https://hub.docker.com/r/wukongdaily/box

### 2. 下载image

`docker pull wukongdaily/box:latest`

### 3. 容器系统默认账号密码或环境变量
容器内运行的就是alpine linux系统

ssh用户名和密码分别是：root和password

推荐ssh端口映射到主机端口为2299

80端口映射到主机端口为2288

调用形式举例

ssh root@宿主机ip地址 -p 2299

SSH常见错误举例和新手指南详见

https://github.com/wukongdaily/HowToUseSSH

容器内的环境变量

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/android-sdk/platform-tools

### 4. 运行
Windows电脑使用-CMD写法,注意不是powershell 且注意💡续行符^后不能有空格。数据目录默认映射到 【我的文档】

```
docker run -d ^
--restart unless-stopped ^
--name tvhelper ^
-p 2299:22 ^
-p 2288:80 ^
-v "%USERPROFILE%\Documents\tvhelper_data:/tvhelper/shells/data" ^
-e PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/android-sdk/platform-tools ^
wukongdaily/box:latest
```

Linux 使用下列命令,数据目录默认映射到linux的/tmp/upload/下

```
docker run -d \
  --restart unless-stopped \
  --name tvhelper \
  -p 2299:22 \
  -p 2288:80 \
  -v "/tmp/upload/tvhelper_data:/tvhelper/shells/data" \
  -e PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/android-sdk/platform-tools \
  wukongdaily/box:latest
```

macOS苹果电脑写法,数据目录默认映射到mac电脑文稿目录下

```
docker run -d \
  --restart unless-stopped \
  --name tvhelper \
  -p 2299:22 \
  -p 2288:80 \
  -v "$HOME/Documents/tvhelper_data:/tvhelper/shells/data" \
  -e PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/android-sdk/platform-tools \
  wukongdaily/box:latest
```

UNRAID 写法,注意容器内的data目录默认映射到 /mnt/user/appdata/，你可以适当修改成别的空间的路径。

```
docker run -d \
  --name='tvhelper' \
  --net='bridge' \
  -e HOST_OS="Unraid" \
  -e HOST_HOSTNAME="unraid" \
  -e HOST_CONTAINERNAME="tvhelper" \
  -e 'PATH'='/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/android-sdk/platform-tools' \
  -l net.unraid.docker.managed=dockerman \
  -p '2299:22/tcp' \
  -p '2288:80/tcp' \
  -v '/mnt/user/appdata/':'/tvhelper/shells/data':'rw' 'wukongdaily/box'
```

### 5. 如何导入本地镜像tar
离线包：https://pan.baidu.com/share/init?surl=lWsaAtuAcwaO_9DtJo0hnA&pwd=1111

Windows 举例

`docker load < "%USERPROFILE%\Documents\tvhelper-amd64.tar"`

Linux/OpenWrt 举例

`docker load < /mnt/sata1.3-1/myboxarm.tar`

辅助视频教程⬇️
[在线教学视频 长视频](https://youtu.be/xAk-3TxeXxQ)