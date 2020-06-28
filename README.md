# 关于docsify
> 一个神奇的文档网站生成工具

## 是什么
docsify 是一个动态生成文档网站的工具。不同于 GitBook、Hexo 的地方是它不会生成将 ```.md``` 转成 ```.html``` 文件，所有转换工作都是在运行时进行。

这将非常实用，如果只是需要快速的搭建一个小型的文档网站，或者不想因为生成的一堆 ```.html``` 文件“污染” commit 记录，只需要创建一个 ```index.html``` 就可以开始写文档而且直接[部署在 GitHub Pages](https://docsify.js.org/#/zh-cn/deploy)。

## 特性
- 无需构建，写完文档直接发布
- 容易使用并且轻量 (压缩后 ~21kB)
- 智能的全文搜索
- 提供多套主题
- 丰富的 API
- 支持 Emoji
- 兼容 IE11
- 支持服务端渲染 SSR (示例)

# 快速开始

推荐安装 ```docsify-cli``` 工具，可以方便创建及本地预览文档网站。

```bash
npm i docsify-cli -g
```

## 初始化项目
如果想在项目的 ```./docs``` 目录里写文档，直接通过 ```init``` 初始化项目。
```bash
docsify init ./docs
```

## 开始写文档
初始化成功后，可以看到 ```./docs``` 目录下创建的几个文件
- ```index.html``` 入口文件
- ```README.md ```会做为主页内容渲染
- ```.nojekyll ```用于阻止 GitHub Pages 会忽略掉下划线开头的文件
直接编辑 ```docs/README.md``` 就能更新网站内容，当然也可以[写多个页面](https://docsify.js.org/#/zh-cn/more-pages)。


## 本地预览网站
运行一个本地服务器通过 ```docsify serve``` 可以方便的预览效果，而且提供 LiveReload 功能，可以让实时的预览。默认访问 [http://localhost:3000](http://localhost:3000) 。
```bash
ocsify serve docs
```
# 多页文档
## 定制侧边栏
为了获得侧边栏，您需要创建自己的_sidebar.md，你也可以自定义加载的文件名。默认情况下侧边栏会通过 Markdown 文件自动生成，效果如当前的文档的侧边栏。

首先配置 ```loadSidebar``` 选项，具体配置规则见[配置项#loadSidebar](https://docsify.js.org/#/zh-cn/configuration?id=loadsidebar)。

```html
<!-- index.html -->

<script>
  window.$docsify = {
    loadSidebar: true
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
```
接着创建 ```_sidebar.md``` 文件，内容如下

```markdown
<!-- docs/_sidebar.md -->

* [首页](zh-cn/)
* [指南](zh-cn/guide)
```
需要在 ```./docs``` 目录创建 ```.nojekyll``` 命名的空文件，阻止 GitHub Pages 忽略命名是下划线开头的文件。


## 显示目录
自定义侧边栏同时也可以开启目录功能。设置 ```subMaxLevel``` 配置项，具体介绍见 [配置项#sub-max-level](https://docsify.js.org/#/zh-cn/configuration?id=sub-max-level)。
```html
<!-- index.html -->

<script>
  window.$docsify = {
    loadSidebar: true,
    subMaxLevel: 2
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
```

## 忽略副标题
当设置了 ```subMaxLevel``` 时，默认情况下每个标题都会自动添加到目录中。如果你想忽略特定的标题，可以给它添加 ```{docsify-ignore}``` 。
```markdown
# Getting Started

## Header {docsify-ignore}

该标题不会出现在侧边栏的目录中。
```

要忽略特定页面上的所有标题，你可以在页面的第一个标题上使用 ```{docsify-ignore-all}``` 。
```markdown
# 入门 {docsify-ignore-all}

## 标题

该标题不会出现在侧边栏的目录中。
```
在使用时， ```{docsify-ignore}``` 和 ```{docsify-ignore-all}``` 都不会在页面上呈现。

# 封面
通过设置 ```coverpage``` 参数，可以开启渲染封面的功能。具体用法见[配置项#coverpage](https://docsify.js.org/#/configuration?id=coverpage)。

## 基本用法

封面的生成同样是从 ```markdown``` 文件渲染来的。开启渲染封面功能后在文档根目录创建 ```_coverpage.md``` 文件。渲染效果如本文档。

index.html
```html
<!-- index.html -->

<script>
  window.$docsify = {
    coverpage: true
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
```
_coverpage.md

```markdown
<!-- _coverpage.md -->

![logo](_media/icon.svg)

# docsify <small>3.5</small>

> 一个神奇的文档网站生成器。

- 简单、轻便 (压缩后 ~21kB)
- 无需生成 html 文件
- 众多主题

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#docsify)
```

## 自定义背景
目前的背景是随机生成的渐变色，我们自定义背景色或者背景图。在文档末尾用添加图片的 Markdown 语法设置背景。

_coverpage.md
```markdown
<!-- _coverpage.md -->

# docsify <small>3.5</small>

[GitHub](https://github.com/docsifyjs/docsify/)
[Get Started](#quick-start)

<!-- 背景图片 -->

![](_media/bg.png)

<!-- 背景色 -->

![color](#f0f0f0)
```

# 插件列表

## 全文搜索
全文搜索插件会根据当前页面上的超链接获取文档内容，在 ```localStorage``` 内建立文档索引。默认过期时间为一天，当然我们可以自己指定需要缓存的文件列表或者配置过期时间。

```html
<script>
  window.$docsify = {
    search: 'auto', // 默认值

    search : [
      '/',            // => /README.md
      '/guide',       // => /guide.md
      '/get-started', // => /get-started.md
      '/zh-cn/',      // => /zh-cn/README.md
    ],

    // 完整配置参数
    search: {
      maxAge: 86400000, // 过期时间，单位毫秒，默认一天
      paths: [], // or 'auto'
      placeholder: 'Type to search',

      // 支持本地化
      placeholder: {
        '/zh-cn/': '搜索',
        '/': 'Type to search'
      },

      noData: 'No Results!',

      // 支持本地化
      noData: {
        '/zh-cn/': '找不到结果',
        '/': 'No Results'
      },

      // 搜索标题的最大层级, 1 - 6
      depth: 2,
    }
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/plugins/search.min.js"></script>
```
# 查看源码
如果你需要源码，可以在我的github上下载或查看 [yangpan-docsify](https://github.com/yangpan4037/yangpan-docsify)

# 参考文档
[docsify中文文档](https://docsify.js.org/#/zh-cn/?id=docsify)