---
title: 利用Github的Webhook功能和Node.js完成项目的自动部署
date: 2020/1/3 20:46:25
tags: 
- git
- webhook
- 自动化
categories: 
- 技术
---

### 背景

我自己有一个博客网站（也就是你现在在阅读的这个网站），放在我在腾讯云的远程服务器上，平常我会写一些博客，由于我的设备很多，一会在公司一会在家，我想要在任何设备上都可以随时写并同步我的博客，于是我把我的博客文章放在github上方便我随处都可以写。

我的博客网站是使[Hexo](https://hexo.io/zh-cn/)搭建的，对于博客网站这样简单的网站，使用hexo会很方便，你只需要专注内容，不需要别的考量，这样可以让我们专心写作。

在hexo项目中运行

```
$ hexo generate -w
```

之后，hexo会自动检测source文件夹里面文件的变化并生成相应的html页面，非常方便，于是我的博客发布流程为：

1. 在我自己的电脑上完成文章
2. 将文章push到github远程仓库上
3. 远程登录我在腾讯云的服务器
4. 在博客网站目录运行git pull

这样四步，前两步并不麻烦也无可避免，但是后两步是比较麻烦的，需要远程登录，虽说花不了多长时间吧，但是就是感觉麻烦，况且作为一个程序员，怎么能忍受这样无趣的重复操作呢！

所以我想，如果我只需要在我本地的电脑push到github之后，我的服务器可以自动拉取更新生成新的文件，那岂不是很爽吗！

事实上github也提供了这样功能的钩子也就是webhook。

接下来就教你手把手从零开始自动实现项目的自动部署。

### 什么是webhook

[Webhook](https://developer.github.com/webhooks/)，也就是人们常说的钩子，是一个很有用的工具。你可以通过定制 Webhook 来监测你在 Github.com 上的各种事件，最常见的莫过于 **push** 事件。如果你设置了一个监测 push 事件的 Webhook，那么每当你的这个项目有了任何提交，这个 Webhook 都会被触发，这时 Github 就会发送一个 HTTP POST 请求到你配置好的地址。

如此一来，你就可以通过这种方式去自动完成一些重复性工作；比如，你可以用 Webhook 来自动触发一些持续集成（CI）工具的运作，比如 Travis CI；又或者是通过 Webhook 去部署你的线上服务器。

Github 开发者平台的文档中对 Webhook 的所能做的事是这样描述的：

> You’re only limited by your imagination.

可见webhook的功能是非常强大的。其实不止github拥有这个功能，gitee同样也拥有这个功能。

### 配置项目的Webhook

打开你在github中需要自动部署的项目，选择settings，选择Webhooks，你就会看到以下界面：

![webhook设置界面](/img/202001.png)

选择add webhook：

![1578043188455](/img/202002.png)

其中Payload URL为webhook事件发生github后会发送一个post请求到你填写的这个地址，这就需要你准备

- 一台外网可以访问的主机
- 一个能够响应 Webhook 的服务器

其他填写项

- Content type 为发送的数据格式
- Secret为密码校验项，如果你不想你服务器提供的这个地址被其他人访问，可以设置一个密码并在服务器校验
- **Which events would you like to trigger this webhook?** github默认只有push操作会触发hook事件，如果你想让其他操作也可以发送hook事件，可以在这里勾选

#### 一台外网可访问的主机

我本人使用的是腾讯云的云服务器，其他品牌的云服务器的只要提供外网IP就可以。

#### 响应 Webhook 的服务器

为了响应 Webhook 所发出的请求，从而做一些我们想做的事情，我们得先实现一个响应服务器。作为一个前端程序员，我首先想到的就是用node写一个服务器程序，为了方便使用了express框架，你当然也可以用 PHP，python 等，全凭个人喜好啦。代码很短，就直接陈列在下方了：

```javascript
var express = require('express');

var app = express();

app.post('/', function (req, res) {
  console.log('github push触发');
  res.send('触发成功');
});

app.listen(7777, function () {
  console.log('Example app listening on port 7777!');
});
```

运行这个脚本，服务器程序就会运行在7777端口。这时你可以将你的主机ip和端口填写在github的webhook中并保存，执行一下push操作，看是否可以触发到。

### 准备执行的脚本文件

当webhook成功访问到正确的地址后，服务器需要做的就是执行git pull操作，所以我们准备一个脚本文件。

```shell
#!/bin/sh

unset GIT_DIR
DeployPath="path/to/your/project" 

echo "==============================================="
cd $DeployPath  #进入web项目目录
echo "deploying the web project"

#git stash
#git pull origin master #不建议使用git pull，后面会有解释

git fetch --all  #这里使用git fetch进行拉取，不建议用git pull
git reset --hard origin/master

time=`date`
echo "web server pull at webserver at time: $time."
echo "================================================"
```

这个脚本文件将执行pull操作，最后把执行这个文件的代码添加到我们的响应中。

```javascript
var express = require('express');
const { exec } = require('child_process');

var app = express();

app.post('/', function (req, res) {
  console.log('github push触发');
  exec('path/to/your/script.sh');
  res.send('触发成功');
});

app.listen(7777, function () {
  console.log('Example app listening on port 7777!');
});
```

这样的话，当你执行一个push操作后，webhook会向你指定的地址发送一个post请求，你可以在这个请求的响应中做更新、部署等操作。

大功告成！

这样每当我将写好的文章push到github之后，等待不到30秒，就可以在我的网站上看到更新的内容啦。