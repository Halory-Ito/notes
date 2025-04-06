该笔记是本人对SpringBoot的重新学习，因为之前学Spring的时候很懵，一堆东西不理解，估计还有很多好用的SpringBoot相关的东西不会用。做过几个项目之后，希望可以充分利用SpringBoot的相关特性，来优化项目或者实现更加高级的功能

该笔记是对尚硅谷的SpringBoot3网课的知识点记录，主要记录我之前不会的，已经会用但是不知道原理的部分。



# 入门

## SpringBoot3-Demo



### 关于依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.halory</groupId>
    <artifactId>SpringBoot3-01-demo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!-- 所有的SpringBoot项目都必须继承自spring-boot-starter-parent -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.5</version>
    </parent>

    <dependencies>
        <!-- Web开发启动器 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

</project>
```



其中`spring-boot-starter-parent`的父级为`spring-boot-dependencies`

```xml
  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>3.2.5</version>
  </parent>
```

而`spring-boot-dependencies`中已经提前声明好了所有依赖的版本，因此我们使用SpringBoot的时候，不需要指定版本号，默认会根据父级的版本号来。

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611133410916.png" alt="image-20240611133410916" style="zoom:50%;" />

如果想要修改某个依赖的版本属性，有两种方式

- 在pom.xml文件中引入`<properties>`标签，统一指定版本

```xml
    <properties>
        <mysql.version>8.3.0</mysql.version>
    </properties>
```

- 在指定的依赖下面加上`<version>`标签

```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.5</version>
    </parent>
```



### 关于主程序

```java
package com.halory;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class,args);
    }
}

```



### 关于Controller

```java
package com.halory.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello() {
        return "Hello World";
    }
}

```

这里可以讲讲`@RestController`

> @RestController = @ResponseBody + @Controller

`@ResponseBody`用于将返回的结果转为JSON

`@Controller`用于表示某个类是控制层



### 关于插件

将项目打包，需要用到maven打包插件，这是SpringBoot官方提供的，如下

```xml
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```



### 关于运维

项目打包完之后，直接执行`java -jar xxx.jar`命令就可以了。

如果想要修改配置的话，可以选择去项目中修改application.yaml文件，然后再重新打包。

也可以在jar包的目录下，创建一个`application.properties`，修改配置，然后重新运行。

比如项目的端口号默认是8080

![image-20240611120631801](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611120631801.png)

创建application.properties，并修改配置

![image-20240611120832060](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611120832060.png)

```properties
server.port = 8888
```

重新运行，看端口，已经变成8888了：

![image-20240611120918533](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611120918533.png)



### 关于目录

![image-20240611132738470](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611132738470.png)





# 自动装配

## 组件

```java
    public static void main(String[] args) {
        var ioc = SpringApplication.run(MainApplication.class,args);
        // 获取容器中所有组件的名字
        String[] names = ioc.getBeanDefinitionNames();
        // 遍历组件
        for (String name : names) {
            System.out.println(name);
        }
    }
