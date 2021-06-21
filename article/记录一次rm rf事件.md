## 记录一次rm -rf事件

### 结果展示

<img src="https://www.picbed.cn/images/2021/06/21/psc.webp" alt="psc.webp" style="zoom: 20%;" />                   <img src="https://www.picbed.cn/images/2021/06/21/psc1.webp" alt="psc1.webp" style="zoom:50%;" />                     <img src="https://www.picbed.cn/images/2021/06/21/psc2.webp" alt="psc2.webp" style="zoom:20%;" />



这是一次较为惨痛的事件，相信大家都知道rm -rf这个命令吧，这是笔者错误执行了某些代码后的结果（桌面十分的干净，删除了包括而不限于我的大英作业、大物仿真作业、科中名词解释作业以及第二天要用到的科中期末大会总结ppt）。

### 事件分析

起因是这样的，笔者打算在虚拟机上安装科学上网工具，在虚拟机中通过sh脚本安装软件，但是笔者发现他的脚本中安装的软件是从外网上面安装的，导致笔者的电脑一直显示安装超时，于是笔者决定先从自己的电脑下载好他要下载的东西，然后修改sh脚本。最终改成了下面的代码（源代码懒得去找了）：

```sh
#!/bin/sh

# Set ARG
PLATFORM=$1
TAG=$2
if [ -z "$PLATFORM" ]; then
    ARCH="64"
else
    case "$PLATFORM" in
        linux/386)
            ARCH="32"
            ;;
        linux/amd64)
            ARCH="64"
            ;;
        linux/arm/v6)
            ARCH="arm32-v6"
            ;;
        linux/arm/v7)
            ARCH="arm32-v7a"
            ;;
        linux/arm64|linux/arm64/v8)
            ARCH="arm64-v8a"
            ;;
        *)
            ARCH=""
            ;;
    esac
fi
[ -z "${ARCH}" ] && echo "Error: Not supported OS Architecture" && exit 1

# Download files
V2RAY_FILE="v2ray-linux-${ARCH}.zip"
DGST_FILE="v2ray-linux-${ARCH}.zip.dgst"


if [ $? -ne 0 ]; then
    echo "Error: Failed to download binary file: ${V2RAY_FILE} ${DGST_FILE}" && exit 1
fi
echo "Download binary file: ${V2RAY_FILE} ${DGST_FILE} completed"

# Check SHA512
LOCAL=$(openssl dgst -sha512 v2ray.zip | sed 's/([^)]*)//g')
STR=$(cat v2ray.zip.dgst | grep 'SHA512' | head -n1)

if [ "${LOCAL}" = "${STR}" ]; then
    echo " Check passed" && rm -fv v2ray.zip.dgst
else
    echo " Check have not passed yet " && exit 1
fi

# Prepare
echo "Prepare to use"
unzip v2ray.zip && chmod +x v2ray v2ctl
mv v2ray v2ctl /usr/bin/
mv geosite.dat geoip.dat /usr/local/share/v2ray/
mv config.json /etc/v2ray/config.json

# Clean
rm -rf ${PWD}/*
echo "Done"
```

笔者直接从主机上下载它要下载的东西，所以需要将代码中对应下载那一行的代码删除，要不还是会出现下载超时的现象，最终改成了这个代码，比较凑巧的是笔者在主机上安装过git bash，于是主机上直接就可以运行这个sh脚本，然后就先在主机上面试了试，结果如上图，桌面变的特别干净。。。

大家应该已经看出这代码有些不对劲了吧，对，rm -rf ${PWD}就这玩意。原来的代码中下载的软件的安装包时顺便将其赋值给了PWD这个变量，最后通过rm -rf ${PWD}来将安装包删除。然而笔者把这一行去了…………

那么对于未赋值的变量它指向什么呢？

```sh
86155@TBH MINGW64 ~/Desktop
$ PWD
/c/Users/86155/Desktop
```

对，就是桌面！

想必大家应该清楚怎么回事了吧…………

### 反思

sh脚本这种东西，执行前还是先大概看看吧，首先拿记事本打开，ctrl+F查找一下rm什么的，看看有没有一些危险的代码，确认无误之后再去执行。

### 后记

笔者痛失大英作业和同学聊天时，同学表示，你跟老师说这些还不如跟老师说你作业被猫撕了可信度高……
