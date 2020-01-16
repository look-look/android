### Troubleshooting

Cannot connect to server:

1. Stop battery saver if it's active;
2. Check your config;
3. Wipe app data.

Crash: [Submit an issue](https://github.com/shadowsocks/shadowsocks-android/issues/new) with logcat attached, or submit a crash report to Google Play. Then, try wiping app data.

### How to create a widget and/or switch profile based on network connectivity?

Use [Tasker](http://tasker.dinglisch.net/) integration.

### Why is NAT mode deprecated?

1. Requiring ROOT permission;
2. No IPv6 support;
3. No UDP relay support.

### How to remove the exclamation mark when using VPN mode?

The exclamation mark in the Wi-Fi/cellular icon appears because the system fails to connect to portal server (defaults to `clients3.google.com`) without VPN connection. To remove it, follow the instructions in [this article](https://www.noisyfox.cn/45.html). (in Simplified Chinese)

### Why is my ROM not supported?

1. Some ROM has broken VPNService implementation, especially for IPv6;
2. Some ROM has aggressive (or called broken) background service killing policy;
3. Some ROM like [Flyme](https://github.com/shadowsocks/shadowsocks-android/issues/1821) is basically broken **in every way possible**;
4. If you have Xposed framework and/or battery saver apps, it's likely that this app wouldn't work well with these either.

* Fixes for MIUI: [#772](https://github.com/shadowsocks/shadowsocks-android/issues/772)
* Fixes for EMUI: [#888](https://github.com/shadowsocks/shadowsocks-android/issues/888)
* Fixes for Huawei: [#1091 (comment)](https://github.com/shadowsocks/shadowsocks-android/issues/1091#issuecomment-276949836)
* Related to Xposed: [#1414](https://github.com/shadowsocks/shadowsocks-android/issues/1414)
* Samsung and/or Brevent: [#1410](https://github.com/shadowsocks/shadowsocks-android/issues/1410)
* Another Samsung: [#1712](https://github.com/shadowsocks/shadowsocks-android/issues/1712)
* Samsung with GMS: [#2138](https://github.com/shadowsocks/shadowsocks-android/issues/2138)
* Don't install this app on SD card because of permission issues: [#1124 (comment)](https://github.com/shadowsocks/shadowsocks-android/issues/1124#issuecomment-307556453)
* `INTERACT_ACROSS_USERS` permission missing: [#1184](https://github.com/shadowsocks/shadowsocks-android/issues/1184)

### How to pause Shadowsocks service?

* For Android 7.0+: Use quick switch tile in Quick Settings;
* Use Tasker integration;
* Add a profile with per-app proxy enabled for Shadowsocks only, bypass mode off.

### Why does Shadowsocks consume so much battery on Android 5.0+?

As Shadowsocks takes over the whole device network, any battery used by network activities from other apps are also counted as those from Shadowsocks. So, the battery usage of Shadowsocks equals to the sum of all the network activities of your device. Shadowsocks itself is a totally I/O bound application on modern Android devices, which is expected not to consume any notable battery.

So if you notice a significant increase in battery usage after you use Shadowsocks, it's most likely caused by other apps. For example, Google Play services can consume more battery after being able to connecting to Google, etc.

More details: https://kb.adguard.com/en/android/solving-problems/battery

### It works fine under Wi-Fi but can't connect through cellular data?

Allow this app to consume background data in app settings.

### How to use Transproxy mode?

1. Install [AFWall+](https://github.com/ukanth/afwall);
2. Set custom script:
```sh
IP6TABLES=/system/bin/ip6tables
IPTABLES=/system/bin/iptables
ULIMIT=/system/bin/ulimit
SHADOWSOCKS_UID=`dumpsys package com.github.shadowsocks | grep userId | cut -d= -f2 - | cut -d' ' -f1 -`
PORT_DNS=5450
PORT_TRANSPROXY=8200
$ULIMIT -n 4096
$IP6TABLES -F
$IP6TABLES -A INPUT -j DROP
$IP6TABLES -A OUTPUT -j DROP
$IPTABLES -t nat -F OUTPUT
$IPTABLES -t nat -A OUTPUT -o lo -j RETURN
$IPTABLES -t nat -A OUTPUT -d 127.0.0.1 -j RETURN
$IPTABLES -t nat -A OUTPUT -m owner --uid-owner $SHADOWSOCKS_UID -j RETURN
$IPTABLES -t nat -A OUTPUT -p tcp --dport 53 -j DNAT --to-destination 127.0.0.1:$PORT_DNS
$IPTABLES -t nat -A OUTPUT -p udp --dport 53 -j DNAT --to-destination 127.0.0.1:$PORT_DNS
$IPTABLES -t nat -A OUTPUT -p tcp -j DNAT --to-destination 127.0.0.1:$PORT_TRANSPROXY
$IPTABLES -t nat -A OUTPUT -p udp -j DNAT --to-destination 127.0.0.1:$PORT_TRANSPROXY
```
3. Set custom shutdown script:
```sh
IP6TABLES=/system/bin/ip6tables
IPTABLES=/system/bin/iptables
$IPTABLES -t nat -F OUTPUT
$IP6TABLES -F
```
4. Make sure to allow traffic for Shadowsocks;
5. Start Shadowsocks transproxy service and enable firewall.

### 移动4G无法连接SSR解决方案汇总--以SAMSUNG S7edge为例
自国庆以来，pin不上服务器的情况经常出现，等逐渐恢复后发现移动4G无法连接，pc端、手机端在有线网络及WiFi下运行正常；以下为我从Github issues、不同forums汇总的一般解决方案。

在确保pin得通的情况下（服务器响应速度检测），可尝试以下方式解决：

1、 先打开手机热点再进行连接；（开启/关闭热点状态下未测试持续有效时间）

2、切换代理模式，开关几次飞行模式后切换为原来的代理模式；（能短暂连接，但持续有效时间不长）

3、移动网络APN设置中选择不接入IPV6。（偶尔会连接不上，但基本解决移动网络下无法正常连接问题）详细步骤：

① 软件服务器设置中IPV6选项保持关闭；
<img src="https://pic4.zhimg.com/80/v2-f683e565f621ff8bf535e5ef98955e7b_hd.jpg">

② 在设置中选择连接；
<img src="https://pic4.zhimg.com/80/v2-9e1fb5079b5d5b4d569f832227da158b_hd.jpg">

③ 选择移动网络；
<img src="https://pic3.zhimg.com/80/v2-aede2f270eb25ac445dd235f82ca6ba6_hd.jpg">

④ 移动网路中选择APN设置；
<img src="https://pic3.zhimg.com/80/v2-e484e3f0180550e7206404e7c05adbea_hd.jpg">

⑤ 系统默认的APN配置无法进行修改，这里选择右上角添加新的APN；
<img src="https://pic1.zhimg.com/80/v2-6ec7b2f913d9d9652b651fcb9feb9d14_hd.jpg">

⑥ 手动设置APN，注意APN协议及APN漫游协议都只选定IPV4；

其中需要手动配置的参数：

Name：CMNET

APN：cmnet

MCC:460（默认）

MNC:02（默认）

APN类型：default，supl，dun
<img src="https://pic1.zhimg.com/80/v2-c1df1eb63b6ca55cabb69df9f8aead5c_hd.jpg">

再回到软件连接，大功告成！
<img src="https://pic3.zhimg.com/80/v2-cc0274f831a59d18c0d0cc8395a5ca06_hd.jpg">

4、与上面方法类似，步骤几乎相同；新增一个CMWAP的接入点，除了只选择IPV4，其余都默认选项。（最为有效，接近完全解决问题）
