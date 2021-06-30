> 今天当我再次进行python打包时又一次地愣住了，又一次地打开了百度……，这时我大概明白该写篇博客记录一下了。

[转载自此文章，只做了一些部分修改](https://my.oschina.net/u/4350311/blog/3311195)

[TOC]

## Pyinstaller

pyinstaller是python的一个第三方模块，使用它可以将python程序打包为可执行文件，实现打包后的程序在没有python环境的机器上也可以运行。

pyinstaller安装：

`pip installer pyinstaller`

通常我们打包程序时，会遇到两种情况：

- 打包一个python脚本文件

- 打包一个python项目程序

## 打包一个py脚本程序

对于只有一个python脚本的简单程序，打包操作很方便，直接使用命令行的方式，输入相关指令即可。

对于一个摄氏温度转华氏温度的小程序（temp.py），我们可以这样做：

```python
# 摄氏温度转华氏温度

temp = input("请输入摄氏温度：")
new_temp = 9/5 * int(temp) + 32
print(f"华氏温度为：{new_temp}F")
q = input("按任意键退出：")
```

首先，打开终端cmd, 进入temp.py文件所在的路径，输入指令：`pyinstaller -F temp.py`

打包结束后，将在当前目录下生成两个文件夹（bulid、dist）和一个文件`temp.spec`，现在不需要理会文件夹`bulid`和文件`temp.spec`

我们需要的打包后的可执行文件在文件夹`dist`中，双击即可运行，实现打包。

补充：如果想修改**可执行文件的图标**，使用指令：`pyinstaller -i icon.ico -F temp.py`

打包结束后，在dist文件夹下降出现temp.exe。你可能会发现它的图标并不是你想要的，这没有关系，你将它重命名或者拷贝到其他地方，你会发现它的图标立刻变成你期待的样子，祝你好运。

## 打包一个py项目程序

对于常用到的py项目程序，包含许多文件夹和py文件，以及配套的资源文件。这种情况下在终端中使用指令的方式打包程序本身也是可以实现的，但是此时打包操作就变得非常复杂，它需要你理解不同指令参数的确切意思，时不时你将入坑爬不起来，苦不堪言。 这段时间使用pygame写了一个像素鸟的游戏，想分享给别人体验，就使用了pyinstaller将程序打包成exe文件。这里分享给大家，希望对你能有所帮助。

这种情况下，一个简单的打包方式，就是通过pyinstaller提供的`spec文件`实现程序打包。

下面通过一个基于`pygame`实现的`FlappyBird`介绍该项目的打包流程。

该项目包含六个文件夹，其中：`bin、conf、core`包含所有的python脚本文件，项目入口程序在`bin\setup.py`，所有音频文件在audios文件夹下，所有的字体文件在fonts文件夹下，所有的图片文件在images文件夹下。

**第一步**：打开终端进入FlappyBird路径下，输入指令：`pyinstaller -F bin\setup.py`，回车，程序结束后，发现当前目录下生成两个文件夹（bulid、dist）和一个文件`setup.spec`，现在删除两个文件夹，只保留`setup.spec`文件。

**setup.spec**

```python
# -*- mode: python ; coding: utf-8 -*-

block_cipher = None


a = Analysis(['bin\\setup.py'],	# 此列表存放项目设计的所有python脚本文件
             pathex=['C:\\Users\\15057\\Desktop\\FlappyBird'], # 此列表为项目绝对路径
             binaries=[],
             datas=[],		# 此列表存放所有资源文件，每个文件是一个2元组元素
             hiddenimports=[],
             hookspath=[],
             runtime_hooks=[],
             excludes=[],
             win_no_prefer_redirects=False,
             win_private_assemblies=False,
             cipher=block_cipher,
             noarchive=False)
pyz = PYZ(a.pure, a.zipped_data,
             cipher=block_cipher)
exe = EXE(pyz,
          a.scripts,
          a.binaries,
          a.zipfiles,
          a.datas,
          [],
          name='setup',		# 打包程序的名字
          debug=False,
          bootloader_ignore_signals=False,
          strip=False,
          upx=True,
          upx_exclude=[],
          runtime_tmpdir=None,
          console=True )	
# 此处console=True表示，打包后的可执行文件双击运行时屏幕会出现一个cmd窗口，不影响原程序运行
# 如果想要修改程序图标，使用在EXE()中加入 icon='xxxxx', 切记：绝对路径
```

**第二步**：修改`setup.spec`文件，修改后的文件如下:

修改位置：

- 添加py_files列表，包含项目需要的所有python脚本
- 添加add_files列表，包含涉及到的所有资源文件，每个文件是2元组的形式存放
- name='FlappBird', 制定可执行程序名字
- console=False， 制定可执行程序执行时不显示控制台窗口
- icon='C:\Users\15057\Desktop\FlappyBird\images\flappy.ico'， 设置程序图标，ico格式文件（16*16）

```python
# -*- mode: python ; coding: utf-8 -*-

block_cipher = None

py_files = [
    'bin\\setup.py',
    'conf\\settings.py',
    'core\\main.py',
    'core\\base.py',
    'core\\bird.py',
    'core\\pipe.py',
    'core\\score.py',
]

add_files = [
    ('fonts\\font.ttf', 'fonts'),
    ('images\\*.png', 'images'),
    ('images\\flappy.ico', 'images'),
    ('audios\\*.wav', 'audios'),
]

a = Analysis(py_files,
             pathex=['C:\\Users\\15057\\Desktop\\FlappyBird'],
             binaries=[],
             datas=add_files,
             hiddenimports=[],
             hookspath=[],
             runtime_hooks=[],
             excludes=[],
             win_no_prefer_redirects=False,
             win_private_assemblies=False,
             cipher=block_cipher,
             noarchive=False)
pyz = PYZ(a.pure, a.zipped_data,
             cipher=block_cipher)
exe = EXE(pyz,
          a.scripts,
          a.binaries,
          a.zipfiles,
          a.datas,
          [],
          name='FlappBird',
          debug=False,
          bootloader_ignore_signals=False,
          strip=False,
          upx=True,
          upx_exclude=[],
          runtime_tmpdir=None,
          console=False,
          icon='C:\\Users\\15057\\Desktop\\FlappyBird\\images\\flappy.ico' )
```

**第三步**：执行`setup.spec`文件。项目路径下输入指令：`pyinstaller -F setup.spec`，

回车，程序结束后，新增`dist`文件夹，且该文件夹下新增`FlappyBird.exe`，最终，打包结束。



## 补充

打包，可以使用许多指令，指令都差不多，不同的是使用不用的参数，如 `-F`、`-i`等，不同的参数有不同的意义。这些可以在网上找到相关解释说明，这里就赘述了。

值得说明的一点是，使用`spec`文件打包程序时，使用`-F`或者使用`-w`，生成的spec文件内容有一点点不同。

这里大家注意即可，因为使用`-F`打包时默认只生成一个单独的可执行文件，如这里的`FlappyBird.exe`； 而使用`-w`打包时会生成一个文件夹，该文件夹里面包含一些库文件和`FlappyBird.exe`，这里的exe需要依赖这些库文件，即资源文件。在生成的spec文件中，会多一点内容。但是基本不影响打包流程和打包思路。

## 笔者补充

### 去除不必要的模块import

有时需要让pyinstaller不打包某些用不到的模块，可通过在excludes=[]中添加此模块实现，如

```python
excludes=['zmq']
```

### 常用参数及含义

| 参数 | 命令                                     | 含义                                                         |
| ---- | ---------------------------------------- | ------------------------------------------------------------ |
| -D   | pyinstaller -D demo.py                   | 默认选项。除了主程序demo.exe外，还会在在dist文件夹中生成很多依赖文件，推荐使用。 |
| -c   | pyinstaller -c demo.py                   | 默认选项。使用控制台，只对windows有效。                      |
| -F   | pyinstaller -F demo.py                   | 只在dist文件夹中生成一个程序demo.exe文件，适用于没有多依赖.py文件的单个文件。 |
| -w   | pyinstaller -w demo.py                   | 不使用控制台，只对windows有效。                              |
| -p   | pyinstaller -p D:\project\demo.py        | 设置导入路径。                                               |
| -i   | pyinstaller -i D:\icons\demo.ico demo.py | 给生成的demo.exe文件设置一个自定义的图标。                   |

