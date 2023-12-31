---
title: '用Python轻松实现视频转gif动图'
date: '2022-7-30 21:40:00'
cover: https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/cover2.png
categories: 
- Python
tags:
- Python
- moviepy
- gif
---

不知道大家是不是有过类似的经历，想把视频转换为gif动态图片，却无从下手。网上搜的网站和软件很多是需要付费的，或者转出来的gif动图是有水印的，就很无语！其实，通过Python几行代码就可以搞定视频转gif

# 安装moviepy库
命令行输入（注：pip 已内置于 Python 3.4 和 2.7 及以上版本，其他版本需另行安装）
> pip install moviepy

这里提一句，请先联网再安装moviepy库!

使用镜像可以让你安装的更快！
> pip install moviepy -i https://pypi.tuna.tsinghua.edu.cn/simple

常用的Python镜像有
> 百度镜像：https://mirror.baidu.com/pypi/simple
清华镜像：https://pypi.tuna.tsinghua.edu.cn/simple

# 写入代码
```Python
from moviepy.editor import *
video_path = input('请输入视频路径：')
video = VideoFileClip(video_path)
video.write_gif("gif_file.gif", fps=20)
# 设置转换后的gif的帧率为20
```

# 转换效果

![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/gif_file.gif)


# gif文件大的解决方案
gif文件一般都很大。22秒的gif动图，就足足有49M的大小！如果需要转换的视频有一分钟，那么gif文件就有100M之多甚至更多！
![](https://ahaoblog.oss-cn-guangzhou.aliyuncs.com/img/gifbig.png)

那我们该如何解决呢?

`设置帧率、截取视频长度再转换`这两种方式可以减少gif的大小
## 设置帧率
设置 fps 的值，来压缩 gif 的大小
```Python
clip.write_gif("gif_file.gif", fps=15)
# 设置转换后的gif的帧率为15
```

## 截取视频长度再转换
我们还可以通过设置 subclip 参数来指定转换的视频范围。 subclip：截取原视频中的自 t_start 至 t_end 间的视频片段

|时间格式|表示方式|示例|
|:-:|:-:|:-:|
|秒|xx.xx的形式|11.02 代表11.02秒|
|分,秒|xx,xx的形式|10,35 代表10分35秒|
|时,分,秒|xx,xx,xx的形式|10,35,30 代表10小时35分30秒|

```Python
subclip(2,3) # 选取的是原视频从 2秒 到 3秒
subclip((1.05),(1.08)) #选取的时原视频从 1.05秒 到 1.08秒
subclip((1,5),(1,8)) #选取的是原视频从 1分5秒 到 1分8秒
subclip((1,5,30),(1,5,35)) #选取的是原视频从 1小时5分30秒 到 1小时5分35秒
```
## 示例
截取原视频的5.01秒到6.36秒，并把转换后的 gif 的帧率设为18
```python
from moviepy.editor import *
video_path = input('请输入视频路径：')
video = VideoFileClip(video_path).subclip(5.01,6.36) #选取原视频5.01秒到6.36秒
video.write_gif("gif_file.gif", fps=18) #设置转换后的gif的帧率为18
```

# 其他说明
该程序已经打包为 exe 文件了
有需要的的朋友，也可以通过以下的链接获取 ***视频转 gif 的软件*** 哟！
[点我下载软件](https://wwp.lanzouq.com/iPMZN08j7wtg)

在用pyinstaller打包成exe文件时，会有一个小坑 moviepy 打包报错，我也是参考了大佬的文章才顺利解决问题 [参考文章跳转](https://blog.csdn.net/J_____Q/article/details/120123232)

最后，若有不足或者不当之处，欢迎指正批评！
相关参考文章：https://blog.csdn.net/m0_59163425/article/details/125154457
https://www.bilibili.com/video/BV1oX4y137vh?p=7&share_source=copy_web&vd_source=41db5528fa268b4e34f279c406002a4c

