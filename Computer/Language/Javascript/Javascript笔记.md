# 入门

```javascript
// 声明变量
let name = '胡桃'

// 字符串模板，必须用反引号圈起来
console.log(`我叫${name}`)

// NaN
计算失误的时候会出现
```



## 数据类型

undefined：变量已声明但未赋值

### 检测数据类型

使用运算符`typeof`或者函数`typeof()`

```javascript
let num = 1
console.log(typeof num)
// number
console.log(typeof(num))
// number
```



### 类型转换

**隐式转换**

prompt()函数接收的数据都是字符串

```javascript
console.log(1 - '1');
// 0，数字型
console.log(1 + '1');
// 11，字符型
console.log(+'1');
// 1，数字型
```



**显示转换**

- 转为数字型

加号（+）可以显示转换为数字型

NaN也是数字型

```javascript
// 转为数字型
let num1 = Number(prompt())
console.log(typeof num1)
// Number
let num2 = +(prompt())
console.log(typeof num2)
// Number
let num3 = +('我是猪')
console.log(num3)
// NaN
console.log(typeof num3)
// Number

let num = '1.23abc'
console.log(parseInt(num))
// 1
console.log(parseFloat(num))
// 1.23
```

- 如果字符串是==以数字开头==的，那parseInt()和parseFloat()可以获取到其数字部分

null经过数字转换之后会变为0

undefined经过数字转换后会变成NaN



- 转为布尔型
  - ''、0、undefined、null、false、NaN转为布尔值后都是false



## 运算符

```javascript
/*
==			两个变量的值是否相同
===			两个变量的值和类型是否相同
!==			两个变量的值和类型是否不相同
*/

// 比较时遇到NaN的都是false，包括NaN == NaN
```



## 语句

**分支语句**

```javascript
/*
if	if-else	if-else if-else
和Java一样
*/

// 三元运算符
1 > 2?'是的':'不是'
// 不是


// switch
switch(case):
    case 1:
    .....
    break
    case 2:
    ....
    break
    default:
    ....
    break
```



**循环语句**

```javascript
// while循环	同Java一样

// for循环	同Java一样
```



## 浏览器断点调试

![image-20240604171539100](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240604171539100.png)





## 数组

```javascript
// 创建数组
let arr = []
let arr = new Array()

// 追加元素
let arr = []
arr[0] = 0
arr[1] = 1
console.log(arr)
// 0, 1

// 修改元素
arr[0] = -1
console.log(arr)
// [-1, 1]

// 添加元素
// push会将元素追加到数组末尾，并返回数组的新长度
arr.push(1, 2, 3,...);
// unshift会将元素追加到数组开头，并返回数组的新长度
arr.unshift(1, 2, 3,...);


// 删除元素
// pop()方法会删除数组的最后一个元素，并返回该元素的值
arr.pop()
// shift()方法会删除数组的第一个元素，并返回该元素的值
arr.shift()
// splice()方法会删除指定起始位置，指定删除的元素数量，如果没有指定第二个参数，则默认删除到最后
arr.splice(start, deleteCount)
```

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240604172534903.png" alt="image-20240604172534903" style="zoom:67%;" />



## 函数

### 函数基础

```javascript
// 声明
function getName(param1 = '我是缺省参数', param2, ...){
    ...
    return
}
    
// 调用
getName(1, 2, ...)
        
/*
函数细节补充：
1. 定义了多个同名函数，那么调用时应该执行哪个？
答：调用最近的那个函数。或者说后面定义的函数会覆盖前面的同名函数

2. 如果传入的实参比形参少，会怎么样？
function a(p1, p2, p3)
a(1, 2)
答：返回NaN，因为p3没有赋值的话，为undefined。1+undefined=NaN

3. 如果传入的实参比形参少，会怎么样？
function a(p1, p2, p3)
a(1, 2, 3, 4)
答：对结果无影响，只是多出来的实参会被忽略
*/
```



### 匿名函数

```javascript
// 函数变量，接收函数表达式
let fun = function() {
    ...
}
fun()
    
    
// 立即执行函数。对，定义即调用。最后一个括号相当于调用函数了，两种写法
(function(){函数体})()
(function(){函数体}())
```





## 对象

Javascript中的对象好像就是json

### 基本操作

```javascript
  // 声明对象
let person = {
"name": "胡桃",
'age': 18,
'gender': '女'
}

// 添加属性
person.address = '璃月往生堂'
console.log(person)

// 删除属性
delete person.age
console.log(person)

// 修改属性
person.address = '往生堂'
console.log(person)

// 查询属性
console.log(person.address);
```



<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240604193047502.png" alt="image-20240604193047502" style="zoom:67%;" />

> 注意，对于复杂的属性名，比如中间带了-的，则必须用:对象['属性名']的形式

```html
<script>
        // 声明对象
        let person = {
            "name": "胡桃",
            'age': 18,
            'gender': '女',
            'a-b-c': '我是测试用的'
        }

        // console.log(person.a - b - c)，报错
        console.log(person['a-b-c'])
    	// 我是测试用的

</script>
```



### 对象的方法

```html
    <script>
        // 声明对象
        let person = {
            "name": "胡桃",
            "address": '往生堂',
            info: function (x, y) {
                console.log(`我是${x}, 我来自${y}`);
            }

        }

        // 调用方法
        person.info(person.name, person.address)

    </script>
```

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240604194004522.png" alt="image-20240604194004522" style="zoom:67%;" />



当然，方法也可以在对象声明外定义

```html
<script>
        // 声明对象
        let person = {
            name: "胡桃",
            address: '往生堂',
            age: 18

        }

        person.info = function (x, y) {
            console.log(`我是${y}的${x}`);
        }

        person.info(person.name, person.address)

</script>
```



<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240604195238143.png" alt="image-20240604195238143" style="zoom:80%;" />



### 遍历对象属性

使用增强for遍历

```html
    <script>
        // 声明对象
        let person = {
            name: "胡桃",
            address: '往生堂',
            age: 18

        }

        // 遍历属性
        for (k in person) {
            // 键
            console.log(k);
            // 值
            console.log(person[k])
            // 此时这里不能用person.k，因为这里的k是字符串，无法引用对应的属性
        }

    </script>
```

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240604194610130.png" alt="image-20240604194610130" style="zoom:67%;" />



### 内置对象

Math





# web APIs

## 介绍

Web APIs：

- DOM：文档对象模型，即操作网页内容，开发网页内容特效和用户与网页的交互
- BOM：浏览器对象模型



## DOM

### 基本概念

DOM树：将HTML文档以树状结构直观的表现出来

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240604201428790.png" alt="image-20240604201428790" style="zoom:67%;" />

==所有标签都可以当作一个DOM对象==，而其中DOM提供的最大的对象，则是document



### 获取DOM元素

- 获取匹配的第一个元素：`document.querySelector()`

```html
    <p id="nav">导航栏</p>
    <script>
        const box = document.querySelector('#nav')
        console.log({ box })
    </script>
```

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240604204540292.png" alt="image-20240604204540292" style="zoom: 80%;" />



- 获取匹配到的所有元素：`document.querySelectorAll()`，返回一个对象集合

```html
<body>
    <ul>
        <li>测试1</li>
        <li>测试2</li>
        <li>测试3</li>
    </ul>
    <script>
        const box = document.querySelectorAll('ul li')
        console.log({ box })
    </script>
</body>
```

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240604204718770.png" alt="image-20240604204718770" style="zoom:80%;" />

- 其他方式：

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240604204956490.png" alt="image-20240604204956490" style="zoom:80%;" />



### 修改元素内容

两种方式：

- innerText：只识别文本，不能解析标签
- innerHTML：除了可以识别文本之外，还可以解析标签

```html
<!-- innerText -->
<body>
    <div id="box">我是胡桃的狗</div>

    <script>
        // 获取标签
        box = document.querySelector('#box')
        // 获取元素内容
        console.log(box.innerText);

        // 修改元素内容
        box.innerText = '我是神里绫华的狗'
        console.log(box.innerText)

    </script>
</body>
```

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240607170233171.png" alt="image-20240607170233171" style="zoom:67%;" />



