## String

### api

位于`java.lang.String`

```java
String str = "Halory";


// 1. 获取字符串的长度
str.length();

// 2. 获取指定索引的字符串
str.charAt(0);

// a. 组合技: 遍历字符串
for(int i = 0; i < str.length(); i++){
    System.out.println(str.charAt(i));
}

// 3. 将字符串转为字符数组
char[] charArray = str.toCharArray();
for (char c : charArray) {
    System.out.println(c);
}

// 4. 判断字符串内容是否一致
boolean isEqual = str.equals("Halory");   // true
String str0 = "Halory";
isEqual = str == str0;						 // true
isEqual = str == "Halory";					// true
String s1 = "Halory";
String s2 = "Halory";
isEqual = s1 == s2;							// false
```

> 需要注意的是：`str == "Halory"` 返回的结果是 `true`，说明这样比较也是可以的。只不过是**针对字面常量**。
>
> 如果是创建了两个对象（**一定是通过 new 得到**）的话，那么即使内容一致，通过`==`比较得到的结果为`false`。

```java
// 5. 忽略大小写比较
str.equalsIgnoreCase("hALORY");		// true

// 6. 截取字符串内容（左闭右开）
String sub1 = str.substring(0);					// Halory
String sub2 = str.substring(0, 2);				// Ha

// 7. 替换内容
String replaceStr = str.replace("H", "h");		// halory

// 8. 判断字符串是否包含子串
boolean isContains = str.contains("ry");		// true

// 9. 字符串以子串开头
boolean isStartWith = str.startsWith("Halo");	// true

// 10. 分割字符串
String[] splits = str.split("l");
```





### 特点

- `String`对象的内容**不可改变**，被称为不可变字符串对象

```java
// 对字符串进行字面量拼接，其实在该过程中，字符串常量池中会增加一个新的对象，然后再将当前的变量指向这个对象
public static void main(String[] args) {
    String name = "Halory";
    System.out.println(name.hashCode());        // -2140759133


    name += "大佬";
    System.out.println(name.hashCode());        // 20535816
}
```



- 只要是以`""`方式字符串对象，会存储到**字符串常量池**，且相同内容的字符串只存储一份（怪不得上面直接用`==`比较字符串的结果会出现`true`，因为地址一致）
- 通过`new`方式创建的字符串对象，每一次都会产生一个新的对象，并且放在**堆内存**中
- 对于多个字符拼接成的字符串字面量，Java中存在**优化机制**，比如下面的`"a" + "b" + "c"`，在编译的时候会被直接转为`"abc"`，以提高程序的性能：

```java
 public static void main(String[] args) {
        String s1 = "abc";
        String s2 = "a" + "b" + "c";
        System.out.println(s1 == s2); // true
    }
```



### 案例

```java
public static void main(String[] args) {
    Scanner sc = new Scanner(System.in);
    System.out.println("请输入用户名: ");
    String username = sc.nextLine();
    System.out.println("请输入密码: ");
    String password = sc.nextLine();
    System.out.println(username);
    System.out.println(password);
}
```





## ArrayList

位于`java.util.ArrayList`

### api

```java
public static void main(String[] args) {
    // 1. 创建一个集合
    ArrayList<String> list = new ArrayList<>();

    // 2. 添加元素
    list.add("Java");
    list.add("Python");

    // 3. 根据索引获取元素
    System.out.println(list.get(1)); // Python

    // 4. 集合大小
    System.out.println(list.size()); // 2

    // 5. 移出集合的某个元素（根据索引）
    System.out.println(list.remove(1)); // Python

    // 6. 移出集合中的某个元素（根据元素内容，并且默认删除的是第一次出现的该元素）
    System.out.println(list.remove("Java")); // true

    // 7. 修改某个索引未知的数据，并且返回旧值
    list.add("Java");
    System.out.println(list.set(0, "Kotlin")); // Java
}
```





## Math

位于`java.lang.Math`，提供的是静态方法



