## Build Github Pages with Hexo

- [Build Github Pages with Hexo](#build-github-pages-with-hexo)
  - [TODOs](#todos)
  - [Environment](#environment)
  - [Install Hexo](#install-hexo)
  - [Hexo Initial](#hexo-initial)
  - [Hexo Project Structure](#hexo-project-structure)
  - [Hexo Generate & Server Start](#hexo-generate--server-start)
  - [Hexo Theme](#hexo-theme)
    - [Install Hexo Theme](#install-hexo-theme)
    - [themes/next/_config.yml](#themesnextconfigyml)
  - [Hexo New Usage](#hexo-new-usage)
    - [Post](#post)
    - [Page](#page)
    - [Draft](#draft)
  - [Hexo Deploy Usage](#hexo-deploy-usage)
  - [What is git subtree, and why should I use it?](#what-is-git-subtree-and-why-should-i-use-it)
  - [Enable Search](#enable-search)
- [Autobuild with Travis](#autobuild-with-travis)
- [Hexo Theme Plugins](#hexo-theme-plugins)

### TODOs
- [x] build & manage Github Pages with Hexo
- [x] autobuild with Travis
- [x] write README document
- [ ] add some Hexo plugins
- [x] enable post views with Leancloud 
- [ ] enable valine the comment plugin

### Environment
```shell
> whereami
In a bookshop with tea.

> git version                                                                                   ⍉
git version 2.13.6 (Apple Git-96)

> hexo -v                                                                                       ⍉
hexo-cli: 1.1.0
os: Darwin 16.7.0 darwin x64
node: 12.1.0
v8: 7.4.288.21-node.16
uv: 1.28.0
zlib: 1.2.11
brotli: 1.0.7
ares: 1.15.0
modules: 72
nghttp2: 1.38.0
napi: 4
llhttp: 1.1.1
http_parser: 2.8.0
openssl: 1.1.1b
cldr: 35.1
icu: 64.2
tz: 2019a
unicode: 12.1
```

### Install Hexo
在安装[Hexo](https://hexo.io/zh-cn/index.html)之前，需要确保已经安装了brew, git, node, npm, cnpm (最好能翻墙)。

安装hexo-cli
```shell
> cnpm install hexo-cli -g
```

### Hexo Initial
1. 在github上创建一个repository，以 `{git.user.name}.github.io`命名

2. 拉取到本地并切换到`develop`分支 **(important!)**
```shell
> git pull https://github.com/{git.user.name}/{git.user.name}.github.io.git
> cd {git.user.name}.github.io && git checkout develop
```

3. 初始化hexo项目
```shell
> hexo init
```
4. 安装node modules
```shell
> npm install
```

### Hexo Project Structure

```
.
├── .gitignore
├── _config.yml
├── db.json
├── node_modules
├── package.json
├── scaffolds
├── source
├── themes
└── yarn.lock
```

### Hexo Generate & Server Start
1. 生成站点文件
站点文件会生成到public目录, `-w`:监听文件变更 `-d` 自动部署
```shell
> hexo g(enerate) [-d] [-w]
```

2. 启动本地服务
```shell
> hexo s(erver)
```
此时，可通过 `http://localhost:4000/`访问本地站点

3. 能成功访问后， 可先发布一个初版到develop分支
```shell
> git add . && git commit -m 'hexo init'
> git push -u origin develop
```
此时是一个绝佳的push时间点，在后面安装主题的时候需要用到subtree来管理本工程的依赖repo，即使subtree管理的repo和本工程的repo相互并没有干扰。这次push会使工程的迭代更清晰和敏捷。

### Hexo Theme 
Hexo提供了主题配置，用优雅的方式让你的站点更优雅！本IO站使用的优雅主题是 [Theme NexT -- 精于心，简于形](https://theme-next.iissnan.com/)，其Hexo支持见[Hexo Theme NexT](https://github.com/theme-next/hexo-theme-next)。

#### Install Hexo Theme
1. fork [Hexo Theme NexT](https://github.com/theme-next/hexo-theme-next). (important!)
2. 给你的站点(`{git.user.name}.github.io`)添加主题remote, url指向刚刚fork的git地址
```shell
> git remote add -f theme-next https://github.com/{git.user.name}/hexo-theme-next.git
```

```shell
> git remote -v
origin	git@github.com:d0ngy1n/d0ngy1n.github.io.git (fetch)
origin	git@github.com:d0ngy1n/d0ngy1n.github.io.git (push)
theme-next	https://github.com/d0ngy1n/hexo-theme-next.git (fetch)
theme-next	https://github.com/d0ngy1n/hexo-theme-next.git (push)
```

3. 添加[subtree](#what-is-git-subtree-and-why-should-i-use-it)连接，并拉取最新文件
```shell
> git subtree add -P themes/next theme-next master --squash

> git fetch theme-next master

> git subtree pull -P themes/next theme-next master --squash

> ls themes/next
```

4. 切换主题为next
修改站点配置文件`_config.yml`
```yml
theme: next
```


#### themes/next/_config.yml
+ Schemes 主题内建样式
```yml
# Schemes
# scheme: Muse
scheme: Mist
#scheme: Pisces
#scheme: Gemini
```

+ Menu 菜单
```yml
menu:
    home: / || home
    categories: /categories/ || th
    tags: /tags/ || tags
    archives: /archives/ || archive
    about: /about/ || user
    #schedule: /schedule/ || calendar
    #sitemap: /sitemap.xml || sitemap
    #commonweal: /404/ || heartbeat
```

menu中开启的菜单项（除home，archives），需要创建对应的页面，创建方式见[Hexo New Usage -> Page](#page)

+ Social 社交组件
```yml
social:
GitHub: https://github.com/yourname || github
#E-Mail: mailto:yourname@gmail.com || envelope
#Weibo: https://weibo.com/yourname || weibo
#Google: https://plus.google.com/yourname || google
#Twitter: https://twitter.com/yourname || twitter
#FB Page: https://www.facebook.com/yourname || facebook
#VK Group: https://vk.com/yourname || vk
#StackOverflow: https://stackoverflow.com/yourname || stack-overflow
#YouTube: https://youtube.com/yourname || youtube
#Instagram: https://instagram.com/yourname || instagram
#Skype: skype:yourname?call|chat || skype
```

+ Sidebar avatar
```yml
# Sidebar Avatar
avatar:
    # In theme directory (source/images): /images/avatar.gif
    # In site directory (source/uploads): /uploads/avatar.gif
    # You can also use other linking images.
    url: 
    # If true, the avatar would be dispalyed in circle.
    rounded: false
    # The value of opacity should be choose from 0 to 1 to set the opacity of the avatar.
    opacity: 0.8
    # If true, the avatar would be rotated with the cursor.
    rotated: false
```
        
+ Tag Cloud 标签云
```yml
# TagCloud settings for tags page.
tagcloud:
    # If true, font size, font color and amount of tags can be customized
    enable: true
    # All values below are same as default, change them by yourself
    min: 12 # min font size in px
    max: 30 # max font size in px
    start: "#ccc" # start color (hex, rgba, hsla or color keywords)
    end: "#111" # end color (hex, rgba, hsla or color keywords)
    amount: 200 # amount of tags, change it if you have more than 200 tags
```
+ hightlight 代码高亮
```yml
# Code Highlight theme
# Available values: normal | night | night eighties | night blue | night bright
# https://github.com/chriskempson/tomorrow-theme
highlight_theme: night 
```

### Hexo New Usage
hexo new 需要指定创建对象的类型，目前有三种类型：post(文章), page(页面), draft(草稿)。

#### Post
+ 创建一篇文章
```shell
> hexo new post {title}
```

+ 文章头内容
```md
---
title: 来自unsplash的喵星人们
date: 2019-05-03 15:27:50
categories:
- 不定时更新的日常
- 宠物们
tags:
- 喵星人
- unsplash
---
```

#### Page
+ 创建tags, categories, about页面
```shell
> hexo new page tags
> hexo new page categories
> hexo new page about
```
设置page的类型
```md
---
title: 分类
date: 2019-05-03 13:13:19
type: "categories"
---
```

```md
---
title: 标签
date: 2019-05-03 13:12:15
type: "tags"
---
```

```md
---
title: 关于
date: 2019-05-03 13:12:15
type: "about"
---
```

#### Draft
使用 `hexo new draft {title}` 创建草稿，草稿保存在`_drafts`目录，草稿中的内容不会被generate，草稿编辑完成以后，使用`hexo publish`发布到`_posts`目录

### Hexo Deploy Usage
`hexo deploy`会把`public`目录中的内容`push`到`github`
在`_config.yml`中配置`github`信息，指定`branch`为`master` (important!)
```yml
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:{git.user.name}/{git.user.name}.github.io.git
  branch: master
```
一个完整的发布流程
```shell
> hero clean
> hero generate
> hero deploy
```

### What is git subtree, and why should I use it?

See [Git subtree: the alternative to Git submodule](https://www.atlassian.com/blog/git/alternatives-to-git-submodule-git-subtree)

![img](https://3kllhk1ibq34qk6sp3bhtox1-wpengine.netdna-ssl.com/wp-content/uploads/BeforeAfterGitSubtreeDiagram.png)

### Enable Search

Hexo中的搜索功能委托给主题提供，Hexo Theme Next 提供了多种搜索实现([Search Services](https://theme-next.org/docs/third-party-services/search-services)): Algolia Search, Local search, Swiftype Search等。

本IO站使用Local Search。配置方式：

`_config.yml`:
```yml
# Local Search 
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

`themes/next/_config.yml`:
```yml
# Local search
# Dependencies: https://github.com/theme-next/hexo-generator-searchdb
local_search:
  enable: true
  # If auto, trigger search by changing input.
  # If manual, trigger search by pressing enter key or search button.
  trigger: auto
  # Show top n results per article, show all results by setting to -1
  top_n_per_article: 1
  # Unescape html strings to the readable one.
  unescape: false
```

## Autobuild with Travis


## Hexo Theme Plugins