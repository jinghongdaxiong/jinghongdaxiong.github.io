---
title: git提交标准及工具
date: 2021-10-27 09:31:21
tags:
categories:
---

# Git Message规范化实践

## 前言
### 为什么（why)
随着项目业务的发展、编程人员的增多，当你想要找到某次代码提交的记录时，git commit message就显得尤为重要，标准化的commit message可以大大提高我们的开发效率，对项目的可持续化维护尤为重要。毕竟谁也不想只看到add、update、优化、新增列表等反馈不了实际修改了哪些内容的git log。

### 是什么(what)
先说结论： 标准化commit message（Angular规范） + Commitizen + Change log
### 怎么做（how）
1. 安装brew(已安装忽略)
2. 安装node(已安装忽略)
3. 安装Commitizen
4. 生成 Change log

## Commit message的作用

### （1）提供更多的历史信息，方便快速浏览
````
$ git log <last tag> HEAD --pretty=format:%s
````
### （2）过滤某些commit（比如文档改动），便于快速查找信息

比如，下面的命令仅仅显示本次发布新增加的功能。

````
$ git log <last release> HEAD --grep feature

````
### （3）可以直接从commit生成Change log

## Commit message的格式

每次提交，Commit message 都包括三个部分：Header，Body 和 Footer。
````

<type>(<scope>): <subject>
// 空一行
<body>
// 空一行
<footer>

````
其中，Header 是必需的，Body 和 Footer 可以省略。

不管是哪一个部分，任何一行都不得超过72个字符（或100个字符）。这是为了避免自动换行影响美观。

### Header
Header部分只有一行，包括三个字段：type（必需）、scope（可选）和subject（必需）。
#### type
type 用于说明commit的类别，只允许使用下面7个标识。

* feat: 新功能（feature）
* fix: 修补bug
* docs: 文档（documentation）
* style: 格式（不影响代码运行的变动）
* refactor: 重构（即不是新增功能，也不是修改bug的代码变动）
* test: 增加测试
* chore: 构建过程或辅助工具的变动

<font color=red>type为feat和fix，则该commit将肯定出现在Change log之中。其他情况（docs、chore、style、refactor、test)由你决定，要不要放入Change log,建议是不要。 </font>
#### scope
scope用于说明commit影响的范围，比如数据层、控制层、视图层等等，或业务影响范围。
#### subject
subject是commit的简短描述,不超过50个字符。
* 以动词开头，使用第一人称现在时，比如change,而不是changed或changes
* 第一个字母小写
* 结尾不加句号（.)
### Body
body部分是对本次commit的详细描述，可以分为多行，下面是一个范例。
````
More detailed explanatory text, if necessary.  Wrap it to 
about 72 characters or so. 

Further paragraphs come after blank lines.

- Bullet points are okay, too
- Use a hanging indent
````
<font color=red>注意：</font>
1. 使用第一人称现在时，比如使用change而不是changed或changes。
2. 应该说明代码变动的动机，以及与以前行为的对比。

### Footer
Footer 部分只用于两种情况。

#### （1）不兼容变动
如果当前代码与上一个版本不兼容，则 Footer 部分以BREAKING CHANGE开头，后面是对变动的描述、以及变动理由和迁移方法。
````
BREAKING CHANGE: isolate scope bindings definition has changed.

    To migrate the code follow the example below:

    Before:

    scope: {
      myAttr: 'attribute',
    }

    After:

    scope: {
      myAttr: '@',
    }

    The removed `inject` wasn't generaly useful for directives so there should be no code using it.

````
#### （2）关闭 Issue

如果当前 commit 针对某个issue，那么可以在 Footer 部分关闭这个 issue 。
````
Closes #234
````
也可以一次关闭多个 issue 。

````        
Closes #123, #245, #992
````        

### Revert
还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以revert:开头，后面跟着被撤销 Commit 的 Header。
````
revert: feat(pencil): add 'graphiteWidth' option

This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
````

Body部分的格式是固定的，必须写成This reverts commit &lt;hash>.，其中的hash是被撤销 commit 的 SHA 标识符。

如果当前 commit 与被撤销的 commit，在同一个发布（release）里面，那么它们都不会出现在 Change log 里面。如果两者在不同的发布，那么当前 commit，会出现在 Change log 的Reverts小标题下面。

## Commitizen

