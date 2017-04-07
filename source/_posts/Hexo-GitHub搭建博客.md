---
title: Hexo+Github搭建博客
---

## 一、正文
之前是在CSDN上写博客的，但是碍于CSDN的界面实在无法直视，朋友推荐了HEXO, HEXO是基于Nodejs的一个静态的博客框架，相关介绍自行Google。和Github搭档堪称完美。当然在搭建的过程中也遇到了很多的坑，在此记录下来. 目前域名使用的依然是github的，后期会更换.

## 二、环境配置
### 2.1 Node安装
到Nodejs[官网](https://nodejs.org/)下载相应平台的最新版本，一路安装即可。
### 2.2 Git安装
Git的安装可以参照[廖雪峰](http://www.liaoxuefeng.com/)的教程（同时申请一个Github的账户）
### 2.3 Hexo安装
Node和Git都安装好后，可执行如下命令安装hexo：
``` bash
$ npm install -g hexo
```
### 2.4 初始化
此时可以新建一个文件夹，假设取名为blog，然后在这个目录下执行init命令：
``` bash
$ hexo init
```
### 2.5 生成静态页面
通过执行hexo generate命令来生成静态页面至当前blog目下的public目录。
``` bash
$ hexo generate （hexo g  也可以）
```
### 2.6 本地启动
启动服务，进行文章预览调试，命令：
``` bash
$ hexo server
```
浏览器输入[http://localhost:4000](就可以看到最原始的效果了)，期初发现本地无法预览，后来发现是4000端口被占用，可以执行如下命令来更改端口。
```bash
$ hexo server -p 5000
```

## 三、配置Github
### 3.1 建立Repository

在自己的GitHub账号下创建一个新的仓库，命名为`username.github.io`（username是你的账号名)。在这里，要知道，GitHub Pages有两种类型：User/Organization Pages 和 Project Pages，而我所使用的是User Pages.

### 3.2 User Pages 与 Project Pages

1. User Pages 是用来展示用户的，而 Project Pages 是用来展示项目的。
2. 用于存放 User Pages 的仓库必须使用username.github.io的命名规则，而 Project Pages 则没有特殊的要求。
3. User Pages 将使用仓库的 master 分支，而 Project Pages 将使用 gh-pages 分支。
4. User Pages 通过 http(s)://username.github.io  进行访问，而 Projects Pages通过 http(s)://username.github.io/projectname 进行访问。

### 3.3 建立关联和部署
建立然后建立关联,当前blog目录下有这些文件：
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
注： type、repository和branch在":"之后有个空格,为了能够使Hexo部署到GitHub上，需要安装一个插件：
``` bash
$ npm install hexo-deployer-git --save
```
然后，执行配置命令：
``` bash
$ hexo deploy
```
此时生成的相关文件已经被push到github的master分支了，直接在网页中访问 username.github.io即可

### 3.4 常用命令：
``` bash
$ hexo new "postName" #新建文章
$ hexo new page "pageName" #新建页面
$ hexo generate #生成静态页面至public目录
$ hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
$ hexo deploy #将.deploy目录部署到GitHub
$ hexo help  # 查看帮助
$ hexo version  #查看Hexo的版本
```
### 3.5 问题解决
Hexo部署到GitHub上的文件，是.md（你的博文）转化之后的.html（静态网页）。因此当我们更换电脑时，就得重新安装环境了，非常的繁琐，可以考虑把hexo以分支的概念放入github中，步骤如下

1. 创建仓库，username.github.io；
2. 使用git clone git@github.com:username/username.github.io.git拷贝仓库，默认为master分支，同时创建一个hexo分支(使用 `git checkout -b hexo`)；
3. 在本地创建一个文件夹假设为blog，切换到blog目录下，执行 npm install hexo -g, hexo init,将blog项目下生成的相关文件移动到`username.github.io`； 
4. 修改_config.yml中的deploy参数，执行npm install hexo-deployer-git；
5. 依次执行git add A、git commit -m "..."、git push origin hexo提交相关的文件到github上；
6. 执行hexo generate -d生成网站并部署到GitHub上；

这样一来，我们每次只需要拉去最新的代码，在hexo分支下，执行hexo clean、hexo g、hexo d先关命令即可，然后再将hexo下修改和新增的文件push到github上即可，master分支上不用显示提交.

## 四、主题更换

### 4.1 主题列表

1. [Cover](https://github.com/daisygao/hexo-themes-cover) - A chic theme with facebook-like cover photo.
2. [Oishi](https://github.com/henryhuang/oishi) - A white theme based on Landscape plus and Writing.
3. [Sidebar](https://github.com/hardywu/hexo-theme-sidebar) - Another theme based on Light with a simple sidebar.
4. [TKL](https://github.com/SuperKieran/TKL) - A responsive design theme for Hexo. 
5. [Tinnypp](https://github.com/levonlin/Tinnypp) - A clean, simple theme based on Tinny.
6. [Writing](https://github.com/yunlzheng/hexo-themes-writing) - A small and simple hexo theme based on Light.
7. [Yilia](https://github.com/litten/hexo-theme-yilia) - Responsive and simple style.
8. [Pacman voidy](https://github.com/Voidly/pacman) - A theme with dynamic tagcloud and dynamic snow.

### 4.2 主题配置

切换到theme目录下，通过git clone url ，将主题拷贝到theme目录下，修改根目录下的`_config.yml`文件，修改其中的参数theme即可：
```bash
theme: yilia
```