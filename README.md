# APK OTA 更新服务器

用于存放 Android App APK 安装包，并为 Android App 提供免费的 OTA 在线升级服务。

本项目使用 **GitHub Repository + GitHub Raw** 作为 APK 文件存储和下载服务器，无需额外购买服务器。

仓库地址：

[GitHub：NFC-BLE-OTA](https://github.com/lizhufeng2024-beep/NFC-BLE-OTA?utm_source=chatgpt.com)

---

## 📦 项目说明

本仓库用于保存 Android App 的正式 APK 安装包，并提供 OTA 在线升级所需的版本信息。

Android App 启动后可以通过网络请求 `version.json`：

```text
检查当前版本
        ↓
获取服务器最新版本
        ↓
比较 versionCode
        ↓
发现新版本
        ↓
提示用户更新
        ↓
下载 APK
        ↓
安装 APK
```

GitHub 负责：

* 保存 APK 安装包
* 保存 OTA 版本信息
* 提供 HTTPS 下载地址
* 提供免费的文件托管服务

---

# 📁 项目结构

当前仓库使用以下结构：

```text
/
├── README.md
├── version.json
└── apk/
    ├── app-1.1.73.apk
    ├── app-1.1.74.apk
    └── app-1.1.75.apk
```

其中：

| 文件 / 目录        | 作用         |
| -------------- | ---------- |
| `README.md`    | 项目说明       |
| `version.json` | OTA 最新版本信息 |
| `apk/`         | 保存 APK 安装包 |
| `apk/*.apk`    | 各个正式版本 APK |

---

# 🔄 OTA 版本检查

App 不需要每次都直接判断 APK 文件名，而是首先请求：

```text
version.json
```

当前版本信息地址：

```text
https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/version.json
```

App 获取到服务器信息后，与当前安装版本进行比较。

例如：

```text
当前版本：
versionName = 1.1.74
versionCode = 74

服务器版本：
versionName = 1.1.75
versionCode = 75
```

因为：

```text
75 > 74
```

所以 App 判断存在新版本，并提示用户升级。

---

# 📦 APK 发布

每次发布新版本时，将新的 Release APK 上传到：

```text
apk/
```

例如：

```text
apk/app-1.1.75.apk
```

建议 APK 文件名统一使用：

```text
应用名称-版本号.apk
```

例如：

```text
app-1.1.73.apk
app-1.1.74.apk
app-1.1.75.apk
```

---

# 🔗 APK 下载地址

当前 APK 使用 GitHub Raw 提供下载。

最新版本 APK：

```text
https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/apk/app-1.1.75.apk
```

GitHub Raw 下载地址格式：

```text
https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/apk/文件名.apk
```

Android App 可以直接通过 HTTPS 下载 APK。

---

# 🔗 version.json 地址

当前 OTA 版本检查地址：

```text
https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/version.json
```

Android App 启动时请求该地址即可获取最新版本信息。

---

# 🚀 完整 OTA 更新流程

```text
┌──────────────────┐
│     启动 App      │
└────────┬─────────┘
         ↓
┌──────────────────┐
│ 请求 version.json │
└────────┬─────────┘
         ↓
┌──────────────────┐
│ 获取服务器版本信息 │
└────────┬─────────┘
         ↓
┌──────────────────┐
│ 比较 versionCode  │
└────────┬─────────┘
         ↓
      有新版本？
      /       \
    否         是
    ↓          ↓
  结束     提示用户更新
               ↓
          下载 APK
               ↓
          APK 下载完成
               ↓
          调用系统安装
               ↓
          用户确认安装
               ↓
          更新完成
```

---

# 📝 版本管理

建议每个正式版本都保留对应的 APK，不要直接覆盖旧版本。

例如：

```text
apk/
├── app-1.1.73.apk
├── app-1.1.74.apk
├── app-1.1.75.apk
└── app-1.1.76.apk
```

这样可以：

* 保留历史版本
* 方便问题排查
* 方便回滚
* 方便测试指定版本
* 避免旧版本下载地址失效

---

# ⚙️ version.json 管理

例如当前版本为：

```json
{
  "versionName": "1.1.75",
  "versionCode": 75,
  "apkUrl": "https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/apk/app-1.1.75.apk"
}
```

其中：

```text
versionName
```

用于显示用户看到的版本号。

```text
versionCode
```

用于 Android OTA 版本比较。

```text
apkUrl
```

用于告诉 App 新版本 APK 的实际下载地址。

---

# ⚠️ 发布顺序

建议每次发布新版本严格按照以下顺序操作：

```text
① 打包新的 APK
       ↓
② 上传 APK 到 apk/
       ↓
③ 确认 APK 可以正常下载
       ↓
④ 修改 version.json
       ↓
⑤ 提交并推送 GitHub
       ↓
⑥ App 检查 OTA
```

**不要在 APK 还没有上传成功时，就先修改 `version.json`。**

否则 App 可能检测到新版本，但下载 APK 时出现 404。

---

# ⚠️ Android OTA 注意事项

## 1. APK 签名必须一致

OTA 升级时，新 APK 必须使用与当前安装版本相同的签名证书。

例如当前 App 使用正式版签名，那么以后所有正式 OTA 版本都必须继续使用相同的签名。

否则 Android 无法直接覆盖安装。

> 不要丢失正式版签名文件和密码。

---

## 2. versionCode 必须递增

Android 判断 APK 是否为更高版本，主要依赖：

```text
versionCode
```

例如：

```text
1.1.73 → versionCode 73
1.1.74 → versionCode 74
1.1.75 → versionCode 75
1.1.76 → versionCode 76
```

必须保证：

```text
新 versionCode > 当前 versionCode
```

推荐每发布一个正式版本就递增一次。

---

## 3. versionName 用于显示

`versionName` 主要用于用户看到的版本号。

例如：

```text
1.1.75
```

用户看到：

```text
发现新版本：1.1.75
```

而 Android 实际判断升级时，应以：

```text
versionCode
```

为准。

---

# 📥 APK 下载

当前 APK 下载地址：

```text
https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/apk/app-1.1.75.apk
```

建议 App 下载时：

* 显示下载进度
* 显示下载百分比
* 下载完成后校验文件
* 再调用 Android 系统安装
* 下载失败时允许重新下载

---

# 🌐 GitHub Raw

本项目使用 GitHub Raw 提供文件访问。

### OTA 版本信息

```text
https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/version.json
```

### 当前 APK

```text
https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/apk/app-1.1.75.apk
```

GitHub Raw 使用 HTTPS，因此 Android App 可以直接进行网络请求和 APK 下载。

---

# 🛠 发布新版本

以后发布新版本时，可以按照下面的流程操作。

## 第一步：修改 Android 版本号

例如：

```text
versionName = "1.1.76"
versionCode = 76
```

---

## 第二步：生成 Release APK

生成：

```text
app-1.1.76.apk
```

---

## 第三步：上传 APK

上传到：

```text
apk/
```

最终：

```text
apk/
├── app-1.1.73.apk
├── app-1.1.74.apk
├── app-1.1.75.apk
└── app-1.1.76.apk
```

---

## 第四步：修改 version.json

修改服务器最新版本：

```json
{
  "versionName": "1.1.76",
  "versionCode": 76,
  "apkUrl": "https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/apk/app-1.1.76.apk"
}
```

---

## 第五步：提交 GitHub

提交并推送：

```text
APK
+
version.json
```

---

## 第六步：测试

首先测试：

```text
https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/version.json
```

确认能够正常获取 JSON。

然后测试 APK 下载地址：

```text
https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/apk/app-1.1.76.apk
```

确认 APK 可以正常下载。

最后打开 App：

```text
检查 OTA
    ↓
发现 1.1.76
    ↓
下载 APK
    ↓
安装
```

---

# 🔐 安全建议

正式发布版本时，建议不要删除旧版本 APK。

同时建议保留正式版签名文件：

```text
release.keystore
```

以及对应的签名配置。

**签名文件一旦丢失，后续可能无法继续对已经安装的正式版本进行 OTA 覆盖升级。**

建议将签名文件和密码妥善备份，不要上传到 GitHub 仓库。

---

# 📌 当前版本

当前最新版本：

```text
1.1.75
```

当前 `versionCode`：

```text
75
```

版本信息：

```text
https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/version.json
```

当前 APK：

```text
app-1.1.75.apk
```

APK 下载：

```text
https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/apk/app-1.1.75.apk
```

---

# 📋 当前目录

```text
/
├── README.md
├── version.json
└── apk/
    ├── app-1.1.73.apk
    ├── app-1.1.74.apk
    └── app-1.1.75.apk
```

---

# 💡 后续扩展

当前 OTA 方案已经可以满足个人 Android App 的在线升级需求。

后续如果需要，可以继续扩展 `version.json`：

```json
{
  "versionName": "1.1.76",
  "versionCode": 76,
  "apkUrl": "https://raw.githubusercontent.com/lizhufeng2024-beep/NFC-BLE-OTA/main/apk/app-1.1.76.apk",
  "updateLog": [
    "优化 MQTT 连接稳定性",
    "修复 OTA 下载问题",
    "优化设备数据展示"
  ],
  "forceUpdate": false,
  "minVersionCode": 70
}
```

这样以后 App 就可以实现：

* 新版本检测
* 更新日志
* 强制更新
* 最低支持版本
* APK 自动下载
* OTA 在线升级

---

# 📄 License

本仓库仅用于个人 Android App OTA 更新及 APK 文件存储。

APK 文件仅供对应 App 的升级使用。
