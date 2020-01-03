---
title: Eslint-前端代码规范检查
tags: Eslint 前端
date: 2020/1/2 20:20:20
---

### 什么是eslint

ESLint 是一个开源的 JavaScript 代码检查工具，由 Nicholas C. Zakas 于2013年6月创建。代码检查是一种静态的分析，常用于寻找有问题的模式或者代码，并且不依赖于具体的编码风格。对大多数编程语言来说都会有代码检查，一般来说编译程序会内置检查工具。

JavaScript 是一个动态的弱类型语言，在开发中比较容易出错。因为没有编译程序，为了寻找 JavaScript 代码错误通常需要在执行过程中不断调试。像 ESLint 这样的可以让程序员在编码的过程中发现问题而不是在执行的过程中。

ESLint 的初衷是为了让程序员可以创建自己的检测规则。ESLint 的所有规则都被设计成可插入的。ESLint 的默认规则与其他的插件并没有什么区别，规则本身和测试可以依赖于同样的模式。为了便于人们使用，ESLint 内置了一些规则，当然，你可以在使用过程中自定义规则。

ESLint 使用 Node.js 编写，这样既可以有一个快速的运行环境的同时也便于安装。

### eslint的特性

eslint的使用非常自由，它拥有以下几个特性

- 所有东西都是可以插拔的。你可以调用任意的rule api或者formatter api 去打包或者定义rule or formatter。
- 任意的rule 都是独立的 ，你可以选择是否开启某个rule，这取决于你和你的团队的代码习惯
- 没有特定的coding style，你可以自己配置

### 如何使用eslint

首先在你的项目中安装eslint，在项目根目录中运行：

```
$ npm install eslint --save-dev
```

紧接着在你的项目中初始化eslint：

```
$ ./node_modules/.bin/eslint --init
```

运行之后eslint会让你做出一些初始配置，例如使用的框架或是是否使用typescript等等，根据项目实际情况选择就可以了。

接着eslint会在你的项目根目录下生成一个配置文件`.eslintrc`，里面是刚刚根据你的回答而生成的配置项，如果你的想法有改变可以随时修改里面的配置。

### 如何配置eslint

打开配置文件你会看到，配置文件主要分成以下几个部分：

- "parserOptions"：配置解析器内容。其中“ecmaVersion”属性配置JS的语法版本。“sourceType”属性配置JS文件加载模式，值为“script”或“module”。“ecmaFeatures”属性配置想要使用的额外语言特性。 

- “env”：配置了预定义的全局环境。

- "plugins"：配置需要加载的第三方插件。这里我们需要先安装对应插件才能使用。
-  “globals”：定义了全局变量集合，包含在这个集合中的属性都会被工具认为是全局变量，no-undef 规则就不会发出警告。

- "extends"：配置基础规则，“rules”属性中配置的规则都是基于这个规则之上配置的。 

- "rules"：配置检查规则。 你可以在这里开启或关闭你想要的规则，或是调整他们的提示等级。提示等级有如下3种：

  - `"off"` 或 `0` - 关闭规则
  - `"warn"` 或 `1` - 将规则视为一个警告
  - `"error"` 或 `2` - 将规则视为一个错误

  例如：

  ```json
  "no-console": "error"
  ```

  则eslint会在你使用console时抛出错误。

其他详细的配置可以查看[官方文档](https://cn.eslint.org/docs/user-guide/configuring)

### 如何在VSCode中配置ESlint

打开VSCode，选择文件 -> 首选项 ->设置，在工作区设置的扩展中找到eslint，勾选Eslint：Enable，这时项目根目录会出现一个工作区设置文件`settings.json`，在这个文件中你可以对工作区的设置进行设置。

如果你想在保存文件时文件自动按照eslint进行格式化，你可以这样设置：

```json
{
  "eslint.validate": [
    {
      "language": "javascript",
      "autoFix": true
    }
  ],
  "eslint.autoFixOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "eslint.format.enable": true,
}
```

### 规则参考 - AlloyTeam ESLint

eslint支持非常多的规则检查，那么使用或不使用哪些规则呢？不同的团队有不同的意见。这里推荐腾讯的AlloyTeam规则作为参考。

AlloyTeam ESLint 规则不仅是一套先进的适用于 React/Vue/Typescript 项目的 ESLint 配置规范，而且也是你配置个性化 ESLint 规则的最佳参考。

具体的使用方法可以阅读[AllotTeam的文档](https://github.com/AlloyTeam/eslint-config-alloy)

### 各司其职-eslint和prettier结合使用

Prettier 是一个代码格式化工具，相比于 ESLint 中的代码格式规则，它提供了更少的选项，但是却更加专业。

如今 Prettier 已经成为前端项目中的必备工具，eslint-config-alloy 也没有必要再去维护 ESLint 中的代码格式相关的规则了，所以我们在 v3 版本中彻底去掉了所有 Prettier 相关的规则，用 ESLint 来检查它更擅长的逻辑错误。

至于缩进要两个空格还是四个空格，末尾要不要分号，可以在项目的 `.prettierrc.js` 中去配置。

你可以直接在vscode中安装prettier插件，然后再工作区设置文件中添加:

```json
"prettier.requireConfig": true,
"editor.formatOnSave": true
```

这样当保存文件的时候，prettier会自动将文件按照配置格式化。

AlloyTeam同样提供一个prettier的[配置参考文件](https://github.com/AlloyTeam/eslint-config-alloy/blob/master/.prettierrc.js)，可以作为配置参考。

这样，ealint负责语法检查，prettier负责格式化代码，各司其职，完美配合。

