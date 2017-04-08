---
title: Hexo主题更换
date: 2016-12-20 10:11:15
categories: [Hexo]
---



## 主题更换

### 主题列表

1. [Cover](https://github.com/daisygao/hexo-themes-cover) - A chic theme with facebook-like cover photo.
2. [Oishi](https://github.com/henryhuang/oishi) - A white theme based on Landscape plus and Writing.
3. [Sidebar](https://github.com/hardywu/hexo-theme-sidebar) - Another theme based on Light with a simple sidebar.
4. [TKL](https://github.com/SuperKieran/TKL) - A responsive design theme for Hexo. 
5. [Tinnypp](https://github.com/levonlin/Tinnypp) - A clean, simple theme based on Tinny.
6. [Writing](https://github.com/yunlzheng/hexo-themes-writing) - A small and simple hexo theme based on Light.
7. [Yilia](https://github.com/litten/hexo-theme-yilia) - Responsive and simple style.
8. [Pacman voidy](https://github.com/Voidly/pacman) - A theme with dynamic tagcloud and dynamic snow.
9. [Next](https://github.com/iissnan/hexo-theme-next) - A theme with high quality elegant 

### 主题配置方式一

最简单的配置就是切换到`themes`目录下，通过`git clone url` ，将主题拷贝到`themes`目录下，修改根目录下的`_config.yml`文件，修改其中的参数`theme`即可：

```bash
theme: next
```


### 主题配置方式二

往往会使用第三方的主题，而这些主题通常是通常是托管在`Github`上的，使用`Github`来管理博客源码，而`themes`子目录里放的是另一个`repo`, 场景就变成了`Git`嵌套问题, 通过`submodule `来解决。

首先`Fork`该第三方主题仓库，这样就会在自己账号下生成一个同名的仓库，并对应一个`url`

<img src="/images/fork_next.png" width = 100% height = 50% align = center>

```bash
$ git submodule add git@github.com:WhiteSnoopy/hexo-theme-next.git themes/next
```

进入子仓库`next`修改主题后提交，之后再切换到主仓库提交修改

```bash
$ git commit -am "refine UI"
$ git push origin master
....
```

子模块的更新

```bash
$ git submodule init
$ git submodule update
```



### 参考资料

- [关于博客同步的解决办法](http://devtian.me/2015/03/17/blog-sync-solution/)
- [使用Git Submodule管理子模块](https://segmentfault.com/a/1190000003076028)

