```html
<!-- innerHTML -->

<body>
    <div id="box">我是胡桃的狗</div>

    <script>
        // 获取标签
        box = document.querySelector('#box')
        // 获取元素内容
        console.log(box.innerHTML);

        // 修改元素内容
        box.innerHTML = '<strong>我是神里绫华的狗</strong>'
        console.log(box.innerHTML)

    </script>
</body>
```

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240607170358556.png" alt="image-20240607170358556" style="zoom:67%;" />



### 操作元素属性

#### 普通属性

跟对象的属性一样操作

`标签对象.属性 = 值`

案例：每次刷新页面，都更换页面的图片

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>随机更换图片</title>
</head>

<body>
    <img src="../images/01.jpg" class="image">
    <script>
        // 获取标签
        const img = document.querySelector('.image')

        // 获取图片数组
        const imgArr = ['01.jpg', '02.jpg', '03.jpg', '04.jpg', '05.jpg']

        // 修改标签属性
        img.src = "../images/" + imgArr[parseInt(Math.random() * imgArr.length)]

    </script>
</body>

</html>
```



#### 样式属性

**使用`对象.style.属性=值`**

注意：

- 多个单词的属性，用小驼峰命名法
- 数字后面要跟单位，比如px
- 一定要定义style标签，且有操作对象的选择器

```html
<style>
    /* 必须给标签样式，给了才能操作样式属性 */
    #box {
        width: 100px;
        height: 1000px;
        background-color: #cc7b7b;
    }
</style>

<body>
    <div id="box"></div>
    <script>
        const box = document.querySelector('#box')
        // 修改样式
        box.style.width = '2000px'
        box.style.marginTop = '15px'
        box.style.backgroundColor = 'red'
    </script>
</body>
```

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240607173356217.png" alt="image-20240607173356217" style="zoom:67%;" />

案例：随机背景图片

```html
<style>
    body {
        background: url(../images/01.jpg) no-repeat top center/cover;
    }
</style>

<body>
    <script>
        function getRandom(N, M) {
            return Math.floor(Math.random() * (M - N + 1)) + N
        }

        // 随机数
        const random = getRandom(1, 6)
        document.body.style.backgroundImage = `url(../images/0${random}.jpg)`
    </script>
</body>
```


**使用类名操作CSS**

语法：

```javascript
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<style>  
    div {  
        width: 200px;  
        height: 200px;  
        background-color: pink;  
    }  
  
    .nav {  
        width: 400px;  
        height: 400px;  
        background-color: antiquewhite;  
    }  
  
    .box {  
        width: 300px;  
        height: 300px;  
        background-color: skyblue;  
    }  
</style>  
<body>  
<div class="nav">  
</div>  
<script>  
    // 1. 获取元素  
    const div = document.querySelector('div')  
    // 2. 添加类名 clas是个关键字，我们用className  
    div.className = 'box'  
    // 该操作其实是修改div的类名  
</script>  
</body>  
</html>
```

**使用classList操作CSS**

好处：不会像className一样容易删去类名。classList支持追加、切换和删除类名

```html
<!DOCTYPE html>  
<html lang="en">  
<head>  
    <meta charset="UTF-8">  
    <title>Title</title>  
</head>  
<style>  
    .box {  
        width: 200px;  
        height: 200px;  
        color: #333;  
    }  
  
    .active {  
        color: red;  
        background-color: pink;  
    }  
</style>  
<body>  
<div class="box">123</div>  
<script>  
    // 获取元素  
    const box = document.querySelector('.box')  
  
    // 追加样式  
    box.classList.add('active')  
  
    // 删除样式  
    box.classList.remove('active')  
  
    // 切换样式，有则删除，无则添加  
    box.classList.toggle('active')  
    box.classList.toggle('active')  
</script>  
</body>  
</html>
```

#### 案例

随机轮播图

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>轮播图点击切换</title>
    <style>
        * {
            box-sizing: border-box;
        }

        .slider {
            width: 560px;
            height: 400px;
            overflow: hidden;
        }

        .slider-wrapper {
            width: 100%;
            height: 320px;
        }

        .slider-wrapper img {
            width: 100%;
            height: 100%;
            display: block;
        }

        .slider-footer {
            height: 80px;
            background-color: rgb(100, 67, 68);
            padding: 12px 12px 0 12px;
            position: relative;
        }

        .slider-footer .toggle {
            position: absolute;
            right: 0;
            top: 12px;
            display: flex;
        }

        .slider-footer .toggle button {
            margin-right: 12px;
            width: 28px;
            height: 28px;
            appearance: none;
            border: none;
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            border-radius: 4px;
            cursor: pointer;
        }

        .slider-footer .toggle button:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        .slider-footer p {
            margin: 0;
            color: #fff;
            font-size: 18px;
            margin-bottom: 10px;
        }

        .slider-indicator {
            margin: 0;
            padding: 0;
            list-style: none;
            display: flex;
            align-items: center;
        }

        .slider-indicator li {
            width: 8px;
            height: 8px;
            margin: 4px;
            border-radius: 50%;
            background: #fff;
            opacity: 0.4;
            cursor: pointer;
        }

        .slider-indicator li.active {
            width: 12px;
            height: 12px;
            opacity: 1;
        }
    </style>
</head>

<body>
<div class="slider">
    <div class="slider-wrapper">
        <img src="./images/slider01.jpg" alt=""/>
    </div>
    <div class="slider-footer">
        <p>对人类来说会不会太超前了？</p>
        <ul class="slider-indicator">
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
        </ul>
        <div class="toggle">
            <button class="prev">&lt;</button>
            <button class="next">&gt;</button>
        </div>
    </div>
</div>
<script>
    // 1. 初始数据
    const sliderData = [
        {url: './images/slider01.jpg', title: '对人类来说会不会太超前了？', color: 'rgb(100, 67, 68)'},
        {url: './images/slider02.jpg', title: '开启剑与雪的黑暗传说！', color: 'rgb(43, 35, 26)'},
        {url: './images/slider03.jpg', title: '真正的jo厨出现了！', color: 'rgb(36, 31, 33)'},
        {url: './images/slider04.jpg', title: '李玉刚：让世界通过B站看到东方大国文化', color: 'rgb(139, 98, 66)'},
        {url: './images/slider05.jpg', title: '快来分享你的寒假日常吧~', color: 'rgb(67, 90, 92)'},
        {url: './images/slider06.jpg', title: '哔哩哔哩小年YEAH', color: 'rgb(166, 131, 143)'},
        {url: './images/slider07.jpg', title: '一站式解决你的电脑配置问题！！！', color: 'rgb(53, 29, 25)'},
        {url: './images/slider08.jpg', title: '谁不想和小猫咪贴贴呢！', color: 'rgb(99, 72, 114)'},
    ]

    // 生成随机数
    const random = parseInt(sliderData.length * Math.random() + '')
    console.log(random)

    // 获取数据
    data = sliderData[random]

    // 获取img元素
    const img = document.querySelector('.slider-wrapper img')
    // 修改图片路径
    img.src = data.url

    // 获取p元素
    const p = document.querySelector('.slider-footer p')
    p.innerHTML = data.title

    // 修改背景颜色
    const footer = document.querySelector('.slider-footer')
    footer.style.backgroundColor = data.color

    // 修改圆点
    const li = document.querySelector(`.slider-indicator li:nth-child(${random + 1})`)
    li.classList.add('active')

</script>
</body>

</html>
```


### 操作表单元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>修改表单元素</title>
</head>
<body>
<label>
    <input type="text" class="text" value="电脑">
    <input type="checkbox" class="checkbox">
    <input type="button" class="btn" value="点我">
</label>
<!-- 修改文本框 -->
<!--<script>-->
<!--    // 获取表单元素-->
<!--    const input = document.querySelector('.text')-->

<!--    // 获取表单的内容-->
<!--    console.log(input.value)-->
<!--    -->
<!--    // 设置表单内容-->
<!--    input.value = '诺亚方舟'-->

<!--    // 修改表单类型-->
<!--    input.type = 'password'-->
<!--</script>-->


