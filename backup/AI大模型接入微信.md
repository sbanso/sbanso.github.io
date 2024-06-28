# 盘点AI大模型接入微信的方法，喂饭级教程

如何将ChatGPT等AI接入微信，这应该是全网最全最细的视频教程了。<br/>目前将AI大模型接入微信主要有三个主流方向， 分别是企业微信，公众号， 还有个人微信。<br/>这里面企业微信分为企业机器人与企业客服两个子流派。公众号又分为订阅号与服务号两个子流派。<br/>这样加起来总共是五个流派，我用一个表格列举了他们的优缺点。

| 方案 | 优点 | 缺点 | 成本 |
| --- | --- | --- | --- |
| 企业微信机器人 | 微信官方API，安全稳定 | 必须先加入对应的企业，才能使用。<br/>配置繁琐<br/>无法参与群聊 | 公网IP的云服务器 |
| 企业微信客服 | 微信官方API，安全稳定<br/>不用加入企业，点开即用 | 未认证的企业只能服务100个人<br/>配置繁琐<br/>无法参与群聊 | 公网IP的云服务器 |
| 公众号订阅号 | 微信官方API，安全稳定 | 公众号有超时限制，有字数限制<br/>无法参与群聊 | 公网IP的云服务器<br/> |
| 公众号服务号 | 微信官方API，安全稳定 | 无法参与群聊<br/>成本较高 | 公网IP的云服务器<br/>服务号认证费用<br/>营业执照 |
| 个人微信 | 可以参与群聊<br/>可以加好友<br/>部署成本低<br/>普通微信号的一切功能 | 给账号带来风险，建议用小号<br/> | 任意服务器/电脑即可 |


