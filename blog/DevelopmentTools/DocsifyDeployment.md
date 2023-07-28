# 从零开始搭建并部署个人知识库(GitHub Page + Docsify)
> 使用Docsify搭建一个快捷、轻量级的个人&团队文档, 并且通过Github Pages免费托管我们的个人知识库文档

## 知识库大概轮廓
![](https://raw.githubusercontent.com/Cris-Cui/ImagesForBlog/master/Blog/1.png)

![](https://raw.githubusercontent.com/Cris-Cui/ImagesForBlog/master/Blog/2.png)

## 什么是Docsify？
　　一个神奇的文档网站生成器。docsify 可以快速帮你生成文档网站。不同于 GitBook、Hexo 的地方是它不会生成静态的 .html 文件，所有转换工作都是在运行时。如果你想要开始使用它，只需要创建一个 index.html 就可以开始编写文档。

## Docsify的特性
> - 无需构建，写完文档直接发布
> - 容易使用并且轻量 (压缩后 ~21kB)
> - 智能的全文搜索
> - 提供多套主题
> - 丰富的 API
> - 支持 Emoji
> - 兼容 IE11
> - 支持服务端渲染 SSR (示例)

## 轻量&完善的Docsify模板
该模板为一个简洁，并且完善的Docsify模板基本上可以满足百分之八十多的团队需求，你可以按照文章中的Docsify环境配置教程把运行Docsify所需要的环境配置起来，通过命令即可查看效果（配置环境顺利的话只要十来分钟）。
```
模板源码地址：Docsify-Guide

模板预览地址：https://ysgstudyhards.github.io/Docsify-Guide/#/
```

## Node.js 安装配置
> Nodejs下载地址
> Node.js最新最详细安装教程

win+r：cmd进入命令提示符窗口，分别输入以下命令查看node和npm的版本能够正常显示版本号，则安装成功：

!!! note ""
    node -v：显示安装的nodejs版本
    npm -v：显示安装的npm版本


## docsify-cli工具安装
!!! tip
    推荐全局安装 docsify-cli 工具，可以方便地创建及在本地预览生成的文档。

```shell
npm i docsify-cli -g
```

## 项目初始化
> 如果想在项目的 `./docs` 目录里写文档，直接通过 `init` 初始化项目。

```shell 
docsify init ./docs
```

初始化成功后，可以看到 ./docs 目录下创建的几个文件

- `index.html` 入口文件，可以对页面总体布局进行设置。

- `README.md` 为主页内容渲染，直接编辑 `docs/README.md` 就能更新文档内容。

- `.nojekyll` 用于阻止 `GitHub Pages` 忽略掉下划线开头的文件

- 本地预览：

	通过运行 `docsify serve` 启动一个本地服务器，可以方便地实时预览效果。默认访问地址 http://localhost:3000/ 。

```shell
docsify serve docs
```

## 配置

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Cris-Cui's Blog</title>
  <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
  <meta name="description" content="Description">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=1.0">
  <!-- 代码高亮主题 -->
  <link href="_themes/prism/prism-vsc-dark-plus.css" rel="stylesheet">

  <link rel="stylesheet" href="//cdn.jsdelivr.net/npm/docsify@4/lib/themes/vue.css">
  
  <!-- <link href="https://cdn.jsdelivr.net/npm/prismjs@1.29.0/themes/prism.min.css" rel="stylesheet"> -->
</head>
<body>
  <div id="app">加载中...</div>
  <script>
    window.$docsify = {
      // 项目名称
      name: '',
      // 仓库地址，点击右上角的Github章鱼猫头像会跳转到此地址
      repo: 'https://github.com/Cris-Cui/',
      // 侧边栏支持，默认加载的是项目根目录下的_sidebar.md文件
      loadSidebar: true,
      // 导航栏支持，默认加载的是项目根目录下的_navbar.md文件
      loadNavbar: true,
      // 封面支持，默认加载的是项目根目录下的_coverpage.md文件
      coverpage: true,
      // 最大支持渲染的标题层级(支持多少个标题打开层级)
      maxLevel: 1,
      // 自定义侧边栏后默认不会再生成目录，设置生成目录的最大层级（建议配置为2-4）
      subMaxLevel: 4,
      // 小屏设备下合并导航栏到侧边栏
      mergeNavbar: true,
      /*搜索相关设置*/
      search: {
          maxAge: 86400000,// 过期时间，单位毫秒，默认一天
          paths: 'auto',// 注意：仅适用于 paths: 'auto' 模式
          placeholder: '搜索',              
          // 支持本地化
          placeholder: {
              '/zh-cn/': '搜索',
              '/': 'Type to search'
          },
          noData: '找不到结果',
          depth: 4,
          hideOtherSidebarContent: false,
          namespace: 'Docsify-Guide',
      },
      /*分页导航相关设置*/
      pagination: {
        previousText: '上一章节',
        nextText: '下一章节',
        crossChapter: true,
        crossChapterText: true,
      },
      /*字数统计相关插件*/
      count:{
        countable:true,
        fontsize:'0.9em',
        color:'rgb(90,90,90)',
        language:'chinese'
      },
    }
  </script>
    <!-- docsify的js依赖 -->
    <script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
    <!-- emoji表情支持 -->
    <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/emoji.min.js"></script>
    <!-- 图片放大缩小支持 -->
    <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/zoom-image.min.js"></script>
    <!-- 搜索功能支持 -->
    <script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/search.min.js"></script>
    <!--在所有的代码块上添加一个简单的Click to copy按钮来允许用户从你的文档中轻易地复制代码-->
    <script src="//cdn.jsdelivr.net/npm/docsify-copy-code/dist/docsify-copy-code.min.js"></script>
    <!--分页导航插件-->
    <script src="//cdn.jsdelivr.net/npm/docsify-pagination/dist/docsify-pagination.min.js"></script>
    <!--字数统计插件-->
    <script src="//unpkg.com/docsify-count/dist/countable.js"></script>
    <!--回到顶部插件-->
    <script src="https://cdn.jsdelivr.net/gh/Sumsung524/docsify-backTop/dist/docsify-backTop.min.js"></script>
    <script>
      var docsifyBackTop = {
        size: 32,
        bottom: 15,
        right: 15,
        logo: '<svg t="1662288563130" class="icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="3633" width="200" height="200"><path d="M513 103.7c-226.1 0-409.4 183.3-409.4 409.4S286.9 922.6 513 922.6s409.4-183.3 409.4-409.4S739.1 103.7 513 103.7z m153.5 364.7c-5.2 5.3-12.1 7.9-19 7.9s-13.8-2.6-19-7.9L545.1 385c0 0.4 0.1 0.7 0.1 1.1V705c0 11.1-5.7 20.9-14.4 26.6-4.7 4.2-10.9 6.7-17.7 6.7-6.8 0-13-2.5-17.7-6.7-8.7-5.7-14.4-15.5-14.4-26.6V386.1c0-0.4 0-0.7 0.1-1.1l-83.4 83.4c-10.5 10.5-27.5 10.5-38 0s-10.5-27.5 0-38L494 295.9c10.5-10.5 27.5-10.5 38 0l134.5 134.5c10.5 10.4 10.5 27.5 0 38z" fill="#2096ff" p-id="3634"></path></svg>',
        bgColor: ''
      };
    </script>
</body>
</html>

```


## 参考文档
- [Docsify中文手册](https://angry-swanson-b4e47b.netlify.app/zh-cn/)
- [Docsify-Guide](https://ysgstudyhards.github.io/Docsify-Guide/#/)
- [独立开发者@董川民](https://www.dongchuanmin.com/operate/5296.html)
- [肆零肆'Blog](https://xmq.plus/posts/1654.html#toc-heading-33)