<!-- 修改勾选框 -->
<!--<script>-->
<!--    // 获取元素-->
<!--    checkbox = document.querySelector('.checkbox')-->
<!--    // 修改勾选状态-->
<!--    checkbox.checked = true-->
<!--    checkbox.disabled = true-->
<!--</script>-->


<!-- 修改按钮 -->
<script>
    const btn = document.querySelector('.btn')
    btn.disabled = true
</script>
</body>
</html>
```

### 自定义属性

自定义属性：
- 由于标签自带的属性有限，故在html5中推出了自定义属性，极大满足了开发者的需求
- 规范：自定义属性命名以`data-`开头，比如`data-id`
- 在DOM对象上使用`dataset`对象方式获取

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>自定义属性</title>
</head>
<body>
<div data-id='1' data-cmp="abc">1</div>
<div data-id='2'>我是2</div>
<div data-id='3'>3</div>
<script>
    // 获取元素
    const one = document.querySelector('div')
    console.log(one.dataset)
    /*
    id: "1"
    cmp: "abc"
     */

    // 获取属性
    console.log(one.dataset.id)
    // 1

    console.log(one.dataset.cmp)
    // abc
</script>
</body>
</html>
```


### 定时器

有两种：==间歇函数==和

#### 间歇函数

可以重复指定某段代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>间歇函数</title>
</head>
<body>
<script>
    // setInterval(函数, 时间)
    //// 开启定时器
    // setInterval(function(){
    //     console.log('一秒一次')
    // }, 1000)

    let i = 0
    fn = function (){
        i++
        document.write(i)
    }
    // setInterval会返回该定时器的编号
    let n = setInterval(fn, 1000)
    console.log(n)
    // 关闭定时器
    // clearInterval(n)
</script>
</body>
</html>
```


## 事件监听

### 入门

事件监听三要素：
- 事件源：哪个dom元素被事件触发了
- 事件类型：用什么方式触发
- 事件调用的函数：要做什么事

语法：`元素.addEventListener('事件', 函数)`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<button class="btn">按钮</button>
<script>
    const btn = document.querySelector('.btn')
    // 事件类型要加引号
    btn.addEventListener('click', function (){
        alert('北电')
    })
</script>
</body>
</html>
```


### 案例

随机抽人

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        h2 {
            text-align: center;
        }

        .box {
            width: 600px;
            margin: 50px auto;
            display: flex;
            font-size: 25px;
            line-height: 40px;
        }

        .qs {

            width: 450px;
            height: 40px;
            color: red;

        }

        .btns {
            text-align: center;
        }

        .btns button {
            width: 120px;
            height: 35px;
            margin: 0 50px;
        }
    </style>
</head>

<body>
<h2>随机点名</h2>
<div class="box">
    <span>名字是：</span>
    <div class="qs">这里显示姓名</div>
</div>
<div class="btns">
    <button class="start">开始</button>
    <button class="end">结束</button>
</div>

<script>
    // 数据数组
    const arr = ['马超', '黄忠', '赵云', '关羽', '张飞']

    // 定时器编号
    let n

    let num

    fstart = function () {
        clearInterval(n)
        n = setInterval(function () {
            num = parseInt(Math.random() * arr.length + '')
            let qs = document.querySelector('.qs')
            qs.innerHTML = arr[num]
        }, 10)
    }

    fend = function (){
        clearInterval(n)
        arr.splice(num, 1)
        if (arr.length === 1)
            start.disabled = end.disabled = true
            // start.disabled = true
    }

    const start = document.querySelector('.start')
    start.addEventListener('click', fstart)

    const end = document.querySelector('.end')
    end.addEventListener('click', fend)


</script>
</body>

</html>
```

### 事件监听版本

- DOM L0
	- 事件源.on事件 = function(){}
	- 比如`btn.onclick = function(){}`
- DOM L2
	- 事件源.addEventListener('事件', 事件处理函数)
- 区别：on方式事件会被覆盖，addEventListener方式可绑定多次，拥有事件更多特性

### 事件类型

- 鼠标事件：鼠标触发
	- click：鼠标点击
	- mouseenter：鼠标经过
	- mouseleave：鼠标离开
	- ~~mouseover：鼠标经过，但是有冒泡，不推荐使用~~
	- ~~mouseout：鼠标离开，但是有冒泡，不推荐使用~~
- 焦点事件：表单获得光标
	- focus：获取焦点
	- blur：失去焦点
- 键盘事件：键盘触发
	- Keydown：键盘按下触发
	- Keyup：键盘抬起触发
- 文本事件：表单输入触发
	- input：用户输入事件

### 案例

#### 完整轮播图

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>轮播图点击切换</title>
    <style>
        * {
            box-sizing: border-box;
        }

        .slider {
            width: 560px;
            height: 400px;
            overflow: hidden;
        }

        .slider-wrapper {
            width: 100%;
            height: 320px;
        }

        .slider-wrapper img {
            width: 100%;
            height: 100%;
            display: block;
        }

        .slider-footer {
            height: 80px;
            background-color: rgb(100, 67, 68);
            padding: 12px 12px 0 12px;
            position: relative;
        }

        .slider-footer .toggle {
            position: absolute;
            right: 0;
            top: 12px;
            display: flex;
        }

        .slider-footer .toggle button {
            margin-right: 12px;
            width: 28px;
            height: 28px;
            appearance: none;
            border: none;
            background: rgba(255, 255, 255, 0.1);
            color: #fff;
            border-radius: 4px;
            cursor: pointer;
        }

        .slider-footer .toggle button:hover {
            background: rgba(255, 255, 255, 0.2);
        }

        .slider-footer p {
            margin: 0;
            color: #fff;
            font-size: 18px;
            margin-bottom: 10px;
        }

        .slider-indicator {
            margin: 0;
            padding: 0;
            list-style: none;
            display: flex;
            align-items: center;
        }

        .slider-indicator li {
            width: 8px;
            height: 8px;
            margin: 4px;
            border-radius: 50%;
            background: #fff;
            opacity: 0.4;
            cursor: pointer;
        }

        .slider-indicator li.active {
            width: 12px;
            height: 12px;
            opacity: 1;
        }
    </style>
</head>

<body>
<div class="slider">
    <div class="slider-wrapper">
        <img src="./images/slider01.jpg" alt=""/>
    </div>
    <div class="slider-footer">
        <p>对人类来说会不会太超前了？</p>
        <ul class="slider-indicator">
            <li class="active"></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
            <li></li>
        </ul>
        <div class="toggle">
            <button class="prev">&lt;</button>
            <button class="next">&gt;</button>
        </div>
    </div>
</div>
<script>
    // 1. 初始数据
    const data = [
        {url: './images/slider01.jpg', title: '对人类来说会不会太超前了？', color: 'rgb(100, 67, 68)'},
        {url: './images/slider02.jpg', title: '开启剑与雪的黑暗传说！', color: 'rgb(43, 35, 26)'},
        {url: './images/slider03.jpg', title: '真正的jo厨出现了！', color: 'rgb(36, 31, 33)'},
        {url: './images/slider04.jpg', title: '李玉刚：让世界通过B站看到东方大国文化', color: 'rgb(139, 98, 66)'},
        {url: './images/slider05.jpg', title: '快来分享你的寒假日常吧~', color: 'rgb(67, 90, 92)'},
        {url: './images/slider06.jpg', title: '哔哩哔哩小年YEAH', color: 'rgb(166, 131, 143)'},
        {url: './images/slider07.jpg', title: '一站式解决你的电脑配置问题！！！', color: 'rgb(53, 29, 25)'},
        {url: './images/slider08.jpg', title: '谁不想和小猫咪贴贴呢！', color: 'rgb(99, 72, 114)'},
    ]
    // 2. 准备元素
    const prev = document.querySelector('.prev')
    const next = document.querySelector('.next')
    const img = document.querySelector('img')
    const p = document.querySelector('.slider-footer p')
    const footer = document.querySelector('.slider-footer')
    let timeId
    let i = 0

    // 3. 准备点击事件
    fnext = function () {
        i++
        i = i > data.length - 1 ? 0 : i
        common()

    }

    fprev = function () {
        i--
        i = i < 0 ? data.length - 1 : i
        common()
    }

    common = function () {
        img.src = data[i].url
        p.innerHTML = data[i].title
        footer.style.backgroundColor = data[i].color
        // 更换圆点
        document.querySelector('.slider-indicator .active').classList.remove('active')
        document.querySelector(`.slider-indicator li:nth-child(${i + 1})`).classList.add('active')
    }

    //4. 注册点击事件
    prev.addEventListener('click', fprev)
    next.addEventListener('click', fnext)

    // 5. 自动播放
    timeId = setInterval(function (){
        next.click()
    }, 1000)

    // 6. 鼠标经过盒子，停止定时器
    const slider = document.querySelector('.slider')
    slider.addEventListener('mouseenter', function (){
        clearInterval(timeId)
    })
    slider.addEventListener('mouseleave', function (){
        timeId = setInterval(function (){
            next.click()
        }, 1000)
    })


</script>
</body>

</html>
```