```

容器中有什么组件，就具有什么功能



## 包扫描

SpringBoot默认扫描主程序以下的包，被`@SpringBootApplication`标注的包即为主程序

比如我们的`MainApplication.class`的路径为：

```
src/main/java/com/halory/MainApplication.java
```

那么扫描的包为`src/main/java/com/halory/*`

或者可以自定义包扫描路径，有两种方式：

- 用`@SpringBootApplication`中的属性指定包和类

![image-20240611135319924](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611135319924.png)

- 用`@ComponentScan("com.halory.xxx")`注解指定包和类

![image-20240611135452797](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611135452797.png)



## 配置文件

[所有的SpringBoot配置](https://docs.spring.io/spring-boot/3.3-SNAPSHOT/appendix/application-properties/index.html)

配置文件中的每个属性其实是跟某个类对象值一一绑定的

比如`server.port`就跟`ServerProperties`类中的`setPort`绑定

```java
    public void setPort(Integer port) {
        this.port = port;
    }
```

我们把绑定了配置文件中每一项值的类，称为==配置属性类==

比如：

- `ServerPorperties`绑定了所有Tomcat服务器有关的配置
- `MultipartProperties`绑定了所有文件上传有关的配置



## 按需加载

场景启动器除了会导入相关功能的依赖，还会导入一个`spring-boot-starter`，这是所有启动类的基础，是核心的`starter`

```xml
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>3.3.0</version>
      <scope>compile</scope>
    </dependency>
```

而`spring-boot-starter`又会导入一个`spring-boot-autoconfig`包。这个包里面都是各种场景的`AutoConfiguration`==自动配置类==

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611143351653.png" alt="image-20240611143351653" style="zoom:67%;" />

但是这些配置类不是全都开启的，导入哪个场景才会开启哪个场景的自动配置



## 核心流程

SpringBoot的自动装配原理大致如下：

- 导入`starter`，就会导入`autoconfigure`包
- `autoconfigure`包里面有一个文件`META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports`，里面指定了所有启动要加载的自动装配类

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611161232755.png" alt="image-20240611161232755" style="zoom:67%;" />

- 主程序上的`@EnableAutoConfiguration`注解会自动把上面文件中写的所有自动配置类都导入进来。而每一个自动装配类（xxxAutoConfiguration.class）都有`条件注解`，从而实现了组件的==按需加载==

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611161756921.png" alt="image-20240611161756921" style="zoom:67%;" />

- 从自动装配类导入进来的组件，会从`xxxProperties`中提取属性值，而`xxxProperties`又是和配置西文件进行了绑定的

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611161953126.png" alt="image-20240611161953126" style="zoom:50%;" />

==最终实现的效果==：导入`starter`、修改配置文件，就能修改底层行为



# 常用注解

## 组件注册

@Configuration、@SpringBootConfiguration

@Bean、@Scope

@Controller、@Service、@Repository、@Component

@Import

@ComponentScan



一般注册步骤：

- 使用`@Configuration`编写一个配置类
- 在配置类中，自定义方法给容器中注册组件。配合`@Bean`使用
- 或者使用`@Import`导入第三方的组件（使用这个导入的话，其bean的名字为全类名）



实例：

```java
package com.halory.controller.config;
@Configuration
public class AppConfig {
    @Scope("prototype")     		// 开启多例模式
    @Bean("druidException")			// 默认为单例模式。注册的bean默认为方法名，可以通过注解指定
    public FastsqlException exception() {
        return new FastsqlException();
    }
}

```

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611145108412.png" alt="image-20240611145108412" style="zoom:67%;" />



**单例和多例**

- 单例模式：从容器中拿到的都是同一个实例对象
- 多例模式：每次从容器中获取都会创建一个新的实例对象



配置：

```java
package com.halory.controller.config;
import com.alibaba.druid.FastsqlException;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

@Configuration
public class AppConfig {
    @Scope("prototype")     // 开启多例模式
    @Bean("druidException")
    public FastsqlException exception() {
        return new FastsqlException();
    }
}

```

测试：

```java
package com.halory;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        var ioc = SpringApplication.run(MainApplication.class,args);
        String[] names = ioc.getBeanDefinitionNames();
        // 获取容器中所有组件的名字
        Object d1 = ioc.getBean("druidException");
        Object d2 = ioc.getBean("druidException");
        System.out.println(d1==d2);
        Object h1 = ioc.getBean("helloController");
        Object h2 = ioc.getBean("helloController");
        System.out.println(h1==h2);
    }
}
/*
false
true
*/
```



## 条件注解

> 如果注解指定的条件成立，则触发指定行为（==其实SpringBoot的按需加载就跟这个有关系==）

一般长成这个样子：`@ConditionalOnXxx`

这里介绍四个，其他的都类似

`@ConditionalOnClass`：如果类路径中存在这个类，则触发指定行为

`@ConditionalOnMissingClass`：如果类路径中不存在这个类，则触发指定行为

`@ConditionalOnMissingBean`：如果类路径中不存在这个Bean，则触发指定行为

`@ConditionalOnBean`：如果类路径中存在这个Bean，则触发指定行为

...

代码示例：

```java
@Configuration

