---
typora-root-url: 链表
---

[TOC]















## 链表

### 一.基础知识和概述

​       链表是一种最常见的数据结构，它动态地进行存储分配。链表由指针处理。链表有单向链表、双向链表、环形链表等形式。[回到顶部](#链表)

#### 连接原则 

​                    （1)前一个结点指向下一个结点
​                    （2)只有通过前一个结点オ能找到下一个结点。[回到顶部](#链表)

#### 链表组成

​                    （1)头指针变量head一存放首元素地址，指向链表的首结点。
​					（2)每个结点由两个域组成：
​								数据域一一存储结点本身的信息。（用户有用信息）
​								指针域一一存放下一个结点的地址
​					（3)尾结点的指针域置为“NULL”，作为链表结束的标志。[回到顶部](#链表)

#### 链表描述举例    

​        使用结构体来进行描述，其中成员num和 score用来存放结点中的有用数据（用户需要用到的数据），next是指针类型的成员，它指向 struct student类型数据（这就是next所在的结构体类型)[回到顶部](#链表)

```c++
struct student
 {
 	long num;
 	float score;
 	struct student*next;/*指向下一结点*/
 };
```

### 二.初始第一步：单链表

​        单链表即只能向一个方向的遍历的链表，换句话说，就是只有**一个指针（域）**（指向后面或前面），单链表是最简单的链表。又因为在单链表中，每次读取元素，都要从头结点开始，故单链表是一种非随机存取的存储结构，是链表的最基础部分。[回到顶部](#链表)

#### 定义结点

```c++
typedef struct LNodes
{
    ElemType data;
    struct LNodes *next;
} LNodes,*LinkList;
```

[回到顶部](#链表)

#### 结点初始化

```cc
if (p!=NULL)
{
    p->data=10;
    p->next=NULL;
}
```

[回到顶部](#链表)

#### 建立链表

```
LinkedList LinkedListInit() {  
    Node *L;  
    L = (Node *)malloc(sizeof(Node));   //申请结点空间   
    if(L == NULL) { //判断是否有足够的内存空间   
        printf("申请内存空间失败\n");  
    }  
    L->next = NULL;                  //将next设置为NULL,初始长度为0的单链表   
    return L;  
}  
//单链表的建立1，头插法建立单链表  
LinkedList LinkedListCreatH() {  
    Node *L;  
    L = (Node *)malloc(sizeof(Node));   //申请头结点空间  
    L->next = NULL;                      //初始化一个空链表  
    ElemType x;                         //x为链表数据域中的数据  
    while(scanf("%d",&x) != EOF) {  
        Node *p;  
        p = (Node *)malloc(sizeof(Node));   //申请新的结点   
        p->data = x;                     //结点数据域赋值   
        p->next = L->next;                    //将结点插入到表头L-->|2|-->|1|-->NULL   
        L->next = p;   
    }  
    return L;   
}   
```

[回到顶部](#链表)

##### 1.头插法

从开头开始插入结点，每次插入都在开头。具体实现看代码

[![rz0fx0.png](https://s3.ax1x.com/2021/01/02/rz0fx0.png)](https://imgchr.com/i/rz0fx0)

###### 有头结点

```c++
typedef struct LNode{
    int data;
    struct LNode * next;
}LNode;
typedef LNode* List;
//用头插法创建带有头结点的单链表
LNode * Createhead1(int length)
{
    int data[12] ={0,1,2,3,4,5,6,7,8,9,10,11};  //为了测试方便，自己写了数组来测试，也可以用下面的随机数来创建（此方法来源于CSDN博主，感觉还不错）
    LNode * p;
    //srand(time(0)); // 初始化随机数种子
    if(length < 1)
        return NULL;
    LNode *head  = (LNode*)malloc(sizeof(LNode));
    head->next = NULL;
    int i = 1;
    while(i<=length)
    {
        p = (LNode*)malloc(sizeof(LNode));
        //p->data = rand()%100+1; //随生成100以内的数字
        p->data = data[i];
        p->next = head->next;
        head->next = p;
        i++;
    }
    return head;
}
```

[回到顶部](#链表)

###### 无头结点

```cc
//头插法无头结点创建
LNode *Createhead2(int length)
{
    int data[12] ={0,1,2,3,4,5,6,7,8,9,10,11};
    LNode*p;
    //srand(time(0));
    if(length<1)
        return NULL;
    LNode *first = (LNode*)malloc(sizeof(LNode));
    first->next = NULL;
    first->data = data[0];
    int i = 1;
    while(i<length)
    {
        p = (LNode*)malloc(sizeof(LNode));
        //p->data = rand()%100+1;
        p->data = data[i];
        p->next = first;
        first = p;
        i++;
    }
    return first;
}
```

[回到顶部](#链表)

##### 2.中间插法

<img src="https://s3.ax1x.com/2021/01/02/rz04MV.md.png" alt="rz04MV.md.png" style="zoom: 67%;" />

结点在插入过程中存在以下几种情况：
    （1）如果原表是空表，只需使链表的头指针`head`指向被插结点即可。
    （2）如果如果被插结点值最小，则应插入第一个结点之前，这种情况下使头指针`head`指向被插结点，被插结点的指针域指向原来的第一结点即可。
    （3）如果在链表中某位置插入，使插入位置的前一结点的指针域指向被插结点，被插结点的指针域指向插入位置的后一结点即可。
    （4）如果被插结点值最大，则在表尾插入，使原表尾结点指针域指向被插结点，被插结点指针域指向`NULL`即可。[回到顶部](#链表)

```c++
node * insert(node * head, int x)
{
     node * last, * current, * p;
     //要插入的结点
     p = (node *)malloc(sizeof(node));
     p->num = x;
     //空表插入
     if(head == NULL)
     {
         head = p;
         p->next = NULL;
         return head;
     }
     //找插入位置
     current = head;
     while(x > current->num && current->next != NULL)
     {
          last = current;
          current = current->next;
     }
     if(x <= current->num)
     {
         if(head == current)//在第一结点之前插入
         {
             p->next = head;
             head = p;
             return head;
         }
         else//中间位置插入
         {
             p->next = current;
             last->next = p;
             return head;
         }
     }
     else//链尾插入
     {
         current->next = p;
         p->next = NULL;
         return head;
     }
}
```

[回到顶部](#链表)

##### 3.尾插法

<img src="https://s3.ax1x.com/2021/01/02/rz0HIJ.png" alt="rz0HIJ.png" style="zoom:67%;" />

在尾部逐次插入结点的方法，具体看代码

[回到顶部](#链表)

###### 带头结点的尾插法

```c++
//尾插法带头结点创建

LNode*Createrail1(int length)
{
    int data[12] ={0,1,2,3,4,5,6,7,8,9,10,11};
    LNode *p;
    //srand(time(0));
    if(length<1)
        return NULL;
    LNode *head =(LNode*)malloc(sizeof(LNode));
    head->next = NULL;
    LNode *temp = head;
    int i = 1;
    while(i<=length)
    {
        p = (LNode*)malloc(sizeof(LNode));
        //p->data = rand()%100+1;
        p->data = data[i];
        temp->next = p;
        temp = p;
        i++;
    }
    temp->next = NULL;
    return head;
}
```

[回到顶部](#链表)

###### 不带头结点的尾插法

```c++
//尾插法无头结点创建

LNode * Createrail2(int length)
{
    int data[12] ={0,1,2,3,4,5,6,7,8,9,10,11};
    LNode *p;
    //srand(time(0));
    if(length<1)
        return NULL;
    LNode *first = (LNode*)malloc(sizeof(LNode));
    first->next = NULL;
    first->data = data[0];
    LNode * temp = first;
    int i =1;
    while(i<length)
    {
        p = (LNode*)malloc(sizeof(LNode));
        //p->data = rand()%100+1;
        p->data = data[i];
        temp->next =p;
        temp = p;
        i++;
    }
    temp->next = NULL;
    return first;
}
```

[回到顶部](#链表)

##### 4.小结

​       从上面的三幅流程图中可以看出，虽然新元素的插入位置不同，但实现插入操作的方法是一致的，都是先执行步骤 1 ，再执行步骤 2。

​       相比较而言，尾插法比头插法简单一些，头插法需要不断地更改头定位指针的位置，而尾插法则不需要这一点，但是具体情况还需具体分析。

​       **注意**：链表插入元素的操作必须是先步骤 1，再步骤 2；反之，若先执行步骤 2，除非再添加一个指针，作为插入位置后续链表的头指针，否则会导致插入位置后的这部分链表丢失，无法再实现步骤 1[回到顶部](#链表)

#### 链表的结点删除

​        从链表中删除指定数据元素时，实则就是将存有该数据元素的节点从链表中摘除，并且要对不再利用的存储空间要及时释放。因此，从链表中删除数据元素需要进行以下 2 步操作：

1.    将结点从链表中摘下来;
2.    手动释放掉结点，回收被结点占用的存储空间;

其中，从链表上摘除某节点的实现非常简单，只需找到该节点的直接前驱节点 temp，执行一行程序：[回到顶部](#链表)

```c++
temp->next=temp->next->next;
```

例如如下图中所示：

![rzwxEQ.gif](https://s3.ax1x.com/2021/01/02/rzwxEQ.gif)

[回到顶部](#链表)

```c++
//p为原链表，add为要删除元素的值
link * delElem(link * p, int add) {
    link * temp = p;
    //遍历到被删除结点的上一个结点
    for (int i = 1; i < add; i++) {
        temp = temp->next;
        if (temp->next == NULL) {
            printf("没有该结点\n");
            return p;
        }
    }
    link * del = temp->next;//单独设置一个指针指向被删除结点，以防丢失
    temp->next = temp->next->next;//删除某个结点的方法就是更改前一个结点的指针域
    free(del);//手动释放该结点，防止内存泄漏
    return p;
}
```

**特别注意**：对于结点的删除，一定要注意首先要让被删除结点的上一个结点先与被删除结点的下一个结点建立联系，再进行被删除结点的free，否则会出现链表的断裂，找不到next的情况。[回到顶部](#链表)

#### 链表的元素更新

​        更新链表中的元素，只需通过遍历找到存储此元素的节点，对节点中的数据域做更改操作即可。[回到顶部](#链表)

```c++
//更新函数，其中，add 表示更改结点在链表中的位置，newElem 为新的数据域的值
link *amendElem(link * p,int add,int newElem){
    link * temp=p;
    temp=temp->next;//在遍历之前，temp指向首元结点
    //遍历到待更新结点
    for (int i=1; i<add; i++) {
        temp=temp->next;
    }
    temp->elem=newElem;
    return p;
}
```

[回到顶部](#链表)

#### 总结代码

下面给出总结性的代码

```c++
#include <stdio.h>
#include <stdlib.h>
typedef struct Link {
    int  elem;
    struct Link *next;
}link;
link * initLink();
//链表插入的函数，p是链表，elem是插入的结点的数据域，add是插入的位置
link * insertElem(link * p, int elem, int add);
//删除结点的函数，p代表操作链表，add代表删除节点的位置
link * delElem(link * p, int add);
//查找结点的函数，elem为目标结点的数据域的值
int selectElem(link * p, int elem);
//更新结点的函数，newElem为新的数据域的值
link *amendElem(link * p, int add, int newElem);
void display(link *p);
int main() {
    //初始化链表（1，2，3，4）
    printf("初始化链表为：\n");
    link *p = initLink();
    display(p);
    printf("在第4的位置插入元素5：\n");
    p = insertElem(p, 5, 4);
    display(p);
    printf("删除元素3:\n");
    p = delElem(p, 3);
    display(p);
    printf("查找元素2的位置为：\n");
    int address = selectElem(p, 2);
    if (address == -1) {
        printf("没有该元素");
    }
    else {
        printf("元素2的位置为：%d\n", address);
    }
    printf("更改第3的位置上的数据为7:\n");
    p = amendElem(p, 3, 7);
    display(p);
    return 0;
}
link * initLink() {
    link * p = (link*)malloc(sizeof(link));//创建一个头结点
    link * temp = p;//声明一个指针指向头结点，用于遍历链表
    //生成链表
    for (int i = 1; i < 5; i++) {
        link *a = (link*)malloc(sizeof(link));
        a->elem = i;
        a->next = NULL;
        temp->next = a;
        temp = temp->next;
    }
    return p;
}
link * insertElem(link * p, int elem, int add) {
    link * temp = p;//创建临时结点temp
    //首先找到要插入位置的上一个结点
    for (int i = 1; i < add; i++) {
        temp = temp->next;
        if (temp == NULL) {
            printf("插入位置无效\n");
            return p;
        }
    }
    //创建插入结点c
    link * c = (link*)malloc(sizeof(link));
    c->elem = elem;
    //向链表中插入结点
    c->next = temp->next;
    temp->next = c;
    return  p;
}
link * delElem(link * p, int add) {
    link * temp = p;
    //遍历到被删除结点的上一个结点
    for (int i = 1; i < add; i++) {
        temp = temp->next;
        if (temp->next == NULL) {
            printf("没有该结点\n");
            return p;
        }
    }
    link * del = temp->next;//单独设置一个指针指向被删除结点，以防丢失
    temp->next = temp->next->next;//删除某个结点的方法就是更改前一个结点的指针域
    free(del);//手动释放该结点，防止内存泄漏
    return p;
}
int selectElem(link * p, int elem) {
    link * t = p;
    int i = 1;
    while (t->next) {
        t = t->next;
        if (t->elem == elem) {
            return i;
        }
        i++;
    }
    return -1;
}
link *amendElem(link * p, int add, int newElem) {
    link * temp = p;
    temp = temp->next;//tamp指向首元结点
    //temp指向被删除结点
    for (int i = 1; i < add; i++) {
        temp = temp->next;
    }
    temp->elem = newElem;
    return p;
}
void display(link *p) {
    link* temp = p;//将temp指针重新指向头结点
    //只要temp指针指向的结点的next不是Null，就执行输出语句。
    while (temp->next) {
        temp = temp->next;
        printf("%d ", temp->elem);
    }
    printf("\n");
}
```

[回到顶部](#链表)

### 三.开始进阶，循环单链表

​        与单链表唯一的区别在于，最后一个结点的后继指向头结点，但就这一个区别，就会造成后后面有很多地方与单链表不同。下面给出一些不同之处的改变例子。

​       **注**：此单元中还包含一些上单元没有详细提到的小方法，例如求长度之类的操作。（带*）[回到顶部](#链表)

#### 1.初始化循环单链表

```c++
void InitList(DLinkList *&L)
{
	L=(DLinkList *)malloc(sizeof(DLinkList));//创建一个空的头结点
	L->next=L;
}
```

[回到顶部](#链表)

#### 2.运用尾插法对循环单链表进行建立

```c++
//创建新的循环单链表
void CreateList(DLinkList *&L)
{
	DLinkList *p,*s;
	ElemType i;
	L=(DLinkList *)malloc(sizeof(DLinkList));
	p=L;
	p->next=L;
	while(1)
	{
		scanf("%c",&i);
		if(i=='z')
			break;
		s=(DLinkList *)malloc(sizeof(DLinkList));
		s->data=i;
		s->next=p->next;
		p->next=s;
		p=s;
	}
}
```

[回到顶部](#链表)

#### 3.输出循环单链表

```c++

//输出循环单链表
void DispList(DLinkList *L)
{
	DLinkList *p;
	p=L->next;
	while(p!=L)
	{
		printf("%c",p->data);
		p=p->next;
	}
	printf("\n");
}
```

[回到顶部](#链表)

#### 4.插入数据元素在循环单链表中

```c++

//插入数据元素
bool ListInsert(DLinkList *&L,int i,ElemType e)
{
	int j=1;
	DLinkList *p=L->next,*s;
 
	while(p!=L&&j<i-1)//寻找第i-1个节点
	{
		p=p->next;
		j++;
	}	
	if(i==1)
	{
		s=(DLinkList *)malloc(sizeof(DLinkList));
		s->data=e;
		s->next=p;
		L->next=s;
		return true;
	}
	else	if(j!=i-1)
				return false;
			else
			{
				s=(DLinkList *)malloc(sizeof(DLinkList));
				s->data=e;
				s->next=p->next;
				p->next=s;
				return true;
			}
	
}
```

[回到顶部](#链表)

#### 5.寻找单链表中某个位置的元素值

```c++

bool GetElem(DLinkList *L,int i,ElemType &e)
{
	int j=1;
	DLinkList *p=L->next;
	while(j<i&&p!=L)
	{
		p=p->next;
		j++;
	
	}
	if(j!=i)
		return false;
	else
	{
		e=p->data;
		return true;
	}
 
}
```

[回到顶部](#链表)

#### 6.按元素值查找该元素在该单链表中的位置

```c++
//按元素值查找
int locateElem(DLinkList *L,ElemType e)
{
	int i=1;
	DLinkList *p=L->next;
	while(p!=L&&p->data!=e)
	{
		i++;
		p=p->next;
	}
	if(p==L)
		return 0;
	else
		return i;
 
 
}
```

[回到顶部](#链表)

#### 7.删除循环单链表中某个元素值

```c++
bool ListDelete(DLinkList *&L,int i,ElemType &e)
{
	int j=1;
	DLinkList *p=L->next,*q;
	while(j<i-1&&p!=L)
	{
		j++;
		p=p->next;
	}
	if(i==1)
	{
		q=L->next;
		if(q==L)
			return false;
		e=q->data;
		L->next=q->next;
		free(q);
		return true;
	
	}else if(j!=i-1)
			return false;
		  else
		  {
			q=p->next;
			if(q==L)
				return false;
			e=q->data;
			p->next=q->next;
			free(q);
			return true;
		  }
}
```

[回到顶部](#链表)

#### 8.销毁循环单链表

```c++
void DestroyList(DLinkList *&L)
{
	DLinkList *pre=L,*p=L->next;
	while(p!=L)
	{
		free(pre);
		pre=p;
		p=pre->next;
	}
	free(pre);
}
```

[回到顶部](#链表)

#### *.判断单链表是否为空

```c++
bool ListEmpty(DLinkList *L)
{
	return (L->next==L);
 
}
```

[回到顶部](#链表)

#### *.求单链表的长度

```c++
int ListLength(DLinkList *L)
{
	int j=0;
	DLinkList *p=L->next;
	while(p!=L)
	{
		j++;
		p=p->next;
	
	}
	return j;
}
```

[回到顶部](#链表)

### 四.双向链表

​       双向链表跟单向链表的区别就是多了一个向前的指针域。

双向链表的结构如下，有两个指针域：[回到顶部](#链表)

```c++

typedef struct _list
{
	struct _list *prev;
	struct _list *next;
	int data;
}list;
```

​       双向链表是没有头结点的，有一个需要注意的点，第一个节点的prev指针要指向NULL，最后一个节点的next要指向NULL.[回到顶部](#链表)

[![rzwzNj.png](https://s3.ax1x.com/2021/01/02/rzwzNj.png)](https://imgchr.com/i/rzwzNj)

[回到顶部](#链表)

#### 1.创建结点

```c++
list *Create_List_Node(int data)
{
	list *pNode = (list *)malloc(sizeof(list));
	pNode->data = data;
	pNode->prev = NULL;
	pNode->next = NULL;
	
	return pNode;
}
```

[回到顶部](#链表)

#### 2.插入结点

#####        1)、尾插法

​        尾插法很简单，只要遍历到末尾，把末尾的指针的next指向新的节点，然后把新的节点的prev指向末尾节点即可。就像前面提到的，比头插法方便。

```c++
list *Add_List_Node_Tail(list *pHead, list *pNode)
{
	if (pHead == NULL || pNode == NULL)
	{
		printf("pHead is null or pNode is null\n");
		return NULL;
	}
	
	while (pHead->next != NULL)
	{
		pHead = pHead->next;
	}
	
	pHead->next = pNode;    //末尾节点的next指向新节点
	pNode->prev = pHead;    //新节点的prev直线末尾节点
	
	return pHead;
}
```

#####         [回到顶部](#链表)

#####        2)、头插法

​        前面没有详细说明具体的做法，但是发现这一步似乎不能缺少，于是在这里介绍。这里给出一个较为详细的做法。这里的头插法是指在第一个节点后面插入新的节点。头插法一般是需要4个步骤来完成节点插入，步骤是有一定顺序的，如果顺序出错是没办法正常插入的。下面来具体说一下要怎么确定这个顺序

![rz09Cn.png](https://s3.ax1x.com/2021/01/02/rz09Cn.png)

​		首先需要注意的是，上面的第一个节点是pHead这个只是习惯命名，你完全可以用其他名字来代替。上面的顺序只是其中一种下面还会介绍另外一种

​		首先是要先把Node1的prev指针域指向新节点pNode，即Node1->prev = pNode。但是我们是不知道Node1的，我们只能通过pHead来知道Node1，也就是pHead->next = Node1，那么就有pHead->next->prev = pNode;

​		第二，我们需要用pNode的next来指向Node1，也就是pNode->next = Node1;同理我们是不知道Node1的，但是我们知道pHead->next = Node1;所以我们就会有pNode->next = pHead->next;

​		第三，直接就是pHead->next = pNode;

​		第四，直接就是pNode->prev = pHead;[回到顶部](#链表)

这里再介绍另外一种方式：

![rz0C3q.png](https://s3.ax1x.com/2021/01/02/rz0C3q.png)

​	    第一， 直接pNode->prev = pHead;

​		第二，pNode的next指向Node1，即pNode->next = Node1，但是我们不知道Node1，可是我们有pHead->next = Node1，所以我们就有pNode->next = pHead->next;

​		第三，直接有pHead->next = pNode;

​		第四，Node1的prev要指向pNode，即Node1->prev = pNode;但是我们 不知道Node1，可是我们有pHead->next = Node1;所以我们有pHead->next->prev = pNode;

以上的方法看着很麻烦，这里我找到了CSDN一位博主的办法，感觉不错，以下是其内容以及博主给出的一部分代码：

​        *“pHead->next如果做了左值，那么pHead->next就不能在后面作为右值了”* [回到顶部](#链表)

```c++
 
//方法一
pHead->next->prev = pNode;
pNode->next       = pHead->next;
pHead->next       = pNode;
pNode->prev       = pHead;
 
 
//方法二
pNode->prev       = pHead;
pNode->next       = pHead->next;
pHead->next       = pNode;
pHead->next->prev = pNode;
 
 
//错误的方法，此方法会进入死循环
pHead->next       = pNode;          //pHead->next作为左值，后面不能再做右值了
pNode->next       = pHead->next;    //这里pHead->next做右值，出错了
pHead->next->prev = pNode;
pNode->prev       = pHead;
 
 
//错误的方法二，此方法会进入死循环
pHead->next       = pNode;        //pHead->next作为左值，后面不能再做右值了
pNode->prev       = pHead;
pHead->next->prev = pNode;
pNode->next       = pHead->next;   //这里pHead->next做右值，出错了
```

[回到顶部](#链表)

### 五.链表的遍历

我们建立链表的目的就在于查看内容，这里我们单独给出方法：

步骤：

1、得到链表第一个节点的地址，即head的值；

2、设一个临时指针变量p_mov，指向第一个节点head,即可获取 p_mov所指节点的信息；

3、使p_mov后移一个节点,即可访问下一节点,直到链表的尾节 点(注意结尾判断条件)。

![rz0S4s.png](https://s3.ax1x.com/2021/01/02/rz0S4s.png)

结构体节点：[回到顶部](#链表)

```c++
typedef struct student {
    int num;        //学号
    int score;    //分数
    char name[20];
    struct student *next;//指针域
}STU;
```

样例：

```c++
void link_print(STU *head)
{
    STU *p_mov=head;
    while(p_mov!=NULL)//条件为当前节点，遍历时主要显示当前节点
    {
        printf("%d %d %s\n",p_mov->num,p_mov->score,p_mov->name);
        p_mov=p_mov->next;
    }
}
```

[回到顶部](#链表)



### 六.链表在算法中的有关应用

关于链表的算法问题有很多，笔者在此仅列举7个具有代表性的问题：约瑟夫环问题、链表反转、合并两个有序链表、查找倒数第k个节点、判断链表是否有环、查找环形入口、查找公共节点。

下面进行详细说明。

#### 1.约瑟夫环问题

***1）问题***

假设有n个人围成一圈，然后对每个人按顺序编号1，2，3，…，n，规定从1号按顺序开始报数，报到k的人出局，之后下一个人再从1开始报数，报到k的人在出局，一直进行下去，问：最后一个出局者为几号？

***2）思路分析：***

　　       （1）可将人的顺序简单编号，从1到N；

　　　　（2）构造一个循环链表，可以解决首位相连的问题，同时如果将人的编号改为人名或者其他比较方便

​               （3）将人的编号插入到结构体的Data域；

​     		  （4）遍历人的编号，输出参与的人的编号；

​     		  （5）开始报数，从头报数，报到k的人出局（删除次结点），（输出出局的人更人性化）避免浪费，可释放次结点。直到人数只						有一个人时，退出循环。输出获胜的人。

​				#<u>注意：在写删除删除结点的函数时都是针对K>=2的情况处理，所以要考虑k=1的情况，要是出局的密码为1时则最后一个获胜。</u>

[回到顶部](#链表)

***3）算法实现：***

（1）构造结构体

```c
typedef struct Node{
        int Num;//Data域
        struct Node *next;
    }JoseNode, *PNode, *HNode;//PNode为指针变量，HNode为头结点 
```

（2）得到头结点

```c
HNode h = ((HNode)malloc(sizeof(JoseNode)));
```

（3）初始化循环单链表

（4）插入算法

（5）遍历算法

（6）找到出局的人，并删除（删除算法）

[回到顶部](#链表)

*完整代码如下：*

```c
#include <stdio.h>
#include <malloc.h>

/*构建结构体*/
typedef struct Node{
    int Num;
    struct Node *next;
}JoseNode, *PNode, *HNode;

/**********初始化循环单链表*********/
int JoseInit(HNode *h)
{
    if (!h)
    {
        printf("初始化链表错误！\n");
        return 0;
    }
    (*h)->next = (*h);//循环单链表
    return 1;

}

/*************单链表插入操作**********/
int JoseInsert(JoseNode *h, int pos, int x)
{
    PNode p=h,q;
    int i=1;
    if (pos == 1)/*尾插法*/
    {
        p->Num = x;
        p->next = p;
        return 1;
    }
    while(i<pos-1)
    {
        p=p->next;
        i++;
    }
    q=(PNode)malloc(sizeof(JoseNode));
    q->Num=x;
    q->next=p->next;
    p->next=q;
    return 1;
}

/*遍历*/
void TraverseList(HNode h, int M)
{
    int i = 0;
    PNode p = h;
    printf("参与的人的编号为：\n");
    while (i<M)
    {
        printf("%d\t", p->Num);
        p = p->next;
        i++;
    }
    printf("\n");
}
/**************出局函数****************/

int JoseDelete(HNode h, int M, int k)
{    int i;
    PNode p=h,q;
    while(M>1)
    {
        for(i=1;i<k-1;i++)
        {
            p=p->next;
        }

        q=p->next;
        p->next=q->next;
        printf("出局的人为：%d号\n",q->Num);
        free(q);

        p=p->next;
        M--;
    }
    printf("***************获胜者为：%d号***************",p->Num);
    return 1;
}


/********************主函数**********************/
int main()
{
    int i;//计数器
    int N;//参与的人数
    int k;//报数密码
    printf("请输入参与人数：");
    scanf("%d",&N);
    printf("请输入出局密码：");
    scanf("%d",&k);

/**************得到头结点****************/
    HNode h = ((HNode)malloc(sizeof(JoseNode)));

/***************初始化单链表************/
    JoseInit(&h);

/******将编号插入到循环单链表中******/
    for (i = 1; i <=N; i++)
    {
        JoseInsert(h, i, i);
    }
/**************遍历单链表***************/
    TraverseList(h,N);

/***************出局函数************/
    if(k > 1)
    JoseDelete(h, N, k);
    else
    {
        for(i = 1; i < N; i++)
            printf("出局的人为：%d号\n",i);
        printf("***************获胜者为：%d号***************",N);
    }

    printf("\n");
    printf("\n");
    return 0;
}
```

[回到顶部](#链表)

#### 2.链表反转

***1）问题***

反转链表，顾名思义，就是改变链表节点的指向。

例如：

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL

***2）思路分析：***

- 先改变第一个结点，定义指针p指向头结点，原链表中p是指向第二个结点的，反转后头结点变尾结点，即p->next=NULL；这个时候我们就会发现链表的第一个结点和第二个结点断了，如下图所示：

<img src="https://s3.ax1x.com/2021/01/02/rz0Pg0.png" alt="rz0Pg0.png" style="zoom: 67%;" />





- 这个时候我们访问不到第二个结点了，所以在定义第一个p指针的同时，还要第一个q指针指向第二个结点，从而备份第二个结点；
- 接着改变第二结点的指向，在原链表中p->next=q，反转后就变成了q->next=p，我们又会发现q的下一个结点也断了，如下图所示：![rz0ivV.png](https://s3.ax1x.com/2021/01/02/rz0ivV.png)
- 这个时候第三个结点也访问不了了，所以我们还需要一个指针r来保存第三个结点；然后将p,q,r逐个往后移动一个结点，继续反转q结点的指向，如下图所示：

<img src="C:\Users\ly\Desktop\链表\反转3.png" style="zoom:67%;" /

- 当r指向NULL，这个时候还剩最后一个结点没有改变指向，如下图，改变其指向，该结点就是头结点了，然后返回该结点。![rz0ADU.png](https://s3.ax1x.com/2021/01/02/rz0ADU.png)

[回到顶部](#链表)

***3）关键算法实现：***

```c
Node* list_reverse(Node* head)
{
    if(head == NULL || head->next == NULL)
    {
         return head;
    }
    Node* p = head;
    Node* q = head->next;
    Node* r = q->next;

    p->next = NULL;
    while(r)
    {
        q->next = p;
        p = q;
        q = r;
        r = q->next;
    }
    q->next = p;

    return q;
}

```

[回到顶部](#链表)

#### 3.合并两个有序链表

***1）问题***

将两个以相同方式排好序的单链表，合并成一个排好序的单链表。

例如：

输入：1->2->4,1->3->4

输出：1->1->2->3->4

***2）思路分析***

- 我们假设链表是从小到大排列的，我们可以总结出规律为：比较两个结点的大小，较小的结点的下一个结点等于接下来两个结点中较小的结点。写成公式：node_min->next = func(node1,node2)；再看函数返回值，node_min下一个结点是node1, node2中较小的，所返回值就是较小的结点；

- 我们再从两个头结点head1，head2分析，先比较head1，head2谁大谁小，有两种情况，分别是：

  1.head1小，继续比较接下来的两个结点，即套用上面的公式。将head1指向小的，所以我们可以知道最后返回的是较小的结点，对       于head1和head2来说，head1小，所以return head1;

  2.head2小，同理。

  [回到顶部](#链表)

#### 4.查找倒数第k个节点

***1）问题***

***2）思路分析：***

有两种方法：

- 方法一：先遍历整个链表，获取链表的长度n，然后从头再次遍历到n-k个结点

- 方法二：

  - 查找倒数第k个结点，其实就是查找第n-k个结点，k是已知的，n是未知的，那有没有方法不需要知道n但是能查找到n-k呢？

  - 我们只有一个已知数k，那我们先来试试让一个指针从头结点移动到第k个结点，我们发现从第k个结点到尾结点就是n-k了；

  - 此时我们再让该指针从第k个结点出发，另一个指针从头结点出发，第一个指针到达尾结点时，第二个指针就到了n-k个结点，即到达倒数第k个结点。

    [回到顶部](#链表)

***3）关键算法实现***

- 1.使用两个指针，开始都指向头结点；

- 2.接着让第一个指针先移动到第k个结点；

- 3.让两个指针同时移动，当第一个指针移到尾结点时，第二个指针也就指向了倒数第k个结点。

  ```c
  Node* last_k(Node* head, int k)
  {
      if(head == NULL)
          return NULL;
      Node* src = head;
      Node* dst = head;
      //这里需要注意，从头结点移动到第k个结点，移动了k-1次
      for(int i=0;i<k-1;i++)
      {
          if(src)
              src = src->next;
          else
              return NULL;
      }
      while(src->next)
      {
          src = src->next;
          dst = dst->next;
      }
      return dst;
  }
  
  ```

  [回到顶部](#链表)

  #### 5.判断链表中是否有环

  ***1）问题***

  ***2)思路分析：***

  定义两个指针slow和fast，开始时都指向头结点，然后slow指针每次移动一个结点，fast每次移动两个结点，如果他们能够相遇在同一结点，则链表有环。

  ***3）关键算法实现***

  ```c
  bool has_cycle(Node* head)
  {
      if(head == NULL || head->next==NULL)
          return false;
      Node* slow = head;
      Node* fast = head;
      while(slow && fast->next)
      {
          slow = slow->next;
          fast = fast->next->next;
          if(slow == fast)
              return true;
      }
      return false;
  }
  ```

  #### 6.查找环形入口

  ***1）问题***

  如果判断出链表有环，那么如何找到环形入口呢？

  ***2)思路分析：***

- 1.使用两个指针slow和fast先判断是否有环，找到相遇的结点;

- 2.设头结点到环形入口的距离为a，环形入口到两个指针相遇结点的距离为b，环的长度为c，如下图;

​       <img src="https://s3.ax1x.com/2021/01/02/rzBeL8.png" alt="rzBeL8.png" style="zoom:50%;" />

- 3.当两指针相遇时，slow指针移动的路程s = a+b+m*c，fast指走的路程2s = a+b+n*c，可得2(a+b+m*c)=a+b+n*c，化解得a=(n-2m)*c-b，再次化解得a=(n-2m-1)*c + (c-b);
- 4.由上述公式我们可以的得出：*如果让一个指针从头结点出发，一个指针从他们上次相遇结点出发，移动速度一致，那么他们再次相遇的地方就是环形链表的入口.*[回到顶部](#链表)

***3）关键算法实现***

- 1.使用两个指针slow和fast找到相遇的结点;

- 2.让其中一个指针从头结点出发，同时另一个指针从相遇的结点出发;

- 3.两个指针以同样的速度移动，当两个指针相遇时，相遇的结点就是环的入口.

  ```c
  Node* cycle_entry(Node* head)
  {
      if(head == NULL || head->next==NULL)
          return NULL;
      Node* slow = head;
      Node* fast = head;
      //找到相遇的结点
      while(slow && fast->next)
      {
          slow = slow->next;
          fast = fast->next->next;
          if(slow == fast)
              break;
      }
      if(slow && fast)
      {
          if(slow == head)
              return head;
          slow = head;
          //当两个指针相遇时，相遇的结点就是环的入口
          while(slow != fast)
          {
              slow = slow->next;
              fast = fast->next;
          }
          return slow;
      }
      return NULL;
  }
  ```

  #### 7.查找公共节点

  ***1）问题***

  ***2）思路分析：***

  有两种方法：

- 方法一：通过双循环嵌套遍历每个结点，但双循环嵌套，效率较低。

- 方法二：

  <img src="https://s3.ax1x.com/2021/01/02/rz0EbF.png" alt="rz0EbF.png" style="zoom:80%;" />

  

  1.由上图我们可以简单的看出，两个链表的长度之差为l2-l1；

  2.如果让l2先走l2-l1,那他们就相当于同一条起跑线了，然后让他们一起以同样的速度移动，则两个指针相遇的结点就是公共结点。

  ***3）关键算法实现***

  - 1.计算链表的长度，找出长度较长的链表

  - 2.让较长的链表先移动两链表长度之差的长度

  - 3.两链表以同样的速度同时出发，相遇的结点就是公共结点

    ```c
  size_t list_len(Node* head)
    {
        int len = 0;
        while(head)
        {
            head = head->next;
            len++;
        }
        return len;
    }
    
    Node* list_public(Node* head1,Node* head2)
    {
        if(head1 == NULL || head2 == NULL)
            return NULL;
        // 计算链表的长度
        int len1 = list_len(head1);
        int len2 = list_len(head2);
    
        // 找出长度较长的链表
        Node* long_list = len1 > len2 ? head1 : head2;
        Node* short_list = len1 > len2 ? head2 : head1;
    
        //较长的链表先移动两链表长度之差的长度
        for(int i = 0;i < abs(len1 - len2); i++)
        {
            long_list = long_list->next;
        }
    
        //两链表以同样的速度同时出发，相遇的结点就是公共结点
        while(short_list && long_list)
        {
            if(short_list == long_list)
                return short_list;
            short_list = short_list->next;
            long_list = long_list->next;
        }
        return NULL;
    }
    ```
  
  
### 七.结语

链表是一种常见的基础数据结构，结构体指针在这里得到了充分的利用，可以动态的进行存储分配。链表在数据结构的后续学习中是基石一般的存在。因此，笔者认为，扎实掌握好链表的基本操作，并能进行一定的实际应用对学好数据结构与算法，提高计算机水平有重要作用。

以上内容即为笔者团队在过去一段时间在链表学习上的总结与感想，由于笔者水平有限，在总结之时难免有错误、疏漏之处，还望科中的同学、学长、学姐们批评指正、不吝赐教！

[回到顶部](#链表)