#### 小米搜索框

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        ul {

            list-style: none;
        }

        .mi {
            position: relative;
            width: 223px;
            margin: 100px auto;
        }

        .mi input {
            width: 223px;
            height: 48px;
            padding: 0 10px;
            font-size: 14px;
            line-height: 48px;
            border: 1px solid #e0e0e0;
            outline: none;
        }

        .mi .search {
            border: 1px solid #ff6700;
        }

        .result-list {
            display: none;
            position: absolute;
            left: 0;
            top: 48px;
            width: 223px;
            border: 1px solid #ff6700;
            border-top: 0;
            background: #fff;
        }

        .result-list a {
            display: block;
            padding: 6px 15px;
            font-size: 12px;
            color: #424242;
            text-decoration: none;
        }

        .result-list a:hover {
            background-color: #eee;
        }
    </style>

</head>

<body>
    <div class="mi">
        <input type="search" placeholder="小米笔记本">
        <ul class="result-list">
            <li><a href="#">全部商品</a></li>
            <li><a href="#">小米11</a></li>
            <li><a href="#">小米10S</a></li>
            <li><a href="#">小米笔记本</a></li>
            <li><a href="#">小米手机</a></li>
            <li><a href="#">黑鲨4</a></li>
            <li><a href="#">空调</a></li>
        </ul>
    </div>
    <script>
        // 获取元素
        const search = document.querySelector('[type=search]')
        const ul = document.querySelector('.result-list')
        search.addEventListener('focus', function (){
            ul.style.display = 'block'
            search.classList.add('search')
        })

        search.addEventListener('blur', function (){
            ul.style.display = 'none'
            search.classList.remove('search')
        })


    </script>
</body>

</html>
```



#### 评论回车发布

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>评论回车发布</title>
    <style>
        .wrapper {
            min-width: 400px;
            max-width: 800px;
            display: flex;
            justify-content: flex-end;
        }

        .avatar {
            width: 48px;
            height: 48px;
            border-radius: 50%;
            overflow: hidden;
            background: url(./images/avatar.jpg) no-repeat center / cover;
            margin-right: 20px;
        }

        .wrapper textarea {
            outline: none;
            border-color: transparent;
            resize: none;
            background: #f5f5f5;
            border-radius: 4px;
            flex: 1;
            padding: 10px;
            transition: all 0.5s;
            height: 30px;
        }

        .wrapper textarea:focus {
            border-color: #e4e4e4;
            background: #fff;
            height: 50px;
        }

        .wrapper button {
            background: #00aeec;
            color: #fff;
            border: none;
            border-radius: 4px;
            margin-left: 10px;
            width: 70px;
            cursor: pointer;
        }

        .wrapper .total {
            margin-right: 80px;
            color: #999;
            margin-top: 5px;
            opacity: 0;
            transition: all 0.5s;
        }

        .list {
            min-width: 400px;
            max-width: 800px;
            display: flex;
        }

        .list .item {
            width: 100%;
            display: flex;
        }

        .list .item .info {
            flex: 1;
            border-bottom: 1px dashed #e4e4e4;
            padding-bottom: 10px;
        }

        .list .item p {
            margin: 0;
        }

        .list .item .name {
            color: #FB7299;
            font-size: 14px;
            font-weight: bold;
        }

        .list .item .text {
            color: #333;
            padding: 10px 0;
        }

        .list .item .time {
            color: #999;
            font-size: 12px;
        }
    </style>
</head>

<body>
<div class="wrapper">
    <i class="avatar"></i>
    <textarea id="tx" placeholder="发一条友善的评论" rows="2" maxlength="200"></textarea>
    <button>发布</button>
</div>
<div class="wrapper">
    <span class="total">0/200字</span>
</div>
<div class="list">
    <div class="item" style="display: none;">
        <i class="avatar"></i>
        <div class="info">
            <p class="name">清风徐来</p>
            <p class="text">大家都辛苦啦，感谢各位大大的努力，能圆满完成真是太好了[笑哭][支持]</p>
            <p class="time">2022-10-10 20:29:21</p>
        </div>
    </div>
</div>

<script>
    // 获取元素
    const tx = document.querySelector('#tx')
    const total = document.querySelector('.total')

    tx.addEventListener('focus', function () {
        total.style.opacity = '1'
    })

    tx.addEventListener('blur', function () {
        total.style.opacity = '0'
    })

    tx.addEventListener('input', function () {
        total.innerHTML = `${tx.value.length}/200字`
    })


</script>

</body>

</html>
```


### 事件对象

#### 概念

事件对象也是一个对象。事件绑定的回调函数的第一个参数就是事件对象，该对象里面有事件触发时的相关信息

比如：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<button>按钮</button>
<script>
    const btn = document.querySelector('button')
    btn.addEventListener('click', function (e){
        console.log(e)
    })
</script>
</body>
</html>
```

![[点击事件信息.png]]


#### 常用属性

- type：获取当前的事件类型
- clientX/clientY：获取光标相对于浏览器可见窗口左上角的位置
- offsetX/offsetY：获取光标相对于当前DOM元素左上角的位置
- key：用户按下的键盘值


### 环境对象

环境对象this，就是当前所调用函数的对象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<button>按钮</button>
<script>
    const btn = document.querySelector('button')
    console.log(this)
    // Window
    btn.addEventListener('click', function (){
        console.log(this)
        // <button>按钮</button>
    })
</script>
</body>
</html>
```


### 回调函数

就是函数中的参数，存在函数参数

比如`setInterval`、`addEventListener`


### 案例-tab栏切换

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>tab栏切换</title>
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    .tab {
      width: 590px;
      height: 340px;
      margin: 20px;
      border: 1px solid #e4e4e4;
    }

    .tab-nav {
      width: 100%;
      height: 60px;
      line-height: 60px;
      display: flex;
      justify-content: space-between;
    }

    .tab-nav h3 {
      font-size: 24px;
      font-weight: normal;
      margin-left: 20px;
    }

    .tab-nav ul {
      list-style: none;
      display: flex;
      justify-content: flex-end;
    }

    .tab-nav ul li {
      margin: 0 20px;
      font-size: 14px;
    }

    .tab-nav ul li a {
      text-decoration: none;
      border-bottom: 2px solid transparent;
      color: #333;
    }

    .tab-nav ul li a.active {
      border-color: #e1251b;
      color: #e1251b;
    }

    .tab-content {
      padding: 0 16px;
    }

    .tab-content .item {
      display: none;
    }

    .tab-content .item.active {
      display: block;
    }
  </style>
</head>

<body>
  <div class="tab">
    <div class="tab-nav">
      <h3>每日特价</h3>
      <ul>
        <li><a class="active" href="javascript:;">精选</a></li>
        <li><a href="javascript:;">美食</a></li>
        <li><a href="javascript:;">百货</a></li>
        <li><a href="javascript:;">个护</a></li>
        <li><a href="javascript:;">预告</a></li>
      </ul>
    </div>
    <div class="tab-content">
      <div class="item active"><img src="./images/tab00.png" alt="" /></div>
      <div class="item"><img src="./images/tab01.png" alt="" /></div>
      <div class="item"><img src="./images/tab02.png" alt="" /></div>
      <div class="item"><img src="./images/tab03.png" alt="" /></div>
      <div class="item"><img src="./images/tab04.png" alt="" /></div>
    </div>
  </div>

