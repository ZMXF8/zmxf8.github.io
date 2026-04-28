> [!TIP]
> 🏠 用旧电脑或树莓派部署 Home Assistant 并接入米家设备（离线本地化教程）

> 本教程将一步步教你如何用本地设备（树莓派、旧电脑、NUC 等）搭建 Home Assistant 智能家居平台，并成功接入小米设备，实现真正的局域网离线智能控制！

---

## 🧰 一、准备条件

### 📦 硬件准备（任选其一）：

- 树莓派 3B/4 + 16GB TF 卡 + USB 电源
- 或 旧笔记本/台式机（建议2G RAM以上）
- 或 x86 设备（如 Intel NUC、小主机）

### 🌐 网络要求：

- 上述设备必须与小米设备在**同一局域网**
- 建议固定 IP 或使用局域网 DHCP 保持 IP 不变

### 🛠️ 工具准备：

- Balena Etcher（烧录系统）
- 可用显示器和键盘（或 SSH）

---

## 🚀 二、安装 Home Assistant

### 方法 A：推荐方式（安装 Home Assistant OS）

> 适用于树莓派、NUC、旧电脑（全盘写入）

1. 下载对应平台的 [Home Assistant 镜像](https://www.home-assistant.io/installation/)
2. 使用 Balena Etcher 烧录到 SD 卡或 U 盘
3. 插入设备后开机，等待 10 分钟
4. 在浏览器访问：`http://homeassistant.local:8123`（或用 IP 访问）

### 方法 B：Docker 安装（适用于 Ubuntu 本地电脑）

```bash
sudo apt update
sudo apt install -y docker.io
sudo docker run -d --name homeassistant \
  --privileged \
  --restart=unless-stopped \
  -v /home/user/homeassistant:/config \
  -e TZ=Asia/Shanghai \
  -p 8123:8123 \
  ghcr.io/home-assistant/home-assistant:stable
```

启动后访问 `http://你的IP:8123`

---

## 🔑 三、首次启动与配置

1. 打开浏览器访问 `http://你的IP:8123`
2. 创建管理员账号
3. 设置时区、语言、位置（建议：中国/上海）
4. 如果局域网中已有米家设备，HA 会自动发现部分设备（如支持局域网协议的插座/灯）

---

## 🔌 四、接入米家（小米）设备

### 方法一：接入 **支持局域网控制** 的米家设备

常见设备包括：

- 米家智能插座（Zigbee/WiFi）
- 米家灯泡
- 小米网关 v3（支持局域网开发）
- 空调伴侣2

#### 步骤如下：

1. 打开米家 App → 进入设备 → 右上角「...」→ 查看设备信息
2. 如果显示「局域网通信协议」或「Token」，记录下来
3. 在 Home Assistant 配置文件中添加：

```yaml
# configuration.yaml
xiaomi_miio:
  gateways:
    - host: 192.168.1.100
      token: your_device_token
```

4. 重启 Home Assistant，设备将出现在集成中

---

### 方法二：使用插件「Xiaomi Miot Auto」

支持更多设备（如扫地机、空调伴侣、电饭煲等）

1. 安装 HACS（社区插件商店）：
   - 进入 HA → 添加 HACS 插件仓库：https://hacs.xyz
2. 在 HACS 中搜索并安装 `Xiaomi Miot Auto`
3. 安装完成后，Home Assistant 左侧栏点击「设置」→「设备与服务」→ 添加 `Xiaomi Miot Auto`
4. 登录小米账号，导入设备

⚠️ 该方式需要**一次性云端登录小米账号**，之后可离线控制大部分设备

---

## 💡 五、自动化示例

例如：每天早上 7 点自动打开客厅灯

```yaml
automation:
  - alias: '早上开灯'
    trigger:
      - platform: time
        at: "07:00:00"
    action:
      - service: switch.turn_on
        entity_id: switch.mi_light_1
```

---

## 📱 六、安装 App 控制

推荐使用 Home Assistant 官方 App：

- 安卓：可从 Play 商店或官网下载 APK
- iOS：App Store 搜索 “Home Assistant”

可实现远程通知、本地控制、小组件操作等

---

## 🔐 七、安全建议

- 设置强密码，避免局域网攻击
- 开启 HTTPS（使用 Let’s Encrypt + DuckDNS）
- 启用 MFA（双重身份验证）

---

## ✅ 八、总结

通过本教程，你学会了如何在本地设备上部署 Home Assistant，并接入小米设备构建智能家居中心。与云平台相比，Home Assistant 更加安全、高效、可扩展，无需依赖外网和广告。

欢迎尝试更多组件如：

- Zigbee2MQTT（搭配 CC2652P）
- ESPHome（自制设备）
- Node-RED（可视化自动化流程）

🌟 打造真正属于你自己的智能家！

---