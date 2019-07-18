---
title: ElementaryOS 使用说明
date: 2019-07-11 00:05:09
tags: Linux/ElementrayOS
---

#### 1、终端执行命令导入软件仓库钥

```
sudo wget -O - http://package.elementaryos.cn/apt/key/package.gpg.key | sudo apt-key add -
```

#### 2、打开sources.list文件并在最后新起一行粘贴下面软件源地址信息然后保存：

```
sudo io.elementary.code /etc/apt/sources.list         //打开sources.list文件
```

```
deb http://package.elementaryos.cn/bionic/ bionic main    //添加软件源地址信息
```

#### 3、更新软件包缓存

```
sudo apt update
```

#### 4、安装软件包命令

```
sudo apt install 软件包名 
```

例如安装TIM命令为：

```
sudo apt install deepin.com.qq.office 
```

现阶段所有软件包如下：现阶段所有软件包如下：

elementary-tweakselementary-tweaks配置工具
wingpanel-indicator-ayatana第三方应用状态栏图标修复，安装后重启
deepin.cn.360.yasuo360压缩
deepin.cn.com.winrarWinRAR
deepin.com.baidu.pan百度网盘(wine)
baidunetdisk百度网盘(原生)
deepin.com.foxmailFoxmail邮件客户端
deepin.com.qq.imQQ
deepin.com.taobao.wangwang阿里旺旺
deepin.com.thunderspeed迅雷
deepin.com.wechat.devtools微信开发者工具
deepin.com.wechat微信
deepin.com.weixin.work企业微信
deepin.org.7-zip7-Zip
deepin.org.foobar2OOOfoobar2000
electron-ssr酸酸乳
navicatNavicat数据库管理客户端
netease-cloud-music网易云音乐
sogoupinyin搜狗拼音输入法
wps-officeWPS
motrixmotrix下载软件
anydeskanydesk跨平台远程协助软件
symbol-fonts wps字体缺失修复包

#### 最后说几句：

博主自己安装tim，是没问题的，就是下载的速度慢，基本上是处于100多k，所以从个人角度来说，其实不建议这样下载软件，通过这个网站下载软件其实速度会慢很多，个人猜测是服务器的问题，所以除非是在网上找不到资源或者其他办法没办法安装成功，才考虑从这个网站下载资源



转载网址：https://elementaryos.cn/storage.html