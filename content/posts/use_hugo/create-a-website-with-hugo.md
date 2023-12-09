+++
title = '使用 Hugo 创建一个网站'
date = 2023-12-09T17:46:42+08:00
draft = false
+++

使用 Hugo 创建网站大致可分为三步：
1. 安装 Hugo
2. 初始化创建一个网站
3. 编写网站配置

在这之后，便可以编写与发布文章。

## 安装 Hugo

> 本文安装环境为 Windows，若需要在其他平台安装请参考 [官方文档](https://gohugo.io/installation/)。

Hugo 目前共有两个版本：标准版和扩展版，官方推荐使用扩展版。本着多多益善我们选择扩展版，目的是为了省事，避免以后在使用过程中功能上缺东少西，影响我们写作。

Hugo 的安装方式有以下两种：
- 预生成的二进制文件
- 包管理器

在这里我们使用最快捷的方法，使用从 Windows 10 开始自带的包管理器 `winget` 进行安装。

### 使用 Winget 安装 Hugo

打开命令行输入下方命令，等待片刻，便可安装成功。

```bash
winget install Hugo.Hugo.Extended
```

## 创建网站

在创建网站的过程中，我们可以选择使用 `git` 来进行版本管理。使用 `git` 进行版本管理，可以方便我们后期将网站上传至 GitHub、GitLab 和 Gitee 等平台，进行托管。

在命令行中运行以下命令，创建一个使用 `PaperMod` 主题的网站。

```bash
hugo new site mywebsite
cd mywebsite
git init
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
echo "theme = 'PaperMod'" >> hugo.toml
hugo server
```

执行完以上命令，Hugo 会在命令行中输出执行日志，而在日志中会显示本地预览网站的 URL（`http://localhost:1313/`）。

按下 `Ctrl + C` 可以停止预览网站。

### 命令解释

- `hugo new site mywebsite` 
	- 在当前文件夹创建一个名为 `mywebsite` 的站点，其本质是一个目录。
- `cd mywebsite`
	- 进入站点目录
- `git init`
	- 使用 `git` 将本站点目录初始化为 `git` 仓库，从而进行版本管理。
- `git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod`
	- 从 GitHub 下载 Hugo 网站的主题，并放入 `themes` 文件夹，并添加为项目子模块。
	- `git submodule add`：将下载的主题作为本仓库的子模块，进行独立管理，可在本仓库覆盖子模块相应内容，而不需要修改子模块内容。在后续升级主题时，就不需要迁移自定义的内容，到新主题中。
	- 关于此命令更详细的内容请参见 [Git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
- `echo "theme = 'PaperMod'" >> hugo.toml`
	- 将 `theme = 'PaperMod'` 追加进 `hugo.toml` 网站的配置文件，设置当前网站主题为 `PaperMod`。
- `hugo server`
	- 启动 Hugo 的开发服务器，来预览网站。
	- 按下 `Ctrl + C` 可停止 Hugo 开发服务器

## 编写网站配置文件

使用任意的文本编辑器，打开位于网站项目的根目录下的配置文件（`hugo.toml`），编写符合 TOML 语法的配置项，来完成对网站的配置。

`hugo.toml` 初始内容如下：

```toml
baseURL = 'https://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
theme = 'PaperMod'
```

- `baseURL`
	- 设置网站的基础网址，作为之后页面的前缀，而不需要每次都将 `https://example.org/` 再写一遍。
- `languageCode`
	- 设置网站的语言
- `title`
	- 设置网站的标题
- `theme`
	- 设置网站的主题

我们将修改 `baseURL`、`languageCode` 和 `title` ，修改结果如下：

```toml
baseURL = 'https://example.org/'
languageCode = 'zh-CN'
title = '我的新网站'
theme = 'PaperMod'
```

## 为网站添加内容

运行以下命令，创建一个名为 `my-first-post` 的 Markdown 文件。

```bash
hugo new content posts/my-first-post.md
```

文件路径位于 `content/posts` 目录下。使用任意文本编辑器打开此文件，发现 Hugo 为我们默认生成了一些内容。

```yaml
+++
title = '使用 Hugo 创建一个网站'
date = 2023-12-09T17:46:42+08:00
draft = true
+++
```

这是 Front matter 块包裹的配置项，使用 yaml 语法，可以在其中添加关于此页面的相关信息，比如标题、更新时间和标签，等等。

需要特别说明的是 `draft` 为 `true`，它表示本文为草稿，在预览网站和正式发布时，Hugo 都不会让其显示出来。

若要让本文可以显示，只需要让 `draft` 值为 `false` 即可。

接下来添加一些符合 Markdown 语法的文本进入此文件。

```markdown
## Introduction

This is **bold** text, and this is *emphasized* text.

Visit the [Hugo](https://gohugo.io) website!
```

至此我们便完成了一篇文章。

预览网站时，若要包含标记为草稿的页面，可以在启动 Hugo 开发服务器时添加 `-D` 或 `--buildDrafts` 参数。

```bash
hugo server -D
hugo server --buildDrafts
```

## 发布网站

终于到最后一步了，我们可以正式发布网站了 🎉

只需要运行一句简单的命令，便可生成整个静态网站。

```bash
hugo
```

`hugo` 运行结束后，会在项目的根目录下创建一个 `public` 目录，来存放生成的静态网站。

在下一站，我们会继续进行我们的探险，将网站部署在服务器上，供大家访问。