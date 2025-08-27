---
title: uni-app Android APP权限说明及可能导致拒审的配置项
date: 2025-08-27 22:00:03 +0800
tags: uniapp
categories: 技术
---

# 前言

在使用 uni-app 开发安卓应用并提交至各大应用商店（如 OPPO、华为、小米等）时，应用会因涉及敏感权限或隐私收集不合规而被拒审。结合之前应用商店的审核结果与常见问题，总结了相关说明及可能导致拒审的配置项。

---

# 一、常见审核拒绝原因

1. **隐私政策未明示权限用途**
   1. 应用存在获取 IMEI、IMSI、设备 MAC 地址、安装应用列表、联系人、通话记录、位置、短信等行为，但在隐私政策中未清晰说明“收集的目的、方式、范围”。
   2. 审核流程中检测到用户点击“同意隐私政策”后，APP 即开始收集敏感信息。
2. **权限配置过多或使用不合理**
   1. 在 `Manifest` 中声明了与实际功能不符的权限，例如：
   
   ```XML
   <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"/>
   <uses-permission android:name="android.permission.READ_LOGS"/>
   <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
   <uses-permission android:name="android.permission.READ_PHONE_STATE"/>
   ```
   
   1. 这些权限可能涉及到用户隐私收集（如设备信息、账户信息、应用列表），容易引发拒审。
3. **检测到调用了系统接口读取应用列表**
   1. 审核方测试日志显示调用了 `queryIntentActivities` 等方法，属于“获取已安装应用列表”的行为。
   2. 如果应用并不需要该能力（如无第三方插件检测、无安全工具场景），应避免声明或调用相关权限。

---

# 二、可能导致拒审的敏感权限

以下为 uni-app 工程中常见但容易引发拒审的权限：

* `android.permission.READ_PHONE_STATE` → 获取设备 IMEI/IMSI、手机号。
* `android.permission.ACCESS_WIFI_STATE` → 可读取 MAC 地址、SSID。
* `android.permission.MOUNT_UNMOUNT_FILESYSTEMS` → 已废弃，涉及文件系统管理，高风险。
* `android.permission.READ_LOGS` → 获取系统日志，可能包含隐私数据。
* `android.permission.GET_ACCOUNTS` → 获取系统账户信息。
* `android.permission.CALL_PHONE` → 发起拨号，涉及隐私安全。
* `android.permission.WRITE_SETTINGS` → 修改系统设置，高敏感权限。

> 注意：即使未主动调用相关 API，仅仅在 `manifest` 中声明权限，也可能被判定为“潜在收集隐私”。

---

# 三、对功能的影响说明

* **文件上传/下载** → 不需要 `MOUNT_UNMOUNT_FILESYSTEMS`。
* **网络状态检测** → 仅需 `ACCESS_NETWORK_STATE`，删除 `ACCESS_WIFI_STATE` 不会影响 `uni.getNetworkType`。
* **第三方登录/支付** → 一般不依赖获取应用安装列表。
* **基础功能（相机、电话等）** → 正常保留对应权限即可。

---

# 四、优化与整改建议

1. **最小化权限配置**
   1. 按需配置权限，不使用的权限应移除。
   2. 定期检查 `manifest.json` 和 `AndroidManifest.xml`。
2. **完善隐私政策**
   1. 在隐私政策中明确列出：收集的数据类型、用途、方式、范围。
   2. 确保“未征得同意前不收集敏感数据”。
3. **测试与自查**
   1. 模拟用户首次启动，观察是否存在立即获取设备信息行为。

---

# 五、总结

uni-app 打包生成的 APP，在默认配置下可能包含一些敏感权限声明。如果这些权限与实际功能不匹配，极易被应用商店审核拒绝。开发时应：

* ​**删除不必要的敏感权限**​；
* ​**隐私政策中明确说明必要权限的使用场景**​；
* ​**严格遵循最小权限原则**​。

---

# 六、权限说明