<script>
  // 需求，鼠标放在哪里，就显示哪个

  // 获取元素
  const qs = document.querySelectorAll('a')
  const items = document.querySelectorAll('.item')
  // 绑定事件
  for(let i = 0;i<qs.length;i++){
    qs[i].addEventListener('mouseenter', function (){
      document.querySelector('.tab-nav .active').classList.remove('active')
      this.classList.add('active')

      document.querySelector('.tab-content .active').classList.remove('active')
      items[i].classList.add('active')
    })
  }
</script>

</body>

</html>
```


### 案例-全选反选

```html
<!DOCTYPE html>

<html lang="en">

<head>
    <meta charset="UTF-8">
    <title></title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        table {
            border-collapse: collapse;
            border-spacing: 0;
            border: 1px solid #c0c0c0;
            width: 500px;
            margin: 100px auto;
            text-align: center;
        }

        th {
            background-color: #09c;
            font: bold 16px "微软雅黑";
            color: #fff;
            height: 24px;
        }

        td {
            border: 1px solid #d0d0d0;
            color: #404060;
            padding: 10px;
        }

        .allCheck {
            width: 80px;
        }
    </style>
</head>

<body>
<table>
    <tr>
        <th class="allCheck">
            <input type="checkbox" name="" id="checkAll"> <span class="all">全选</span>
        </th>
        <th>商品</th>
        <th>商家</th>
        <th>价格</th>
    </tr>
    <tr>
        <td>
            <input type="checkbox" name="check" class="ck">
        </td>
        <td>小米手机</td>
        <td>小米</td>
        <td>￥1999</td>
    </tr>
    <tr>
        <td>
            <input type="checkbox" name="check" class="ck">
        </td>
        <td>小米净水器</td>
        <td>小米</td>
        <td>￥4999</td>
    </tr>
    <tr>
        <td>
            <input type="checkbox" name="check" class="ck">
        </td>
        <td>小米电视</td>
        <td>小米</td>
        <td>￥5999</td>
    </tr>
</table>
<script>
    // 需求：全选
    const check = document.querySelector('#checkAll')
    const cks = document.querySelectorAll('.ck')

    // 全选和全不选
    check.addEventListener('click', function () {
        for (let i = 0; i < cks.length; i++) {
            cks[i].checked = this.checked
        }
    })

    // 小按钮控制大按钮，当单选按钮都选上时，全选按钮也选上
    for (let j = 0; j < cks.length; j++) {
      cks[j].addEventListener('click', function (){
          check.checked = cks.length === document.querySelectorAll('.ck:checked').length
      })
    }

</script>
</body>

</html>
```


## 事件流

分为两个阶段：捕获阶段和冒泡阶段

捕获阶段：从父到子
冒泡阶段：从子到父

### 事件捕获

开启事件捕获，只需要在`addEventListener()`的第三个参数设置为true就行了。

简单理解：当一个元素触发事件后，会依次向下调用所有父级元素的 ==同名事件==

### 事件冒泡

开启事件捕获，只需要在`addEventListener()`的第三个参数设置为false就行了。

>默认就是false

简单理解：当一个元素触发事件后，会==依次向上==调用所有父级元素的 ==同名事件==

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
    .father {
        width: 400px;
        height: 400px;
        background-color: #0099cc;
    }

    .son {
        width: 200px;
        height: 200px;
        background-color: #424242;
    }
</style>
<body>
<div class="father">
    <div class="son"></div>
</div>
<script>
    document.addEventListener('click', function (){
        alert('我是爷爷')
    }, true)
    document.querySelector('.father').addEventListener('click', function (){
        alert('我是父亲')
    }, true)
    document.querySelector('.son').addEventListener('click', function (){
        alert('我是儿子')
    }, true)
</script>
</body>
</html>
```


### 阻止冒泡/捕获

利用事件对象的`stopPropagation()`方法，即可阻断事件流的传播

```html
<script>
document.querySelector('.son').addEventListener('click', function (e){
        alert('我是儿子')
        e.stopPropagation()
    }, true)
</script>
```


### 阻止默认行为

比如链接的跳转、比如表单的提交
如果表单填写的内容不符合要求，肯定是不可以提交的

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="https://.itcast.cn">
    <input type="submit" value="注册">
</form>
<a href="https://www.baidu.com">跳转到百度</a>
<script>
    const form = document.querySelector('form')
    form.addEventListener('submit', function (e){
        // 阻止提交表单
        e.preventDefault()
    })
    const a = document.querySelector('a')
    a.addEventListener('click', function (e){
        // 阻止跳转
        e.preventDefault()
    })
</script>
</body>
</html>
```

### 事件解绑

事件可以添加，也可以取消

- L0：on事件 = null
- L2：元素.removeEventListener('事件类型', 函数)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<button>按钮</button>
<script>
    fn = function () {
        alert('哒咩')
    }
    const btn = document.querySelector('button')
    btn.addEventListener('click', fn)
    // 事件解绑
    btn.removeEventListener('click', fn)
</script>
</body>
</html>
```

>[!note] 注意
>匿名函数无法被解绑


## 事件委托

事件委托是利用事件流的特征解决一些开发需求的知识技巧

- 优点：减少注册次数，提高程序性能。比如之前我们给ul里的li标签注册点击事件，是利用循环依次添加的。但是学了事件委托之后，我们就可以把点击事件注册到ul标签身上。根据事件冒泡的特点，如果我们点击了子元素，那么后面是会回到父元素身上的，从而触发了父元素的事件
- 原理：利用事件冒泡的特点
- 实现：事件对象.target.tagName可以获得真正触发事件的元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
    <p>我是p</p>
</ul>
<script>
    const ul = document.querySelector('ul')
    ul.addEventListener('click', function (e){
        if(e.target.tagName === 'LI')
            e.target.style.color = 'red'
    })
</script>
</body>
</html>
```


## 其他事件


### 页面加载事件

给window添加==load==事件，意味着会等待页面的样式、图片都加载完成之后，再执行js代码

此时的`<script>`标签就可以写在`<head>`中了，甚至可以说是任意位置

不管是window，给某个特定资源添加页面加载事件也成，比如`<img>`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script>
    // 添加页面加载事件
    window.addEventListener('load', function (){
        // 资源加载完成之后，给按钮添加事件
        document.querySelector('button').addEventListener('click', function (){
            alert(11)
        })
    })
    
    img.addEventListener('load', function (){
        // 等待图片加载完成之后，再添加事件
    })
</script>
<button>按钮</button>
</body>
</html>
```

但是真要给window添加页面加载事件的话，等要等个半天

所以，我们可以给`document`添加`DOMContentLoad`事件，意味着只要html文档被完全加载和解析完成之后，就可以实现交互了，无需等待图片、样式表完全加载出来。

```html
<script>
    document.addEventListener('DOMContentLoaded', function (){
        // html文档内容被加载和解析出来后，就可以添加事件
    })
</script>
```


### 页面滚动事件

滚动条在滚动时触发的事件，不论是往上滚还是往下滚

给window添加`scroll`事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
    body {
        height: 3000px;
    }
</style>
<body>
<script>
    window.addEventListener('scroll', function (){
        console.log('滚啊滚')
    })
</script>
</body>
</html>
```

页面滚动事件具有一些属性

- scrollLeft：往左滚的距离（即距最左侧的长度）
- scrollTop：往下滚的距离（即距最上方的长度）

```html
<script>
    // 给某个元素添加滚动事件
    div.addEventListener('scroll', function (){
        console.log(div.scrollTop)
    })
</script>
```

有时候会有这么一种效果：往下滚动一定像素，就会显示某些东西。否则不显示。

比如：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
    body {
        height: 3000px;
    }

    div {
        margin: 300px;
        overflow: scroll;
        width: 200px;
        height: 200px;
        border: 1px solid #000;
    }
</style>
<body>
<div>
    女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
    女大十八变女大十八变女大十八变女大十八变女大十八变
</div>
<script>

    const div = document.querySelector('div')

    window.addEventListener('scroll', function (){
        // console.log('滚啊滚')
        const n = document.documentElement.scrollTop
        if (n >= 100)
            div.style.display = 'none'
        else
            div.style.display = 'block'
    })
</script>
</body>
</html>
```