public class AppConfig {
    // 如果组件中存在这个类，则触发创建dog组件的行为
    @ConditionalOnClass(FastsqlException.class)
    @Bean
    public Dog dog() {
        return new Dog();
    }

    // 如果组件中不存在Dog类，则触发创建cat组件的行为
    @ConditionalOnMissingClass(value = "com.halory.controller.entity.Dog")
    @Bean
    public Cat cat(){
        return new Cat();
    }
}
```

测试：

```java
@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        var ioc = SpringApplication.run(MainApplication.class,args);
        String[] bean = ioc.getBeanNamesForType(Dog.class);
        for (String s : bean) {
            System.out.println("bean = " + s);
        }
    }
}

/*
bean = dog
*/
```

如果声明在==类==上，只有dog组件存在时，整个配置类才生效。

```java
@ConditionalOnMissingClass(value = "com.halory.controller.entity.Dog")
@Configuration
public class AppConfig {
    // 如果组件中存在这个类，则触发创建dog组件的行为
    @Bean
    public Dog dog() {
        return new Dog();
    }

    // 如果组件中不存在Dog类，则触发创建cat组件的行为
    @ConditionalOnMissingClass(value = "com.halory.controller.entity.Dog")
    @Bean
    public Cat cat(){
        return new Cat();
    }

}
```



## 属性绑定



### @ConfigurationProperties

> 将容器中任意组件（bean）的属性值和配置文件的配置项的值进行绑定

案例：

我将`Cat.class`注册为一个组件，并带上`@ConfigurationProperties`注解

```java
@Component
@Data
// 指定前缀为cat，则跟配置文件中以cat开头的属性进行绑定
@ConfigurationProperties("cat")
public class Cat {
    private int id;
    private String name;
}
```

然后去配置文件中添加配置信息

```yaml
cat:
  name: 汤姆
  id: 10
```

打印测试

```java
@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        var ioc = SpringApplication.run(MainApplication.class,args);
        Cat cat = ioc.getBean(Cat.class);
        System.out.println("cat = " + cat);
    }
}
// cat = Cat{id=10, name='汤姆'}zz
```



当然，`@ConfigurationProperties`也可以标注在一个方法上

```java
    @ConfigurationProperties(prefix = "cat")
    @Bean
    public Cat cat(){
        return new Cat();
    }
```

打印的效果跟上面的一致



### @EnableConfigurationProperties

至于`@EnableConfigurationProperties`，它主要用来将导入第三方写好的组件进行属性绑定

因为第三方包即使有`@Component`注解，由于它不在主程序及其子包中，是不会被扫描到的。

比如说druid

![image-20240611155355961](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611155355961.png)



# 日志配置

规范：以后开发不要编写`System.out.println()`，而是用日志记录信息

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611173139730.png" alt="image-20240611173139730" style="zoom:67%;" />

日志门面相当于一个接口层

而SpringBoot默认的日志接口是`SLF4j`，默认的日志实现是`Logback`



## 简介

- Spring使用`commons-logging`作为内部日志，但是==底层日志实现的开发==的。可对接其他日志框架。
  - 详见依赖包中的类`org.apache.commons.logging.LogAdapter`
  - 该日志适配器就实现了：导入了哪种日志，就是用哪种日志

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611173754548.png" alt="image-20240611173754548" style="zoom:80%;" />

- SpringBoot是支持`jul`、`log4j2`、`logback`之间进行切换的



那么SpringBoot是怎么把日志默认配置好的？

- 每个`starter`场景都会导入`spring-boot-starter`
- 而每个`spring-boot-starter`都会引入日志用的`spring-boot-starter-logging`
- 而每个`spring-boot-starter-logging`中提供了可以直接使用的日志`Logback`，以及通过实现`slf4j`接口的其他日志（说明了SpringBoot默认使用`logback + slf4j`组合作为底层日志）

```xml
  <dependencies>
      ...
      <artifactId>logback-classic</artifactId>
      ...
      <artifactId>log4j-to-slf4j</artifactId>
      ...
      <artifactId>jul-to-slf4j</artifactId>
      ...
  </dependencies>
