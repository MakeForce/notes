# 搞机

## 安卓类原生优化

### 修改NTP时间服务器

- 部分公开的服务：
  - 国家授时中心[中国科学院](https://www.cas.cn/tz/201809/t20180921_4664344.shtml)：`ntp.ntsc.ac.cn`
  - 阿里云公共NTP服务：`ntp.aliyun.com`
  - 苹果：`time.asia.apple.com`
  - 百度：`ntp1.baidu.com`
  - 腾讯：`http://time1.cloud.tencent.com`
  - 清华大学：`ntp.tuna.tsinghua.edu.cn`
  - 等其他服务器

- 修改命令：
  ```shell
  adb shell settings put global ntp_server 上边的服务器选一个，或者是其他的NTP服务器
  ```

- 查询命令：
  ```shell
  adb shell settings get global ntp_server
  ```

- 修改示例：

  ```shell
  adb shell settings put global ntp_server ntp.ntsc.ac.cn
  ```

### 网络可用性测试服务

1. 删除默认测试服务器

   ```shell
   adb shell settings delete global captive_portal_https_url
   adb shell settings delete global captive_portal_http_url
   ```

2. 设置测试服务器  
   - 测试服务器地址：
     - 小米：`https://connect.rom.miui.com/generate_204`
     - 小米：`http://connect.rom.miui.com/generate_204`
     - 华为：`http://connectivitycheck.platform.hicloud.com/generate_204`
     - vivo：`http://wifi.vivo.com.cn/generate_204`
     - 高通：`http://www.qualcomm.cn/generate_204`
     - Cloudflare：`http://cp.cloudflare.com/generate_204`

   - 修改示例：

    ```shell
    adb shell settings put global captive_portal_https_url https://connect.rom.miui.com/generate_204
    adb shell settings put global captive_portal_http_url http://connect.rom.miui.com/generate_204
    ```

3. 查询配置

  ```shell
  adb shell settings get global captive_portal_https_url
  adb shell settings get global captive_portal_http_url
  ```

## 安卓Root

### Magisk

- 仓库：https://github.com/topjohnwu/Magisk
- 下载地址：https://github.com/topjohnwu/Magisk/releases/latest
- 文档：https://topjohnwu.github.io/Magisk/
  - 安装文档：https://topjohnwu.github.io/Magisk/install.html
  - 构建和开发者文档：https://topjohnwu.github.io/Magisk/build.html

#### 模块

##### Riru

> 这个模块和

!> 该模块已经不推荐安装，因为它被`Zygisk`模块替代了

- 仓库：https://github.com/RikkaApps/Riru
- 文档：https://github.com/RikkaApps/Riru?tab=readme-ov-file#guide
- 下载地址：https://github.com/RikkaApps/Riru/releases/latest



### KernelSU

- 仓库：https://github.com/tiann/KernelSU
- 下载地址：https://github.com/tiann/KernelSU/releases/latest
- 文档：https://kernelsu.org/zh_CN/guide/what-is-kernelsu.html

### Shizuku

- 仓库地址：https://github.com/RikkaApps/Shizuku
- 说明文档：https://shizuku.rikka.app/zh-hans/
- 下载地址：https://github.com/RikkaApps/Shizuku/releases/latest
- Sui
- https://github.com/RikkaApps/Sui
- https://github.com/RikkaApps/Sui/releases/latest


## 抓包

### 桌面软件

- Charles：https://www.charlesproxy.com/download/
- Fiddler：https://www.telerik.com/download/fiddler
- Mitmproxy：https://mitmproxy.org/
- Wireshark：https://www.wireshark.org/download.html
- 等其他软件

### 安卓抓包

#### 方式一

> 该方法需要使用`Charles使用`、`Fiddler`等其他`MITM`软件配合使用；`Magisk`、`LSPosed` 安装相关模块实现SSL相关函数的Hook；

##### 步骤

  1. 安装[Magisk](#magisk);
    1. ~~安装`Riru`模块~~，该模块被`Magisk`的内置`Zygisk`模块替代，需要注意的是，`Zygisk` 模块需要主动开启；

    2. 安装`LSPosed`模块，**请注意要下载的是`Zygisk`版本**，*很可惜开发者受到恶意攻击，已经放弃该软件的维护了*；
        - 仓库地址：https://github.com/LSPosed/LSPosed
        - 安装文档：https://github.com/LSPosed/LSPosed?tab=readme-ov-file#install
        - 下载地址：https://github.com/LSPosed/LSPosed/releases/latest
  2. 安装`Lsposed-manager`应用；
      - 下载地址：https://github.com/LSPosed/LSPosed/releases/latest
      - 下载文件后解压，**请注意要下载的是`Zygisk`版本**，在压缩文件中找到`manager.apk` 并安装；
  3. 安装`TrustMeAlready`，**该软件已归档**；
      - 仓库地址：https://github.com/ViRb3/TrustMeAlready
      - 下载地址：https://github.com/ViRb3/TrustMeAlready/releases/latest
  4. 在`Lsposed`软件中切换到模块，选择`TrustMeAlready`模块并开启该模块，然后选择需要抓包的软件；

##### 备注

- **warning：使用SSL Pinning并开启混淆、等其他技术的应用，该方法可能不能成功**
- `JustTrustMe`是类似于`TrustMeAlready`的软件，由于`JustTrustMe`很久没有更新了，才出现的`TrustMeAlready`，两者本质上是没有区别的，都是通过Hook相关函数实现的；
  - 仓库地址：https://github.com/Fuzion24/JustTrustMe
  - 下载地址：https://github.com/Fuzion24/JustTrustMe/releases/latest

#### 方式二

> 该方法主要解决安卓7.0之后`MITM`软件CA证书无法被信任的问题

##### 步骤

  1. 安装[Magisk](#magisk);
    - 安装`TrustUserCertificates`模块；
        - 仓库地址：https://github.com/lupohan44/TrustUserCertificates
        - 说明文档：https://github.com/lupohan44/TrustUserCertificates?tab=readme-ov-file#installation
        - 下载地址：https://github.com/lupohan44/TrustUserCertificates/releases/latest
  2. 安装`MITM`软件的CA证书；
  3. 重启设备；
  4. 尝试抓包；

##### 备注

- **warning：使用SSL Pinning并开启混淆、等其他技术的应用，该方法无法抓取**
- `TrustUserCertificates` 该模块理论上支持`Android 8.0 ~ 14`，但是只测试了14；
- `MagiskTrustUserCerts` 该模块适用于 `Android 7.0` 至 `Android 13`
  - 仓库地址：https://github.com/NVISOsecurity/MagiskTrustUserCerts
  - 说明文档：https://github.com/NVISOsecurity/MagiskTrustUserCerts?tab=readme-ov-file#installation
  - 下载地址：https://github.com/NVISOsecurity/MagiskTrustUserCertsreleases/latest
- `Adguardcert` 该模块仅适用于`AdGuard`软件，其实现脚本具备参考价值
  - 仓库地址：https://github.com/AdguardTeam/adguardcert

#### 方式三

> 利用VPN转发截取，该方式未测试，看一下原理比较合适

##### 安卓软件

- NetWorkPacketCapture：https://github.com/huolizhuminh/NetWorkPacketCapture
- AndroidHttpCapture：https://github.com/JZ-Darkal/AndroidHttpCapture
- NetBare-Android：
  - 这是一个框架并非应用
  - 仓库地址(建议Fork)：https://github.com/MegatronKing/NetBare-Android

### iOS

#### 传统软件

- Charles：https://www.charlesproxy.com/download/
- Fiddler：https://www.telerik.com/download/fiddler
- Mitmproxy：https://mitmproxy.org/
- 等其他软件

#### 移动APP抓包

> 利用VPN/代理转发截取

- Stream：https://apps.apple.com/cn/app/stream/id1312141691?l=zh
- HTTP Catcher：https://apps.apple.com/cn/app/http-catcher/id1445874902?l=zh
  - 官网：https://httpcatcher.com/

### 跨平台

- https://github.com/wanghongenpin/network_proxy_flutter

### 相关资料

- [移动安全面试题—抓包](https://zhuanlan.zhihu.com/p/650527206)
- [https与证书原理 | SSL Pinning及其绕过](https://blog.csdn.net/qq_39441603/article/details/126183031)
- [证书锁定SSL Pinning简介及用途](https://zhuanlan.zhihu.com/p/58204817)
  - [原文](https://www.infinisign.com/faq/what-is-ssl-pinning#)
  - [Android SSL证书设置和锁定](https://www.infinisign.com/faq/android-ssl-pinning)
  - [IOS SSL证书设置和锁定](https://www.infinisign.com/faq/ios-ssl-pinning-three-method)
- [安卓抓包工具开发](https://www.jianshu.com/p/ae4d433597ce)
- [JustTrustMe 原理分析](https://blog.csdn.net/tangsilian/article/details/86612470)
- [安卓手机mitmproxy抓包](https://blog.csdn.net/m0_57468722/article/details/127852867)
- 安卓`mount /system`相关文章
  - https://blog.csdn.net/u014711665/article/details/111307613
  - https://blog.csdn.net/A_I_H_L/article/details/125764664
- [安卓 IOS 抓包工具介绍、下载及配置](https://www.cnblogs.com/mswei/p/14168553.html)

## other

- https://github.com/kdrag0n/safetynet-fix

### Frida

- Frida入门总结：https://zhuanlan.zhihu.com/p/559514512
- [使用Frida绕过Android SSL Re-Pinning](https://www.anquanke.com/post/id/86507)
- [Frida自吐证书密码](https://www.anquanke.com/post/id/272672)

### VirtualApp

- https://github.com/asLody/VirtualApp




### 逆向

- https://github.com/java-decompiler/jd-gui
- https://github.com/pxb1988/dex2jar
- Frida


### EdXposed

- https://www.behttp.com/archives/127.html
- https://sspai.com/post/73294

### 太极

- https://taichi.cool/zh/

### httptoolkit

- https://github.com/httptoolkit

### Epic

- https://github.com/tiann/epic


### genuine

- https://github.com/brevent/genuine/


### lamda

- https://github.com/rev1si0n/lamda