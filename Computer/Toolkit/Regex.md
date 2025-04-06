# 正则表达式


## 匹配北美电话号码

>707-827-7090

```python
import re
phone = '707-827-7090'

# 使用字符串字面量
re.findall('707-827-7090', phone)

# 使用字符组匹配数字
re.findall('[0-9] [0-9] [0-9] [0-9] [0-9] [0-9] [0-9] [0-9] [0-9] [0-9] ', phone)

# 使用字符组简写式
re.findall('\d\d\d-\d\d\d-\d\d\d\d', phone)

# 匹配任意字符
re.findall('\d\d\d.\d\d\d.\d\d\d\d', phone)

# 捕获分组
re.findall('(\d)\d\1', phone)
'''
解释：
	(\d)匹配第一个数字并将其捕获(数字7)
	\d匹配第二个数字(数字0)，但没有捕获，因为没有括号
	\1对捕获的数字进行反向引用(数字7)
'''

# 使用量词
re.findall('\d{3}-?\d{3}-?d{4}', phone)
# 更加简洁(但不完全对)
re.findall('(\d{3,4}[.-]?)+', phone)
# 改进
re.findall('(\d{3}[.-]?){2}\d{4}', phone)
'''
该表达式匹配的是两个无括号的三位数字，每三位数字后可以带连字符也可以不带，最后一个是四位数字
'''

# 括选文字符
re.findall('^(\(\d{3}\)|^\d{3}[.-]?)?\d{3}[.-]?\d{4}$', phone)
'''
解释：该正则表达式表示第一个3位数字可以带括号也可以不带，即区号是可选的
	\(:左括号本身
	\):右括号本身
'''

```


## 简单的模式匹配

- 匹配字符串字面量

- 匹配数字字符：`\d`、或者`[0-9]`
- 匹配非数字字符：`\D`、或者`[^\d]`、或者`[^0-9]`

- 匹配单词字符：`\w`或`[_a-zA-Z0-9]`
- 匹配非单词字符：`\W`或`[^_a-zA-Z0-9]`或`[^\w]`

>[!note] 比较
>`\D`：会匹配空格、其他标点符号
>`\w`：只匹配字母、数字和下划线

- 匹配空白符：`\s`或`[ \t\n\r]`
- 匹配非空白符：`\S`或`[^ \t\n\r]`或`[^\s]`

>也就是说，`\s`会匹配到：`空格`、`\t`、`\n`、`\r`


- 匹配任意符：`.`
- 匹配某个字符一次以上：`+`
- 匹配某个字符零次以上：`*`


## 边界

>断言标记边界，但是并不耗用字符

- 行的起始：`^`、`$`


### 单词边界与非单词边界

- 匹配单词，对只是一个单词：`\bthe\b`

![[单词边界.png]]


非单词边界，：`\Bthe\B`

![[非单词边界.png]]


>[!note] 发现
>`\bthe\b` + `\Bthe\B` = `(the)`


### 使用元字符的字面值

可以用`\Q我是字符\E`来匹配字符串字面值，因为在`\Q`和`\E`之间的字符都视为普通字符

好处：之前必须用`\.`转义来表示`.`，使用了`\Q.\E`就可以匹配到`.`了，省去了转义

缺点：用得少


## 选择、分组和后向引用

### 选择操作

如果想要匹配`the`，包括大小写等形式，那么就可以写成如下的形式：
`(the|The|THE)`

但是这个复杂了点，且笨，此时可以借助==选项==，来指定查找的模式，比如：==`(?i)`==，此时就不会区分大小写

`(?i)the`

==正则表达式中的选项==

| 选项 | 描述           | 支持平台         |
| ---- | -------------- | ---------------- |
| (?d) | Unix中的行     | Java             |
| (?i) | 不区分大小写   | PCRE、Perl、Java |
| (?J) | 允许重复的名字 | PCRE             |
| (?m) | 多行           | PCRE、Perl、Java |
| (?s) | 单行           | PCRE、Perl、Java |
| (?u) | Unicode        | Java             |
| (?U) | 默认最短匹配   | PCRE             |
| (?x) | 忽略空格和注释 | PCRE、Perl、Java |


