[枫枫知道](https://www.fengfengzhidao.com/)



# 环境搭建

## 创建项目

```sh
go mod init blog
```

然后在根目录下创建`main.go`文件：

```go
package main

func main() {
}

```



## 读取配置文件

在项目的根目录下创建一个`app.yaml`文件，作为项目的配置文件，现在可以简单编写几个配置：

```yaml
system:
  ip:
  port: 8080
  env: dev
```



然后创建一个`cmd`目录，里面有一个`Enter.go`的文件，用来设置命令行参数（方便切换配置、数据库迁移）：

```go
package cmd

import "flag"

type Options struct {
    File    string
    DB      bool
    Version bool
}

var FlagOptions = new(Options)

func Parse() {
    flag.StringVar(&FlagOptions.File, "f", "app.yaml", "配置文件")
    flag.BoolVar(&FlagOptions.DB, "db", false, "数据库迁移")
    flag.BoolVar(&FlagOptions.Version, "v", false, "版本")
    flag.Parse()
}
```



将`Parse()`方法在`main.go`中使用：

```go
func main() {
	cmd.Parse()
}
```



在命令行查看效果：

```sh
# -h用于查看程序的参数
go run .\main.go -h
Usage of xxx\main.exe:
  -db
        数据库迁移
  -f string
        配置文件 (default "app.yaml")
  -v    版本
```



---

下面再创建一个`core`目录，并增加一个`ConfigInit.go`文件，该文件用来读取配置：

```go
package core

import (
    "Study/blog/cmd"
    "fmt"
    "gopkg.in/yaml.v2"
    "os"
)

type System struct {
    Ip   string `yaml:"ip"`
    Port int    `yaml:"port"`
}

type Config struct {
    System `yaml:"system"`
}

func ReadConf() {
    byteData, err := os.ReadFile(cmd.FlagOptions.File)
    if err != nil {
        panic(err)
    }

    var config Config
    err = yaml.Unmarshal(byteData, &config)
    if err != nil {
        panic(fmt.Sprintf("yaml文件格式错误 %s", err))
    }

    fmt.Printf("读取配置文件 %s 成功", cmd.FlagOptions.File)
}

```

然后将读取配置文件的函数在`main.go`中使用：

```go
func main() {
    cmd.Parse()
    fmt.Println(cmd.FlagOptions)
    core.ReadConf()
}
```



## 日志初始化

这里我们使用`logrus`，同样也是创建一个`core/LogInit.go`文件：

```go
package core

import (
	"Study/blog/global"
	"bytes"
	"fmt"
	"github.com/sirupsen/logrus"
	"os"
	"path"
	"time"
)

// 颜色
const (
	red    = 31
	yellow = 33
	blue   = 36
	gray   = 37
)

type LogFormatter struct{}
type FileDateHook struct {
	file     *os.File
	logPath  string
	fileDate string //判断日期切换目录
	appName  string
}

func (hook FileDateHook) Levels() []logrus.Level {
	return logrus.AllLevels
}
func (hook FileDateHook) Fire(entry *logrus.Entry) error {
	timer := entry.Time.Format("2006-01-02")
	line, _ := entry.String()
	if hook.fileDate == timer {
		hook.file.Write([]byte(line))
		return nil
	}
	// 时间不等
	hook.file.Close()
	os.MkdirAll(fmt.Sprintf("%s/%s", hook.logPath, timer), os.ModePerm)
	filename := fmt.Sprintf("%s/%s/%s.log", hook.logPath, timer, hook.appName)

	hook.file, _ = os.OpenFile(filename, os.O_WRONLY|os.O_APPEND|os.O_CREATE, 0600)
	hook.fileDate = timer
	hook.file.Write([]byte(line))
	return nil
}
func InitFile(logPath, appName string) {
	fileDate := time.Now().Format("2006-01-02")
	//创建目录
	err := os.MkdirAll(fmt.Sprintf("%s/%s", logPath, fileDate), os.ModePerm)
	if err != nil {
		logrus.Error(err)
		return
	}

	filename := fmt.Sprintf("%s/%s/%s.log", logPath, fileDate, appName)
	file, err := os.OpenFile(filename, os.O_WRONLY|os.O_APPEND|os.O_CREATE, 0600)
	if err != nil {
		logrus.Error(err)
		return
	}
	fileHook := FileDateHook{file, logPath, fileDate, appName}
	logrus.AddHook(&fileHook)
}

// Format 实现Formatter(entry *logrus.Entry) ([]byte, error)接口
func (t *LogFormatter) Format(entry *logrus.Entry) ([]byte, error) {
	//根据不同的level去展示颜色
	var levelColor int
	switch entry.Level {
	case logrus.DebugLevel, logrus.TraceLevel:
		levelColor = gray
	case logrus.WarnLevel:
		levelColor = yellow
	case logrus.ErrorLevel, logrus.FatalLevel, logrus.PanicLevel:
		levelColor = red
	default:
		levelColor = blue
	}
	var b *bytes.Buffer
	if entry.Buffer != nil {
		b = entry.Buffer
	} else {
		b = &bytes.Buffer{}
	}
	//自定义日期格式
	timestamp := entry.Time.Format("2006-01-02 15:04:05")
	if entry.HasCaller() {
		//自定义文件路径
		funcVal := entry.Caller.Function
		fileVal := fmt.Sprintf("%s:%d", path.Base(entry.Caller.File), entry.Caller.Line)
		//自定义输出格式
		fmt.Fprintf(b, "[%s] \x1b[%dm[%s]\x1b[0m %s %s %s\n", timestamp, levelColor, entry.Level, fileVal, funcVal, entry.Message)
	} else {
		fmt.Fprintf(b, "[%s] \x1b[%dm[%s]\x1b[0m %s\n", timestamp, levelColor, entry.Level, entry.Message)
	}
	return b.Bytes(), nil
}

func LogrusInit() {
	logrus.SetOutput(os.Stdout)          //设置输出类型
	logrus.SetReportCaller(true)         //开启返回函数名和行号
	logrus.SetFormatter(&LogFormatter{}) //设置自己定义的Formatter
	logrus.SetLevel(logrus.DebugLevel)   //设置最低的Level
	InitFile(global.Config.Log.Dir, global.Config.Log.App)
}

```

为了将配置拆分开来，创建一个`conf`文件夹：

```go
// Enter.go
package conf

type Config struct {
	System `yaml:"system"`
	Log    `yaml:"log"`
}

// LogrusConf.go
package conf

type Log struct {
	App string `yaml:"app"`
	Dir string `yaml:"dir"`
}

// SystemConf.go
package conf

type System struct {
	Ip   string `yaml:"ip"`
	Port int    `yaml:"port"`
}

```



再创建一个`global`目录，用于存放全局变量：

```go
package global

import "Study/blog/conf"

var Config *conf.Config

```

读取完配置文件之后，就需要给该全局变量赋值，所以`ReadConf()`函数需要返回配置：

```go
func ReadConf() (c *conf.Config) {
    byteData, err := os.ReadFile(cmd.FlagOptions.File)
    if err != nil {
        panic(err)
    }

    c = new(conf.Config)
    err = yaml.Unmarshal(byteData, c)
    if err != nil {
        panic(fmt.Sprintf("yaml文件格式错误 %s\n", err))
    }

    fmt.Printf("读取配置文件 %s 成功\n", cmd.FlagOptions.File)
    return c
}
```

给全局变量赋值：

```go
func main() {
	cmd.Parse()
	global.Config = core.ReadConf()
	core.LogrusInit()
}
```

> 至此，日志配置完成，每一次打印日志的时候，都会在`logs`目录下根据日期创建对应的文件夹及日志文件



## 数据库连接



1. 在`app.yaml`文件中配置数据库的信息：

```yaml
db:
  user: haibara
  password: 200414
  host: 127.0.0.1
  port: 3306
  db: blog_db
  debug: false
  source: mysql
```

2. 在`conf/DatabaseConf.go`文件中，读取数据库的配置：

```go
package conf

import "fmt"

type DB struct {
    User     string `yaml:"user"`
    Password string `yaml:"password"`
    Host     string `yaml:"host"`
    Port     int    `yaml:"port"`
    DB       string `yaml:"db"`
    // 打印全部的日志
    Debug  bool   `yaml:"debug"`
    Source string `yaml:"source"`
}

func (d DB) DSN() string {
    return fmt.Sprintf("%s:%s@tcp(%s:%d)/%s?charset=utf8mb4&parseTime=True&loc=Local",
                       d.User, d.Password, d.Host, d.Port, d.DB)
}

```

3. 在`global`文件夹中创建一个数据库的全局变量：

```go
var (
    Config *conf.Config
    DB     *gorm.DB
)
```


4. 创建一个`core/DatabaseInit.go`的文件，用于连接数据库：

```go
package core

import (
    "Study/blog/global"
    "github.com/sirupsen/logrus"
    "gorm.io/driver/mysql"
    "gorm.io/gorm"
    "time"
)

func DbInit() *gorm.DB {
    dc := global.Config.DB
    db, err := gorm.Open(mysql.Open(dc.DSN()), &gorm.Config{
        // 不生成外键约束
        DisableForeignKeyConstraintWhenMigrating: true,
    })
    if err != nil {
        logrus.Fatalf("数据库连接失败 %s", err)
    }
    // 连接池配置
    sqlDB, err := db.DB()
    sqlDB.SetMaxIdleConns(10)
    sqlDB.SetMaxOpenConns(100)
    sqlDB.SetConnMaxLifetime(time.Hour)

    logrus.Infof("数据库连接成功！")
    return db
}
```

5. 在`main.go`中执行数据库初始化：

```go
func main() {
    cmd.Parse()
    global.Config = core.ReadConf()
    core.LogrusInit()
    global.DB = core.DbInit()
}
```



## 读写分离

暂时不搞



## 根据IP获取地理位置

经常用于社交平台

1. 从[这里](https://github.com/lionsoul2014/ip2region/blob/master/data/ip2region.xdb)下载`ip2region.xdb`文件
2. 创建`core/IpDbInit.go`，并进行配置：

```go
package core

import (
    "github.com/lionsoul2014/ip2region/binding/golang/xdb"
    "github.com/sirupsen/logrus"
)

var searcher *xdb.Searcher

func IpDbInit() {
    var dbPath = "init/ip2region.xdb"
    _searcher, err := xdb.NewWithFileOnly(dbPath)
    if err != nil {
        logrus.Fatalf("IP地址数据库加载失败 %s", err)
        return
    }

    searcher = _searcher
}
// 获取IP地址
func GetIpAddr(ip string) (addr string, err error) {
    region, err := searcher.SearchByStr(ip)
    if err != nil {
        return
    }
    return region, nil
}

```

---

这里我们可以测试一下：

```go
func main() {
    core.IpDbInit()
    fmt.Println(core.GetIpAddr("175.0.201.207"))
    // 中国|0|湖南省|长沙市|电信 <nil>
    fmt.Println(core.GetIpAddr("2.0.201.207"))
    // 法国|0|Loire-Atlantique|0|橘子电信 <nil>
    fmt.Println(core.GetIpAddr("87.0.201.207"))
    // 意大利|0|0|0|0 <nil>
    fmt.Println(core.GetIpAddr("1.2.3.4"))
    // 美国|0|华盛顿|0|谷歌 <nil>
}
```

---

3. 判断IP地址是否有效、是否为内网，创建一个工具包`utils/ipUtils`：

```go
package utils

import "net"

func HasLocalIPAddr(ip string) bool {
    return HasLocalIP(net.ParseIP(ip))
}

func HasLocalIP(ip net.IP) bool {
    if ip.IsLoopback() {
        return true
    }
    ip4 := ip.To4()

    if ip4 == nil {
        return false
    }

    return ip4[0] == 10 || (ip4[0] == 172 && ip4[1] >= 16 && ip4[1] < 32) ||
    (ip4[0] == 169 && ip4[1] == 254) || (ip4[0] == 192 && ip4[1] == 168)
}

```



4. 进一步简化返回的ip信息：

```go
func GetIpAddr(ip string) (addr string) {
    if utils.HasLocalIPAddr(ip) {
        return "内网"
    }
    region, err := searcher.SearchByStr(ip)
    if err != nil {
        logrus.Warnf("错误的ip地址 %s", err)
        return "异常地址"
    }
    _addList := strings.Split(region, "|")
    if len(_addList) != 5 {
        logrus.Warnf("异常的ip地址 %s", ip)
        return "未知地址"
    }

    country := _addList[0]
    province := _addList[2]
    city := _addList[3]
    if province != "0" && city != "0" {
        return fmt.Sprintf("%s·%s", province, city)
    }
    if country != "0" && province != "0" {
        return fmt.Sprintf("%s·%s", country, province)
    }
    if country != "0" {
        return country
    }
    return region
}
```



# 表结构设计



极其复杂



# 日志管理

## 概述

为什么要有日志，有了日志之后，排错就可以不用到服务器上去了，而且大家都能看，客户也能够从日志中排查问题，用起来方便。

日志需要记录**请求体**（响应体不需要）、**错误信息**、**报错的代码行数**、**错误等级**等。

日志又可以分为：**登录日志**、**操作日志**、**运行日志**等

- 登录日志：记录哪些ip有登录操作，防止**恶意请求**（一般是封境外ip，国内一般是小区内会使用公网ip，如果这个ip一封，那么整个小区都访问不了），需要记录登录的类型、用户名、登录状态；如果是用户米密码登录的，登录失败需要把密码记录下来。
