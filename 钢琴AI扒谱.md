---
title: 钢琴AI扒谱安装与体验
date: 2023-06-18 15:36:10
tags: [技术, 音乐]
---

# 钢琴AI扒谱，软件安装与体验

## 简介

这个软件是一个github上开源的AI扒谱软件，效果极端优秀，可以把任何力度、节奏等等的扒到位。

github地址<https://kgithub.com/qiuqiangkong/piano_transcription_inference>

这个软件是只能扒成midi, 也就是一个通用的音乐格式，但是这个midi是不会有任何谱面记号的，也正因为如此，midi可以做到很好的自由度和还原度。

musescore软件可以直接打开midi。

因为这个原因，所以如果你直接用musescore软件打开的话，虽然谱面的音符你是直接可以看到了，但是midi原本的还原度也丧失了很多，听起来怪怪的，而且谱面是很差劲的，很乱的。所以尽量安装一个其他的软件来读取midi文件。

虽然不能直接制作成谱子，但是可以参考音符的位置，然后再人工添加记号，省下了很多听音的时间。

archlinuxWiki上说，vlc在archlinux官方源里面没有支持midi, however, vlc-git in AUR是支持的，或者你也可以安装vlc-plugin-fluidsynth-bin来使vlc支持。

就算支持了，midi也需要软音源来播放，可以理解为一个包括了很多乐器发出的声音的文件，然后midi就是乐谱，电脑就是演奏者，根据乐谱来实时演奏。

这个软音源，比较流行的有Fluid-soundfont.tar.gz，在archlinux community源里面，可以直接使用yay或者pacman安装。其他的linux distrobutions我暂时不清楚可不可以。

这个项目的要下载的东西不多的，不像stable-diffusion那样把我剩下了10多个GB的内存全部卡爆。

```bash
yay -S soundfont-fluid
```

安装好了以后要找到VLC的工具/偏好设置（在左下角点显示设置->全部）/输入、编解码器/音频编解码器/FluidSynth，把右边的SoundFont文件里面改为`/usr/share/soundfonts/FluidR3_GM.sf2`然后就可以播放了。

顺带一提，这个音源好像不是很好听，但是凑合着用吧。

「教程原视频」<https://www.bilibili.com/video/BV1e5411E7vA/?p=2&vd_source=4663f9a4c778e1a074fa26b38bf7f76b>

我几乎完全没有按照这个教程来。

## 亲自踩坑安装，和效果体验

在linux上应该会方便一点，因为我看了原视频，如果在windows上需要安装wget什么的

1. 安装torch

```bash
pip install torch
```

这个命令敲好之后基本上如果你有nvidia的显卡的话应该会把cuda安装好。



如果慢的要记得换源，后面加-i再加清华源，可以自己去<https://tuna.moe>上查看教程。

2. 把github上的项目克隆下来。

```bash
git clone https://github.com/qiuqiangkong/piano_transcription_inference
```

如果你网络不幸不行，可以在github前面加个k使用他的镜像站。

github都上不去，中国的开源环境真是没救了。

```bash
git clone https://kgithub.com/qiuqiangkong/piano_transcription_inference
```

3. 运行项目

```bash
python3 example.py --audio_path='input.mp3' --output_midi_path='output.mid' --cuda
```

如果你没有nvidia显卡的话后面--cuda别往上加了。

这个项目如果有显卡的话，是很快的，歌都放不完就扒好了，然后显卡的要求也没什么，我的MX350在linux下，很快就扒好了

4. 自动脚本

这个项目直接使用pip安装下来的有点奇怪，他没有`example.py`, 也没有`__init__.py`，所以不能直接运行，所以我使用克隆。
但是克隆的问题就是它不能在除了这个目录以外的地方运行，不然`no such file`。
所以我写了个自动的，可以少敲命令。

```bash
#!/bin/bash

ex_path='/media/xht/Data/github/piano_transcription_inference'
pwd1=$PWD

cd $ex_path

python3 example.py --audio_path="$pwd1/$1" --output_midi_path="$pwd1/$2" --cuda
```

自行修改`ex_path`







## 可能出现的问题

也许在`python3 example.py ……`的时候会出现`No librosa.core attribute audio`这样的错误提示，这样的提示大概说明了librosa的版本不兼容，所以要指定版本安装

```bash
pip install librosa==0.9.2
```


也许会提示什么`no such file`什么什么的error提示，那么这个时候你需要在运行example.py的时候当前目录PWD必须要在example.py那里，如果从其他的地方来运行的话，会报错。如果不在的花切换到那里。
对于这个问题，可以使用我之前提到过的脚本来运行。


README里面的Installation里面说要pip安装这个项目，实测不装，只是git clone也可以。


第一次运行的时候可能会大约200KB/s地下载一个160多MB的文件，这是正常的，等待一会就可以了。

