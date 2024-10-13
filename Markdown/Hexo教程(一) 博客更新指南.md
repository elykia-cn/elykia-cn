# Hexo教程(一) 博客更新指南

Hexo 是一个强大的静态网站生成器，然而[Hexo 官方文档](https://hexo.io/zh-cn/docs/writing.html)往往缺乏对日常操作的指导性，难以分清阅读顺序和内容的重要性。为了更便捷地使用 Hexo 更新博客，这里总结了一套完整的操作流程，并附上每一步背后的原理解释。

## 一次 Hexo 更新博客的基础操作流程

1. 新建一篇文章
2. 文章写作
3. 本地预览更新后的博客
4. 远程部署更新博客

## 1. 新建文章

Hexo 渲染一篇 post 的流程大致如下：

1. 获取一个 Markdown 文件
2. 根据该文件的 layout 类型进行样式排版
3. 生成 HTML 文件
4. 最终展示到网页端

### 方法一：使用 Hexo 的命令

创建文章的常见命令：

```bash
hexo new [layout] <title>
```

通常我们只需使用默认 layout（post）：

```bash
hexo new "我的第一篇文章"
```

该命令会生成一个 **layout** 为 `post` 类型的 Markdown 文件，存储在 `source/_posts/` 目录下，命名为 `我的第一篇文章.md`。

### 方法二：手动创建 Markdown 文件

你也可以直接在编辑器（如 Typora）中新建文件，并将其保存到 `source/_posts/` 文件夹中。

这种方法的优点是快捷灵活，尤其是当需要同时处理多个文档时。但缺点是需要手动添加文章头部信息（Front Matter），例如：

```yaml
---
title: "我的第一篇文章"
date: 2024-09-01
author: "作者"
categories: ["分类1"]
tags: ["标签1"]
---
```

**注意：** 如果文章包含图片或其他附件，需要在 `source/_posts/` 目录下创建一个同名文件夹存放这些附件。

## 2. 文章写作

按照 Markdown 格式编写文章内容，下面介绍一些常用的额外功能：插入本地图片、展示本地 PDF、展示数学公式、链接到本站其他博客。

### 插入本地图片

可以使用以下几种方式插入本地图片：

1. **绝对路径引用：**

   将图片存储于 `source/images` 文件夹中，然后在 Markdown 文件中插入：

   ```markdown
   ![图片描述](images/图片名.格式)
   ```

2. **相对路径引用：**

   将图片存储于与文章同名的文件夹中，然后插入：

   ```markdown
   ![图片描述](图片名.格式)
   ```

3. **HTML 语法引用：**

   ```html
   <img src="图片名.格式" width="50%" height="100%" title="描述" alt="替代描述"/>
   ```

**推荐使用相对路径或绝对路径引用，简洁且兼容性好。**

### 展示本地 PDF

使用插件 `hexo-pdf` 或 Markdown 自带的文件引用方式：

1. **使用插件：**

   安装插件：

   ```bash
   npm install --save hexo-pdf
   ```

   在 Markdown 文件中插入：

   ```markdown
   {% pdf 文件名.pdf %}
   ```

2. **Markdown 文件引用：**

   ```markdown
   [PDF 文件描述](文件名.pdf)
   ```

插件方式更适合直接阅读 PDF 内容，而文件引用则生成下载链接。

### 展示数学公式

1. 安装渲染器插件：

   ```bash
   npm install hexo-renderer-kramed --save
   ```

2. 修改 `_config.yml` 配置文件：

   ```yaml
   mathjax:
     enable: true
   ```

3. 在 Markdown 文件的 Front Matter 中启用 MathJax：

   ```yaml
   mathjax: true
   ```

4. 使用 Markdown 语法插入公式：

   - 行内公式：`$ 公式 $`
   - 块级公式：`$$ 公式 $$`

### 链接到本站其他博客

使用相对路径引用已发布的 HTML 文件。例如，链接到另一篇文章：

```markdown
[另一篇文章](../文章路径/index.html)
```

非常抱歉我遗漏了这些重要内容。我会把它们加回来,并稍作调整以使整体结构更加连贯。以下是修改后的部分:

### 草稿

可以在`source/_drafts/`文件夹下创建markdown文件作为草稿。这些文档不会被渲染,也不会显示在最终的博客页面中。当你准备发布时,可以使用`hexo publish [layout] <title>`命令将草稿移动到`_posts`文件夹。

## 3. 本地预览

常用命令:

- `hexo clean`: 清理生成的文件
- `hexo g` 或 `hexo generate`: 生成静态文件
- `hexo s` 或 `hexo server`: 启动本地服务器
- `hexo s -p 5000`: 指定端口号(例如5000)

这些命令的具体作用:

- `hexo clean`: 删除整个`public/`文件夹。通常在更新了`_config.yml`或删除了已有博文后使用。由于耗时较长,建议偶尔使用。
- `hexo generate`: 生成静态文件夹`public/`,将markdown转换为html等网页文件。
- `hexo server`: 将生成的静态文件部署到本地指定端口。默认可通过`localhost:4000`访问。

通常的预览流程:

1. `hexo g; hexo s` 
2. 浏览器访问`http://localhost:4000`预览
3. 修改文章内容,刷新页面即可看到更新

Hexo的一个便利特性是,在本地预览时,你可以直接修改Markdown文件,然后刷新浏览器就能看到更新,无需重新执行`hexo g`。这包括修改文章内容、Front-Matter属性等。

但对于某些深层次修改(如主题文件、js函数、css样式等),可能仍需要重新生成。

注意: 使用`Ctrl+C`(而非`Ctrl+Z`)来停止本地服务器。如果遇到端口被占用的错误,可以使用以下命令解决:

```bash
lsof -i:4000
kill -9 <pid>
```

## 4. 远程部署  

常用命令:

- `hexo d` 或 `hexo deploy`: 部署到远程服务器
- `hexo g -d`: 生成并部署

`hexo deploy`命令会将网站部署到服务器上,实际上是更新你GitHub仓库`**.github.io`的指定分支。

完整的部署流程:

1. `hexo g; hexo s` 本地预览直到满意
2. `hexo d` 或 `hexo g -d` 部署到远程

3. 如果使用双分支管理,还需要更新源文件分支:
   ```
   git add .
   git commit -m "更新说明"
   git push
   ```

## 引用

1. [hexo官方文档](https://hexo.io/zh-cn/docs/writing.html)
2. [知乎Hexo博客写文章及基本操作](https://zhuanlan.zhihu.com/p/156915260)
3. [Front-Matter official manual](https://hexo.io/zh-cn/docs/front-matter)
4. [YouTube Hexo系列视频](https://www.youtube.com/watch?v=Jiwbmyc4nCA&list=PLLAZ4kZ9dFpOMJR6D25ishrSedvsguVSm&index=6)
5. [赵大宝hexo-images](https://fuhailin.github.io/Hexo-images/)
6. [csdn博客hexo添加pdf插件](https://blog.csdn.net/u010820857/article/details/82356974)
7. [csdn:hexo博客支持数学公式](https://blog.csdn.net/qq_38496329/article/details/104065659)
8. [一次完整的Hexo写作流程](https://fuguigui.github.io/hexo2/)

