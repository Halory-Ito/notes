# 复合数据类型

## 模板类

`vector`是标准库的一部分，类似于Java中的集合，可以动态扩容。

引入:

```c++
#include<vector>
using namesapce std;
```

基本用法:

- 创建及初始化容器

```c++
#include <iostream>
#include <vector>
using namespace std;
int main()
{
    // 默认初始化，创建了一个空的容器
    vector<int> v1;

    // 列表初始化，也叫做拷贝初始化
    vector<int> v2 = {1, 2, 3, 4, 5};

    // 列表初始化时，等号可以省略
    vector<int> v3{1, 2, 3, 4, 5};

    // 直接初始化，创建一个有5个元素的容器，元素的默认值同类型的默认值
    vector<int> v4(5);

    // 直接初始化，创建了有5个元素的容器，且每个元素的值为10
    vector<int> v5(5, 10);
}
```

- 访问和修改元素

```c++
// 访问元素（访问第4个元素）
cout << v5[3] << endl;

// 修改元素
v5[3] = 20;
```

- 遍历元素

```c++
int size = v5.size();

// 遍历容器(普通for循环)
for (int i = 0; i < size; i++)
{
    cout << v5[i] << "\t";
}

cout << endl;
// 遍历容器(增强for循环)
for (int num : v5)
{
    cout << num << "\t";
}
```

- 添加元素

```c++
// 添加元素
v5.push_back(11);
```



## 简单读写文件



引入:

```c++
#include <fstream>
using namespace std;
```



### 读文件



- 打开指定文件

```c++
// input为变量名
ifstream input("input.txt");
```

- 逐个单词读取

```c++
string word;
while (input >> word)
{
    cout << word << endl;
}
```

- 逐行读取

```c++
string line;
while (getline(input, line))
{
    cout << line << endl;
}
```

- 逐个字符读取

```c++
char ch;
while (input.get(ch))
{
    cout << ch << endl;
}
```



### 写文件

```c++
#include <iostream>
#include <string>
#include <fstream>
using namespace std;
int main()
{
    // 指定源文件
    ifstream input("input.txt");

    // 指定目标文件
    ofstream output("output.txt");

    // 按照单词逐个写入
    string word;
    while (input >> word)
    {
        output << word << endl;
    }
    
}
```



## 指针