<a name="izKex"></a>
# 企业微信
项目主页：[https://github.com/zhayujie/chatgpt-on-wechat](https://github.com/zhayujie/chatgpt-on-wechat)<br/>项目文档：[https://docs.link-ai.tech/cow/quick-start](https://docs.link-ai.tech/cow/quick-start)
<a name="XKilL"></a>
## 注册企业微信
[https://work.weixin.qq.com/wework_admin/register_wx?from=loginpage](https://work.weixin.qq.com/wework_admin/register_wx?from=loginpage)
<a name="sJJ5s"></a>
## 创建企业机器人
应用管理-->应用-->创建应用 ，应用名称跟描述随便填,可见范围选整个公司<br/>这样就创建好了一个企业机器人。<br/>
![](/doc/images/240627/1.png)
<a name="wDctl"></a>
## 配置可信域名
如果未认证的企业可以使用华为云的域名，认证企业则必须使用企业自己的域名
<a name="r1EeZ"></a>
###  登陆华为云
   [https://activity.huaweicloud.com/](https://activity.huaweicloud.com/) ，完成实名认证
<a name="ANkZe"></a>
###  创建云函数
搜索云函数，点击立即使用。右上角创建函数，选择HTTP函数，函数名称随便填。 <br/>![](/doc/images/240627/2.png)
<a name="Xa6OX"></a>
###  创建触发器
切换到设置 触发器-创建触发器<br/>
![](/doc/images/240627/3.png)<br/>
安全认证选None ，分组随意创建一个<br/>
![](/doc/images/240627/4.png)

<a name="t6WM9"></a>
###  获得URL
![](/doc/images/240627/5.png)<br/>

下一步 打开企业微信 -->进入刚才创建的机器人-->设置可信域名<br>

![](/doc/images/240627/6.png)<br/>

可信域名填3.4步骤中的URL，类似以下格式（注意去掉http:// 与 最后的/<br>

> d1c0cceabb04d8e8413c2b615790846.apig.cn-north-1.huaweicloudapis.com

<a name="Ve9vy"></a>
### 获得校验码
点击申请校验域名->下载文件, 将文件里面的校验复制下来  大约长这样： **1tg27Cpi9hYTjFFq**<br/>![](/doc/images/240627/7.png)
<a name="eslpb"></a>
### 绑定可信域名
回到华为云，  代码->index.js 修改第九行为文件里的校验码（见3.5小节获得的校验码），点击部署<br/>![](/doc/images/240627/8.png)<br/>部署完成 回到企业微信->点击确定  可信域名即完成绑定

<a name="g71Rc"></a>
## Gemini对接企业微信

我们使用这个项目  chatgpt-on-wechat<br/>[https://github.com/zhayujie/chatgpt-on-wechat](https://github.com/zhayujie/chatgpt-on-wechat)

<a name="Zu9H7"></a>
### 申请Google Gemini
申请一个API key<br/>[https://makersuite.google.com/app/apikey](https://makersuite.google.com/app/apikey)
> 需要一个Google账号，以及可以访问Google的网络环境<br>
> 如果没有对应网络环境可以选择其他国内大模型

<a name="jrv9R"></a>
### 安装docker
需要有一个公网IP的云服务器，安装docker<br/>安装docker可以参考这个视频<br/>[https://www.bilibili.com/video/BV1fS411A71Y/](https://www.bilibili.com/video/BV1fS411A71Y/)<br/>也可以参考这个<br/>[https://github.com/tech-shrimp/docker_installer](https://github.com/tech-shrimp/docker_installer)

> 使用Gemini, 服务器需要访问Google的网络环境<br>
> 如果没有对应网络环境可以选择其他国内大模型

<a name="HEvBq"></a>
### 下载docker compose
执行以下命令下载 docker-compose.yml：
```shell
wget https://open-1317903499.cos.ap-guangzhou.myqcloud.com/docker-compose.yml

```

修改docker compose文件，主要配置从24行以下开始 
```yaml
version: '2.0'
services:
  chatgpt-on-wechat:
    image: zhayujie/chatgpt-on-wechat
    container_name: chatgpt-on-wechat
    security_opt:
      - seccomp:unconfined
    environment:
      OPEN_AI_API_KEY: 'YOUR KEY'
      PROXY: ''
      SINGLE_CHAT_PREFIX: '[""]'
      SINGLE_CHAT_REPLY_PREFIX: '"[bot] "'
      GROUP_CHAT_PREFIX: '["@bot"]'
      GROUP_NAME_WHITE_LIST: '["测试群", "测试群2"]'
      IMAGE_CREATE_PREFIX: '["画", "看", "找"]'
      CONVERSATION_MAX_TOKENS: 1000
      SPEECH_RECOGNITION: 'False'
      CHARACTER_DESC: '你是基于大语言模型的AI智能助手，旨在回答并解决人们的任何问题，并且可以使用多种语言与人交流。'
      EXPIRES_IN_SECONDS: 3600
      USE_GLOBAL_PLUGIN_CONFIG: 'True'
      USE_LINKAI: 'False'
      LINKAI_API_KEY: ''
      LINKAI_APP_CODE: ''
      ## 配置从以下开始
      ## 模型名称 注意此文件只保留一个模型名称，其余删掉
      MODEL: 'gemini'
      # 4.1小节申请的 gemini api key
      GEMINI_API_KEY: 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
      CHANNEL_TYPE: "wechatcom_app"                
      # 企业微信->我的企业->企业ID       
      WECHATCOM_CORP_ID: "xxxxxxxxxxxxxxxxxx"
      # 企业微信->应用管理->应用->Secret                      
      WECHATCOMAPP_SECRET: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
      # 企业微信->应用管理->应用->AgentId             
      WECHATCOMAPP_AGENT_ID: "xxxxxx"
      # 企业微信->应用管理->应用->接收消息->设置API接收->Token          
      WECHATCOMAPP_TOKEN: "xxxxxxxxxxxxxxxxxxxxxxxxxx"
      # 企业微信->应用管理->应用->接收消息->设置API接收->EncodingAESKey  
      WECHATCOMAPP_AES_KEY: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
      WECHATCOMAPP_PORT": 9898       
    ports:
      - 9898:9898

```
<a name="mNJaX"></a>
### 启动docker
修改完成后将docker启动起来
```shell
# 启动docker
sudo docker compose up -d
# 查看日志
sudo docker logs -f chatgpt-on-wechat
# 停止docker 如果修改配置文件必须先停止再启动
sudo docker compose down
```

<a name="VR7KE"></a>
### 企业微信设置
回到企业微信，填写好URL  ,按如下格式

> http://服务器IP:9898/wxcomapp

注意Token与EncodingAESKey与docker配置一致。

![](/doc/images/240627/9.png)


点击企业可信IP，填入服务器的公网IP

![](/doc/images/240627/10.png)


### 微信加入企业
我的企业->微信插件->邀请关注 ，使用微信扫码即加入企业，然后就可以开始应用机器人


# 微信公众号
<a name="uaJ5A"></a>
## 注册公众号
[https://mp.weixin.qq.com/](https://mp.weixin.qq.com/)
<a name="h2EPP"></a>
### 获取环境变量
公众号->设置与开发->基本配置<br/>![](/doc/images/240627/11.png)
<a name="flcmE"></a>
## Docker Compose
执行以下命令下载 docker-compose.yml：
```shell
wget https://open-1317903499.cos.ap-guangzhou.myqcloud.com/docker-compose.yml
```
修改文件
```yaml
version: '2.0'
services:
  chatgpt-on-wechat:
    image: zhayujie/chatgpt-on-wechat
    container_name: chatgpt-on-wechat
    security_opt:
      - seccomp:unconfined
    environment:
      OPEN_AI_API_KEY: 'YOUR API KEY'
      PROXY: ''
      SINGLE_CHAT_PREFIX: '[""]'
      SINGLE_CHAT_REPLY_PREFIX: '"[bot] "'
      GROUP_CHAT_PREFIX: '["@bot"]'
      GROUP_NAME_WHITE_LIST: '["ChatGPT测试群", "ChatGPT测试群2"]'
      IMAGE_CREATE_PREFIX: '["画", "看", "找"]'
      CONVERSATION_MAX_TOKENS: 1000
      SPEECH_RECOGNITION: 'False'
      CHARACTER_DESC: '你是基于大语言模型的AI智能助手，旨在回答并解决人们的任何问题，并且可以使用多种语言与人交流。'
      EXPIRES_IN_SECONDS: 3600
      USE_GLOBAL_PLUGIN_CONFIG: 'True'
      USE_LINKAI: 'False'
      LINKAI_API_KEY: ''
      LINKAI_APP_CODE: ''
      ## 配置从以下开始
      ## 模型名称 注意此文件只保留一个模型名称，其余删掉
      MODEL: 'gemini'
      # 申请的 gemini api key
      GEMINI_API_KEY: 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
      CHANNEL_TYPE: 'wechatmp'
      # 公众号->设置与开发->基本配置->开发者ID
      WECHATMP_APP_ID: 'xxxxxxxxxxxxxx'
      # 公众号->设置与开发->基本配置->开发者密码
      WECHATMP_APP_SECRET: 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
      # 公众号->设置与开发->基本配置->服务器配置->EncodingAESKey(可以随机生成)
      WECHATMP_AES_KEY: 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
      # 公众号->设置与开发->基本配置->服务器配置->Token(可以任意填写)
      WECHATMP_TOKEN: 'xxxxxxx'
      WECHATMP_PORT: 80
    ports:
      - 80:80
```

启动docker
```shell
# 启动docker
sudo docker compose up -d
# 查看日志
sudo docker logs -f chatgpt-on-wechat
# 停止docker 如果修改配置文件必须先停止再启动
sudo docker compose down
```

<a name="hAC3J"></a>
## 公众号完成配置
公众号->设置与开发->基本配置->服务器配置->修改配置<br/>注意修改URL格式  http://服务器公网IP:80/wx<br/>Token & EncodingAESKey 与 Docker Compose文件保持一致。<br/>选择明文模式<br/>![](/doc/images/240627/12.png)<br/>最后提交即可。<br/>如果提交不成功，请检查服务器防火墙，检查80端口是否暴露在公网。


# 个人微信

## Docker
执行以下命令下载 docker-compose.yml：
```shell
wget https://open-1317903499.cos.ap-guangzhou.myqcloud.com/docker-compose.yml
```
修改文件
```yaml
version: '2.0'
services:
  chatgpt-on-wechat:
    image: zhayujie/chatgpt-on-wechat
    container_name: chatgpt-on-wechat
    security_opt:
      - seccomp:unconfined
    environment:
      OPEN_AI_API_KEY: 'YOUR API KEY'
      PROXY: ''
      SINGLE_CHAT_PREFIX: '[""]'
      SINGLE_CHAT_REPLY_PREFIX: '"[bot] "'
      GROUP_CHAT_PREFIX: '["@bot"]'
      GROUP_NAME_WHITE_LIST: '["ChatGPT测试群", "ChatGPT测试群2"]'
      IMAGE_CREATE_PREFIX: '["画", "看", "找"]'
      CONVERSATION_MAX_TOKENS: 1000
      SPEECH_RECOGNITION: 'False'
      CHARACTER_DESC: '你是基于大语言模型的AI智能助手，旨在回答并解决人们的任何问题，并且可以使用多种语言与人交流。'
      EXPIRES_IN_SECONDS: 3600
      USE_GLOBAL_PLUGIN_CONFIG: 'True'
      USE_LINKAI: 'False'
      LINKAI_API_KEY: ''
      LINKAI_APP_CODE: ''
      ## 配置从以下开始
      MODEL: 'gemini'
      # 申请的 gemini api key
      GEMINI_API_KEY: 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
    ports:
      - 9898:9898
```

启动docker
```shell
# 启动docker
sudo docker compose up -d
# 查看日志 #扫码登录
sudo docker logs -f chatgpt-on-wechat
```

<a name="Bcuk1"></a>
## 扫码登录
查看日志以后，屏幕上会出现一个登录二维码，扫这个二维码即可登录微信。<br>
![](/doc/images/240627/13.png)<br/>
登录完成后，其他人与这个微信即可进行AI微信对话。<br>