另外，scrollLeft和scrollTop是==可读写的==

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
    body {
        height: 3000px;
    }
</style>
<body>
<script>
    // document.documentElement：用于获取<html>标签
    
    // 将初始位置设置为800
    document.documentElement.scrollTop = 800
    window.addEventListener('scroll', function (){
        const n = document.documentElement.scrollTop
        // n得到的是数字型，不带单位
        console.log(n)
    })
</script>
</body>
</html>
```

### 小兔鲜儿导航案例

```html
<script>
    // 滚动显示电梯导航
    const elevator = document.querySelector('.xtx-elevator')
    window.addEventListener('scroll', function () {
        const n = document.documentElement.scrollTop
        elevator.style.opacity = n >= 300 ? 1 : 0
    })

    // 点击返回，回到页面顶部
    const backTop = document.querySelector('#backTop')
    backTop.addEventListener('click', function (){
        // document.documentElement.scrollTop = 0
        window.scrollTo(0, 0)
    })
</script>
```


### 页面尺寸事件

用户修改页面大小时触发

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
</style>
<body>
<div>
    嘿嘿嘿
</div>
<script>
    const div = document.querySelector('div')
    window.addEventListener('resize', function (){
        div.innerHTML = '好痛哦'
        // 检测屏幕宽度，不包含边框
        let w = document.documentElement.clientWidth
        console.log(w)
    })
    div.innerHTML = '嘿嘿嘿'
</script>
</body>
</html>
```


## 元素尺寸位置

即获取元素所在的位置

之前我们定位都是自己算出来的，这次我们可以使用js来动态获取元素的位置

==获取宽高==：
- 获取元素自身的宽高，包含元素自身设置的宽高、padding、border
- offsetWidth和offsetHeight
- 获取出来的是数值，方便计算
- 注意：获取的是可视宽高。如果盒子是隐藏的，获取的结果是0

==获取位置==：
- 获取元素距离自己定位==父级元素==的左、上距离
- ==offsetLeft和offsetTop 是只读属性==

### 其他获取位置的方法

`element.getBoundingClientRect()`

该方法返回元素的大小及其相对于视口的位置

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
    body {
        height: 2000px;
    }
    div {
        width: 200px;
        height: 200px;
        background-color: pink;
        margin: 100px;
    }
</style>
<body>
<div>

</div>
<script>
    const div = document.querySelector('div')
    console.log(div.getBoundingClientRect())
    /*
    bottom: -551
    height: 200
    left: 108
    right: 308
    top: -751
    width: 200
    x: 108
    y: -751
     */
</script>
</body>
</html>
```

### 小兔鲜儿导航代码优化

指定滚动到某个位置，就显示电梯导航

```html
<srcipt>
<script>
    const entry = document.querySelector('.xtx_entry')
    // 滚动显示电梯导航
    const elevator = document.querySelector('.xtx-elevator')
    window.addEventListener('scroll', function () {
        const n = document.documentElement.scrollTop
        elevator.style.opacity = n >= entry.offsetTop ? 1 : 0
    })
    // 点击返回，回到页面顶部
    const backTop = document.querySelector('#backTop')
    backTop.addEventListener('click', function (){
        // document.documentElement.scrollTop = 0
        window.scrollTo(0, 0)
    })
</script>
</script>
```



## 日期对象


### 实例化

用关键字`new`操作出来的对象，都叫做实例化

```html
<script>
    // 获取当前时间
    const date = new Date()
    console.log(date)

    // 指定时间（可用作倒计时）
    const pointDate = new Date('2024-05-08 08:30:20')
    console.log(pointDate)
</script>
```

### 日期对象方法

形式为：`getXxx()`

```html
<script>
    const date = new Date('2024-05-08 08:30:20')
    // 得到8.因为是5月8号
    console.log(date.getDate())
    // 得到5
    console.log(date.getMonth())
    // 2024
    console.log(date.getFullYear())
    // 得到星期
    console.log(date.getDay())
    console.log(date.getHours());
    console.log(date.getMinutes());
    console.log(date.getSeconds());
</script>
```


>[!note] 注意
>getMonth()返回值为0~11，其中0代表12月
>getDay()返回值为0~6，其中0代表周日

### 格式化日期输出

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div>

</div>
<script>

    const div = document.querySelector('div')

    // 方法一，自己写
    // getDate = function (){
    //     const date = new Date()
    //     let y = date.getFullYear()
    //     let M = date.getMonth()
    //     let d = date.getDay()
    //     let h = date.getHours()
    //     let m = date.getMinutes()
    //     let s = date.getSeconds()
    //
    //     M = M < 10 ? '0' + M : M
    //     d = d < 10 ? '0' + d : d
    //     h = h < 10 ? '0' + h: h
    //     m = m < 10 ? '0' + m : m
    //     s = s < 10 ? '0' + s : s
    //
    //     div.innerHTML = `${y}-${M}-${d} ${h}:${m}:${s}`
    // }
    // getDate()
    // setInterval(getDate, 1000)

    // 方法二，使用对象函数
    const date = new Date()
    div.innerHTML = date.toLocaleString()
    setInterval(function (){
        const date = new Date()
        div.innerHTML = date.toLocaleString()
    }, 1000)

</script>
</body>
</html>
```


### 时间戳使用

三种方式

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script>
    const date = new Date()

    // 三种方式
    console.log(date.getTime());
    console.log(+new Date());
    console.log(Date.now());
</script>
</body>
</html>
```



## 节点操作

### DOM节点

DOM文档树中的每一个内容都成为节点

节点类型：
- 元素节点：所有的标签，比如div
- 属性节点：比如href
- 文本节点：所有的文本
- 其他


### 查找节点

查找父节点：`元素.parentNode`

查找子节点：
- `childNodes`：获得所有子节点，包括文本节点（空格、换行）、注释节点等
- `children`：仅获得所有元素节点，返回的还是一个伪数组（querySelectorAll()方法得到的也是个伪数组）

查找兄弟节点：
- `nextElementSibling`：获取下一个兄弟节点
- `previousElementSibling`：获取上一个兄弟节点


### 增加节点

分为两步：创建节点(`createElement`)和追加节点(`appendChild`或`insertBefore`)

```html
<body>
<ul>
</ul>
<script>
    // 创建节点
    const li1 = document.createElement('li')
    li1.innerHTML = '新生'

    // 追加方式一，追加到父元素的最后一个子元素
    document.body.appendChild(li1)

    const li2 = document.createElement('li')
    li2.innerHTML = '我才是老大'
    // 追加方式二，放到父元素的第一个子元素
    document.body.insertBefore(li2, document.body.children[0])
    // insertBefore（插入元素， 插入到哪个元素的前面）
</script>
```


### 克隆节点

元素.`cloneNode(bool)`：
- 参数为true，则代表克隆时会包含后代节点一起克隆
- 参数为false，则只会克隆当前节点
- 默认为false


### 删除节点

要删除节点必须通过==父元素删除==

语法：`父元素.removeChild(要删除的元素)`


### M端事件

M端：即移动端

M端跟PC端的主要区别在于，M端可以触屏

常见的触屏事件：
- touchstart：手指触摸到一个DOM元素时触发
- touchmove：手指在一个DOM元素上滑动时触发
- touchend：手指在一个DOM元素上移开时触发

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<style>
    div {
        width: 200px;
        height: 200px;
        background-color: pink;
    }
</style>
<body>
<div>
</div>
<script>
    const div = document.querySelector('div')
    div.addEventListener('touchstart', function (){
        console.log('摸我')
    })
    // 这个touchmove需要鼠标长按，然后移动
    div.addEventListener('touchmove', function (){
        console.log('别停')
    })
    div.addEventListener('touchend', function (){
        console.log('别走')
    })
</script>
</body>
</html>
```

# Windows对象

## BOM

BOM：即浏览器对象模型

