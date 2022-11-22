# Nodejs入门知识记录

## NodeJs基础

### 什么是NodeJs？

JS是脚本语言，脚本语言都需要一个解析器才能运行。对于写在HTML页面里的JS，浏览器充当了解析器的角色。而对于需要独立运行的JS，NodeJS就是一个解析器。

每一种解析器都是一个运行环境，不但允许JS定义各种数据结构，进行各种计算，还允许JS使用运行环境提供的内置对象和方法做一些事情。例如运行在浏览器中的JS的用途是操作DOM，浏览器就提供了`document`之类的内置对象。而运行在NodeJS中的JS的用途是操作磁盘文件或搭建HTTP服务器，NodeJS就相应提供了`fs`、`http`等内置对象。

### 如何安装？

网址：https://nodejs.org/en/download/

### 如何运行？

创建文件夹，编写JavaScript文件

```javascript
function hello() {
    console.log("HelloWorld");
}
hello();
```

命令行输入命令：

```cmd
node HelloWorld.js
```

控制台打印日志：

```
D:\private_project\web\my-first-app-with-nodejs>node HelloWorld.js
HelloWorld
```

### 模块

编写稍大一点的程序时一般都会将代码模块化。在NodeJS中，一般将代码合理拆分到不同的JS文件中，每一个文件就是一个模块，而文件路径就是模块名。

在编写每个模块时，都有`require`、`exports`、`module`三个预先定义好的变量可供使用。

#### require

`require`函数用于在当前模块中加载和使用别的模块，传入一个模块名，返回一个模块导出对象。模块名可使用相对路径（以`./`开头），或者是绝对路径（以`/`或`C:`之类的盘符开头）。

```javascript
var foo1 = require('./foo');
var foo2 = require('./foo.js');
var foo3 = require('/home/user/foo');
var foo4 = require('/home/user/foo.js');
// foo1至foo4中保存的是同一个模块的导出对象。
var data = require('./data.json'); //也可以加载一个json文件内容，自动读取
```

#### exports

`exports`对象是当前模块的导出对象，用于导出模块公有方法和属性。别的模块通过`require`函数使用当前模块时得到的就是当前模块的`exports`对象。

```javascript
exports.hello = function () {
    console.log('Hello World!');
};
```

#### module

通过`module`对象可以访问到当前模块的一些相关信息，但最多的用途是替换当前模块的导出对象。例如模块导出对象默认是一个普通对象，如果想改成一个函数的话，可以使用以下方式。

```javascript
// 模块默认导出对象被替换为一个函数。
module.exports = function () {
    console.log('Hello World!');
};
```

#### 模块初始化

一个模块中的JS代码仅在模块第一次被使用时执行一次，并在执行过程中初始化模块的导出对象。之后，缓存起来的导出对象被重复利用。

#### 主模块

通过命令行参数传递给NodeJS以启动程序的模块被称为主模块。主模块负责调度组成整个程序的其它模块完成工作。

根据`node main.js`启动。

#### 完整示例

```cmd
- /my-first-app-with-nodejs/
    - util/
        counter.js
    main.js
```

counter.js：

```javascript
let i = 0;
function count() {
    return ++i;
}
exports.count = count;
```

main.js：

```javascript
const counter1 = require('./util/counter');
const counter2 = require('./util/counter');
console.log(counter1.count());
console.log(counter2.count());
console.log(counter2.count());
```

运行该程序的结果如下：

```cmd
D:\private_project\web\my-first-app-with-nodejs>node main.js
1
2
3
```

可以看到，`counter.js`并没有因为被require了两次而初始化两次。

## 工程目录

### 模块路径解析规则

#### 内置模块

如果传递给`require`函数的是NodeJS内置模块名称，不做路径解析，直接返回内部模块的导出对象，例如`require('fs')`。

#### node_modules目录

NodeJS定义了一个特殊的`node_modules`目录用于存放模块。例如某个模块的绝对路径是`/home/user/hello.js`，在该模块中使用`require('foo/bar')`方式加载模块时，则NodeJS依次尝试使用以下路径。

```cmd
 /home/user/node_modules/foo/bar
 /home/node_modules/foo/bar
 /node_modules/foo/bar
```

#### NODE_PATH环境变量

与PATH环境变量类似，NodeJS允许通过NODE_PATH环境变量来指定额外的模块搜索路径。NODE_PATH环境变量中包含一到多个目录路径，路径之间在Linux下使用`:`分隔，在Windows下使用`;`分隔。例如定义了以下NODE_PATH环境变量：

```cmd
 NODE_PATH=/home/user/lib:/home/lib
```

当使用`require('foo/bar')`的方式加载模块时，则NodeJS依次尝试以下路径。

```cmd
 /home/user/lib/foo/bar
 /home/lib/foo/bar
```

### 包（package）

为了便于管理和使用，我们可以把由多个子模块组成的大模块称做`包`，并把所有子模块放在同一个目录里。

在组成一个包的所有子模块中，需要有一个入口模块，入口模块的导出对象被作为包的导出对象。

我们可以在包目录下创建一个package.json文件，用来指定入口模块的路径。

```
- /home/user/lib/
    - cat/
        + doc/
        - lib/
            head.js
            body.js
            main.js
        + tests/
        package.json
```

在package.json文件中，内容类似于：

```json
{
    "name": "cat",
    "main": "./lib/main.js"
}
```

## 文件操作常见API文档

### Buffer（数据块）

官方文档： http://nodejs.org/api/buffer.html

### Stream（数据流）

官方文档： http://nodejs.org/api/stream.html

### File System（文件系统）

官方文档： http://nodejs.org/api/fs.html

### Path（路径）

官方文档： http://nodejs.org/api/path.html

## 网络操作常见API文档

## HTTP

官方文档： http://nodejs.org/api/http.html

### HTTPS

官方文档： http://nodejs.org/api/https.html

### URL

官方文档： http://nodejs.org/api/url.html

### Query String

官方文档： http://nodejs.org/api/querystring.html

### Zlib

官方文档： http://nodejs.org/api/zlib.html

### Net

官方文档： http://nodejs.org/api/net.html

## 进程管理常见API文档

### Process

官方文档： http://nodejs.org/api/process.html

### Child Process

官方文档： http://nodejs.org/api/child_process.html

### Cluster

官方文档： http://nodejs.org/api/cluster.html

## 异步编程常见API文档

### 域（Domain）

官方文档： http://nodejs.org/api/domain.html