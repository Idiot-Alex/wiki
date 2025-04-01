# 使用 Cloudflare 部署 Docsify 博客

## 简介

[Docsify](https://docsify.js.org/#/) 是一个神奇的文档站点生成器，可以快速搭建个人博客或文档站点。结合 [Cloudflare Pages](https://www.cloudflare.com/zh-cn/pages/)，可以免费、快速、安全地部署你的 Docsify 博客。

我尝试过不少的 blog 框架或者平台，像是：
* [Hexo](https://hexo.io/zh-cn/)
* [Hugo](https://gohugo.io/)
* [csdn](https://www.csdn.net/)
* [博客园](https://www.cnblogs.com/)
* [Github Pages](https://pages.github.com/)

她们都有各自的优缺点，像是：
* **Hexo** 和 **Hugo**：需要搭建环境，配置较复杂，总是会陷入各种主题的选择。
* **CSDN** 和 **博客园**：需要注册账号，且有广告。
* **GitHub Pages**：需要配置 Jekyll 等框架作为内容的渲染层，且中国大陆可能无法访问。

我认为 blog 的本质是：
* 一个静态的文档站点，内容是 Markdown 格式的文本，渲染成 HTML 页面。
* 搭建之后就不用管框架的升级和维护，只需要专注于内容的撰写。
* 可以随时随地访问，也就是托管平台最好带 CDN 加速。
* 文章最好还支持搜索，像一个 wiki 一样，不光方便读者，也方便自己检索、重新整理。

于是，我发现了这个 Docsify，目前看上去是满足我的需求，但是官方的文档上并没有提到如何部署到 Cloudflare Pages 上，于是我自己尝试了一下，下面是我的一些经验。

## 前提条件
* 你需要安装 [Node.js](https://nodejs.org/) 和 [npm](https://www.npmjs.com/)。
* 你需要根据 Docsify 创建一个项目：[Docsify](https://docsify.js.org/#/quickstart?id=install-docsify-cli)。
* 你需要一个 [Cloudflare 账户](https://www.cloudflare.com/zh-cn/)。

## 步骤

### 1. 创建 Docsify 项目

首先，创建一个目录作为你的博客根目录，例如 `my-blog`。

```bash
mkdir my-blog
cd my-blog
```

然后，初始化 Docsify 项目：

```bash
docsify init ./docs
```

这会在 `my-blog` 目录下创建一个 `docs` 目录，包含以下文件：

*   `index.html`:  入口文件
*   `README.md`:  默认的首页内容

### 2. 配置 Cloudflare Pages

1.  在 my-blog 目录创建一个名为 wrangler.jsonc 的配置文件
2.  里面的内容（name 可以随意修改，会作为 Cloudflare Pages 的项目名称）参考如下：
```wrangler.jsonc
  {
    "name": "wiki",
    "compatibility_date": "2025-03-25",
    "assets": {
      "directory": "./docs"
    }
  }
```
3.  把 my-blog 推送到 GitHub 上，创建一个新的仓库，例如 `my-blog`。
4.  **创建 Pages 项目：** 在 Cloudflare 控制台中，选择 "Pages"，然后点击 "Create project"。
5.  **连接 GitHub 仓库：** 选择你刚刚创建的 GitHub 仓库。
6.  **配置构建：**
    *   **Production branch：** 选择你的主分支（例如 `main` 或 `master`）。
    *   **Build command：**  留空，Docsify 不需要构建命令。
    *   **Deploy command：**  npx wrangler deploy
    *   **Build output directory：**  使用默认的 /，因为我们已经在 wrangler.jsonc 中指定了 assets 目录。
7.  **点击 "Save and Deploy"：** Cloudflare Pages 会自动部署你的博客。

### 3. 部署

Cloudflare Pages 会自动构建和部署你的 Docsify 博客。你可以在 Cloudflare 控制台中查看部署状态。部署完成后，你将获得一个 Cloudflare Pages 提供的域名，例如 `your-blog.pages.dev`。

你还可以将你的博客绑定到自定义域名。

## 总结

通过以上步骤，你可以快速地将 Docsify 博客部署到 Cloudflare Pages 上。你可以随时更新你的博客内容，只需在 `docs` 目录下修改 Markdown 文件，然后提交到 GitHub，Cloudflare Pages 会自动重新部署。

本质上 Docsify 就是一个静态的文档站点生成器，它不会修改你的 Markdown 文件，只会将它们渲染成 HTML 页面。

我很喜欢这样的方式，因为它让我专注于内容的撰写，而不必担心框架的升级和维护。

它的页面像一个 wiki，我觉得这才是博客的本质，博客内容不一定是完整的、逻辑清晰的文章；也可以是一些随笔、笔记、想法的集合，也许现在不完整，但是未来会被重新整理成一篇篇文章。

相比于一次性写出一篇完整的文章，我更喜欢这种随时随地记录的方式。