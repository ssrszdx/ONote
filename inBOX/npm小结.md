# npm小结

## NPM是什么

NPM（node package manager），通常称为node包管理器。顾名思义，它的主要功能就是管理node包，包括：安装、卸载、更新、查看、搜索、发布等。

npm的背后，是基于couchdb的一个数据库，详细记录了每个包的信息，包括作者、版本、依赖、授权信息等。它的一个很重要的作用就是：将开发者从繁琐的包管理工作（版本、依赖等）中解放出来，更加专注于功能的开发。

npm官网：[https://npmjs.org/](https://npmjs.org/)

npm官方文档：[https://npmjs.org/doc/README.html](https://npmjs.org/doc/README.html)

## 我们需要了解什么

1. npm的安装、卸载、升级、配置
2. npm的使用：package的安装、卸载、升级、查看、搜索、发布
3. npm包的安装模式：本地 vs 全局
4. package.json：包描述信息
5. package版本：常见版本声明形式

## npm包安装模式

在具体介绍npm包的管理之前，我们首先得来了解一下npm包的两种安装模式。

### 本地安装 vs 全局安装（重要）

node包的安装分两种：本地安装、全局安装。两者的区别如下，后面会通过简单例子说明

* 本地安装：package会被下载到当前所在目录，也只能在当前目录下使用。
* 全局安装：package会被下载到到特定的系统目录下，安装的package能够在所有目录下使用。

### npm install pkg - 本地安装

运行如下命令，就会在当前目录下安装`grunt-cli`（grunt命令行工具）

```
npm install grunt-cli
```

安装结束后，当前目录下回多出一个`node_modules`目录，grunt-cli就安装在里面。同时注意控制台输出的信息：

<pre><a href="mailto:grunt-cli@0.1.9">grunt-cli@0.1.9</a> node_modules/grunt-cli</pre>

```
├── resolve@0.3.1
```

```
├── nopt@1.0.10 (abbrev@1.0.4)
```

```
└── findup-sync@0.1.2 (lodash@1.0.1, glob@3.1.21)
```

简单说明一下：

* grunt-cli@0.1.9：当前安装的package为grunt-cli，版本为0.19
* node_modules/grunt-cli：安装目录
* resolve@0.3.1：依赖的包有resolve、nopt、findup-sync，它们各自的版本、依赖在后面的括号里列出来

### npm install -g pkg- 全局安装

上面已经安装了grunt-cli，然后你跑到其他目录下面运行如下命令

```
grunt
```

果断提示你grunt命令不存在，为什么呢？因为上面只是进行了 **本地安装** ，grunt命令只能在对应安装目录下使用。

```
-bash: grunt: command not found
```

如果为了使用grunt命令，每到一个目录下都得重新安装一次，那不抓狂才怪。肿么办呢？

很简单，采用全局安装就行了，很简单，加上参数`-g`就可以了

```
npm install -g grunt-cli
```

于是，在所有目录下都可以无压力使用`grunt`命令了。这个时候，你会注意到控制台输入的信息有点不同。主要的区别在于安装目录，现在变成了`/usr/local/lib/node_modules/grunt-cli`，`/usr/local/lib/node_modules/`也就是之前所说的全局安装目录啦。

<pre><a href="mailto:grunt-cli@0.1.9">grunt-cli@0.1.9</a> /usr/local/lib/node_modules/grunt-cli</pre>

```
├── resolve@0.3.1
```

```
├── nopt@1.0.10 (abbrev@1.0.4)
```

```
└── findup-sync@0.1.2 (lodash@1.0.1, glob@3.1.21)
```

## npm包管理

npm的包管理命令是使用频率最高的，所以也是我们需要牢牢记住并熟练使用的。其实无非也就是几个动作：安装、卸载、更新、查看、搜索、发布等。

### 安装最新版本的grunt-cli

```
npm install grunt-cli
```

### 安装0.1.9版本的grunt-cli

```
npm install grunt-cli@"0.1.9"
```

### 通过package.json进行安装

如果我们的项目依赖了很多package，一个一个地安装那将是个体力活。我们可以将项目依赖的包都在package.json这个文件里声明，然后一行命令搞定

```
npm install
```

### 其他package安装命令

运行如下命令，列出所有`npm install`可能的参数形式

```
npm install --help
```

输出如下，有兴趣的童鞋可以了解下

```
npm install <tarball file>
```

```
npm install <tarball url>
```

```
npm install <folder>
```

```
npm install <pkg>
```

```
npm install <pkg>@<tag>
```

```
npm install <pkg>@<version>
```

```
npm install <pkg>@<version range>
```

### 卸载grunt-cli

比如卸载grunt-cli

```
npm uninstall grunt-cli
```

### 卸载0.1.9版本的grunt-cli

```
npm uninstall grunt-cli@"0.1.9"
```

### npm ls：查看安装了哪些包

运行如下命令，就可以查看当前目录安装了哪些package

```
npm ls
```

输出如下

```
/private/tmp/npm
```

```
└─┬ grunt-cli@0.1.9
```

```
  ├─┬ findup-sync@0.1.2
```

```
  │ ├─┬ glob@3.1.21
```

```
  │ │ ├── graceful-fs@1.2.3
```

```
  │ │ ├── inherits@1.0.0
```

```
  │ │ └─┬ minimatch@0.2.12
```

```
  │ │   ├── lru-cache@2.3.0
```

```
  │ │   └── sigmund@1.0.0
```

```
  │ └── lodash@1.0.1
```

```
  ├─┬ nopt@1.0.10
```

```
  │ └── abbrev@1.0.4
```

```
  └── resolve@0.3.1
```

输出如下，同样，如果是要查看package的全局安装信息，加上`-g`就可以

### npm ls pkg：查看特定package的信息

运行如下命令，输出grunt-cli的信息

```
npm ls grunt-cli
```

输出的信息比较有限，只有安装目录、版本，如下：

```
/private/tmp/npm
```

```
└── grunt-cli@0.1.9 
```

如果要查看更详细信息，可以通过`npm info pkg`，输出的信息非常详尽，包括作者、版本、依赖等。

```
npm info grunt-cli
```

### npm update pkg：package更新

```
npm update grunt-cli
```

### npm search pgk：搜索

输入如下命令

```
npm search grunt-cli
```

返回结果如下

```
npm http GET http://registry.npmjs.org/-/all/since?stale=update_after&amp;startkey=1375519407838
```

```
npm http 200 http://registry.npmjs.org/-/all/since?stale=update_after&amp;startkey=1375519407838
```

```
NAME                  DESCRIPTION                                        AUTHOR            DATE              KEYWORDS
```

```
grunt-cli             The grunt command line interface.                  =cowboy =tkellen  2013-07-27 02:24
```

```
grunt-cli-dev-exitprocess The grunt command line interface.              =dnevnik          2013-03-11 16:19
```

```
grunt-client-compiler Grunt wrapper for client-compiler.                 =rubenv           2013-03-26 09:15  gruntplugin
```

```
grunt-clientside      Generate clientside js code from CommonJS modules  =jga              2012-11-07 01:20  gruntplugin
```

### npm发布

这个命令我自己也还没实际用过，不误导大家，语法如下，也可参考官方对于package发布的说明[https://npmjs.org/doc/developers.html](https://npmjs.org/doc/developers.html)：

```
npm publish <tarball>
```

```
npm publish <folder>
```

## NPM配置

npm的配置工作主要是通过`npm config`命令，主要包含增、删、改、查几个步骤，下面就以最为常用的proxy配置为例。

### 设置proxy

内网使用npm很头痛的一个问题就是代理，假设我们的代理是 [http://proxy.example.com:8080，那么命令如下：](http://proxy.example.com:8080%EF%BC%8C%E9%82%A3%E4%B9%88%E5%91%BD%E4%BB%A4%E5%A6%82%E4%B8%8B%EF%BC%9A)

```
npm config set proxy http://proxy.example.com:8080
```

由于`npm config set`命令比较常用，于是可以如下简写

```
npm set proxy http://proxy.example.com:8080    
```

### 查看proxy

设置完，我们查看下当前代理设置

```
npm config get proxy
```

输出如下：

```
http://proxy.example.com:8080/
```

同样可如下简写：

```
npm get proxy
```

### 删除proxy

代理不需要用到了，那删了吧

```
npm delete proxy
```

### 查看所有配置

```
npm config list
```

### 直接修改配置文件

有时候觉得一条配置一条配置地修改有些麻烦，就直接进配置文件修改了

```
npm config edit
```

## 关于package.json

这货在官网似乎没有详细的描述，其实就是包的描述信息啦。假设当我们下载了node应用，这个node应用依赖于A、B、C三个包，如果没有package.json，我们需要人肉安装这个三个包（如果对版本有特定要求就更悲剧了）：

```
npm install A
```

```
npm install B
```

```
npm install C
```

有了package.json，一行命令安装所有依赖。

```
npm install
```

### package.json字段简介

字段相当多，但最重要的的是下面几个

1. name: package的名字（由于他会成为url的一部分，所以 non-url-safe 的字母不会通过，也不允许出现"."、"_"），最好先在[http://registry.npmjs.org/上搜下你取的名字是否已经存在](http://registry.npmjs.org/%E4%B8%8A%E6%90%9C%E4%B8%8B%E4%BD%A0%E5%8F%96%E7%9A%84%E5%90%8D%E5%AD%97%E6%98%AF%E5%90%A6%E5%B7%B2%E7%BB%8F%E5%AD%98%E5%9C%A8)
2. version: package的版本，当package发生变化时，version也应该跟着一起变化，同时，你声明的版本需要通过semver的校验（semver可自行谷歌）
3. dependencies: package的应用依赖模块，即别人要使用这个package，至少需要安装哪些东东。应用依赖模块会安装到当前模块的node_modules目录下。
4. devDependencies：package的开发依赖模块，即别人要在这个package上进行开发
5. 其他：参见官网

## package版本

在package.json里，你经常会在包名后看到类似"~0.1.0"这样的字符串，这就是包的版本啦。下面会列举最常见的版本声明形式，以及版本书写的要求：

### 常见版本声明形式

a、"~1.2.3" 是神马意思呢，看下面领悟

```
"~1.2.3" = ">=1.2.3 <1.3.0"
```

```
"~1.2" = ">=1.2.0 <1.3.0"
```

```
"~1" = ">=1.0.0 <1.1.0"
```

b、"1.x.x"是什么意思呢，继续自行领悟

```
"1.2.x" = ">=1.2.0 <1.3.0"
```

```
"1.x.x" = ">=1.0.0 <2.0.0"
```

```
"1.2" = "1.2.x"
```

```
"1.x" = "1.x.x"
```

```
"1" = "1.x.x"
```

### 版本书写要求

1. 版本可以v开头，比如 v1.0.1（v只是可选）
2. 1.0.1-7，这里的7是所谓的“构建版本号”，不理是神马，反正版本大于1.0.1
3. 1.0.1beta，或者1.0.1-beta，如果1.0.1后面不是 “连字符加数字” 这种形式，那么它是pre release 版本，即版本小于 1.0.1
4. 根据b、c，有：0.1.2-7 > 0.1.2-7-beta > 0.1.2-6 > 0.1.2 > 0.1.2beta

## 写在后面

内容只是简单地把最常见的命令，以及一些需要了解的内容列了出来。如要进一步了解，可参考官网说明。此外，`npm help`是我们最好的朋友，如果忘了有哪些命令，命令下有哪些参数，可通过help进行查看。

最关键的：如果文章内容有误，请指出！！！

---

‍

# [npm install 时--save-dev和--save的区别](https://www.cnblogs.com/blackgan/p/7678868.html)

### package.json中两个字段含义简介

一直在使用npm包管理器，对于npm install module --save-dev 和 npm install module --save这两个的区别做了一些浅析的理解：

##### dependencies

> dependencies属性被声明在一个简单的对象中，用来控制包名在一定的版本范围内，版本范围是一个字符串，可以被一个或多个空格分割。dependencied也可以被指定为一个压缩包地址或者一个 git URL 地址。

不要把测试工具或transpilers转义器(babel, webpack, gulp, postcss...)写到dependencies中。 （这些应该写到devDependencies）配置中，因为在别的项目中npm install 该包的时候会去下载dependencies中的依赖。

##### devDependencies

> 如果你的包被别人依赖或者安装时，在对方主项目中进行npm install便不会安装依赖包中的devDependencies中的npm包，所以如果你的项目中依赖的一些包不是在使用该项目时必须进行安装的，那就将包放在devDependencies中。

### ****整体功能比较****

npm install module：

* 会把module包安装到node_modules目录中
* 不会修改package.json
* 之后运行npm install 命令时，不会自动安装module包

npm install module --save

* 会把module包安装到node_modules目录汇总
* 会修改package.json，将模块名和版本号添加到dependencies部分
* 之后运行npm install 命令时，会自动安装module包
* 之后运行npm install --production或者注明NODE_ENV变量值为production时，会自动安装 module到node_modules目录中,即是在线上环境运行时会将包安装

npm install module --save-dev

* 会把module包安装到node_modules目录汇总
* 会修改package.json，将模块名和版本号添加到devDependencies部分
* 之后运行npm install 命令时，会自动安装module包
* 之后运行npm install --production或者注明NODE_ENV变量值为production时，不会自动安装msbuild到node_modules目录中，即是在线上环境并不会进行安装。

> 首先，--save和--save-dev可以省掉我们手动修改package.json文件的步骤。我们使用的一些打包工具、非项目必须依赖的都放在devDependencies中。

‍

---

镜像使用方法（三种办法任意一种都能解决问题，建议使用第三种，将配置写死，下次用的时候配置还在）:

1.通过config命令

来源： [https://cnodejs.org/topic/4f9904f9407edba21468f31e](https://cnodejs.org/topic/4f9904f9407edba21468f31e)

```
npm config set registry https://registry.npm.taobao.org 
```

```
npm info underscore （如果上面配置正确这个命令会有字符串response）
```

2.命令行指定

```
npm --registry https://registry.npm.taobao.org info underscore 
```

3.编辑 `~/.npmrc` 加入下面内容

```
registry = https://registry.npm.taobao.org
```

搜索镜像: [https://npm.taobao.org](https://npm.taobao.org)

建立或使用镜像,参考: [https://github.com/cnpm/cnpmjs.org](https://github.com/cnpm/cnpmjs.org)

---

**I have an app that uses the Rails framework and implements AngularJs as part of the front end.**

**I have pushed everything to Heroku and have the Heroku Toolbelt installed, but when I try to migrate the db using &quot;heroku run rake db:migrate&quot; I receive the following error(s):**

```
Installing core plugins heroku-cli-addons, heroku-apps, heroku-fork, heroku-git, heroku-local, heroku-run, heroku-status...
Error installing package. Try running again with GODE_DEBUG=info to see more output.
 !    `run` is not a heroku command.
 !    Perhaps you meant `-h`, `2fa`, `auth`, `join`, `open`, `orgs`, `pg`, `ps` or `rake`.
 !    See `heroku help` for a list of available commands.
```

> I then run the command "GODE_DEBUG=info heroku run rake db:migrate" and receive this error:

```
npm ERR! Darwin 14.5.0
npm ERR! argv "/Users/Christopher_Pelnar/.heroku/node-v4.2.1-darwin-x64/bin/node" "/Users/Christopher_Pelnar/.heroku/node-v4.2.1-darwin-x64/lib/node_modules/npm/cli.js" "install" "heroku-cli-addons" "heroku-apps" "heroku-fork" "heroku-git" "heroku-local" "heroku-run" "heroku-status" "--loglevel=info"
npm ERR! node v4.2.1
npm ERR! npm  v3.3.8
npm ERR! code ECONNRESET

npm ERR! network tunneling socket could not be established, cause=connect ETIMEDOUT 198.105.254.228:8080
npm ERR! network This is most likely not a problem with npm itself
npm ERR! network and is related to network connectivity.
npm ERR! network In most cases you are behind a proxy or have bad network settings.
npm ERR! network
npm ERR! network If you are behind a proxy, please make sure that the
npm ERR! network 'proxy' config is set properly.  See: 'npm help config'

npm ERR! Please include the following file with any support request:
npm ERR!     /Users/Christopher_Pelnar/.heroku/npm-debug.log
```

解决方案

**Removing the proxy settings resolved the issue:**

npm config rm proxy  
npm config rm https-proxy

‍

‍

# NPM 使用介绍

NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

* 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
* 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
* 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

由于新版的nodejs已经集成了npm，所以之前npm也一并安装好了。同样可以通过输入 **"npm -v" **来测试是否成功安装。命令如下，出现版本提示表示安装成功:

来源： [https://www.runoob.com/nodejs/nodejs-npm.html](https://www.runoob.com/nodejs/nodejs-npm.html)

```
$ npm -v
```

```
2.3.0
```

如果你安装的是旧版本的 npm，可以很容易得通过 npm 命令来升级，命令如下：

```
$ sudo npm install npm -g
```

```
/usr/local/bin/npm -> /usr/local/lib/node_modules/npm/bin/npm-cli.js
```

```
npm@2.14.2 /usr/local/lib/node_modules/npm
```

如果是 Window 系统使用以下命令即可：

```
npm install npm -g
```

> 使用淘宝镜像的命令：

```
cnpm install npm -g
```

---

## 使用 npm 命令安装模块

npm 安装 Node.js 模块语法格式如下：

```
$ npm install <Module Name>
```

以下实例，我们使用 npm 命令安装常用的 Node.js web框架模块  **express** :

```
$ npm install express
```

安装好之后，express 包就放在了工程目录下的 node_modules 目录中，因此在代码中只需要通过 **require('express')** 的方式就好，无需指定第三方包路径。

```
var express = require('express');
```

---

## 全局安装与本地安装

npm 的包安装分为本地安装（local）、全局安装（global）两种，从敲的命令行来看，差别只是有没有-g而已，比如

```
npm install express          # 本地安装
```

```
npm install express -g   # 全局安装
```

如果出现以下错误：

```
npm err! Error: connect ECONNREFUSED 127.0.0.1:8087 
```

解决办法为：

```
$ npm config set proxy null
```

### 本地安装

* 1. 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
* 2. 可以通过 require() 来引入本地安装的包。

### 

全局安装

* 1. 将安装包放在 /usr/local 下或者你 node 的安装目录。
* 2. 可以直接在命令行里使用。

如果你希望具备两者功能，则需要在两个地方安装它或使用  **npm link** 。

接下来我们使用全局方式安装 express

```
$ npm install express -g
```

安装过程输出如下内容，第一行输出了模块的版本号及安装位置。

```
express@4.13.3 node_modules/express
```

```
├── escape-html@1.0.2
```

```
├── range-parser@1.0.2
```

```
├── merge-descriptors@1.0.0
```

```
├── array-flatten@1.1.1
```

```
├── cookie@0.1.3
```

```
├── utils-merge@1.0.0
```

```
├── parseurl@1.3.0
```

```
├── cookie-signature@1.0.6
```

```
├── methods@1.1.1
```

```
├── fresh@0.3.0
```

```
├── vary@1.0.1
```

```
├── path-to-regexp@0.1.7
```

```
├── content-type@1.0.1
```

```
├── etag@1.7.0
```

```
├── serve-static@1.10.0
```

```
├── content-disposition@0.5.0
```

```
├── depd@1.0.1
```

```
├── qs@4.0.0
```

```
├── finalhandler@0.4.0 (unpipe@1.0.0)
```

```
├── on-finished@2.3.0 (ee-first@1.1.1)
```

```
├── proxy-addr@1.0.8 (forwarded@0.1.0, ipaddr.js@1.0.1)
```

```
├── debug@2.2.0 (ms@0.7.1)
```

```
├── type-is@1.6.8 (media-typer@0.3.0, mime-types@2.1.6)
```

```
├── accepts@1.2.12 (negotiator@0.5.3, mime-types@2.1.6)
```

```
└── send@0.13.0 (destroy@1.0.3, statuses@1.2.1, ms@0.7.1, mime@1.3.4, http-errors@1.3.1)
```

### 查看安装信息

你可以使用以下命令来查看所有全局安装的模块：

```
$ npm list -g
```

```
```

```
├─┬ cnpm@4.3.2
```

```
│ ├── auto-correct@1.0.0
```

```
│ ├── bagpipe@0.3.5
```

```
│ ├── colors@1.1.2
```

```
│ ├─┬ commander@2.9.0
```

```
│ │ └── graceful-readlink@1.0.1
```

```
│ ├─┬ cross-spawn@0.2.9
```

```
│ │ └── lru-cache@2.7.3
```

```
……
```

如果要查看某个模块的版本号，可以使用命令如下：

```
$ npm list grunt
```

```
projectName@projectVersion /path/to/project/folder
```

```
└── grunt@0.4.1
```

---

## 使用 package.json

package.json 位于模块的目录下，用于定义包的属性。接下来让我们来看下 express 包的 package.json 文件，位于 node_modules/express/package.json 内容：

```
{
```

```
  "name": "express",
```

```
  "description": "Fast, unopinionated, minimalist web framework",
```

```
  "version": "4.13.3",
```

```
  "author": {
```

```
    "name": "TJ Holowaychuk",
```

```
    "email": "tj@vision-media.ca"
```

```
  },
```

```
  "contributors": [
```

```
    {
```

```
      "name": "Aaron Heckmann",
```

```
      "email": "aaron.heckmann+github@gmail.com"
```

```
    },
```

```
    {
```

```
      "name": "Ciaran Jessup",
```

```
      "email": "ciaranj@gmail.com"
```

```
    },
```

```
    {
```

```
      "name": "Douglas Christopher Wilson",
```

```
      "email": "doug@somethingdoug.com"
```

```
    },
```

```
    {
```

```
      "name": "Guillermo Rauch",
```

```
      "email": "rauchg@gmail.com"
```

```
    },
```

```
    {
```

```
      "name": "Jonathan Ong",
```

```
      "email": "me@jongleberry.com"
```

```
    },
```

```
    {
```

```
      "name": "Roman Shtylman",
```

```
      "email": "shtylman+expressjs@gmail.com"
```

```
    },
```

```
    {
```

```
      "name": "Young Jae Sim",
```

```
      "email": "hanul@hanul.me"
```

```
    }
```

```
  ],
```

```
  "license": "MIT",
```

```
  "repository": {
```

```
    "type": "git",
```

```
    "url": "git+https://github.com/strongloop/express.git"
```

```
  },
```

```
  "homepage": "http://expressjs.com/",
```

```
  "keywords": [
```

```
    "express",
```

```
    "framework",
```

```
    "sinatra",
```

```
    "web",
```

```
    "rest",
```

```
    "restful",
```

```
    "router",
```

```
    "app",
```

```
    "api"
```

```
  ],
```

```
  "dependencies": {
```

```
    "accepts": "~1.2.12",
```

```
    "array-flatten": "1.1.1",
```

```
    "content-disposition": "0.5.0",
```

```
    "content-type": "~1.0.1",
```

```
    "cookie": "0.1.3",
```

```
    "cookie-signature": "1.0.6",
```

```
    "debug": "~2.2.0",
```

```
    "depd": "~1.0.1",
```

```
    "escape-html": "1.0.2",
```

```
    "etag": "~1.7.0",
```

```
    "finalhandler": "0.4.0",
```

```
    "fresh": "0.3.0",
```

```
    "merge-descriptors": "1.0.0",
```

```
    "methods": "~1.1.1",
```

```
    "on-finished": "~2.3.0",
```

```
    "parseurl": "~1.3.0",
```

```
    "path-to-regexp": "0.1.7",
```

```
    "proxy-addr": "~1.0.8",
```

```
    "qs": "4.0.0",
```

```
    "range-parser": "~1.0.2",
```

```
    "send": "0.13.0",
```

```
    "serve-static": "~1.10.0",
```

```
    "type-is": "~1.6.6",
```

```
    "utils-merge": "1.0.0",
```

```
    "vary": "~1.0.1"
```

```
  },
```

```
  "devDependencies": {
```

```
    "after": "0.8.1",
```

```
    "ejs": "2.3.3",
```

```
    "istanbul": "0.3.17",
```

```
    "marked": "0.3.5",
```

```
    "mocha": "2.2.5",
```

```
    "should": "7.0.2",
```

```
    "supertest": "1.0.1",
```

```
    "body-parser": "~1.13.3",
```

```
    "connect-redis": "~2.4.1",
```

```
    "cookie-parser": "~1.3.5",
```

```
    "cookie-session": "~1.2.0",
```

```
    "express-session": "~1.11.3",
```

```
    "jade": "~1.11.0",
```

```
    "method-override": "~2.3.5",
```

```
    "morgan": "~1.6.1",
```

```
    "multiparty": "~4.1.2",
```

```
    "vhost": "~3.0.1"
```

```
  },
```

```
  "engines": {
```

```
    "node": ">= 0.10.0"
```

```
  },
```

```
  "files": [
```

```
    "LICENSE",
```

```
    "History.md",
```

```
    "Readme.md",
```

```
    "index.js",
```

```
    "lib/"
```

```
  ],
```

```
  "scripts": {
```

```
    "test": "mocha --require test/support/env --reporter spec --bail --check-leaks test/ test/acceptance/",
```

```
    "test-ci": "istanbul cover node_modules/mocha/bin/_mocha --report lcovonly -- --require test/support/env --reporter spec --check-leaks test/ test/acceptance/",
```

```
    "test-cov": "istanbul cover node_modules/mocha/bin/_mocha -- --require test/support/env --reporter dot --check-leaks test/ test/acceptance/",
```

```
    "test-tap": "mocha --require test/support/env --reporter tap --check-leaks test/ test/acceptance/"
```

```
  },
```

```
  "gitHead": "ef7ad681b245fba023843ce94f6bcb8e275bbb8e",
```

```
  "bugs": {
```

```
    "url": "https://github.com/strongloop/express/issues"
```

```
  },
```

```
  "_id": "express@4.13.3",
```

```
  "_shasum": "ddb2f1fb4502bf33598d2b032b037960ca6c80a3",
```

```
  "_from": "express@*",
```

```
  "_npmVersion": "1.4.28",
```

```
  "_npmUser": {
```

```
    "name": "dougwilson",
```

```
    "email": "doug@somethingdoug.com"
```

```
  },
```

```
  "maintainers": [
```

```
    {
```

```
      "name": "tjholowaychuk",
```

```
      "email": "tj@vision-media.ca"
```

```
    },
```

```
    {
```

```
      "name": "jongleberry",
```

```
      "email": "jonathanrichardong@gmail.com"
```

```
    },
```

```
    {
```

```
      "name": "dougwilson",
```

```
      "email": "doug@somethingdoug.com"
```

```
    },
```

```
    {
```

```
      "name": "rfeng",
```

```
      "email": "enjoyjava@gmail.com"
```

```
    },
```

```
    {
```

```
      "name": "aredridel",
```

```
      "email": "aredridel@dinhe.net"
```

```
    },
```

```
    {
```

```
      "name": "strongloop",
```

```
      "email": "callback@strongloop.com"
```

```
    },
```

```
    {
```

```
      "name": "defunctzombie",
```

```
      "email": "shtylman@gmail.com"
```

```
    }
```

```
  ],
```

```
  "dist": {
```

```
    "shasum": "ddb2f1fb4502bf33598d2b032b037960ca6c80a3",
```

```
    "tarball": "http://registry.npmjs.org/express/-/express-4.13.3.tgz"
```

```
  },
```

```
  "directories": {},
```

```
  "_resolved": "https://registry.npmjs.org/express/-/express-4.13.3.tgz",
```

```
  "readme": "ERROR: No README data found!"
```

```
}
```

### Package.json 属性说明

* **name** - 包名。
* **version** - 包的版本号。
* **description** - 包的描述。
* **homepage** - 包的官网 url 。
* **author** - 包的作者姓名。
* **contributors** - 包的其他贡献者姓名。
* **dependencies** - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下。
* **repository** - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。
* **main** - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。
* **keywords** - 关键字

---

## 卸载模块

我们可以使用以下命令来卸载 Node.js 模块。

```
$ npm uninstall express
```

卸载后，你可以到 /node_modules/ 目录下查看包是否还存在，或者使用以下命令查看：

```
$ npm ls
```

---

## 更新模块

我们可以使用以下命令更新模块：

```
$ npm update express
```

---

## 搜索模块

使用以下来搜索模块：

```
$ npm search express
```

---

## 创建模块

创建模块，package.json 文件是必不可少的。我们可以使用 NPM 生成 package.json 文件，生成的文件包含了基本的结果。

```
$ npm init
```

```
This utility will walk you through creating a package.json file.
```

```
It only covers the most common items, and tries to guess sensible defaults.
```

```
```

```
See `npm help json` for definitive documentation on these fields
```

```
and exactly what they do.
```

```
```

```
Use `npm install <pkg> --save` afterwards to install a package and
```

```
save it as a dependency in the package.json file.
```

```
```

```
Press ^C at any time to quit.
```

```
name: (node_modules) runoob                   # 模块名
```

```
version: (1.0.0) 
```

```
description: Node.js 测试模块(www.runoob.com)  # 描述
```

```
entry point: (index.js) 
```

```
test command: make test
```

```
git repository: https://github.com/runoob/runoob.git  # Github 地址
```

```
keywords: 
```

```
author: 
```

```
license: (ISC) 
```

```
About to write to ……/node_modules/package.json:      # 生成地址
```

```
```

```
{
```

```
  "name": "runoob",
```

```
  "version": "1.0.0",
```

```
  "description": "Node.js 测试模块(www.runoob.com)",
```

```
  ……
```

```
}
```

```
```

```
```

```
Is this ok? (yes) yes
```

以上的信息，你需要根据你自己的情况输入。在最后输入 "yes" 后会生成 package.json 文件。

接下来我们可以使用以下命令在 npm 资源库中注册用户（使用邮箱注册）：

```
$ npm adduser
```

```
Username: mcmohd
```

```
Password:
```

```
Email: (this IS public) mcmohd@gmail.com
```

接下来我们就用以下命令来发布模块：

```
$ npm publish
```

如果你以上的步骤都操作正确，你就可以跟其他模块一样使用 npm 来安装。

---

## 

版本号

使用NPM下载和发布代码时都会接触到版本号。NPM使用语义版本号来管理代码，这里简单介绍一下。

语义版本号分为X.Y.Z三位，分别代表主版本号、次版本号和补丁版本号。当代码变更时，版本号按以下原则更新。

* 如果只是修复bug，需要更新Z位。
* 如果是新增了功能，但是向下兼容，需要更新Y位。
* 如果有大变动，向下不兼容，需要更新X位。

版本号有了这个保证后，在申明第三方包依赖时，除了可依赖于一个固定版本号外，还可依赖于某个范围的版本号。例如"argv": "0.0.x"表示依赖于0.0.x系列的最新版argv。

NPM支持的所有版本号范围指定方式可以查看[官方文档](https://npmjs.org/doc/files/package.json.html#dependencies)。

---

## NPM 常用命令

除了本章介绍的部分外，NPM还提供了很多功能，package.json里也有很多其它有用的字段。

除了可以在[npmjs.org/doc/](https://npmjs.org/doc/)查看官方文档外，这里再介绍一些NPM常用命令。

NPM提供了很多命令，例如install和publish，使用npm help可查看所有命令。

* NPM提供了很多命令，例如`install`和`publish`，使用`npm help`可查看所有命令。
* 使用`npm help <command>`可查看某条命令的详细帮助，例如`npm help install`。
* 在`package.json`所在目录下使用`npm install . -g`可先在本地安装当前命令行程序，可用于发布前的本地测试。
* 使用`npm update <package>`可以把当前目录下`node_modules`子目录里边的对应模块更新至最新版本。
* 使用`npm update <package> -g`可以把全局安装的对应命令行程序更新至最新版。
* 使用`npm cache clear`可以清空NPM本地缓存，用于对付使用相同版本号发布新版本代码的人。
* 使用`npm unpublish <package>@<version>`可以撤销发布自己发布过的某个版本代码。

---

## 使用淘宝 NPM 镜像

大家都知道国内直接使用 npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。

淘宝 NPM 镜像是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。

你可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:

```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

这样就可以使用 cnpm 命令来安装模块了：

```
$ cnpm install [name]
```

更多信息可以查阅：[http://npm.taobao.org/](http://npm.taobao.org/)。
