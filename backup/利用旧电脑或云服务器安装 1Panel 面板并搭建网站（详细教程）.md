> 想让吃灰的旧电脑重获新生，或是利用手上的云服务器搭建属于你的网站？本教程将带你从零开始，用一台旧电脑或云服务器，安装 Linux 系统，部署 1Panel 面板，进而一键搭建 WordPress 网站，开启你的站长之路！

## 🧰 一、准备工作

### ✅ 硬件环境（二选一）：

- **旧电脑一台**：建议配置为双核CPU、2GB内存、20GB硬盘以上，能联网即可。
- **云服务器一台**：推荐 Ubuntu 20.04 / 22.04 或 CentOS 7，需支持 SSH。

### 🔧 工具准备：

- SSH 工具：Xshell、PuTTY 或 Mac/Linux 终端
- Rufus（制作启动盘）：https://rufus.ie
- Ubuntu Server 镜像：https://ubuntu.com/download/server

---

## 💿 二、旧电脑安装 Linux 系统（跳过可用于云服务器）

1. 下载 Ubuntu Server 镜像，使用 Rufus 写入 U 盘；
2. 启动旧电脑，进入 BIOS 选择 U 盘启动；
3. 进入 Ubuntu 安装界面，按提示操作，安装时选择安装 `OpenSSH Server`；
4. 安装完后输入 `ip a` 获取设备局域网 IP，用于远程连接。

---

## 🔌 三、远程连接服务器

在本地终端中运行如下命令连接你的服务器：

```bash
ssh yourusername@服务器IP
```

首次登录需输入密码，之后就进入 shell 操作环境。

---

## 🛠️ 四、一键安装 1Panel 面板

执行官方安装脚本：

```bash
curl -sSL https://1panel.cn/install.sh | sudo bash
```

等待约 5~10 分钟，完成后会显示访问地址（例如 http://your-ip:8800）及初始账号密码。忘记可用此命令查看：

```bash
sudo 1panel show-admin
```

---

## 🌐 五、访问后台并部署网站

浏览器访问 http://服务器IP:8800，使用默认账号密码登录。登录后建议修改密码。

点击左侧菜单「应用商店」，找到 WordPress 应用，点击「安装」，设置站点名称、数据库密码等，点击确认。系统将自动部署网站服务。

几分钟后访问 http://你的服务器IP，若出现 WordPress 安装界面即表示成功。

---

## 🌍 六、绑定域名与 HTTPS 配置（可选）

将域名解析至你的公网 IP，例如：

| 类型 | 主机 | 值（服务器IP） |
|------|------|----------------|
| A    | @    | 1.2.3.4        |
| A    | www  | 1.2.3.4        |

然后在 1Panel 的「网站」菜单中，添加新站点，绑定域名并启用 HTTPS，即可自动申请 SSL 证书。

---

## 📦 七、其他一键部署应用推荐

- **Cloudreve**：自建不限速网盘
- **Nextcloud**：团队文件协作
- **Typecho / Halo**：极简博客系统
- **phpMyAdmin**：数据库可视化管理
- **Nginx Proxy Manager**：反代工具

---

## 🔄 八、局域网公网访问方案（无公网IP用户）

使用以下方案将局域网主机暴露到公网：

- [frp](https://github.com/fatedier/frp)：推荐，稳定灵活；
- [Ngrok](https://ngrok.com)：快速配置；
- [ZeroTier](https://www.zerotier.com/)：构建虚拟局域网；
- 花生壳、向日葵：国产图形化方案。

---

## 🔐 九、安全设置建议

- 修改 SSH 端口，例如改为 2222：
  ```bash
  sudo nano /etc/ssh/sshd_config
  ```
- 安装 fail2ban 防暴力破解；
- 使用 UFW 设置防火墙：
  ```bash
  sudo ufw allow 8800
  sudo ufw allow 80
  sudo ufw allow 443
  sudo ufw enable
  ```
- 定期在 1Panel 设置备份计划。

---

## ✅ 十、总结

通过本教程你学会了如何使用旧电脑或云服务器搭建 1Panel 面板并部署网站。无论你是想搭建博客、图床、网盘、资料库、内网穿透服务，1Panel 都能轻松胜任。结合 Docker 与现代开源工具，它将让你的服务器焕发第二春。赶快动手试一试吧！

---