```

- 由于日志是系统一启动就要使用的，所以没有自动装配，而是"直接装配"
- 日志是利用监听器机制配置好的。`ApplicationListener`

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611175119467.png" alt="image-20240611175119467" style="zoom:80%;" />

- 日志的配置可以通过修改配置文件实现。以`logging`开头的配置



## 日志格式

SpringBoot的默认日志格式如下

```java
/*
2024-06-11T17:53:21.962+08:00  INFO 15068 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
*/

// 时间和日期，精确到毫秒级别
2024-06-11T17:53:21.962+08:00

// 日志级别，包括ERROR、WARN、INFO、DEBUG和TRACE
INFO
    
// 进程ID 
15068
    
// 线程名
[           main]
    
// Logger名：通常是产生日志的类名
o.apache.catalina.core.StandardService
    
// 消息：日志记录的内容
Starting service [Tomcat]
```



我们可以自定义日志格式，通过`logging`相关属性

```yaml
logging:
  pattern:
  # 需要注意，yaml中修改console的话，属性值要带上引号
    console: '%d{yyyy-MMM-dd HH:mm:ss.SSS} %-5level [%thread] %logger{15} - %msg%n'
```

![image-20240612203541909](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240612203541909.png)

写程序时使用日志：

- 第一种是使用Logger类

```java
@RestController
public class HelloController {
    Logger log = Logger.getLogger(this.getClass().getName());
    @GetMapping("/hello")
    public String hello() {
        log.info("hello");
        return "Hello World";
    }
}
```



- 第二种是使用`@Slf4j`注解

```java
@Slf4j
@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello() {
        log.info("hello");
        return "Hello World";
    }
}
```



多次发送`localhost:8888/hello`请求，控制台都会打印如下的信息：

![image-20240612204311431](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240612204311431.png)



## 日志级别

由低到高，总共八种：

- ALL：打印所有日志
- TRACE：追踪框架详细流程日志
- DEBUG：开发调试细节日志
- INFO：关键、感兴趣信息日志
- WARN：警告但不是错误的信息日志，比如：版本过时
- ERROR：业务错误日志，比如出现各种异常
- FATAL：致命错误日志，比如jvm系统崩溃
- OFF：关闭所有日志记录

SpringBoot默认使用`INFO`级别的日志

配置文件中日志级别的修改：

```yaml
logging: 
  level:
    # 如果所有日志都没有精确指定级别的话，就使用root的日志级别
    root: debug
    # 指定com.halory.controller包下所有日志的级别为error
    com.halory.controller: error
```



## 日志分组

便于统一修改日志级别

```yaml
logging:
  group:
  # 将这三个包都放在aaa组（自定义名字）
    aaa:
      - com.halory.controller
      - com.halory.entity
      - com.halory.config
  # 修改日志级别的话，就可以根据组名来统一进行修改
  level:
    aaa: error
```



另外，SpringBoot底层预定义了两个组：`web`和 `sql`

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240612210111963.png" alt="image-20240612210111963" style="zoom:80%;" />

他们的组中涉及的包见下图：

![image-20240612210217665](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240612210217665.png)



## 日志输出

我们可以使日志不在控制台中打印，而是保存在日志文件中。

主要有两个相关配置：

```properties
# 设置日志文件名，也可以指定路径
logging.file.name
# 设置日志文件的文件夹，也可以指定日志文件名
logging.file.path
```



示例：

```yaml
# 日志记录到D盘下的my.log文件中
logging:
  file:
    name: D:\\my.log
    
# 日志记录到项目所在文件夹下的my.log文件中
logging:
  file:
    name: my.log
    
# 日志记录到D盘的 a.log文件夹 下的spring.log文件中
logging:
  file:
    path: D:\\a.log
    
# path失效，记录到项目所在文件夹下的a.log文件中
logging:
  file:
    path: D:\\
    name: a.log
