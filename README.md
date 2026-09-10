# APK OTA 更新服务器

用于存放 Android App APK 安装包，为 App 提供 OTA 在线升级下载服务。

## 📦 项目说明

本仓库用于保存 Android App 的正式 APK 安装包。

App 可以通过网络获取 APK 下载地址，当检测到新版本后，自动下载并安装更新。

本项目使用 GitHub 作为免费的 APK 文件存储和下载服务器。

## 📁 文件结构

```text
/
├── README.md
└── apk/
    ├── app-1.0.0.apk
    ├── app-1.1.0.apk
    ├── app-1.2.0.apk
    └── app-1.5.0.apk
```

## 🚀 APK 发布

每次发布新版本时，将新的 APK 上传到 `apk` 目录。

例如：

```text
apk/app-1.5.0.apk
```

建议 APK 文件名统一使用：

```text
应用名称-版本号.apk
```

例如：

```text
HYTool-1.5.0.apk
HYTool-1.6.0.apk
HYTool-2.0.0.apk
```

## 🔗 APK 下载地址

GitHub Raw 下载地址格式：

```text
https://raw.githubusercontent.com/用户名/仓库名/main/apk/app-1.5.0.apk
```

例如：

```text
https://raw.githubusercontent.com/your-name/android-ota/main/apk/app-1.5.0.apk
```

Android App 可以直接使用该地址下载 APK。

## 🔄 OTA 更新流程

App 的升级流程：

```text
启动 App
   ↓
检查服务器最新版本
   ↓
当前版本 < 最新版本？
   ↓
   是
   ↓
提示用户更新
   ↓
下载 APK
   ↓
安装 APK
   ↓
完成升级
```

## 📝 版本管理

建议每个版本保留对应的 APK，不要直接覆盖旧版本。

例如：

```text
apk/
├── HYTool-1.4.0.apk
├── HYTool-1.5.0.apk
├── HYTool-1.6.0.apk
└── HYTool-1.7.0.apk
```

这样可以方便后续查找历史版本。

## ⚠️ 注意事项

### 1. APK 签名必须一致

OTA 升级时，新 APK 必须使用与当前 App 相同的签名证书。

否则 Android 无法直接覆盖安装。

### 2. versionCode 必须递增

例如：

```text
1.0.0 → versionCode 1
1.1.0 → versionCode 2
1.5.0 → versionCode 3
```

新版本的 `versionCode` 必须大于当前安装版本。

### 3. APK 文件名建议带版本号

例如：

```text
HYTool-1.5.0.apk
```

不要所有版本都使用：

```text
app.apk
```

这样更方便版本管理。

## 🛠 发布新版本

发布新 APK 时：

1. 修改 Android App 版本号
2. 打包生成 Release APK
3. 将 APK 上传到 `apk` 目录
4. 提交并推送到 GitHub
5. 等待 GitHub 更新完成
6. App 即可通过新的下载地址获取 APK

## 📌 当前版本

当前最新版本：

```text
1.5.0
```

APK：

```text
apk/app-1.5.0.apk
```

## 📄 License

本仓库仅用于个人 Android App OTA 更新及 APK 文件存储。
