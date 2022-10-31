---
title: "Hugo | Hugo-Cactus-theme-mod"
date: 2022-10-31T03:02:52+08:00
description: 我怎么又在做样板间啊
tags:
  - Hugo
categories:
  - 甘普基本变形定律
image: 
slug: cactus-mod


---

> {{< emoji name="artist"  ext="png" width="35" >}} 修改主题的时候没有想过公开，很多地方改得很乱，见笑——能用就行！


Github仓库地址：[Mantyke/hugo-theme-cactus-mod
](https://github.com/Mantyke/hugo-theme-cactus-mod)

原主题为[hugo-theme-cactus](https://github.com/monkeyWzr/hugo-theme-cactus)，这个仓库是由[Mantyke](https://mantyke.icu/)修改的魔改版本

预览：[Demo站](https://cactus-mod.mantyke.icu/)

<br>

## 使用方式

**从零建立博客**：Fork仓库到自己账号下，用Github 注册 Vercel，依次点击Overview → New Project → import刚刚Fork的仓库，设置`FRAMEWORK PRESET`为Hugo → 点击`Environment Variables`，设置`NAME`为`HUGO_VERSION`，`Value`为`Hugo版本号（如0.89.0）` → 点击Add → 点击Deploy，稍等十来秒即可部署完成。下载仓库到本地后使用Github Desktop更新文章。（注，本地预览需安装Hugo，具体请参照[Hugo | 一起动手搭建个人博客吧](https://mantyke.icu/2021/hugo-build-blog/)相关内容）

**旧博客更换主题**：不同主题方式不同，推荐只保留原博客的content文件夹，迁移到本仓库content文件夹后再按情况调整。

**注意**：Vercel 已在国内被 GFW 封锁，如需要在国内地区访问，推荐自购域名绑定至 Vercel，或者使用 Github/Netlify 等类似服务。
具体步骤可以参考[Hugo | 一起动手搭建个人博客吧](https://mantyke.icu/2021/hugo-build-blog/)中的 “Q：我觉得 Vercel 分配的域名太丑了”/“Q：我不想花钱，但还是觉得 Vercel 分配的域名太丑了”两条问答。

## 魔改内容
1. 删除原仓库中部分代码，优化了在国内地区的访问速度。
2. 修改菜单栏，可以将不同类型的文章分开放置（即Weekly/Other两个页面），并各自独立归档。
3. About页面显示个人标签墙 （by [Mengru](https://mengru.space/?posts/2022/07/——magic-toys#%E6%A0%87%E7%AD%BE%E5%A2%99)）
4. 书影游墙，点击About页面-偶尔读书/有时电影/抽空游戏三个Tag打开 （[同步workflow](https://imnerd.org/doumark.html) by怡红公子，[页面代码](https://immmmm.com/doumark-action/)by林木木，稍有修改）
5. 随机访问友链（by [Mengru](https://mengru.space/?posts/2022/07/magic-toys#%E5%86%B2%E6%B5%AA%E6%97%B6%E9%97%B4)）
6. 内置Waline评论，需要自己配置serverURL
7. 单图首页页面布局
8. 根据个人喜好修改了一部分CSS



## 部分使用说明
### 基础配置
博客名称、描述等修改根目录下`config.toml`文件

```
title = "Hugo-Cactus-theme-mod" //引号内写博客名称
description = "Hugo-cactus-mod by 塔塔" //引号内写描述内容，会出现在首页
```

### 配色修改
mod默认使用主题中提供的classic配色方案，如需修改标题颜色/菜单栏颜色，修改`assets/scss/colors/classic.scss`文件中的`$color-accent-1: #000000;`一行。

修改加粗字体颜色，修改`assets/scss/colors/classic.scss`文件中

```
strong {
  font-weight: bold;
  color: #000000;
}
```
### Weekly/Other
新建文章命令：（以新建到Weekly页面为例）使用`hugo new weekly/2022/文章名称.md`，2022是年份名，用于在文件中整理文件，可以忽略。

修改页面名称与路径：根目录下打开`config.toml`文件

```
[[menu.main]]
name = "Weekly"
url = "/weekly"
weight = 2

[[menu.main]]
name = "Others"
url = "/posts"
weight = 3
```

### 标签墙
标签墙修改：`/layouts/_default/tags-wall.html`，如果要给Tag带链接，链接中所有的`/`均需要写成`&#47;`，否则链接无法正常访问。同样地，不规定标签大小时（随机大小），也需要把中间的一个`/`写成`&#47;`  
页面标题/路径修改：`content`文件夹下`tags-wall.md`

另外注意：如果使用书影游墙，请一定记得替换相应Tag的网址。

### 书影游墙
数据放在`/data/douban`文件夹`book.csv`/`game.csv`/`movie.csv`三个文件中，修改后可以通过`http://localhost:1313/books/`等地址预览效果。

页面标题/路径修改：`content`文件夹下`book.md`、`movies.md`、`games.md`

数据同步、表格填写及其他定制参考：[Blog | 主题重新施工，和书影游展示墙](https://mantyke.icu/posts/2022/a-flower-upon-your-return/)

### 随机友链

使用方式：建立一个公开的Github Repo，在Repo中新建一个Issues，名字可以随意，填入以下内容后提交：

```
{
personal: {
name: '个人网站',
items: [
'https://mengru.space/',
'https://mantyke.icu/',
],
}
}
```
复制链接，在前面加上`api.`，例如，复制下来的链接是`https://github.com/你的ID/friend-link/issues/1`，把它修改为`https://api.github.com/你的ID/friend-link/issues/1`，将修改后的链接写入`content/friends/index.html`中

```
fetch('填入链接地址')
  .then(res => res.json())
  .then(data =>
    toObject(
      data.body.replace(/`/g, '').replace(/\n/g, '')  
  )
  .then(data => {
    init(data)
  })
```
修改“全部友链”页面：`content/friendslink/index.md`

### 评论（使用Waline）
相关配置位置：`layouts/partials/comments/cactus_comments.html`      
根据[官方文档](https://waline.js.org/guide/get-started.html)配置完leancloud和Vercel后，打开`cactus_comments.html`，将域名填入`serverURL: '',`。     
修改了一些css，配置了blobcat表情包，默认头像为`monsterid`。

### 首页布局设置
更换首页图片：`layouts/index.html`，修改图片链接：

```
<div>
  <img id="feiyu" src="">
</div>
```

### 设置博客头像

位置：`/static/img`，请将头像命名为`avatar.png`，替换原头像文件

### 修改页尾信息

站点名称及建站时间请修改以下代码

站点名称及链接：

```
#位置：/layouts/partials/footer.html

    <section class="copyright">
        &copy; 
        {{ if and (.Site.Params.footer.since) (ne .Site.Params.footer.since (int (now.Format "2006"))) }}
            {{ .Site.Params.footer.since }} - 
        {{ end }}
        {{ now.Format "2006" }} <a href="https://stack-theme-mod.vercel.app/">Example Site</a>·<i class="fas fa-bell"></i> <a id="days">0</a>Days<br>
      {{$var :=  $scratch.Get "total"}}{{$var = div $var 100.0}}{{$var = math.Ceil $var}}{{$var = div $var 10.0}}共书写了{{$var}}k字·共 {{ len (where .Site.RegularPages "Section" "post") }}篇文章</br><span><p>
    </section>
```

```
#位置：/layouts/partials/footer.html

var s1 = '2022-02-13';//设置为建站时间
s1 = new Date(s1.replace(/-/g, "/"));
s2 = new Date();
var days = s2.getTime() - s1.getTime();
var number_of_days = parseInt(days / (1000 * 60 * 60 * 24));
document.getElementById('days').innerHTML = number_of_days;
```