```



## 文档归档和滚动切割

归档：每天的日志单独存到一个文档中

切割：设置每个文件的最大字节，超过规定的字节大小则切割成另外一个文件

由于SpringBoot默认使用的是logback，所以可以直接在yaml文件中配置

```yaml
# 使用logback指定规则

logging:
  logback:
    rolling-policy:
      # 存档的文件名格式（下面是默认值）
      file-name-pattern: '${LOG_FILE}.%d{yyyy-MM-dd}.%i.gz'
      # 启动时清除以前的存档（默认为false）
      clean-history-on-start: false
      # 每个日志文件的最大字节
      max-file-size: 1MB
      # 设置总日志的最大字节，如果超过规定的10MB，则会删除旧文件
      total-size-cap: 10MB
      # 日志文件存在超过7天，则会删掉
      max-history: 7
  file:
    name: my.log
```



如果是其他日志系统，需要自行配置（添加`log4j2.xml`或者`log4j2-spring.xml`）





# Web开发

## 自动配置

SpringBoot底层自动配置了SpringMVC

绑定了配置文件的一堆配置项：

- SpringMVC的所有配置：`spring.mvc`
- Web场景通用配置：`spring.web`
- 文件上传配置：`spring.servlet.multipart`
- 服务器配置：`server`



## 静态资源

### WebMvcAutoConfiguration原理

下面是该接口的==生效条件==：

```java
@AutoConfiguration(
    // 在这些自动配置完之后，WebMvcAutoConfiguration才自动装配
    after = {DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class, ValidationAutoConfiguration.class}
)

// 如果是web应用，则生效。这里是Servlet
@ConditionalOnWebApplication(
    type = Type.SERVLET
)

@ConditionalOnClass({Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class})

// 容器中没有这个Bean，配置才生效
@ConditionalOnMissingBean({WebMvcConfigurationSupport.class})

// 优先级
@AutoConfigureOrder(-2147483638)

@ImportRuntimeHints({WebResourcesRuntimeHints.class})
public class WebMvcAutoConfiguration {
}
```



==效果==

- 该接口下面放了两个Filter

```java
//  hiddenHttpMethodFilter，它使得了页面表单也可以提交Rest请求（GET、POST、PUT、DELETE）
public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
        return new OrderedHiddenHttpMethodFilter();
    }

// formContentFilter，使得了PUT、DELETE请求的请求体中的数据可以保留。否则是只有GET和POST才可以携带数据的。
public OrderedFormContentFilter formContentFilter() {
        return new OrderedFormContentFilter();
    }

```



- 给容器放了`WebMvcConfigurer`组件，给SpringMVC添加各种定制功能
  - 所有的功能最终与配置文件绑定
  - WevMvcProperties：`spring.mvc`
  - WebProperties：`spring.web`

```java
@Configuration(
        proxyBeanMethods = false
    )   
@Import({EnableWebMvcConfiguration.class})
@EnableConfigurationProperties({WebMvcProperties.class, WebProperties.class})  
@Order(0)    
public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer, ServletContextAware {
    }