### 子模式

多数情况下的子模式，指的就是分组中的一个或多个分组

`(the|The|THE)`有三个子模式：the是第一个、The是第二个、THE是第三个

多数情况下，子模式中的条件能匹配到的前提是前面的模式得到匹配，比如：

>`(t|T)h(e|eir)`

该正则表达式匹配到their的前提是，第一个子模式匹配到了t，而第二个子模式匹配到了eir


### 捕获分组和后向引用

**分组**

一个括号就是一个分组

`(\w)`

**捕获分组**

根据括号的位置，可以标记为1、2、3等，然后据此实现分组的反向引用，比如：

`(\w)\1`：可以用来匹配连续相同的两个字符

![[连续相同的两个字符.png]]

**非捕获分组**

如果不想要把括号里的内容分组的话，那就是非捕获分组，需要用`?:`标注一下，一般用于限定变量的范围会使用非捕获变量，比如：

`(?:\w)`：这就是一个非捕获分组

回到章节开始，我们利用`(the|The|THE)`进行了一个分组，实现其非捕获分组的话：`(?:the|The|THE)`

添加选项的话，可以简化为：`(?i)(?:the)`

或者：`(?:(?i)the)`

但是最好的是：==`(?i:the)`==

**命名分组**

除了按照1、2、3的顺序给每一个组命名，还可以自定义命名

>Python中的分组命名：`(?P<name>)`
>Python中的引用分组名：`(?P=name)`

例如：
![[Python中的命名分组.png]]




## 字符组

### 字符组取反

在中括号中使用脱字符（`^`）
`[^aeiou]`

### 并集和差集

==Java中才有==

并集：`[0-3[6-9]]`：既匹配0-3的数字，又匹配6-9的数字

差集：`[0-3&&[^3-5]]`：匹配0-3和3-5的差集，即0-2


### POSIX字符组

这个可以说是对`\w`的一种细分，形式为：`[[:xxxx:]]`
比如：`[[:alnum:]]`，可以匹配数字跟字母；`[[:ascii:]]`可以匹配ascii字符


| 字符组         | 描述             |
| -------------- | ---------------- |
| `[[:alnum]]`   | 匹配字母及数字   |
| `[[:alpha:]]`  | 匹配字母         |
| `[[:ascii:]]`  | 匹配ascii字符    |
| `[[:blank:]]`  | 匹配空白字符     |
| `[[:ctrl]]`    | 匹配控制字符     |
| `[[:digit]]`   | 匹配数字         |
| `[[:graph:]]`  | 匹配图形字符     |
| `[[:lower:]]`  | 匹配小写字母     |
| `[[:print:]]`  | 匹配可打印字符   |
| `[[:punct:]]`  | 匹配标点符号     |
| `[[:space:]]`  | 匹配空格字符     |
| `[[:upper:]]`  | 匹配大写字母     |
| `[[:word:]]`   | 匹配单词字符     |
| `[[:xdigit:]]` | 匹配十六进制数字 |


## 量词

### 贪心、懒惰和占有

这是量词的三个特性

### 基本量词

`?`：零个或一个
`+`：一个或多个
`*`：零个或多个

### 匹配特定次数

使用花括号来限制某个模式在某个范围内的匹配次数，未经修饰的量词就是贪心量词

比如：`7{1}`：只会匹配出现的第一个7

==范围语法总结==

`{n}`：精确匹配n次
`{n,}`：匹配n次以上
`{m,n}`：匹配m至n次
`{0,1}`：与`?`同
`{1,0}`：与`+`同
`{0,}`：与`*`同


### 贪心量词

比如`,.*,`，它会匹配到下面高亮的部分。

