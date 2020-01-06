---
title: CasperJS中文文档-1-安装
date: 2018/8/17 20:46:25
tags: 
- casperjs
- JavaScript
categories: 
- 技术
---

casperjs中文文档正式开坑。
<!-- more -->
### 安装
CasperJs可以在Mac OSX，Windows和大部分Linux系统上安装。

### 依赖
* [PhantomJS](phantomjs.org) 1.9.1 或更高版本。具体请查阅[PhantomJS的安装说明](phantomjs.org/download.html)
* 在casperjs的`bin/`目录中安装[Python](python.org) 2.6或更高版本。

> ##### 提示
PhantomJS 2.0.0及以上并不本地支持CoffeeScript。如果你想使用CoffeeScript，你需要先将其转换为Vanilla JavaScript。你可以在[已知问题](http://docs.casperjs.org/en/latest/known_issues.html#known-issues)这里查看详情。

*版本1.1新增*
* **实验特性：**根据1.1.0-beta1，SlimerJS 0.8或更高版本默认在Gecko（Firefox）而不是Webkit中运行测试（如果你想在Webkit中运行测试，只需将-engine = slimerjs添加到您的命令行选项）。SlimerJS的开发者已经撰写过文章[PhantomJS API与SlimerJS的兼容性](https://github.com/laurentj/slimerjs/blob/master/API_COMPAT.md)和[PhantomJS与SlimerJS的不同](https://docs.slimerjs.org/current/differences-with-phantomjs.html)。请注意，SlimerJS对CoffeeScript的支持在0.9.6版本后中断。我们正在探究这个问题。

*版本1.1.0-beta4新增*
> ##### 警告
通过npm安装的1.1.0-beta4之前的版本需要一个通过npm依赖安装的非特定的PhantomJS版本。当你通过npm安装后，CasperJS不会正常运行并会导致一些问题出现。从1.1.0版本之后，不管你使用哪种安装方法安装CasperJS，你都必须先安装一个引擎（PhantomJS，SlimerJS），它们是你使用CasperJS的先决条件。

### 从Homebrew安装（OSX）
Homebrew是一个流行的为Mac OS X开发的包管理器，你同样可以使用它来安装PhantomJS 和 CasperJS 。

首先可不要忘了你的更新命令：
```
    $ brew update
```
安装1.1以上版本（推荐）：
```
$ brew install casperjs
```
如果你已经安装了casperjs并想获取它的最新版本（稳定版|开发版），使用`upgrade`命令：
```
$ brew upgrade casperjs
```
升级只更新到最新版本的分支(1.0.x|1.1.0-dev)。

### 从npm安装
你可以通过npm安装CasperJS：
- 常用：
```
$ npm install -g casperjs
```

- 安装某个特定版本：
    * beta3：`npm install -g casperjs@1.1.0-beta3`
    * beta2：`$ npm install -g casperjs@1.1.0-beta2`

- 如果你想使用npm安装主干（master）版本：
```
$ npm install -g git https://github.com/casperjs/casperjs.git
```

> ##### 提示
-g 命令安装后，你可以在系统全局中使用CasperJS。


> ##### 警告
虽然CasperJS通过npm安装，但它并不是一个NodeJS模块，也不可以仅通过NodeJS执行。所以你不可以使用`require(‘casperjs’)`来加载CasperJS。请注意，CasperJS无法使用绝大多数NodeJS模块。亲身测试可以帮助你做出最好的判断。

### 从git安装
你也可以通过git安装，CasperJS的代码主要是托管在github上的。
##### 安装主干分支
```
$ git clone git://github.com/casperjs/casperjs.git
$ cd casperjs
$ ln -sf `pwd`/bin/casperjs /usr/local/bin/casperjs
```

如果PhantomJS和CasperJS已经安装成功，输入以下命令你应该得到相应的结果：
```
$ phantomjs --version
1.9.2
$ casperjs
CasperJS version 1.1.0-beta4 at /Users/niko/Sites/casperjs, using phantomjs version 1.9.2
# ...
```
如果你喜欢使用PhantomJS：
```
$ slimerjs --version
Innophi SlimerJS 0.8pre, Copyright 2012-2013 Laurent Jouanneau & Innophi
$ casperjs
CasperJS version 1.1.0 at /Users/niko/Sites/casperjs, using slimerjs version 0.8.0
```
你现在就可以编写你的第一个脚本啦！

### 从仓库下载
你可以直接在github上下载CasperJS的代码：

最新的开发版本（主干分支）：
- https://github.com/casperjs/casperjs/zipball/master (zip)
- https://github.com/casperjs/casperjs/tarball/master (tar.gz)

最新的稳定版本：
- https://github.com/casperjs/casperjs/zipball/1.1.0 (zip)
- https://github.com/casperjs/casperjs/tarball/1.1.0 (tar.gz)

其它操作均与git命令相同。

### Windows中的CasperJS
#### Phantomjs的附加安装
- 将`;C:\phantomjs`添加到你的环境变量中。
- 如果你的Phantomjs安装在其他地方，请填写正确的路径。

#### Casperjs的附加安装
*版本1.1.0-beta3新增*
- 添加`;C:\casperjs\bin`到你的环境变量（如果你的版本是1.1.0-beta3之前的版本，添加`;C:\casperjs\batchbin`到你的环境变量。
- 如果你的CasperJS安装在其他地方，请务必填写正确的安装路径。
- 如果你的计算机同时使用了离散和集成的图形，你需要禁用自动选择和显式地选择图形处理器——否则`exit()`不会退出casper。

你可以像这样运行你的casper脚本：
```
C:> casperjs myscript.js
```

#### 彩色输出
> ##### 提示
*版本1.1.0-beta1新增*
如果你的windows安装了[ansicon](https://github.com/adoxa/ansicon)或你的[ConEmu](https://conemu.github.io/)使用了ANSI颜色，你将会得到彩色的输出。

#### 编译（可选）
- 对于bin /目录中的casperjs.exe，需要NET Framework 3.5或更高版本（或Mono 2.10.8或更高版本）。

#### 已知Bugs和限制
- 由于CasperJS的异步性质，它无法与PhantomJS'PEPL很好的协同工作。