```



### WebMvcConfigurer

接口：

![image-20240613203728215](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240613203728215.png)





### 映射规则

```java
public void addResourceHandlers(ResourceHandlerRegistry registry) {
            
    if (!this.resourceProperties.isAddMappings()) {
                
        logger.debug("Default resource handling disabled");
            
    } else {
                
        this.addResourceHandler(registry, this.mvcProperties.getWebjarsPathPattern(), "classpath:/META-INF/resources/webjars/");
                
        this.addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {
                    registration.addResourceLocations(this.resourceProperties.getStaticLocations());
                    
            if (this.servletContext != null) {
                      
                ServletContextResource resource = new ServletContextResource(this.servletContext, "/");
                        
                registration.addResourceLocations(new Resource[]{resource});
            }

                });
    }
}
"classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/"
```



- 规则一：访问`/webjars/**`路径，就去`classpath:/META-INF/resources/webjars/`下找资源

  - maven导入一个webjars

    ```xml
            <dependency>
                <groupId>org.webjars.npm</groupId>
                <artifactId>vue</artifactId>
                <version>3.4.27</version>
            </dependency>
    ```

  - 此时该外部依赖就有`/META-INF/resources/webjars/`的目录

  <img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240618115313996.png" alt="image-20240618115313996" style="zoom:67%;" />



- 规则二：访问`/**`路径，就去`静态资源位置默认的四个位置找资源`
  - `classpath:/META-INF/resources/`
  - `classpath:/resources/`
  - `classpath:/static/`
  - `classpath:/public/`

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240618114720802.png" alt="image-20240618114720802" style="zoom:67%;" />

- 规则三：静态资源默认都有缓存规则的实现
  - 所有缓存的设置，直接通过配置文件：`spring.web`
  - cachePeriod：缓存周期；缓存存在超过一定的时间，就去找服务器要新的。默认为0s
  - cacheControl：HTTP缓存控制
  - useLastModified：是否使用最后一次修改。主要用于决定是直接从缓存中获取，还是向服务器发送请求。
  - 如果一个浏览器访问了一个静态资源`index.js`，如果该资源没有发生变化，那么下次访问的时候就可以直接让浏览器使用自己缓存中的东西，而不用给服务器发送请求

```java
        private void addResourceHandler(ResourceHandlerRegistry registry, String pattern, Consumer<ResourceHandlerRegistration> customizer) {
            if (!registry.hasMappingForPattern(pattern)) {
                ResourceHandlerRegistration registration = registry.addResourceHandler(new String[]{pattern});
                customizer.accept(registration);
                registration.setCachePeriod(this.getSeconds(this.resourceProperties.getCache().getPeriod()));
                registration.setCacheControl(this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl());
                registration.setUseLastModified(this.resourceProperties.getCache().isUseLastModified());
                this.customizeResourceHandlerRegistration(registration);
            }
        }
```


## EnableWebMvcConfiguration源码

```java
	// SpringBoot 给容器中放 WebMvcConfigurationSupport 组件
	// 我们如果自己放了 WebMvcConfigurationSupport 组件，Boot的WebMvcConfiguration都会失效
	@Configuration(
        proxyBeanMethods = false
    )
    @EnableConfigurationProperties({WebProperties.class})
    public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration implements ResourceLoaderAware {
    }
```




# 模板引擎

由于SpringBoot使用了`嵌入式Servlet容器`，所以JSP默认是不能使用的

如果需要==服务端页面渲染==，优先考虑使用==模板引擎==

![[开发模式.png]]

开发模式目前有两种：前后端分离和前后端不分离（服务端渲染）

两者的主要区别是：
- 前后端分离时，浏览器会先给前端发送请求，然后前端再给后端发送请求，后端根据请求处理响应的业务，然后将业务处理后的数据响应给前端，前端再根据响应的内容渲染页面。
- 服务端渲染则是由浏览器直接给后端发送请求，然后后端处理对于的业务，获取到数据。找到模板引擎，在页面模板上渲染数据，生成页面，然后再响应页面。

==模板引擎==页面默认放在`src/main/resources/templates`下

SpringBoot包含以下模板引擎的自动配置：
- FreeMarker
- Groovy
- Thymeleaf
- Mustache

## SpringBoot整合Themeleaf


```xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-thymeleaf -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
    <version>3.3.1</version>
</dependency>
```

自动配置原理：
- 开启了`org.springframework.boot.autoconfigure.thymeleaf.ThemeleafAutoConfiguration`自动配置
- 属性绑定在`ThemeleafProperties`中，对应配置文件`spring.thymeleaf`内容
- 所有的模板页面默认在`classpath:/templates`文件夹下
- 默认效果：
	- 所有的模板页面在`classpath:/templates`下面找
	- 找后缀为`.html`的页面

## 入门案例

在`classpath:/templates`目录下创建了一个`welcome.html`文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!-- span标签内使用的是JSP的语法 -->
<h1>Welcome <span th:text="${msg}"></span></h1>
</body>
</html>
```

创建了一个`WelcomeController`

```java
package com.halory.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;

// 这里不使用RestController，因为它是适配前后端分离的
@Controller// 适配 服务端渲染
public class WelcomeController {

    @GetMapping("/wel")
    public String wel(@RequestParam("name") String name, Model model) {

        // 把需要给页面共享的数据放到model中，该类由SpringMVC提供
        model.addAttribute("msg", name);

        /*
        模板的逻辑视图名
        物理视图 = 前缀 + 逻辑视图名 + 后缀
        真实地址 = classpath:/templates/welcome.html
         */
        return "welcome";
    }
}