| 权限                                                     | 名称                            | 描述                                                                             |
| ---------------------------------------------------------- | --------------------------------- | ---------------------------------------------------------------------------------- |
| android.permission.ACCESS\_CHECKIN\_PROPERTIES           | 访问登记属性                    | 读取或写入登记check-in数据库属性表的权限                                         |
| android.permission.ACCESS\_COARSE\_LOCATION              | 获取错略位置                    | 通过WiFi或移动基站的方式获取用户错略的经纬度信息,定位精度大概误差在30\~1500米    |
| android.permission.ACCESS\_FINE\_LOCATION                | 获取精确位置                    | 通过GPS芯片接收卫星的定位信息,定位精度达10米以内                                 |
| android.permission.ACCESS\_LOCATION\_EXTRA\_COMMANDS     | 访问定位额外命令                | 允许程序访问额外的定位提供者指令                                                 |
| android.permission.ACCESS\_MOCK\_LOCATION                | 获取模拟定位信息                | 获取模拟定位信息,一般用于帮助开发者调试应用                                      |
| android.permission.ACCESS\_NETWORK\_STATE                | 获取网络状态                    | 获取网络信息状态,如当前的网络连接是否有效                                        |
| android.permission.ACCESS\_SURFACE\_FLINGER              | 访问Surface Flinger             | Android平台上底层的图形显示支持,一般用于游戏或照相机预览界面和底层模式的屏幕截图 |
| android.permission.ACCESS\_WIFI\_STATE                   | 获取WiFi状态                    | 获取当前WiFi接入的状态以及WLAN热点的信息                                         |
| android.permission.ACCOUNT\_MANAGER                      | 账户管理                        | 获取账户验证信息,主要为GMail账户信息,只有系统级进程才能访问的权限                |
| android.permission.AUTHENTICATE\_ACCOUNTS                | 验证账户                        | 允许一个程序通过账户验证方式访问账户管理ACCOUNT\_MANAGER相关信息                 |
| android.permission.BATTERY\_STATS                        | 电量统计                        | 获取电池电量统计信息                                                             |
| android.permission.BIND\_APPWIDGET                       | 绑定小插件                      | 允许一个程序告诉appWidget服务需要访问小插件的数据库,只有非常少的应用才用到此权限 |
| android.permission.BIND\_DEVICE\_ADMIN                   | 绑定设备管理                    | 请求系统管理员接收者receiver,只有系统才能使用                                    |
| android.permission.BIND\_INPUT\_METHOD                   | 绑定输入法                      | 请求InputMethodService服务,只有系统才能使用                                      |
| android.permission.BIND\_REMOTEVIEWS                     | 绑定RemoteView                  | 必须通过RemoteViewsService服务来请求,只有系统才能用                              |
| android.permission.BIND\_WALLPAPER                       | 绑定壁纸                        | 必须通过WallpaperService服务来请求,只有系统才能用                                |
| android.permission.BLUETOOTH                             | 使用蓝牙                        | 允许程序连接配对过的蓝牙设备                                                     |
| android.permission.BLUETOOTH\_ADMIN                      | 蓝牙管理                        | 允许程序进行发现和配对新的蓝牙设备                                               |
| android.permission.BRICK                                 | 变成砖头                        | 能够禁用手机,非常危险,顾名思义就是让手机变成砖头                                 |
| android.permission.BROADCAST\_PACKAGE\_REMOVED           | 应用删除时广播                  | 当一个应用在删除时触发一个广播                                                   |
| android.permission.BROADCAST\_SMS                        | 收到短信时广播                  | 当收到短信时触发一个广播                                                         |
| android.permission.BROADCAST\_STICKY                     | 连续广播                        | 允许一个程序收到广播后快速收到下一个广播                                         |
| android.permission.BROADCAST\_WAP\_PUSH                  | WAP PUSH广播                    | WAP PUSH服务收到后触发一个广播                                                   |
| android.permission.CALL\_PHONE                           | 拨打电话                        | 允许程序从非系统拨号器里输入电话号码                                             |
| android.permission.CALL\_PRIVILEGED                      | 通话权限                        | 允许程序拨打电话,替换系统的拨号器界面                                            |
| android.permission.CAMERA                                | 拍照权限                        | 允许访问摄像头进行拍照                                                           |
| android.permission.CHANGE\_COMPONENT\_ENABLED\_STATE     | 改变组件状态                    | 改变组件是否启用状态                                                             |
| android.permission.CHANGE\_CONFIGURATION                 | 改变配置                        | 允许当前应用改变配置,如定位                                                      |
| android.permission.CHANGE\_NETWORK\_STATE                | 改变网络状态                    | 改变网络状态如是否能联网                                                         |
| android.permission.CHANGE\_WIFI\_MULTICAST\_STATE        | 改变WiFi多播状态                | 改变WiFi多播状态                                                                 |
| android.permission.CHANGE\_WIFI\_STATE                   | 改变WiFi状态                    | 改变WiFi状态                                                                     |
| android.permission.CLEAR\_APP\_CACHE                     | 清除应用缓存                    | 清除应用缓存                                                                     |
| android.permission.CLEAR\_APP\_USER\_DATA                | 清除用户数据                    | 清除应用的用户数据                                                               |
| android.permission.CWJ\_GROUP                            | 底层访问权限                    | 允许CWJ账户组访问底层信息                                                        |
| android.permission.CELL\_PHONE\_MASTER\_EX               | 手机优化大师扩展权限            | 手机优化大师扩展权限                                                             |
| android.permission.CONTROL\_LOCATION\_UPDATES            | 控制定位更新                    | 允许获得移动网络定位信息改变                                                     |
| android.permission.DELETE\_CACHE\_FILES                  | 删除缓存文件                    | 允许应用删除缓存文件                                                             |
| android.permission.DELETE\_PACKAGES                      | 删除应用                        | 允许程序删除应用                                                                 |
| android.permission.DEVICE\_POWER                         | 电源管理                        | 允许访问底层电源管理                                                             |
| android.permission.DIAGNOSTIC                            | 应用诊断                        | 允许程序到RW到诊断资源                                                           |
| android.permission.DISABLE\_KEYGUARD                     | 禁用键盘锁                      | 允许程序禁用键盘锁                                                               |
| android.permission.DUMP                                  | 转存系统信息                    | 允许程序获取系统dump信息从系统服务                                               |
| android.permission.EXPAND\_STATUS\_BAR                   | 状态栏控制                      | 允许程序扩展或收缩状态栏                                                         |
| android.permission.FACTORY\_TEST                         | 工厂测试模式                    | 允许程序运行工厂测试模式                                                         |
| android.permission.FLASHLIGHT                            | 使用闪光灯                      | 允许访问闪光灯                                                                   |
| android.permission.FORCE\_BACK                           | 强制后退                        | 允许程序强制使用back后退按键,无论Activity是否在顶层                              |
| android.permission.GET\_ACCOUNTS                         | 访问账户Gmail列表               | 访问GMail账户列表                                                                |
| android.permission.GET\_PACKAGE\_SIZE                    | 获取应用大小                    | 获取应用的文件大小                                                               |
| android.permission.GET\_TASKS                            | 获取任务信息                    | 允许程序获取当前或最近运行的应用                                                 |
| android.permission.GLOBAL\_SEARCH                        | 允许全局搜索                    | 允许程序使用全局搜索功能                                                         |
| android.permission.HARDWARE\_TEST                        | 硬件测试                        | 访问硬件辅助设备,用于硬件测试                                                    |
| android.permission.INJECT\_EVENTS                        | 注射事件                        | 允许访问本程序的底层事件,获取按键、轨迹球的事件流                                |
| android.permission.INSTALL\_LOCATION\_PROVIDER           | 安装定位提供                    | 安装定位提供                                                                     |
| android.permission.INSTALL\_PACKAGES                     | 安装应用程序                    | 允许程序安装应用                                                                 |
| android.permission.INTERNAL\_SYSTEM\_WINDOW              | 内部系统窗口                    | 允许程序打开内部窗口,不对第三方应用程序开放此权限                                |
| android.permission.INTERNET                              | 访问网络                        | 访问网络连接,可能产生GPRS流量                                                    |
| android.permission.KILL\_BACKGROUND\_PROCESSES           | 结束后台进程                    | 允许程序调用killBackgroundProcesses(String).方法结束后台进程                     |
| android.permission.MANAGE\_ACCOUNTS                      | 管理账户                        | 允许程序管理AccountManager中的账户列表                                           |
| android.permission.MANAGE\_APP\_TOKENS                   | 管理程序引用                    | 管理创建、摧毁、Z轴顺序,仅用于系统                                               |
| android.permission.MTWEAK\_USER                          | 高级权限                        | 允许mTweak用户访问高级系统权限                                                   |
| android.permission.MTWEAK\_FORUM                         | 社区权限                        | 允许使用mTweak社区权限                                                           |
| android.permission.MASTER\_CLEAR                         | 软格式化                        | 允许程序执行软格式化,删除系统配置信息                                            |
| android.permission.MODIFY\_AUDIO\_SETTINGS               | 修改声音设置                    | 修改声音设置信息                                                                 |
| android.permission.MODIFY\_PHONE\_STATE                  | 修改电话状态                    | 修改电话状态,如飞行模式,但不包含替换系统拨号器界面                               |
| android.permission.MOUNT\_FORMAT\_FILESYSTEMS            | 格式化文件系统                  | 格式化可移动文件系统,比如格式化清空SD卡                                          |
| android.permission.MOUNT\_UNMOUNT\_FILESYSTEMS           | 挂载文件系统                    | 挂载、反挂载外部文件系统                                                         |
| android.permission.NFC                                   | 允许NFC通讯                     | 允许程序执行NFC近距离通讯操作,用于移动支持                                       |
| android.permission.PERSISTENT\_ACTIVITY                  | 永久Activity                    | 创建一个永久的Activity,该功能标记为将来将被移除                                  |
| android.permission.PROCESS\_OUTGOING\_CALLS              | 处理拨出电话                    | 允许程序监视,修改或放弃播出电话                                                  |
| android.permission.READ\_CALENDAR                        | 读取日程提醒                    | 允许程序读取用户的日程信息                                                       |
| android.permission.READ\_CONTACTS                        | 读取联系人                      | 允许应用访问联系人通讯录信息                                                     |
| android.permission.READ\_FRAME\_BUFFER                   | 屏幕截图 读取帧缓存用于屏幕截图 |                                                                                  |
| com.android.browser.permission.READ\_HISTORY\_BOOKMARKS  | 读取收藏夹和历史记录            | 读取浏览器收藏夹和历史记录                                                       |
| android.permission.READ\_INPUT\_STATE                    | 读取输入状态                    | 读取当前键的输入状态,仅用于系统                                                  |
| android.permission.READ\_LOGS                            | 读取系统日志                    | 读取系统底层日志                                                                 |
| android.permission.READ\_PHONE\_STATE                    | 读取电话状态                    | 访问电话状态                                                                     |
| android.permission.READ\_SMS                             | 读取短信内容                    | 读取短信内容                                                                     |
| android.permission.READ\_SYNC\_SETTINGS                  | 读取同步设置                    | 读取同步设置,读取Google在线同步设置                                              |
| android.permission.READ\_SYNC\_STATS                     | 读取同步状态                    | 读取同步状态,获得Google在线同步状态                                              |
| android.permission.REBOOT                                | 重启设备                        | 允许程序重新启动设备                                                             |
| android.permission.RECEIVE\_BOOT\_COMPLETED              | 开机自动允许                    | 允许程序开机自动运行                                                             |
| android.permission.RECEIVE\_MMS                          | 接收彩信                        | 接收彩信                                                                         |
| android.permission.RECEIVE\_SMS                          | 接收短信                        | 接收短信                                                                         |
| android.permission.RECEIVE\_WAP\_PUSH                    | 接收Wap Push                    | 接收WAP PUSH信息                                                                 |
| android.permission.RECORD\_AUDIO                         | 录音                            | 录制声音通过手机或耳机的麦克                                                     |
| android.permission.REORDER\_TASKS                        | 排序系统任务                    | 重新排序系统Z轴运行中的任务                                                      |
| android.permission.RESTART\_PACKAGES                     | 结束系统任务                    | 结束任务通过restartPackage(String)方法,该方式将在外来放弃                        |
| android.permission.SEND\_SMS                             | 发送短信                        | 发送短信                                                                         |
| android.permission.SET\_ACTIVITY\_WATCHER                | 设置Activity观察其              | 设置Activity观察器一般用于monkey测试                                             |
| com.android.alarm.permission.SET\_ALARM                  | 设置闹铃提醒                    | 设置闹铃提醒                                                                     |
| android.permission.SET\_ALWAYS\_FINISH                   | 设置总是退出                    | 设置程序在后台是否总是退出                                                       |
| android.permission.SET\_ANIMATION\_SCALE                 | 设置动画缩放                    | 设置全局动画缩放                                                                 |
| android.permission.SET\_DEBUG\_APP                       | 设置调试程序                    | 设置调试程序,一般用于开发                                                        |
| android.permission.SET\_ORIENTATION                      | 设置屏幕方向                    | 设置屏幕方向为横屏或标准方式显示,不用于普通应用                                  |
| android.permission.SET\_PREFERRED\_APPLICATIONS          | 设置应用参数                    | 设置应用的参数,已不再工作具体查看addPackageToPreferred(String)介绍               |
| android.permission.SET\_PROCESS\_LIMIT                   | 设置进程限制                    | 允许程序设置最大的进程数量的限制                                                 |
| android.permission.SET\_TIME                             | 设置系统时间                    | 设置系统时间                                                                     |
| android.permission.SET\_TIME\_ZONE                       | 设置系统时区                    | 设置系统时区                                                                     |
| android.permission.SET\_WALLPAPER                        | 设置桌面壁纸                    | 设置桌面壁纸                                                                     |
| android.permission.SET\_WALLPAPER\_HINTS                 | 设置壁纸建议                    | 设置壁纸建议                                                                     |
| android.permission.SIGNAL\_PERSISTENT\_PROCESSES         | 发送永久进程信号                | 发送一个永久的进程信号                                                           |
| android.permission.STATUS\_BAR                           | 状态栏控制                      | 允许程序打开、关闭、禁用状态栏                                                   |
| android.permission.SUBSCRIBED\_FEEDS\_READ               | 访问订阅内容                    | 访问订阅信息的数据库                                                             |
| android.permission.SUBSCRIBED\_FEEDS\_WRITE              | 写入订阅内容                    | 写入或修改订阅内容的数据库                                                       |
| android.permission.SYSTEM\_ALERT\_WINDOW                 | 显示系统窗口                    | 显示系统窗口                                                                     |
| android.permission.UPDATE\_DEVICE\_STATS                 | 更新设备状态                    | 更新设备状态                                                                     |
| android.permission.USE\_CREDENTIALS                      | 使用证书                        | 允许程序请求验证从AccountManager                                                 |
| android.permission.USE\_SIP                              | 使用SIP视频                     | 允许程序使用SIP视频服务                                                          |
| android.permission.VIBRATE                               | 使用振动                        | 允许振动                                                                         |
| android.permission.WAKE\_LOCK                            | 唤醒锁定                        | 允许程序在手机屏幕关闭后后台进程仍然运行                                         |
| android.permission.WRITE\_APN\_SETTINGS                  | 写入GPRS接入点设置              | 写入网络GPRS接入点设置                                                           |
| android.permission.WRITE\_CALENDAR                       | 写入日程提醒                    | 写入日程,但不可读取                                                              |
| android.permission.WRITE\_CONTACTS                       | 写入联系人                      | 写入联系人,但不可读取                                                            |
| android.permission.WRITE\_EXTERNAL\_STORAGE              | 写入外部存储                    | 允许程序写入外部存储,如SD卡上写文件                                              |
| android.permission.WRITE\_GSERVICES                      | 写入Google地图数据              | 允许程序写入Google Map服务数据                                                   |
| com.android.browser.permission.WRITE\_HISTORY\_BOOKMARKS | 写入收藏夹和历史记录            | 写入浏览器历史记录或收藏夹,但不可读取                                            |
| android.permission.WRITE\_SECURE\_SETTINGS               | 读写系统敏感设置                | 允许程序读写系统安全敏感的设置项                                                 |
| android.permission.WRITE\_SETTINGS                       | 读写系统设置                    | 允许读写系统设置项                                                               |
| android.permission.WRITE\_SMS                            | 编写短信                        | 允许编写短信                                                                     |
