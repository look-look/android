## Troubleshooting
## 常问问题

### v2ray Server:
### v2ray服务器：
Cause China Mobile have high limitaion that cannot connect to internet use normal server. So we add v2ray server solve this issue.
由于中国移动限制比较严格，导致普通方式无法上网，为此专门增加了v2ray服务器解决这个问题。
(Note: install v2ray plugin before use v2ray server. Please download from this URL: https://github.com/look-look/android-plugin/releases/download/1.0.0.0/v2ray-universal-release.apk)

（使用v2ray服务器必须安装v2ray插件，请通过些链接下载：https://github.com/look-look/android-plugin/releases/download/1.0.0.0/v2ray-universal-release.apk）

### Cannot connect to server:
### 无法连接到服务器：

① Exit&Restart application

① 退出程序，重新启用

② Check your WIFI/4G connection

② 检查WIFI或4G是否连接

③ Contact to administrator if no. 1&2 cannot solve

③ 通过上面两点无法解决，联系管理员


### 移动4G无法连接SSR解决方案汇总--以SAMSUNG S7edge为例:
自国庆以来，ping不上服务器的情况经常出现，等逐渐恢复后发现移动4G无法连接，pc端、手机端在有线网络及WiFi下运行正常；以下为我从Github issues、不同forums汇总的一般解决方案。

在确保pin得通的情况下（服务器响应速度检测），可尝试以下方式解决：
1、 先打开手机热点再进行连接；（开启/关闭热点状态下未测试持续有效时间）
2、切换代理模式，开关几次飞行模式后切换为原来的代理模式；（能短暂连接，但持续有效时间不长）
3、移动网络APN设置中选择不接入IPV6。（偶尔会连接不上，但基本解决移动网络下无法正常连接问题）详细步骤：

① 在设置中选择连接；

<a href="https://pic4.zhimg.com/80/v2-9e1fb5079b5d5b4d569f832227da158b_hd.jpg"><img src="https://pic4.zhimg.com/80/v2-9e1fb5079b5d5b4d569f832227da158b_hd.jpg" height="256"></a>

② 选择移动网络；

<a href="https://pic3.zhimg.com/80/v2-aede2f270eb25ac445dd235f82ca6ba6_hd.jpg"><img src="https://pic3.zhimg.com/80/v2-aede2f270eb25ac445dd235f82ca6ba6_hd.jpg" height="256"></a>

③ 移动网路中选择APN设置；

<a href="https://pic3.zhimg.com/80/v2-e484e3f0180550e7206404e7c05adbea_hd.jpg"><img src="https://pic3.zhimg.com/80/v2-e484e3f0180550e7206404e7c05adbea_hd.jpg" height="256"></a>

④ 系统默认的APN配置无法进行修改，这里选择右上角添加新的APN；

<a href="https://pic1.zhimg.com/80/v2-6ec7b2f913d9d9652b651fcb9feb9d14_hd.jpg"><img src="https://pic1.zhimg.com/80/v2-6ec7b2f913d9d9652b651fcb9feb9d14_hd.jpg" height="256"></a>

⑤ 手动设置APN，注意APN协议及APN漫游协议都只选定IPV4；

其中需要手动配置的参数：
Name：CMNET

APN：cmnet

MCC:460（默认）

MNC:02（默认）

APN类型：default，supl，dun

<a href="https://pic1.zhimg.com/80/v2-c1df1eb63b6ca55cabb69df9f8aead5c_hd.jpg"><img src="https://pic1.zhimg.com/80/v2-c1df1eb63b6ca55cabb69df9f8aead5c_hd.jpg" height="256"></a>

再回到软件连接，大功告成！

与上面方法类似，步骤几乎相同；新增一个CMWAP的接入点，除了只选择IPV4，其余都默认选项。（最为有效，接近完全解决问题）