>The star may dissolve==, and the fountain of light May sink intoe'er ending chao's and night,Our mansions must fall and earth vanish away;But thy courage, O Erin! may never decay.See! the wide wasting ruin extends all around,Our ancestors' dwellings lie sunk on the ground,Our foes ride in triumph throughout our domains,And our mightiest horoes streched on the plains.Ah! dead is the harp which was wont to give pleasure,Ah! sunk in our sweet country's rapturous measure,But the war note is weaked, and the clangour of spears,==The dread yell of Slogan yet sounds in our ears.Ah! where are the heroes! triumphant in death


对于`,.*,`，它会先匹配`,.*`的部分，而这部分将会匹配到

>The star may dissolve==, and the fountain of light May sink intoe'er ending chao's and night,Our mansions must fall and earth vanish away;But thy courage, O Erin! may never decay.See! the wide wasting ruin extends all around,Our ancestors' dwellings lie sunk on the ground,Our foes ride in triumph throughout our domains,And our mightiest horoes streched on the plains.Ah! dead is the harp which was wont to give pleasure,Ah! sunk in our sweet country's rapturous measure,But the war note is weaked, and the clangour of spears,The dread yell of Slogan yet sounds in our ears.Ah! where are the heroes! triumphant in death==

然后根据正则表达式最后有一个`,`，因此它会进行回溯，把不符合的字符一个个吐出来，最终匹配到：

>The star may dissolve==, and the fountain of light May sink intoe'er ending chao's and night,Our mansions must fall and earth vanish away;But thy courage, O Erin! may never decay.See! the wide wasting ruin extends all around,Our ancestors' dwellings lie sunk on the ground,Our foes ride in triumph throughout our domains,And our mightiest horoes streched on the plains.Ah! dead is the harp which was wont to give pleasure,Ah! sunk in our sweet country's rapturous measure,But the war note is weaked, and the clangour of spears,==The dread yell of Slogan yet sounds in our ears.Ah! where are the heroes! triumphant in death

贪心量词会尽可能匹配到多的部分，然后再根据正则表达式的规则进行回溯。

### 懒惰量词

懒惰的基本特性就是匹配尽可能少的字符

比如：`5?`会匹配第一个5，而`5??`则不会匹配，因为最少的情况就是不匹配，这就是懒惰

再比如：`5{2,5}?`只会匹配两个5，因为这是最少的情况

如果想要匹配最少，而不是最多的字符，就可以使用懒惰量词

==懒惰量词总结==

`??`：懒惰匹配零次或者一次
`+?`：懒惰匹配一次或者多次
`*?`：懒惰匹配零次或者多次
`{n}?`：懒惰匹配n次
`{n,}?`：懒惰匹配n次或多次
`{m,n}?`：懒惰匹配m到n次


### 占有量词

没搞懂

## 环视

环视是断言的一个比较重要的特性

### ==正前瞻==

==`(?=)`==

`\w+(?=th)`：会匹配到所有右边是th的字符串

>The star may dissolve, and the fountain of light May sink intoe'er ending chao's and night,Our mansions must fall and ==ear==th vanish away;But thy courage, O Erin! may never decay.See! the wide wasting ruin extends all around,Our ancestors' dwellings lie sunk on the ground,Our foes ride in triumph throughout our domains,And our mightiest horoes streched on the plains.Ah! dead is the harp which was wont to give pleasure,Ah! sunk in our sweet country's rapturous measure,But the war note is weaked, and the clangour of spears,The dread yell of Slogan yet sounds in our ears.Ah! where are the heroes! triumphant in ==dea==th

### ==反前瞻==

==`(?!)`==
`\b\w{4}(?!ght)`：匹配所有左边是单词边界、字符数为4、右边不紧挨着ght的字符串

>The ==star== may ==diss==olve, and the ==foun==tain of ==ligh==t


### ==正后顾==

==`(?<=)`==
跟正前瞻相反

`(?i)(?<=th)\w+`：匹配左边紧挨着th，忽略大小写的字符串

>Th==e== star may dissolve, and th==e== fountain of light


### ==反后顾==

==`(?<!)`==

跟反前瞻相反

`(?i)(?<!th)\b\w{3}`：