[Commitizen](https://github.com/commitizen/cz-cli) 是一个撰写合格 Commit message的工具。

### 安装命令
````
$ npm install -g commitizen
````
### 在项目根目录里，运行下面的命令，使其支持 Angular 的 Commit message 格式。
````
$ commitizen init cz-conventional-changelog --save --save-exact
````
注意 执行node项目一般没问题，但我们是Java项目，可能会一下错误
````
Attempting to initialize using the npm package cz-conventional-changelog
npm WARN saveError ENOENT: no such file or directory, open '/Users/owner/my/huifan/hf-kernel/package.json'
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN enoent ENOENT: no such file or directory, open '/Users/owner/my/huifan/hf-kernel/package.json'
npm WARN hf-kernel No description
npm WARN hf-kernel No repository field.
npm WARN hf-kernel No README data
npm WARN hf-kernel No license field.

Error: ENOENT: no such file or directory, open '/Users/owner/my/huifan/hf-kernel/package.json'
    at Object.openSync (fs.js:458:3)
    at Object.readFileSync (fs.js:360:35)
    at addPathToAdapterConfig (/usr/local/lib/node_modules/commitizen/dist/commitizen/adapter.js:1257:62)
    at init (/usr/local/lib/node_modules/commitizen/dist/commitizen/init.js:1035:5)
    at Object.bootstrap (/usr/local/lib/node_modules/commitizen/dist/cli/commitizen.js:34:30)
    at Object.<anonymous> (/usr/local/lib/node_modules/commitizen/bin/commitizen.js:2:38)
    at Module._compile (internal/modules/cjs/load
````
上述报错的原因为没有package.json文件，可执行以下命令生成默认的package.json
````
npm init --yes
````
### git commit 替换为 git cz
以后，<font color=red>凡是用到git commit命令，一律改为使用git cz。</font>这时，就会出现选项，用来生成符合格式的 Commit message。

## validate-commit-msg
[validate-commit-msg](https://github.com/conventional-changelog-archived-repos/validate-commit-msg) 用于检查 Node 项目的 Commit message 是否符合格式。

### 安装
````
npm install --save-dev validate-commit-msg

npm install ghooks --save-dev

````
### 配置

在项目的根目录下, 新建一个名为.vcmrc的文件, 并输入以下内容:

````
{
  "types": ["feat", "fix", "docs", "style", "refactor", "perf", "test", "build", "ci", "chore", "revert"],
  "scope": {
    "required": false,
    "allowed": ["*"],
    "validate": false,
    "multiple": false
  },
  "warnOnFail": false,
  "maxSubjectLength": 100,
  "subjectPattern": ".+",
  "subjectPatternErrorMsg": "subject does not match subject pattern!",
  "helpMessage": "",
  "autoFix": false
}
````

然后, 在package.json中添加如下配置
````
  "config": {
    "ghooks": {
      "commit-msg": "validate-commit-msg"
    }
  }

````
### 使用场景
每次git commit的时候，这个脚本就会自动检查 Commit message 是否合格。如果不合格，就会报错。
````
$ git add -A 
$ git commit -m "edit markdown" 
INVALID COMMIT MSG: does not match "<type>(<scope>): <subject>" ! was: edit markdown
````

## 生成Change log
如果你的所有 Commit 都符合 Angular 格式，那么发布新版本时， Change log 就可以用脚本自动生成。

生成的文档包括以下三个部分。

````        
New features
Bug fixes
Breaking changes
````        

每个部分都会罗列相关的 commit ，并且有指向这些 commit 的链接。当然，生成的文档允许手动修改，所以发布前，你还可以添加其他内容。

[conventional-changelog](https://github.com/conventional-changelog/conventional-changelog) 就是生成 Change log 的工具，运行下面的命令即可。

````        
$ npm install -g conventional-changelog
$ cd my-project
$ conventional-changelog -p angular -i CHANGELOG.md -w

````        

上面命令不会覆盖以前的 Change log，只会在CHANGELOG.md的头部加上自从上次发布以来的变动。

如果你想生成所有发布的 Change log，要改为运行下面的命令。

````    
$ conventional-changelog -p angular -i CHANGELOG.md -w -r 0

````    
为了方便使用，可以将其写入package.json的scripts字段。

````
{
  "scripts": {
      "changelog": "conventional-changelog -p angular -i CHANGELOG.md -s -r 0"

  }
}

````
以后，直接运行下面的命令即可。

````
$ npm run changelog
````

## 参考文档
[Git - Git 钩子](https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-Git-%E9%92%A9%E5%AD%90)

[优雅的使用Git](https://juejin.cn/post/6844904008025391118)

[Linux统计文件夹下的文件数目](http://noahsnail.com/2017/02/07/2017-02-07-Linux%E7%BB%9F%E8%AE%A1%E6%96%87%E4%BB%B6%E5%A4%B9%E4%B8%8B%E7%9A%84%E6%96%87%E4%BB%B6%E6%95%B0%E7%9B%AE/)

[git commit 、CHANGELOG 和版本发布的标准自动化](https://zhuanlan.zhihu.com/p/51894196)

[git教程](https://git-scm.com/book/zh/v2)

[优雅的使用Git](https://juejin.cn/post/6844904008025391118)

[ghooks](https://www.npmjs.com/package/ghooks)

[npm 安装参数中的 --save-dev 是什么意思](https://segmentfault.com/q/1010000000403629)

