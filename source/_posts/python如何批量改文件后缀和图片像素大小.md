---
title: python如何批量改文件后缀和图片像素大小
date: 2021-12-21 22:05:20
categories: python进阶


---

hello 大家好，我是Monday，就在我写完pandas学习案例100第一篇的时候，在图片素材里找封面图时，发现已经没有新的封面图了

都是使用过的了，于是乎，我从网上下载了一批新的图片素材，正当我满意的时候，上传时，发现



<!--more-->

<img src="./python如何批量改文件后缀和图片像素大小/2.png" style="zoom: 50%;" />

我下载的图片都是webp后缀的，格式不被允许上传，这时候我们改文件的后缀为平台支持的格式就可以了，

重点来了，那么图片难道要一个一个手动改吗，No No No，会Python的你，当然应该是写个小工具批量改了

附带源码：

```python
import os

# 列出当前目录下所有的文件
path = r"C:\Users\xx\Desktop\公众号素材"
files = os.listdir(path)
for index, filename in enumerate(files):
    portion = os.path.splitext(filename)
    # 判断后缀
    if portion[1] == ".webp":
        # 重新组合文件名和后缀名
        newname = path + "\\" + str(index) + ".jpg"
        filename = path + "\\" + filename
        os.rename(filename, newname)
```

看下效果：



<img src="./python如何批量改文件后缀和图片像素大小/3.jpg" style="zoom: 50%;" />



本以为完美的落幕，然并卵再次上传，发现如下图的问题

<img src="./python如何批量改文件后缀和图片像素大小/0.jpg" style="zoom: 50%;" />



继续盘他，写个脚本批量更改像素:

源码如下：

```python
import os
from PIL import Image

IPHONE5_WIDTH = 530 #改变后的宽
IPHONE5_HEIGHT = 300#改变后的高


# 改变图片的尺寸大小
def reset_pic_size(file_path, new_path, width=IPHONE5_WIDTH, height=IPHONE5_HEIGHT):
    image = Image.open(file_path)
    image_width, image_height = image.size

    if image_width > width:
        image_height = width * image_height // image_width
        image_width = width
    if image_height > height:
        image_width = height * image_width // image_height
        image_height = height

    new_image = image.resize((image_width, image_height), Image.ANTIALIAS)  # 滤镜缩放
    new_image.save(new_path)


# 从文件夹中循环改变每一张图片
def find_and_resize_pic_from_dir(dir_path, new_path):
    for root, dirs, files in os.walk(dir_path):
        for file_name in files:
            if file_name.lower().endswith('jpg') or file_name.lower().endswith('png'):
                file_path = os.path.join(root, file_name)
                new_file_path = new_path + file_name
                reset_pic_size(file_path=file_path, new_path=new_file_path)

if __name__ == '__main__':
    
    file_new_path = r'C:\Users\XX\Desktop\公众号素材\new_'
    find_and_resize_pic_from_dir(r'C:\Users\XX\Desktop\公众号素材', file_new_path)
```



万事具备，看看能不能如愿上传：

<img src="./python如何批量改文件后缀和图片像素大小/1.png" style="zoom: 50%;" />

完美