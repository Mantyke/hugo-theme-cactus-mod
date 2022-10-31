---
title: Hugo Cactus主题国内优化
date: 2021-11-24T19:50:25+08:00
---

<!--more-->

Hugo是一款基于Golang的博客框架，速度极其快，通常远高于Vuepress、Hexo、Jekyll等基于解释型语言的框架。

Cactus主题是一款十分简洁的主题，从Hexo平台移植过来的。然而，它对于国内用户而言不太友好，Cactus主题内部的部分文件引用来自国外。因此，笔者将引用改为本地引用。在此之上，笔者还增加了一种新的主题，命名为Autumn。

即使使用了笔者修改过的Cactus主题，相比其他主题，该主题依然有更多的步骤。因此在此给出指导方向。基于Windows安装，其他操作系统更加简单。

### 1. Golang、MinGW的安装

这是[Golang官网](https://go.dev/)，依据操作系统下载最新版本，并且安装。安装完毕后，请保证Golang的环境变量已经正确设置。

```shell
go version
```

这是[Sourceforge的系统文件目录](https://jaist.dl.sourceforge.net/project/mingw-w64/Toolchains%20targetting%20Win32/Personal%20Builds/mingw-builds/installer/)，下载mingw-w64-install.exe，并安装。在安装时，请注意记录其安装位置，需要手动添加Path环境变量。

```shell
gcc --version
```

### 2. 安装Hugo与Hugo extended

推荐采用Golang的安装方式，而不采用Choco的方式，以便于Golang后续对包的管理。

我们必须安装extended版本，因为Cactus主题中使用了SCSS。

```shell
mkdir %USERPROFILE%/src
cd %USERPROFILE%/src
git clone https://github.com/gohugoio/hugo.git
cd hugo
$CGO_ENABLED = 1
go install --tags extended
```

随后测试hugo是否安装成功。

```shell
hugo version
```

### 3. 使用Hugo创建项目

创建Hugo项目需要实现准备一个空的文件夹，假设名为hugo-blog。

```shell
mkdir ./hugo-blog
hugo new site ./hugo-blog
cd ./hugo-blog
```

下载笔者修改过的cactus主题。
```shell
mkdir themes
git remote add origin git@github.com:seekwindJH/hugo-theme-cactus-seekwind-edition.git
```

### 4. TOML配置文件

下面给出了配置文件的示例。

```toml
languageCode = "zh-CN"
title = "追风の物语"
theme = "cactus"
copyright = "SeekWind" # cactus will use title if copyright is not set


# summaryLength = 2

# Main menu which appears below site header.
[[menu.main]]
name = "Home"
url = "/"
weight = 1

[[menu.main]]
name = "Posts"
url = "/posts"
weight = 2

[[menu.main]]
name = "Docs"
url = "/docs"
weight = 3

[[menu.main]]
name = "Categories"
url = "/categories"
weight = 4

[[menu.main]]
name = "Tags"
url = "/tags"
weight = 5

[[menu.main]]
name = "About"
url = "/about"
weight = 6

[markup]
  [markup.tableOfContents]
    endLevel = 4
    ordered = false
    startLevel = 2
  [markup.highlight]
    codeFences = true
    guessSyntax = false
    hl_Lines = ""
    lineNoStart = 1
    lineNos = true
    # line Number will Not Be copied
    lineNumbersInTable = true
    noClasses = true
    # perldoc paraiso-dark pastie
    style = "perldoc"
    tabWidth = 4

[params]
  
  colortheme = "autumn" # dark, light, white, classic, autumn
  rss = false # generate rss feed. default value is false
  showAllPostsArchive = false # default

  # Home page Main Section settings
  description = "SeekWind的文档站。愿你所追寻的风，都能如期而至。"
  mainSection = "posts" # your main section
  mainSectionTitle = "笔记"
  showAllPostsOnHomePage = false # defaultmnm     bnmnmfmj676     vbnvnvnbbbbbbbbbbbbbbbbbbbbbbbb    vny=t
  postsOnHomePage = 6 # this option will be ignored if showAllPostsOnHomePage is set to true
  
  # Home page Project Section settings
  projectsSectionTitle = "作品"
  tagsOverview = true # show tags overview by default.
  showProjectsList = true # show projects list by default (if projects data file exists).
  projectsUrl = "https://github.com/seekwindJH/seekwindJH.github.io" # title link for projects list

  # https://gohugo.io/functions/format/#hugo-date-and-time-templating-reference
  dateFormat = "Mon, 15:04:05" # post Date Format
  homeDatePageFormat = "2006-01-02 Mon" # Home page Date Format, more purity

  # Post page settings
  show_updated = true # default
  showReadTime = true # default

  [params.comments]
    enabled = true # default
    engine = "utterances" # only disqus, utterances, and cactus_comments is supported
    [params.comments.utterances]
      repo = "seekwindJH/seekwindJH.github.io"
      label = "追风の物语" # you can use however you want to label your name in your repo's issues
      theme = "gruvbox-dark"


  # the value of name should be an valid font awesome icon name (brands type)
  # https://fontawesome.com/icons?d=gallery&s=brands
  [[params.social]]
    name = "github"
    link = "https://github.com/seekwindJH"

  [[params.social]]
    name = "email"
    link = "seekwind@foxmail.com"  # no need for "mailto:" in the head

[author]
    name = "SeekWind"
    homepage = "https://github.com/seekwindJH"
tags = ["blog", "simple", "clean"]
features = ["mathjax", "customizable", "color"]
description = "SeekWind的文档站"
```