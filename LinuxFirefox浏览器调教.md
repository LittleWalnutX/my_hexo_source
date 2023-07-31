---
title: LinuxFirefox浏览器调教
date: 2023-07-31 11:15:40
tags: [技术, linux]
---

我刚开始尝试linux系统的时候是一年前，那时候我装的是linux mint, 碰巧这个系统是基于ubuntu 20.04, 而这个系统出来的时候我的笔记本都没有出来，所以里面好多的问题，比如屏幕缩放的问题、视频硬解的问题，chromium鼠标滚动速度慢的问题、触控板不支持多点触控的问题……

好在我最终没有放弃，用到了现在，然而时间过去了一年，我也亲眼见证了linux系统的好多严重妨碍使用体验的小小bug被修复了，可喜可贺。

让我们一起见证linux变得越来越好！

## -1. 关于选linux发行版的一个建议

建议如果选择LTS发行版用作个人linux电脑，则不要选择太老的，比如你像我一样，在2022年7月初装了linuxMint, 然而这个时候LinuxMint只有20.x版，Ubuntu的22.04已经出了，LinuxMint开发有一定的时间延迟，但是我的电脑是2020年的款，所以我阴差阳错的装上了2年前、我的电脑出生之前的distrobution, 所以导致了很多的驱动、软件等问题

建议选择新一点的，最好滚动更新，因为linux作为PC是个还在进步的系统，直接装上大概率是不如Windows的完美体验的，总有点小毛病，比如视频硬解、比如Firefox的触控板和触摸屏支持。过了一段时间这些bug可能会一个一个修复，但是你的发行版是LTS就收不到更新了。好像现在python3.11都出了但是ubuntu 20.04还是最高支持到3.8

## 1. linux firefox支持触控板和触屏平滑滚动

linux的firefox默认是不支持触控板的平和滚动的，所以滚动起来就是模拟鼠标，然后是一顿一顿的，卡顿不跟手。还有一个问题是他不支持平滑缩放，对于笔记本电脑来说这是一个十分影响使用体验的问题。更离谱的是如果你的笔记本电脑支持触屏，那么你用手指在屏幕上滑动的时候火狐会认为你是用鼠标在屏幕滑动，于是选中了很多文字。好在现在我们可以通过更改配置来实现这两个功能。

引自网页：

[使linux版firefox支持触屏操作](https://beekc.top/2019/04/01/firefox-touchscreen/#:~:text=%E7%AC%AC%E4%B8%80%E4%B8%AA%E6%98%AF%E5%9C%A8about%3Aconfig%E4%B8%AD%E6%89%BE%E5%88%B0dom.w3c_touch_events.enabled%E9%A1%B9%E6%94%B9%E4%B8%BA1%EF%BC%88%E5%90%AF%E7%94%A8%EF%BC%89%EF%BC%8C%E9%BB%98%E8%AE%A4%E4%B8%BA2%EF%BC%88%E8%87%AA%E5%8A%A8%EF%BC%89%E3%80%82%20%E7%AC%AC%E4%BA%8C%E4%B8%AA%E5%9C%B0%E6%96%B9%E6%98%AF%E4%BF%AE%E6%94%B9%E6%96%87%E4%BB%B6%2Fetc%2Fsecurity%2Fpam_env.conf%EF%BC%8C%E5%9C%A8%E6%96%87%E4%BB%B6%E6%9C%80%E5%90%8E%E6%B7%BB%E5%8A%A0%E4%B8%8B%E9%9D%A2%E4%BB%A3%E7%A0%81,MOZ_USE_XINPUT2%20DEFAULT%3D1%20%E4%BF%AE%E6%94%B9%E5%AE%8C%E6%88%90%E5%90%8E%E9%87%8D%E5%90%AFFirefox%E5%B0%B1%E5%8F%AF%E4%BB%A5%E4%BD%BF%E7%94%A8%E8%A7%A6%E6%91%B8%E5%B1%8F%E8%BF%9B%E8%A1%8C%E6%93%8D%E4%BD%9C%E4%BA%86%E3%80%82)

> 一共有两个地方需要修改：
> 
> 第一个是在about:config中找到dom.w3c_touch_events.enabled项改为1（启用），默认为2（自动）。
> 
> 第二个地方是修改文件/etc/security/pam_env.conf，在文件最后添加下面代码
> 
> MOZ_USE_XINPUT2 DEFAULT=1
> 
> 修改完成后重启Firefox就可以使用触摸屏进行操作了。支持的操作有拖拽和双指缩放。
> 
> 此外还发现鼠标滚轮滚送速度有些慢，会经常抽动。通过设置mousewheel.min_line_scroll_amount项为40，设置 general.smoothScroll项为true，设置 general.smoothScroll.pages项为false。
> 

这样设置完重启firefox可能是不够的，可能还要重启系统

如果还不行就下载一个Wayland的桌面，然后你去Wayland里面看看开了没，开了以后应该X11也能用的

装完后好像还是不如windows丝滑

我这么弄能成功纯属瞎折腾瞎猫碰上死耗子

## 2. Firefox开启浏览器的视频硬件加速

linux作为一台个人电脑的一个很重要的功能是放视频，毕竟如果用CPU软解然后卡顿发热，那么是真的不可接受的

Firefox开启视频硬件加速的方法，你首先要在电脑上装好VA-API的显卡驱动，我只知道intel的。

然后要开启firefox里面的一些选项

推荐直接看这篇文章

<https://zhuanlan.zhihu.com/p/268401890>

能看懂Archwiki的推荐Archwiki


## 3. 总结

linux的一些小毛病是很难折腾的，说不定不是我的问题干脆就是软件bug, 比如我最近在KDE看到我的CPU总是有一个核心100%占用，但是我下载了gnome的系统monitor, 又用htop发现全正常。所以折腾很久是很正常的，但是linux是在进步的！我非常高兴看到进步。
