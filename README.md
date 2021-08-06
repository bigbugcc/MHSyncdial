# -----Mentohust校园网多播提升网速解决方案-----
### &ensp;&ensp;&ensp;&ensp;在这之前你得确保配置好基础的多播(luci-app-syncdial); 无论是单线还是多线都可;
---
## 原理   
### &ensp;&ensp;&ensp;&ensp;验证客户端基于mentohust，需要在openwrt中安装该插件(编译时记得添加锐捷组件)，Padvan固件已经自带，该脚本主要通过干掉mh(mentohust)运行的进程号实现多进程从而实现多播，需配合openwrt的多播插件 (luci-app-syncdial) 来使用，该方法简单易用；也可以通过修改mh源码删除创建进程部分的代码来实现，都可。通过绑定虚拟出来的macvlan来进行多播提升网速，前提得需要多个校园网账号 (若贵校支持多设备登录就当我没说)。
---
## 优化
### &ensp;&ensp;&ensp;&ensp;为了避免学校监管对账号进行行为侦测再次限速限流等操作 (固定使用一俩个账号流量太大还是会被限速 毕竟下片那啥...) ，特此添加了随机账户登录 每天登陆的账号都是随机的(将流量较为平均的分载)，将舍友的校园网账号添加到数组中去，密码统一下就可。虽然基本已经起飞了，但是 下片的兄弟还是收敛一点 : ) 奔放了大半年回来分享一下。
---
## 脚本适用Openwrt(lede测试通过)
1. 单线多播
2. 多线多播
(绑定多播的虚拟网卡就可 一般为macvlan1，macvlan2...)
---
## 多播选择(视具体网络环境而定)
1. 如果显示接入为千兆线也支持千兆传输(直连后网络适配器中查看连接速率)那单线多播就够用了。
2. 若百兆单账号也能跑满那就果断多线多播。
---
## 安装使用
将脚本上传到(openwrt) /etc/config/ 目录下
```
cd /etc/config

chmod +x mentohust.sh

./mentohust.sh
```
配置(自行替换)
```
# 存放校园网账号的数组
stuid=("26352371732" "20323821931" "202123210072" "2022187382174")

# 修改"-pabc1234"密码为abc1234 其他保持默认
# 修改"-nmacvlan1"虚拟网卡(默认为)macvlan1 修改为自己的虚拟网卡
mentohust -u${stuid[$rnd1]} -pabc1234 -nmacvlan1 -b3 -o223.5.5.5 -a1 -d0 -v6.82 -cdhclient 

# 该脚本默认为双播，可自行修改添加。 
```
  
开机自启
```
vim /etc/rc.local

# exit0 前添加(脚本位置)
/etc/config/mentohust.sh

# 最重要(不然无法启动脚本)
chmod +x /etc/rc.local
```

## 其他
[openwrt 编译教程](https://www.bilibili.com/read/cv9714518)👈   
[openwrt 自动编译脚本(持续更新中)👈](https://github.com/bigbugcc/Openwrt)