```


启动测试：

![[Thymeleaf入门测试.png]]




## 基础语法

>[!note] 提示
>在html标签中，加上`xmlns:th="http://www.thymeleaf.org"`会有语法提示


### 核心用法

**`th:xxx`：动态渲染指定的html标签属性值、或者th指令（遍历、判断等）**

- `th:text`：标签体内文本值渲染
	- `th:utext`：遇到html标签会转义
- `th:属性`：标签指定属性渲染
- `th:arrt`：同时指定多个属性进行渲染
- `th:if`、`th:each`...：其他th指令


# 雷哥的建议：如何学好SpringBoot

学SpringBoot好比学**摄影**，摄影有**傻瓜**相机和**单反**相机

傻瓜相机：不学用摄影也能拍出好的照片，但是不能够定制化

单反相机：需要调整一些参数，如焦距、光圈、快门等，可以根据自己的需要来定制化自己想要的效果，从而拍出自己想要的照片。

使用SpringBoot进行**普通开发**，就好比是使用单反相机拍照。只需要导入依赖，然后写Controller、Service、Mapper，偶尔修改几条配置

使用SpringBoot进行**高级开发**，就好比使用单反相机，可以自定义组件、自定义配置、甚至可以自定义`starter`



学好SpringBoot的话，基本要做到这三点：

- 理解自动配置原理
- 理解其他框架的底层
  - 比如拦截器、全局异常处理器等
- 随时定制化任何组件
  - 配置文件
  - 自定义组件



而SpringBoot的最佳实战路线：

- 选场景，导入到项目
  - 官方：starter
  - 第三方：maven仓库
- 写配置，修改配置文件关键项
  - 数据库参数
  - 线程池个数
  - ...
- 分析这个场景导入了哪些能用的组件
  - 自动装配这些组件进行后续使用
  - 不满足boot提供的自动配置好的默认组件，则进行定制化
    - 改配置
    - 自定义组件



# 其他小知识点

## yaml

### 写法

```yaml
person:
  id: 1
  name: Halo
  birth-day: 2004/09/12 23:34:00
  # 对于数组或集合，用-表示其中的一个元素。也可以直接写成： [胡桃, 流萤]
  roles:
    - 胡桃
    - 流萤
  # 对于Map类型，可以先指定key的值，然后再写value属性。也可以写成cat1: {id: 1, name: 小黑}
  cats:
    cat1:
      id: 1
      name: 小黑
    cat2:
      id: 2
      name: 小白
```

结果：

```java
/*
person = Person(id=1, name=Halo, birthDay=Sun Sep 12 23:34:00 CST 2004, roles=[胡桃, 流萤], cats={cat1=Cat(id=1, name=小黑), cat2=Cat(id=2, name=小白)})
*/
```



### 细节

- 驼峰建议改成-
  - 比如birthDay，写成`birth-day`
- 文本：
  - 单引号不能进行转义
  - 双引号可以转义
- 大文本：
  - `|`开头，大文本写在下面，==保留文本格式==，换行符正确显示
  - `>`开头，大文本写在下面，会折叠换行符（即输出时换行会消失）
- 多文档合并：
  - 使用`---`可以把多个yaml文档合并在一个文档中，主要是折叠配置（不能写在最顶层）。

比如server和person是两个不同的配置，是不可以一起折叠的：

![image-20240611171638765](C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611171638765.png)

点那是有了`---`之后，就可以了：

<img src="C:\Users\魏佳乐\AppData\Roaming\Typora\typora-user-images\image-20240611171601155.png" alt="image-20240611171601155" style="zoom:67%;" />

