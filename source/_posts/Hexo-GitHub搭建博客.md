
---
title: Hexo+Github搭建博客
date: 2016-12-18 22:11:44
categories: [Hexo]
---

## 前言
由于之前是在`CSDN`上写博客的，但是碍于`CSDN`的界面实在无法直视，朋友推荐了`HEXO`,` HEXO`是基于`Nodejs`的一个静态的博客框架，相关介绍自行`Google`。和`Github`搭档堪称完美。当然在搭建的过程中也遇到了很多的坑，在此记录下来. 目前域名使用的依然是`Github`的，后期会更换.

## 环境配置
### Node安装
到`Nodejs`[官网](https://nodejs.org/)下载相应平台的最新版本，一路安装即可。
### Git安装
`Git`的安装可以参照[廖雪峰](http://www.liaoxuefeng.com/)的教程（同时申请一个`Github`的账户）
### Hexo安装
`Node`和`Git`都安装好后，可执行如下命令安装`hexo`：
``` bash
$ npm install -g hexo
```
### 初始化
此时可以新建一个文件夹，假设取名为`blog`，然后在这个目录下执行`init`命令：
``` bash
$ hexo init
```
### 生成静态页面
通过执行`hexo generate`命令来生成静态页面至当前`blog`目录下的`public`目录。
``` bash
$ hexo generate （hexo g  也可以）
```
### 本地启动
启动服务，进行文章预览调试，命令：
``` bash
$ hexo server
```
浏览器输入[http://localhost:4000](就可以看到最原始的效果了)，期初发现本地无法预览，后来发现是4000端口被占用，可以执行如下命令来更改端口。
```bash
$ hexo server -p 5000
```

## 配置Github
### 建立Repository

在自己的`GitHub`账号下创建一个新的仓库，命名为`username.github.io`（`username`是你的账号名)。在这里，要知道，GitHub Pages有两种类型：User/Organization Pages 和 Project Pages，而我所使用的是User Pages.

### User Pages 与 Project Pages

1. `User Pages` 是用来展示用户的，而 `Project Pages` 是用来展示项目的。
2. 用于存放 `User Pages`的仓库必须使用`username.github.io`的命名规则，而 `Project Pages` 则没有特殊的要求。
3. `User Pages` 将使用仓库的 `master` 分支，而 `Project Pages` 将使用 `gh-pages` 分支。
4. `User Pages` 通过 `http(s)://username.github.io`  进行访问，而 `Projects Pages`通过 `http(s)://username.github.io/projectname` 进行访问。

### 建立关联和部署
建立然后建立关联,当前`blog`目录下有这些文件：
``` bash
_config.yml	node_modules	public		source
db.json		package.json	scaffolds	themes
```
现在我们需要通过_config.yml文件，来建立关联，命令：
``` bash
$ vim _config.yml
```
翻到最下面，改成如下格式
``` bash
deploy:
  type: git
  repository: giuhub中相应【username.github.io】项目的地址
  branch: master
```
注： type、repository和branch在":"之后有个空格,为了能够使`Hexo`部署到`GitHub`上，需要安装一个插件：
``` bash
$ npm install hexo-deployer-git --save
```
然后，执行配置命令：
``` bash
$ hexo deploy
```
此时生成的相关文件已经被`push`到`Github`的`master`分支了，直接在网页中访问 `username.github.io`即可

### 常用命令：
``` bash
$ hexo new "postName" #新建文章
$ hexo new page "pageName" #新建页面
$ hexo generate #生成静态页面至public目录
$ hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
$ hexo deploy #将.deploy目录部署到GitHub
$ hexo help  # 查看帮助
$ hexo version  #查看Hexo的版本
```
### 问题解决
`Hexo`通过`deploy`命令部署到`GitHub`上的文件，是将`.md`（你的博文）转化之后的`.html`（静态网页），会将`public`目录下的文件自动`push`到远程仓库的`master`分支。因此当我们更换电脑时，就得重新安装环境了，非常的繁琐，可以考虑添加额外分支`hexo`来解决这个问题，`master`分支存放生成的网站文件，`hexo`分支存放源文件，步骤如下

1. 创建仓库，`username.github.io`

2. 本地创建文件夹为`blog`,进行初始化操作

   ```bash
   $ cd blog
   $ hexo init
   ```

3. 删除当前`blog`文件夹下的`.git`文件夹，否则后期会和通过`git init`生成的`blog`仓库冲突。

4. 创建`.gitignore`文件，进行编辑，用于声明不被记录的文件

   ```
   .DS_Store
   Thumbs.db
   db.json
   *.log
   node_modules/
   public/
   .deploy*/%
   ```

5. 然后对`blog`进行初始化仓库，切换到`hexo`分支，并`push`到远程仓库

   ```bash
   $ cd blog
   $ git init
   $ git checkout -b hexo
   $ git add .
   $ git commit -m "first commit"
   $ git remote add origin git@github.com:WhiteSnoopy/WhiteSnoopy.github.io.git
   $ git push origin hexo
   ```

6. 操作完成后，将远程仓库克隆下来，安装依赖包，进行操作

   ```bash
   $ git clone git@github.com:WhiteSnoopy/WhiteSnoopy.github.io.git
   $ git checkout hexo
   $ npm install
   $ npm install hexo-deployer-git
   ```

7. 部署配置，修改`_config.yml`文件，参照上面

   ```bash
   $ hexo generate
   $ hexo deploy
   ```

这样一来，我们每次只需要拉去最新的代码，在`hexo`分支下，执行`hexo  g`、`hexo d`命令即可，然后再将`hexo`下修改和新增的文件`push`到`Github`上即可，`master`分支上不用显示提交.

### 参考资料
- [使用Hexo和Github Pages搭建博客](http://javyzheng.github.io/2016/10/23/hexo/build-blog-with-hexo-and-github-pages/)
- [利用git解决hexo博客多PC间同步问题](http://webcache.googleusercontent.com/search?q=cache:6B1O5X76pscJ:chitanda.me/2015/06/18/hexo-sync-in-multiple-pc/+&cd=5&hl=en&ct=clnk&gl=us)