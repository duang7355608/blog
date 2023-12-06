---
layout: "post"
title:  "Initial GitHub pages site with Jekyll"
date:   "2023-12-02 00:00:00 +0800"
categories: "jekyll github github-pages"
---
{% include toc.md %}

> **官方文档** [ Initial GitHub pages site with Jekyll](https://docs.github.com/zh/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)

## 远程仓库准备

> **普通账户只有公开库才能使用githubPages功能**
>

* 或者在github上新创建一个仓库，作为站点发布专用仓库

* 或者在原有仓库上新建一个空内容分支作为站点发布专用分支，比如：gh-pages

* 或者在原有仓库上根目录下新建一个空目录作为站点发布专用目录，比如：/docs

### 常用命令

create a new repository on the command line

```
echo "# blog" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/duang7355608/blog.git
git push -u origin main
```

…or push an existing repository from the command line

```
git remote add origin https://github.com/duang7355608/blog.git
git branch -M main
git push -u origin main
```

## Jekyll快速开始

[windows环境]

### 1. 安装一个完整的Ruby开发环境

- [官方文档](https://jekyllrb.com/docs/installation/)
- [中文文档](https://www.jekyll.com.cn/docs/installation/)
- Windows 安装：[https://jekyllrb.com/docs/installation/windows/](https://jekyllrb.com/docs/installation/windows/)

### 2. 安装 Jekyll 和 bundler gems

   ```bash
   gem install jekyll bundler
   ```

检查环境

   ```bash
  ruby -v
  bundler -v
  jekyll -v
   ```

### 3. 创建Jekyll站点

任意仓库新建一个gh-pages分支doc目录的方式

   ```bash
$ mkdir docs
# Creates a new folder called docs
$ cd docs

$ git checkout --orphan gh-pages
# Creates a new branch, with no history or contents, called gh-pages, and switches to the gh-pages branch
$ git rm -rf .
# Removes the contents from your default branch from the working directory
   ```

若要创建新 Jekyll 站点，请使用 jekyll new 命令

   ```bash
$ jekyll new --skip-bundle .
# Creates a Jekyll site in the current directory
   ```

### 4. 修改配置文件适配githubPages

打开 Jekyll 创建的 Gemfile 文件。

#### 将“#”添加到以 gem "jekyll" 开头的行首，以注释禁止此行。

   ```
#gem "jekyll", "~> 4.3.2"
   ```

#### 编辑以 # gem "github-pages" 开头的行，以添加 github-pages gem。

将此行更改为：

   ```
gem "github-pages", "~> GITHUB-PAGES-VERSION", group: :jekyll_plugins
   ```

将 GITHUB-PAGES-VERSION 替换为 github-pages gem 的最新支持版本。 可以在以下位置找到这个版本：["依赖项版本"](https://pages.github.com/versions/) 。

正确版本的 Jekyll 将安装为 github-pages gem 的依赖项。

#### 安装 github-pages gem

```bash
bundle install
```

#### （可选）对 _config.yml 文件进行任何必要的编辑。 当仓库托管在子目录时相对路径需要此设置。

 ```bash
> domain: my-site.github.io       # if you want to force HTTPS, specify the domain without the http at the start, e.g. example.com
> url: https://my-site.github.io  # the base hostname and protocol for your site, e.g. http://example.com
> baseurl: /REPOSITORY-NAME/      # place folder name if the site is served in a subfolder
 ```

### 5. 运行jekyll服务,使用 Jekyll 在本地测试 GitHub Pages 站点

#### 本地运行jekyll服务

  ```bash
  bundle exec jekyll serve
  ```

要强制浏览器在每次更改时刷新，请使用

  ```bash
  bundle exec jekyll serve --livereload
  ```

#### 可能遇到的错误

> 注意：如果已安装 Ruby 3.0 或更高版本（如果通过 Homebrew 安装了默认版本，则表示可能已经安装），你可能会在此步骤中遇到错误。 这是因为这些版本的 Ruby 不再附带安装 webrick。 要修复错误，请尝试运行 bundle add webrick，然后重新运行 bundle exec jekyll serve。

或者在Gemfile文件中最后添加一行

```
gem "webrick", "~> 1.8"
```

#### 更新 GitHub Pages gem

Jekyll 是一个活跃的开源项目，经常更新。 如果计算机上的 github-pages gem 与 GitHub Pages 服务器上的 github-pages gem 已过期，则站点外观在本地构建时可能与在 GitHub
上发布时不同。 为避免这种情况，请定期更新计算机上的 github-pages gem。

- 打开Git Bash。
- 更新 github-pages gem。
    - 如果已安装 Bundler，请运行 bundle update github-pages。
    - 如果尚未安装 Bundler，请运行 gem update github-pages。

### 6. 在浏览器中打开 [http://localhost:4000](http://localhost:4000)

如果存在冲突或者您希望 Jekyll 在不同的 URL 上为您的开发站点提供服务，请使用 `--host` 和 `--port`
参数，如 [serve 命令选项](https://jekyllrb.com/docs/configuration/options/#serve-command-options) 中所述。

## 发布 GitHub pages site

### 确认效果后提交推送到远程仓库

  ```bash
git add .
git commit -m 'Initial GitHub pages site with Jekyll'
  ```

### 在github网址对应仓库中设置pages选项

在对应仓库的setting设置中找到pages选项中设置对应分支和目录，save后，会自动发布构建并显示对应地址



---

## 参考资料

- [使用 Jekyll 创建 GitHub Pages 站点 - GitHub 文档](https://docs.github.com/zh/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)

- [使用 Jekyll 在本地测试 GitHub Pages 站点 - GitHub 文档](https://docs.github.com/zh/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)

- [Jekyll官网](https://jekyllrb.com/)

- [Jekyll中文文档](https://www.jekyll.com.cn/)