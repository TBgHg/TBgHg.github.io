# pwn入门的一些学习资料

> 原文链接：https://www.jianshu.com/p/7133863623e6

总结记录一下pwn入门的一些学习资料

pwn入门学习的网站：[CTF Wiki](https://ctf-wiki.github.io/ctf-wiki/)

## **<u>必备技能</u>：**

1. ### **汇编语言**

   要搞pwn首先要懂汇编吧，毕竟是搞二进制的
   建议看王爽写的那本汇编语言，将书本的实验做一遍，然后再了解一下AT&T和intel两种汇编代码风格有什么不同
   可以看下这篇博客[intel汇编 和 AT&T汇编 的区别](https://blog.csdn.net/kennyrose/article/details/7575952)

2. ### **编程语言（C语言和python）**

   C语言这门语言必须要会吧，题目大部分是用C语言写的，而且很多那些知识都要读libc的源代码，不懂C，读libc源码时就会很痛苦，虽然懂也会很痛苦
   要会python吧，毕竟exp用python写的，CTF中pwn主要是用pwntools这个python库来写利用脚本,要熟悉这个库
   学习python的话上菜鸟教程学吧
   pwntools库的话看下面
   这个库的document [pwntools document](http://pwntools.readthedocs.io/en/stable/about.html)
   还有常见的用法 [pwntools 的常见用法](http://www.91ri.org/14382.html)

3. ### **计算机组成与原理**

   玩pwn，入门是栈溢出，要玩栈溢出，栈的构造要了解吧，函数的调用约定要了解吧，函数参数的在栈上的分布要知道吧，然后balbala很多知识点要刷，建议读下《深入理解计算机系统》这本书 还有（《程序员的自我修养》这个选读）
   下面举下经常用到的知识
   函数调用栈：
   [C语言函数调用栈(一)](https://www.cnblogs.com/clover-toeic/p/3755401.html)
   [C语言函数调用栈(二)](https://www.cnblogs.com/clover-toeic/p/3756668.html)



elf的文件结构：了解下就好了
看程序员的自我修养

PLT和GOT：[Linux中的GOT和PLT到底是个啥？](http://www.freebuf.com/articles/system/135685.html)

系统的防护机制了解一波：[系统防护机制](https://blog.betamao.me/2017/09/06/系统保护/)

- IDA 和gdb的常见工具的操作
- linux的常见操作

pwn入门视频：可以关注一下B栈上的一个up主 (君莫笑hhhhhhhh)
下面是他的一些视频

- [环境搭建](https://www.bilibili.com/video/av14550200) （环境的搭建 建议初学者用peda peda的安装我另一篇[博客](https://www.jianshu.com/p/1476f38e3aa3)有讲）
- [逆向基础速成](https://www.bilibili.com/video/av15099229)
- [pwn入门系列-1-pwn基础知识](https://www.bilibili.com/video/av14821631)
- [pwn入门系列-2-一个简单的例子](https://www.bilibili.com/video/av14824239)

可以看下国外的 liveOverflow系列的教程：[LiveOverflow\]-binary hacking](https://www.bilibili.com/video/av18860370?from=search&seid=11125347866510739031)
台湾 Angelboy的教学视频 强烈推荐：[视频地址](https://www.youtube.com/channel/UC_PU5Tk6AkDnhQgl5gARObA)

pwn入门博客

- [手把手教你栈溢出从入门到放弃（上）](https://paper.seebug.org/271/)
- [手把手教你栈溢出从入门到放弃（下）](https://paper.seebug.org/272/)
- [一步一步学ROP之linux_x86篇 – 蒸米](http://www.vuln.cn/6645)
- [一步一步学ROP之linux_x64篇 – 蒸米](http://www.vuln.cn/6644)
- [一步一步学ROP之gadgets和2free篇 – 蒸米](http://www.vuln.cn/6643)

刷题的网站
浙大的[jarvisoj平台](https://www.jarvisoj.com/)
国外的[pwnable.kr](http://www.pwnable.kr/)
台湾的[pwnable.tw](https://pwnable.tw/)
i春秋平台上的题目
还有一个国外的[Scoreboard](https://hackme.inndy.tw/scoreboard/) 不过需要科学上网

先大概写这么多吧，以后再补充
