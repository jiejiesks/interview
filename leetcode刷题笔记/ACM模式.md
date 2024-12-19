# ACM模式

## 万能头文件

`#include <bits/stdc++.h>`





## 读入

## 2.C++常用的输入输出方法



C++的输入输出有很多种方式，既有继承自C语言的，也有其自己独特的。这里呢，不会把全部输入输出函数进行罗列，只会介绍几个在笔试面试中经常被用到的，我认为，**掌握这几个足够了**，如果有余力，可以去**官方文档**查看更多关于输入输出的函数进行深度学习。



### 2.1 输入

首先，在C++语言中，要使用标准的输入，需要包含头文件`<iostream>`

#### （1）cin

cin是C++中， 标准的输入流对象，下面列出cin的两个用法，**单独读入**，和**批量读入**

cin的原理，简单来讲，是有一个缓冲区，我们键盘输入的数据，会先存到缓冲区中，用cin可以从缓冲区中读取数据。

> 注意1：cin可以连续从键盘读入数据
>
> 注意2：cin以空格、tab、换行符作为分隔符
>
> 注意3：cin从第一个非空格字符开始读取，直到遇到分隔符结束读取

示例：

```c++
// 用法1，读入单数据
int num;
cin >> num;
cout << num << endl;  // 输出读入的整数num

// 用法2，批量读入多个数据
vector<int> nums(5);
for(int i = 0; i < nums.size(); i++) {
	cin >> nums[i];
}
// 输出读入的数组
for(int i = 0; i < nums.size(); i++) {
	cout << nums[i] << " ";
}

//使用
cin.ignore()忽略最后的换行符
```

#### （2）getline()

从cin的注意中，也可以看出，当我们要求读取的字符串中间存在空格的时候，cin会读取不全整个字符串，这个时候，可以采用getline()函数来解决。

> 注意1：使用getline()函数的时候，需要包含头文件`<string>`
>
> 注意2：getline()函数会读取一行，读取的字符串包括空格，遇到换行符结束

示例：

```c++
string s;
getline(cin, s);
// 输出读入的字符串
cout << s << endl;

getline(cin,s, ',') // 以','作为分隔符 111111,111111
getline(cin, s ,' ') // 以空格作为分隔符

    
getline(cin, s);
stringstream ss(s);
string t;
getline(ss,t,',')// 对读入的一行s，继续以','作为分隔符分割，每次输入到字符串t中
```

#### （3）getchar()

该函数会从缓存区中读出一个字符，经常被用于判断是否换行

示例：

```c++
char ch;
ch = getchar();
// 输出读入的字符
cout << ch << endl;
```

### 不固定输入

#### 不固定数目

##### 输入格式：

```
1 2 3 4
```

##### 解析：

输入的数据为四个用空格间隔的整数，没有指定整数个数，因此可以用`while`循环结合`cin`来处理该问题。

##### 答案：

```c++
vector<int> nums;
int num;
while(cin >> num) {
	nums.push_back(num);
	// 读到换行符，终止循环
	if(getchar() == '\n') {
		break;
	}
}

// 验证是否读入成功
for(int i = 0; i < nums.size(); i++) {
	cout << nums[i] << " ";
}
cout << endl;
```

## 常见数据结构定义

在ACM模式中，链表、二叉树这些数据结构的定义也需要自己去定义，接下来就给出二者的定义、输入和输出。

这里就直接给出代码了，想必大伙对数据结构都是了如指掌的。

### 1.链表

```c++
#include<bits/stdc++.h>

using namespace std;

struct ListNode{
    ListNode *next;
    int val;
    ListNode(int _val, ListNode *_next):val(_val),next(_next){};
    ListNode(int _val):val(_val),next(nullptr){};
};

int main()
{
    auto head = new ListNode(-1);
    auto tail = head;

    int num;
    while(cin >> num)
    {
        if(num == -1) break;
        auto t = new ListNode(num);
        tail = tail->next = t;
    }

    auto a = head->next;
    while(a)
    {
        cout << a->val << " ";
        a = a->next;
    }

    cout << endl;
    return 0;
}
```

### 2.二叉树

```c++
#include <bits/stdc++.h>
using namespace std;

struct Treenode{
    int val;
    Treenode *left, *right;
    Treenode(int _val):val(_val), left(nullptr), right(nullptr){}
};

Treenode* build(vector<int> &nums)
{
    int n = nums.size();
    vector<Treenode *> v(n + 1);
    for(int i = 1; i <= n; i ++)
    {
        if(nums[i] != -1)  v[i] = new Treenode(nums[i]);
        else v[i] = nullptr;
    }
    for(int i = 1; 2 * i + 1 <= n; i ++)
    {
        if(v[i])
        {
            v[i]->left = v[2 * i];
            v[i]->right = v[2 * i + 1];
        }
    }
    return v[1];
}

void dfs(Treenode *root)
{
    if(!root) return ;
    dfs(root->left);
    cout << root->val << " ";
    dfs(root->right);
}

int main() {
    int n;
    cin >> n;
    vector<int> nums(n + 1);
    for(int i = 1; i <= n; i ++) cin >> nums[i];
    auto root = build(nums);
    dfs(root);
    return 0;
    /*
    	输入15
   		4 1 6 0 2 5 7 -1 -1 -1 3 -1 -1 -1 8
   		输出 0 1 2 3 4 5 6 7 8
    */ 
}
// 64 位输出请用 printf("%lld")
```

