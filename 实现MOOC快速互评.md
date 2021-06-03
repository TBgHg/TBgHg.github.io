## 实现MOOC快速互评

MOOC互评总是一件令人头疼的事，尤其是工数这种二十多道题需要评5份以上，笔者饱受互评之苦久矣，于近期用C语言写了个小小的脚本，可供参考。

在MOOC互评界面，当我们按tab键时光标会依次从打分选项跳到评语再跳到打分选项那种，并且我们可以惊奇的发现，当我们在打分选项使用←键时就会从零分跳到最高分，在这两个前提下，我们就可以操作了。
依次执行：

> 按下←键
> 抬起←键
> 
> 按下 ctrl 
> 按下v键
> 
> 抬起v键
> 抬起 ctrl 

这时我们就执行完了一个模块了：

> 如果是评语，←键对他无影响，相当于只是一个ctrl+v
> 
> 如果是打分模块，我们←键就会让他从0到最高分，ctrl+v对他无影响。

在执行完这一套之后我们再执行 按下tab键 抬起tab键，并一直循环这些就可以实现自动互评了。

代码如下，读者可自行探索

```c
#include<stdio.h>
#include<windows.h>
int main()
{
	int a;
	system("pause");	//暂停一下留有准备时间
	while(1)	//一直执行以下代码
	{//keybd_event函数的用法见下面
		keybd_event(37,0,0,0);			//按下←键
		keybd_event(37,0,KEYEVENTF_KEYUP,0);	//抬起←键
		keybd_event(17,0,0,0);			//按下ctrl
		keybd_event(86,0,0,0);			//按下v键
		keybd_event(17,0,KEYEVENTF_KEYUP,0);	//抬起v键 
		keybd_event(86,0,KEYEVENTF_KEYUP,0);	//抬起ctrl 
		keybd_event(9,0,0,0);			//按下tab键
		keybd_event(9,0,KEYEVENTF_KEYUP,0); 	//抬起tab键
		Sleep(300);	//让程序休眠0.3秒，怕电脑跟不上
	//上面这个已经没问题了，只不过我们评完之后还会继续运行，需要再关闭
	//那我们应该怎么做才能让他停下来的更方便些呢？我们可以进行以下操作
		a = GetKeyState(VK_SPACE);			//获取空格键的状态，这个函数自行百度即可
		if(a<0)		//如果空格键被按下
		{
			return 0;	//结束了这个程序
		}
	//就是说现在我们运行完之后只要按下空格就可以退出程序了，大功告成！
	}
	return 0;
}
```

[keybd_event的使用方法 及 常用模拟键的键值对照表](https://blog.csdn.net/tianyuzhixina/article/details/101633101?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-5.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromBaidu%7Edefault-5.control)



