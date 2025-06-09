  今天我们主要来说下如果通过 Cloudflare 的 worker 、Pages 搭建永久免费的VPN，真正做到无限流量！

![Image](https://github.com/user-attachments/assets/3e3be5fc-6844-4d62-9da3-e79d02e92d00)

1. 首先你得先注册个cloudflare账号[点击前往](https://dash.cloudflare.com/sign-up)
2. 接着你需要准备好开源软件：V2rayN，因为一会要用到[立即下载](https://github.com/2dust/v2rayN)
3. 在Cloudflare 上创建worker，并填写下面的代码，这个代码来自CM在【 [Github](https://github.com/cmliu/edgetunnel?tab=readme-ov-file) 】社区的开源项目，代码已经内置IP优选和代理功能，自带动态的UUID，可以大大减少手动配置过程，非常适合新手和特殊用户。
### 注意:创建的worker项目名称最好使用系统默认的，别自定义，以免被系统识别到特殊字符而被屏蔽。
## 获取Cloudflare 的 worker 开源代码
加密版 [点击获取](https://github.com/cmliu/edgetunnel/blob/main/_worker.js)
明文版 [点击获取](https://github.com/cmliu/edgetunnel/blob/main/%E6%98%8E%E6%96%87%E6%BA%90%E7%A0%81.js)
### 重要声明：仅限海外用户使用，大陆用户切勿使用（可能违法），别给站长找麻烦！！

4. 注册一个免费的域名，可以使用零度解说前几期介绍的注册方法，来获取你自己的免费域名[点击前往](https://www.freedidi.com/17434.html)

5. 如果想获得更多的优质节点，那么推荐使用IP优选网站或优选工具进行获取！[点击前往](https://www.freedidi.com/10143.html)
### 视频教程[点击前往](https://youtu.be/DYGu71OaqGI?si=rFiCoTsUYMiSBIPH)
详细教程[点击](https://www.freedidi.com/)搜索标题。
### 警告:仅限海外用户使用，大陆用户切勿使用（可能违法），别给站长找麻烦！！