---
title: hexo搭建github博客

date: 2017-10-30 11:06:24

category: 总结

tags: 其它

---

### 使用hexo搭建github博客

搭建环境:Mac

使用hexo的优点：可以使用Markdown编辑页面，各种各样的Markdown编辑器使用起来非常方便

#### 步骤一、创建github仓库

在github创建与用户名对应的仓库，如用户名为Lie8466，则需要创建的仓库为 Lie8466.github.io。


*参考文档：https://pages.github.com/ 中的步骤1
 注：如果已经创建过，请忽略此步。hexo部署完毕后会更改默认的index页面*

#### 步骤二、安装hexo


```
$ npm install -g hexo
```

手动创建hexo文件夹,假如创建的目录为/Users/yaweili/Documents/hexo

```
$ cd /Users/yaweili/Documents/hexo
$ hexo init
$ npm install
$ hexo server
```

Hexo Server已经启动了，在浏览器中打开 http://localhost:4000 ，
你可以按Ctrl+C 停止Server。

打开新的命令行窗口(command+T)，创建新文章，"My New Post"


```
$ hexo new "My New Post"
```

执行下面的命令，将markdown文件生成静态网页。


```
$ hexo generate
```
##### 编辑文章

在/Users/yaweili/Documents/hexo/source/_posts下可以看到My-New-Post.md文件，利用markdown编辑器编辑该文件

##### 部署github

在/Users/yaweili/Documents/hexo下找到文件_config.yml，用编辑器打开，找到


```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: 
```
修改为：
```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: https://github.com/Lie8466/Lie8466.github.io.git
  branch: master
```
安装hexo-deployer-git， 以及部署

```
$ npm install hexo-deployer-git --save
$ hexo deploy
```

*参考文档 https://hexo.io/zh-cn/docs/index.html
部署参考 https://hexo.io/zh-cn/docs/deployment.html *

##### 部署成功

在浏览器中打开 Lie8466.github.io ，正常显示网页，表明部署成功。

#### 调试

修改/Users/yaweili/Documents/hexo/source/_posts下的.md文件，刷新localhost:4000可以看到实时的更新

部署：

```
$ hexo clean
$ hexo generation // 或者hexo g
$ hexo deploy // 或者hexo d
```

#### 其它

/Users/yaweili/Documents/hexo/_config.yml有很多可以自己配置的小功能点，理解每一个配置项的含义可以个性化定制自己的页面


##### 配置标题

```
# Site
title: Yawei Li // 配置网站首页的大标题，默认为hexo
subtitle: knowing something of everything and everything of something // 配置网站首页的副标题，默认无
description:
author: Yawei Li
language: zh
timezone:
```

##### 配置页面路径
```
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://yoursite.com
root: /
permalink: :year/:title/  // 配置页面的路径，默认为年/月/日/标题，会显示太长，可以自行删掉一些信息
permalink_defaults:

```

##### 配置分类及标签

```
# Category & Tag
default_category: conclusion
category_map: ['学习笔记', '总结', '分享', '其它']
tag_map: ['JS', 'Vue', '前端性能', '其它']

```

上述配置的分类和标签可以在新建的.md文件中使用，如下：

```
---
title: hexo搭建github博客

date: 2017-10-30 11:06:24

category: 总结

tags: 其它

---
```

配置完成的分类和标签会在博客首页有相应的筛选项，可以快速查找某一个类型或者某个标签下的文章。

更多功能等待发现。。。


*
参考文档：http://www.cnblogs.com/zhcncn/p/4097881.html
*