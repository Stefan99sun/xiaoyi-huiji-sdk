# 小艺慧记 · Android 接入

把离线中文语音转写面板加到现有 Android 应用。用户点击宿主里的入口后进入「小艺慧记」，授权麦克风即可开始转写。点击底部音波栏可暂停或继续，关闭面板返回宿主页面。

## 1. 添加 Maven 仓库

在宿主工程根目录的 `settings.gradle.kts` 或 `settings.gradle` 中，将以下 `maven` 块加入已有的 `dependencyResolutionManagement.repositories`。保留工程的 `google()` 和 `mavenCentral()`。

```kotlin
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
        maven {
            url = uri("https://raw.githubusercontent.com/Stefan99sun/xiaoyi-huiji-sdk/main/maven")
            content { includeGroup("io.github.stefan99sun") }
        }
    }
}
```

## 2. 引入组件

在宿主应用模块的 `app/build.gradle.kts` 或 `app/build.gradle` 的 `dependencies` 块中添加这一行，然后同步工程。

```kotlin
implementation("io.github.stefan99sun:xiaoyi-huiji:1.0.0")
```

Gradle 会下载面板、中文识别模型、Vosk 和 JNA。宿主需支持 Android 7.0（API 24）及以上。

## 3. 从自己的页面打开

在宿主 Activity 的按钮点击回调中调用以下代码：

**Kotlin**

```kotlin
startActivity(android.content.Intent(this, cn.xiaoyi.huiji.HuiJiActivity::class.java))
```

**Java**

```java
startActivity(new android.content.Intent(this, cn.xiaoyi.huiji.HuiJiActivity.class));
```

SDK 的 Manifest 会自动合入麦克风权限和页面声明。用户首次使用时，中文模型会解包到应用私有目录。录音和转写在设备上完成，暂停、退出页面或切到后台都会停止收音。本版本提供完整的 Activity，转写结果显示在面板内，暂未提供向宿主返回文字的回调。