![[BOM对象模型.png]]

- window对象是一个全局对象
- 像document、alert()、console.log()这些都是window的属性，基本BOM的属性和方法都是window的
- 所有通过var定义在全局作用域中的变量、函数都会变成window对象的属性和方法
- window对象下的属性和方法调用的时候可以省略window

## 定时器-延时函数

js中用来让代码延迟执行的函数，叫做setTimeout

语法：`setTimeout(回调函数, 等待的毫秒数)`

>[!note] 区别
>setTimeout只会执行一次
>setInterval会多次执行回调函数

**清除延时函数**：

```javascript
let timer = setTimeout(..,..)
// 一般用不上，但是总有特殊用法
clearTimeout(timer)
```

## js执行机制

js有两个东西：==执行栈==和==消息队列==

js会优先执行执行栈中的函数，然后才会执行消息队列中的。

一般普通时间、加载和定时器就会放进消息队列中


## location对象

location的数据类型是对象，它拆分保存了URL地址的各个组成部分

比如打开下面的网页，将会跳转到百度

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script>
    console.log(location)
    location.href = 'https://www.baidu.com'
</script>
</body>
</html>
```

location对象的常用属性和方法：
- search：获取路径参数
- href：获取url
- hash：获取地址中的哈希值。符号#后面的部分（后期vue路由的铺垫，经常用于不刷新页面，显示不同页面，比如网易云音乐）
- `reload()`：用于刷新当前页面，传入参数为true时，表示强制刷新当前页面。类似ctrl + f5.
	- `location.reload(true)`

## navigator对象

navigator对象中记录了浏览器自身的相关信息。比如`UserAgent`

常用属性和方法：
- 通过UserAgent检测浏览器的版本及平台
```html
<script>
    !function (){
        const userAgent = navigator.userAgent
        const android = userAgent.match(/(Android);?[\s\/]+([\d.]+)?/)
        const iphone = userAgent.match(/(iPhone\sOS);\s([\d_]+)/)
        if (android || iphone)
            location.href = 'http://m.itcast.cn'
    }()
</script>
```


## history对象

该对象主要管理历史记录，该对象与浏览器地址栏的操作相对应，如前进、后退、历史记录等

常用属性和方法：
- `back()`：后退功能
- `forward()`：前进功能
- `go(参数)`：参数1表示前进一个页面，如果是-1则表示后退一个页面

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<button>后退</button>
<button>前进</button>
</body>
<script>
    const back = document.querySelector('button:first-child')
    const forward = back.nextElementSibling
    back.addEventListener('click', function (){
        // 后退
        history.back()
    })
    forward.addEventListener('click', function (){
        // 前进
        history.forward()
    })
</script>
</html>
```
  



# 本地存储

## localStorage

作用：可以将数据永久存储在本地，除非手动删除，否则关闭页面也会存在

特性：
- 可以多窗口共享
- 以键值对的形式存储

语法：
- 存/改数据：`localStorage.setItem(key, value)`
- 取数据：`localStorage.getItem(key)`
- 删数据：`localStorage.removeItem(key)`

>[!note] 注意
>本地存储只能字符串，如果想要取出数字的话，记得转为数字型


## sessionStorage

特性：
- 生命周期为关闭浏览器窗口
- 在同一个窗口下数据可以共享
- 以键值对的形式存储用法跟localStorage基本相同

## 存储复杂数据类型

如果要存储复杂数据类型的话，是不可以直接存的。

否则只会得到`[Object object]`

需要借助`JSON.stringify(复杂数据类型)`将对象转为字符串

取出复杂数据的话，需要借助`JSON.parse(str)`将json字符串转为对象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<script>
    // localStorage.setItem('name', '胡桃')
    // localStorage.getItem('name')
    // localStorage.removeItem('name')

    obj = {
        'name': '胡桃',
        'age': 18
    }

    localStorage.setItem('obj', JSON.stringify(obj))
    const str = localStorage.getItem('obj')
    console.log(JSON.parse(str))
</script>
</body>
</html>
```

## map()和join()

这是两个常用的数组处理方法

map()可以给数组元素进行统一处理

```html
<script>
    const arr = ['红', '黄', '绿']
    const arr1 = arr.map(function (ele, index){
	    // 打印元素
	    console.log(ele)
	    // 打印索引
	    console.log(index)
        return ele + '色'
    })
    console.log(arr1)
    // ['红色', '黄色', '绿色']
</script>
```

join()方法可以将一个数组拼接为字符串，可以传递参数，填充在元素之间

```html
<script>
    const arr = ['红', '黄', '绿']
    const a = arr.join(', ')
    const b = arr.join('')
    console.log(a)
    // 红, 黄, 绿
    console.log(b)
    // 红黄绿
</script>
```


# 进阶

## 作用域进阶

### 作用域链

作用域链本质上是底层的==变量查找机制==

- 在函数被执行时，会优先查找当前函数作用域中查找变量
- 如果当前作用域查找不到则会依次==逐级查找父级作用域==直到全局作用域

比如：
```javascript
// 全局作用域
let a = 1
let b = 2
// 局部作用域
function f(){
    let a = 1
    // 局部作用域
    function g() {
        a = 2
        console.log(a)
    }
    g() // 调用g()
}
f() // 调用f()
```

这个就是一个作用域链，从上到下依次是全局、局部、局部。那么中间的`console.log(a)`输出的值是多少呢？

>g -> f -> global

结果是`2`，该代码会往上查找变量a，找到离它最近的那个。

总结：
- 嵌套关系的作用域串联形成了作用域链
- 相同作用域链中按着从小到大的规则查找变量
- 子作用域能够访问父作用域，父级作用域无法访问子级作用域


### GC

JS环境中分配的内存，一般有如下生命周期：
- 内存分配：声明对象、函数、变量的时候，系统会自动给他们分配内存
- 内存使用：即读写内存，也即是使用变量、函数等
- 内存回收：使用完毕后，垃圾回收器自动回收不再使用的内存

说明：
- ==全局变量一般不会回收（关闭页面回收）==
- 局部变量会被自动回收

>[!note] 内存泄漏
>程序中分配的==内存==由于某种原因程序未释放或者无法释放叫做内存泄漏


### 闭包

概念：一个函数对周围状态的引用捆绑在一起，内层函数中访问到其外层函数的==作用域==

==没错，闭包是全局作用域和局部作用域之外的第三种作用域==

简单理解：**闭包 = 内层函数 + 外层函数的变量**

```javascript
function outer() {
    const a = 1
    function f() {
        console.log(a)
    }
    f()
}
outer()
// 此时f()函数使用到了outer()函数的局部变量，而f()又是outer()的内部函数。相当于outer()是一个人，而f()是他身上带着的一个背包。
```

闭包的作用：封闭数据，提供操作，外部也可以访问函数内部的变量

基本格式：
```javascript
function outer() {
    let a = 1
    return function f() {
        console.log(a)
    }
}
const fun = outer()
fun()
```


闭包应用：实现数据的私有，比如我们要做一个统计函数调用次数，函数调用一次，就++

通常情况下，我们会声明一个全局变量，然后在函数中++

但是这种情况下，数据是很容易被篡改的，因为是全局变量，谁都可以使用。

而使用了闭包，就可以实现数据的私有：

```javascript
function fn() {
    let count = 1
    function fun() {
        count++
        console.log(`函数被调用了${count}次`)
    }
    return fun
}

const res = fn()
res()
```

### 变量提升

使用`var`声明变量时，该变量会被提升到当前作用域的最前面，然后就会产生一种现象：变量没有被声明，也可以被访问

使用`let`声明的话，该现象就不存在了。

同理，还有函数提升，跟变量提升类似。


## 函数进阶

### 函数参数

#### 动态参数

`arguments`是函数内部内置的伪数组变量，它包含了调用函数时传入的所有实参

```javascript
    function sum() {
        let s = 0
        for (let i = 0; i < arguments.length; i++) {
            s += arguments[i]
        }
        console.log(s)
        // 45
    }

    sum(1, 2, 3, 4, 5, 6, 7, 8, 9)
