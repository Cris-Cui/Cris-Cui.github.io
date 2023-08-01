![](https://raw.githubusercontent.com/Cris-Cui/ImagesForBlog/master/Blog/20230728152642.png)

# PicGo+Github搭建自己的免费图床

## **前言**

本文将手把手带你使用`PicGo+Github`搭建自己专属的免费图床。

文中操作是在`Windows`系统下完成的，`Manjaro`系统下操作相差不大。

`PicGo`是啥?

> PicGo是一个用于快速上传图片并获取图片 URL 链接的工具。

`Github`是啥？

> GitHub是一个在线软件源代码托管服务平台，是世界上最大的代码托管网站和开源社区。

为啥要用`PicGo+Github`?

> 大概因为免费+可靠吧

## **`PicGo`的下载与安装**

### Windows系统

打开下载地址：[PicGo 2.3.0](https://link.zhihu.com/?target=https%3A//github.com/Molunerfinn/PicGo/releases/tag/v2.3.0)

我这里选择的是`2.3.0`稳定版本 (目前最新版本为`2.3.1-beta.4`)，点击进行安装包的下载

## **新建Github仓库**

首先，我们需要登录 [Github](https://link.zhihu.com/?target=https%3A//github.com/) 网站，然后点击`左上角`的`New`

在弹出的界面输入仓库`名称`，然后点击`Create repository`即可（这里默认选择的是`Public`，即公共仓库）

## **生成`token`**

`token`的中文意思是`令牌`，简单来说是进行用户身份验证的一种方式。

> 生成的token不要告诉任何人哟！

点击`右上角`的头像，选择`Settings`

在左侧选项卡中找到`Developer settings`

找到`Personal acess token`，点击`Generate new token`

接下来我们需要设置的一共有三处：

1. 为创建的`token`添加描述
2. 设置`token`的有效期
3. 为`token`赋予权限(repo权限)

需要注意的是`token`的有效期有六种不同的选择：`7天`、`30天`、`60天`、`90天`、`自定义`和`不过期`，这里大家按需选择就好，我这里选择的是`不过期`。

设置完成后，翻到页面最底部，点击`Generate token`

这样`token`就生成好了，我们先复制下来，后面`PicGo`软件配置里要用到。

生成的`token`只会显示一次，当我们离开这个界面后，就没法再次查看了，所以一定要保存好，建议存到自己的备忘录里。

## **PicGo配置**

打开我们安装好的`PicGo`，点击`图床设置`选择`GitHub图床`，安装提示要求进行设置，设置完成后一定要点击确定！

设定仓库名：`你的用户名/figure`

- 设置分支名：`main`
- 设定Token：这里粘贴我们刚才复制的`token`
- 指定存储路径：默认会上传到`根目录`，指定目录要以`/`结尾，比如我想上传图片到`Manjaro`目录下，那么存储路径应该设置为`Manjaro/`
- 设定自定义域名：没有忽略即可

点击`上传区`，更改图片上传图床为`GitHub图床`

上传成功后，`PicGo`会自动把图片的链接复制到粘贴板中，我们可以直接粘贴。

## **`typora`偏好设置**

![](https://raw.githubusercontent.com/Cris-Cui/ImagesForBlog/master/Blog/20230728153054.png)

## 参考文档

[2022【保姆级教程】手把手带你使用PicGo+Github搭建自己的免费图床](https://zhuanlan.zhihu.com/p/553533337)