---
title: tkinter插入窗口图标的方法
date: 2023-08-09 11:00:31
tags: 技术
---

这个问题的发现和解决是有将近一年了，当时我在开发一个背单词的一个小项目，但是我在想打包的时候出现问题了。

我想更换窗口的图标，就是windows下左上角的那个小图标，但是tkinter给的方法的参数却是一个文件名，这意味着我不能完美地把这个项目打包成一个单文件，而是需要把这个icon文件和打包好的exe放在一起，变成两个文件发布。

网上所给出的方法一般是先把这个ico文件打包进去，需要的时候在写到cache, 然后再让tkinter用这个文件名来导入。这个方法一看就极其不优雅，虽然性能也就这样了，但是这样的方法是我所不能接受的。

最后，我查看了ttkbootstrap的源码解决了问题。

我想，ttkbootstrap有一个自带的图标，是不管在哪个代码里面执行都会有这个图标，于是我想参考官方的做法。

我当初发现的ttkbootstrap的代码是在哪里我早就已经忘记了，但是我解决问题的思路仍清晰。

之前我看到ttkbootstrap的源码里面有一段base64，然后使用了这段base64来导入，并非网上的先写到cache, 这也符合我对一个成熟的项目的预期。

所以我贴上我解决的代码：

```python
root = tk.Tk()
root.title("Xdict背单词")
photo = ttk.PhotoImage(master=root, data=myIcon.img)
root.iconphoto(True, photo)
```


这段代码中的data项就是一个base64字符串。

制作myIcon.py需要编写一个mkicon.py.

```python
#!/bin/python3
import base64
with open("icon.py","a") as f:
    f.write("img='")
with open("bell.png","rb") as i:
    b64str = base64.b64encode(i.read())
    with open("icon.py","ab+") as f:
        f.write(b64str)
with open("icon.py","a") as f:
    f.write("'")
```


这段代码的意思就是读取目录下的bell.png的文件内容并把他转换为base64, 输出到myIcon.py, 这样pyinstaller也可以直接知道要调用。

最后要在main.py里面引入myIcon

写的不太清楚，若不能理解请通过文章“博客介绍”里面的联系方式联系我