```

总结：
- arguments是一个伪数组，只存在于函数中
- arguments的作用是动态获取函数的实参
- 可以通过for循环依次得到传递过来的实参

#### 剩余参数

- `...`是语法符号，置于最末函数形参之前，用于获取多余的实参
- 借助`...`获取的剩余实参，是个==真数组==

```javascript
    function config(...abc){
        console.log(abc)
    }

    config(1,2,3,4,5,6,7)
    //[1, 2, 3, 4, 5, 6, 7]
```


#### 展开运算符

`...`其实就是展开运算符，它可以将一个数组拆分一个个元素进行函数传递。且不会修改原数组

```javascript
    const arr = [1, 2, 3, 4, 5]
    function fn() {
        let sum = 0
        for(let i=0;i<arguments.length;i++){
            sum+=arguments[i]
        }
        console.log(sum)
    }
    fn(...arr)
    // 15
```


使用场景：求数组最大最小值、合并数组等

```javascript
// 求最大值
const arr = [1, 3, 3, 3, 5]
console.log(Math.max(...arr))
// 5


// 合并数组
const arr1 = [1, 2, 3]
const arr2 = [4, 5, 6]
const newArr = [...arr1, ...arr2]
// [1, 2, 3, 4, 5, 6]
```

### 箭头函数

引入箭头函数的目的是更简短的函数写法，且不绑定this，箭头函数的语法比函数表达式更简洁

使用场景：箭头函数更使用于那些本来需要匿名函数的地方


#### 基本语法

```javascript
    // 无参数
    const f0 = () =>{}

    // 一个参数
    const f1 = (x) =>{}

    // 只有一个参数是括号可以省略
    const f1_ = x =>{}

    // 函数体只有一行代码时，花括号可以省略，以及return
    const f2 = x => x + x
    console.log(f2(2))

    // 箭头函数可以直接返回一个对象，但是需要用括号裹住
    const f3 = (name) =>({name:name})
    console.log(f3('刘德华'))
    // "name": "刘德华"
    
```


#### 参数

没有动态参数`arguments`，但是有`...arr`

#### this

箭头函数里面没有this，如果在箭头函数中使用this的话，那么这个this一定是上一个作用域的this

```javascript
    const obj = {
        uname: 'sensei',
        say: function (){
            console.log(this)
            // obj
            let i = 10
            const count = () =>{
                console.log(this)
                // obj
            }
            count()
        }
    }
    obj.say()
```


## 解构赋值

### 基本使用

数组解构就是将数组的单元值快速批量赋值给一系列变量的简介语法

语法：`const [a, b, c] = [1, 2, 3]`

使用：
- 快速交换变量值：
```javascript
let a = 1
// 这里的分号必须加
let b = 2;
[b, a] = [a, b]
```

解释下为什么要加分号，目前遇到过两种需要加分号的情况，立即执行函数和上面这种：
```javascript
    // 立即执行函数
    (function () {
    })();
    (function () {
    })();

    // 使用数组
    const str = 'abc';
    [1, 2, 3].map((item) => {
        console.log(item)
    })
    // 这里如果不加分号的话，系统会认为'abc'[1, 2, 3]才是一个整体
```

### 细节

```javascript
    // // 1. 变量多，赋值少
    // const [a, b, c] = [1, 2]
    // console.log(a)  // 1
    // console.log(b)  // 2
    // console.log(c)  // undefined

    // 2. 变量少，赋值多
    // const [a, b, c] = [1, 2, 3, 4]
    // console.log(a)  // 1
    // console.log(b)  // 2
    // console.log(c)  // 3

    // 3. 剩余参数解决变量少，赋值多
    // const [a, b, ...c] = [1, 2, 3, 4]
    // console.log(a)  // 1
    // console.log(b)  // 2
    // console.log(c)  // [3, 4]

    // 4. 防止 undefined 传递单元值的情况，可以设置默认值
    // const [a = 0, b = 1] = []

    // 5. 按需导入
    // const [a, b, , c] = [1, 2, 3, 4]
    // a=1, b=2, c=4

    // 多维数组解构
    const [a, b, [c, d]] = [1, 2, [3, 4]]
    // a=1, b=2, c=3, d=4
```

### 对象解构

类似

```javascript
    // 默认情况下，变量名跟属性名要一致
    // const {name, age} = {name: '胡桃', age: 18}
    // console.log(name)
    // console.log(age)

    // 改名也可以
    // const {name:username, age}= {name: '胡桃', age: 18}
    // console.log(username)
    // console.log(age)

    // 数组对象解构
    // const pig = [
    //     {
    //         name:'佩奇',
    //         age:6
    //     },
    //     {
    //         name:'乔治',
    //         age:4
    //     }
    // ]
    // const [{name:name1, age:age1}, {name:name2, age:age2}] = pig
    // console.log(name1)  // 佩奇
    // console.log(name2)  // 乔治
    // console.log(age1)   // 6
    // console.log(age2)   // 4

    // 多级对象解构
    const pig = {
        name:'佩奇',
        famliy: {
            'father': '爸爸',
            'mother': '妈妈',
            'sister': '姐姐'
        }
    }
    const {name, famliy:{father, mother, sister}} = pig
    console.log(name)
    console.log(father)
    console.log(mother)
    console.log(sister)
```

### forEach()函数

该函数用于遍历每个元素，跟map()的最大区别在于forEach()没有返回值

```javascript
    const arr = [1, 2, 3]
    arr.forEach((ele, index) => {
        console.log(ele)
        // 1 2 3
        console.log(index)
        // 0 1 2
    })
```


### fliter()函数

该函数用于过滤符合条件的数组，并返回该数组

```javascript
    const arr = [34, 56, 44]
    // const newArr = arr.filter(item =>{
    //     return item > 40
    // })
    const newArr = arr.filter(item => item > 40)
    console.log(newArr)
```


## 对象

### 创建对象

三种方式：
- 字面量创建：`const o = {name: '胡桃', age: 18}`
- 利用`new Object`：`const o = new Object({name: '胡桃'})`
- 利用构造函数进行创建

### 构造函数

构造函数语法：大写字母开头的函数

创建构造函数：

```javascript
    function Op(name, age, address){
        this.name = name
        this.age = age
        this.address = address
    }
    function T(){}
    // 创建对象
    const HuTao = new Op('胡桃', 18, '璃月')
    console.log(HuTao)
    
    // 没有参数时创建对象，构造函数可以省略括号
    const t = new T
```

说明：
- 使用`new`关键字调用函数的行为称为==实例化==
- 实例化构造函数时没有参数时可以省略（）
- 构造函数内部无需写return，返回值即为新创建的对象
- 构造函数内部的return返回的值无效，所以不要写return
- new Object 和 new Date() 也是实例化构造函数


## 包装类

### Object

它代表一个对象类

有三种常用的静态方法：
- `Object.keys(o)`：获取所有的属性名
- `Object.values(o)`：获取所有的属性值
- `Object.assign(o, {键值对})`：在o的基础上，再添加一些键值对，并返回新的对象（用来批量添加属性）


### Array

核心方法：
- `forEach()`：用来遍历数组
- `filter()`：根据条件过滤数组
- `map`：对数组进行批处理
- `reduce`：累计器

这里写一下`reduce`的语法：

```javascript
    const arr = [2, 19, 34]
    const total = arr.reduce((prev, current)=>prev + current, 10)
    console.log(total)
    // 2 + 19 + 34 + 10 = 65
```

其他常用方法：

见文档[Array的其他方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)


### String

见文档[String的实例方法](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)



### Number

核心方法`toFixed()`：设置保留小数位的长度

```javascript
const price = 12.34567
console.log(price.toFixed(2))
// 12.35
```


## 原型

使用`prototype`关键字，作用是可以把一个方法声明为某一类对象的==类方法==，节省内存空间

用法：

```javascript
Star.prototype.sing = function(){

}
```


此外，使用prototype还可以用于==扩展函数==

比如我想要Array类有一个Max()方法，而Array类本身是没有的，那么我们就可以这么搞：

```javascript
Array.prototype.Max = function() {
	return Math.max(...this)
}
```


后面感觉用不上，先不学了。




