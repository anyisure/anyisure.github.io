---
title: themes-next
tags:
  - Hexo
  - 教程
  - 主题
categories:
  - 教程
  - Hexo
abbrlink: f2549d20
date: 2024-06-25 17:30:32
top_img:
cover:
---

## 一、Hexo主题选择
进入主题网站 https://hexo.io/themes/ 选择自己喜欢的主题，我选择的是Next主题，比较简洁美观，而且不需要封面图片。参考github的仓库 https://github.com/next-theme/hexo-theme-next 可以进行安装。

1. 进入博客根目录
2. 执行下面代码。
```bash
git clone https://github.com/next-theme/hexo-theme-next themes/next
```
3. 这时候进入 `themes` 文件夹可以发现一个 `next` 文件夹，说明安装成功。

4. 打开项目根目录下的 `_config.yml` ，修改 `theme` 为 `next`
```diff
- theme: landscape
+ theme: next
```
5. 执行网页生成和预览命令可以查看

## 二、个性化配置
参考 Next使用文档 进行各种配置。需要注意的是在 Hexo 中有两份主要的配置文件，其名称都是 _config.yml。 其中，
- 一份位于**站点根目录下**，主要包含 Hexo 本身的配置；
- 另一份位于**主题目录下**，这份配置由主题作者提供，主要用于配置主题相关的选项。

为了描述方便，在以下说明中，将前者称为 `站点配置文件`， 后者称为 `主题配置文件`。

1. 主题配置文件可以选择 scheme，最新版有四种外观，这里选择最新增加的 Gemini。
```yml
# Schemes
# scheme: Muse
# scheme: Mist
# scheme: Pisces
scheme: Gemini
```

2. 站点配置文件可以选择语言，这里选择简体中文
```yml
language: zh-CN
```

3. 在主题配置文件设置菜单，找到 `menu`，可以先全部打开，显得比较丰富，之后选择性关闭部分菜单页面，但是除了归档页，其他页面都需要自己生成。
```yml
menu:
  home: / || fa fa-home
  about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  schedule: /schedule/ || fa fa-calendar
  sitemap: /sitemap.xml || fa fa-sitemap
  commonweal: /404/ || fa fa-heartbeat
```
生成`分类页`，在博客根目录执行 hexo new page categories 即可生成页面，打开根目录下的source/categories/index.md，添加`type: "categories"`
```diff
title: categories
date: 2023-01-05 22:02:23
+ type: "categories"
```
`标签页`做法类似，只需要把categories改成tags即可，关于页则是生成page about之后自己编辑即可。

4. 在站点配置文件设置个人信息
```yml
title: anyisure的blog
description: '个人博客'
author: anyisure
```

5. 在主题配置文件设置个人头像，文件放在主题目录的`source/images/`文件夹下面
```yml
avatar:
  url: /images/avatar.jpg
``` 

6. 侧边栏社交功能，只显示图标
```yml
social:
  GitHub: https://github.com/anyisure || fab fa-github
  E-Mail: xx@qq.com || fa fa-envelope
social_icons:
  enable: true
  icons_only: true
  transition: false
``` 

7. 增加版权声明，在主题配置文件中找到下面的并且修改
```yml
creative_commons:
  license: by-nc-sa
  sidebar: false
  post: true
  language:
``` 

8. 增加加载进度条，在主题配置文件中找到`pace`，设为`true`。

## 三、第三方服务集成
### 数学公式渲染
主题配置文件把 mathjax 的 enable 设为 true

### 不蒜子统计
修改主题配置文件
```yml
busuanzi_count:
  enable: true
  total_visitors: true
  total_visitors_icon: fa fa-user
  total_views: true
  total_views_icon: fa fa-eye
  post_views: true
  post_views_icon: far fa-eye
```  

### Local Search
1. 修改 `BLOG\node_modules\hexo-generator-searchdb\dist `文件夹下的`js`后缀为`json`

2. 安装插件
```bash
npm install hexo-generator-searchdb --save
```
3. 修改站点配置文件
```yml
search:
  path: search.json
  field: post
  format: html
  limit: 10000
#修改主题配置文件
local_search:
  enable: true
    top_n_per_article: -1
```

### 标签云
1. 安装依赖
```bash
npm install hexo-tag-cloud
```
2. 配置文件

在 `BLOG\themes\next\layout_macro\sidebar.njk` 文件中添加代码
```javascript
{% if site.tags.length > 1 %}
<script type="text/javascript" charset="utf-8" src="/js/tagcloud.js"></script>
<script type="text/javascript" charset="utf-8" src="/js/tagcanvas.js"></script>
<div class="widget-wrap">
    <h3 class="widget-title">Tag Cloud</h3>
    <div id="myCanvasContainer" class="widget tagcloud">
        <canvas width="250" height="250" id="resCanvas" style="width=100%">
            {{ list_tags() }}
        </canvas>
    </div>
</div>
{% endif %}
```


### 音乐
Next 8.x 已经集成了需要的插件

编辑需要插入的代码
```javascript
<!--音乐插件-->
<!-- require APlayer -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.css">
<script src="https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.js"></script>
<!-- require MetingJS-->
<script src="https://cdn.jsdelivr.net/npm/meting@2.0.1/dist/Meting.min.js"></script> 
<!--QQ音乐外链地址-->   
<meting-js
    server="tencent"
    type="song" 
    id="001oqXcA34gsMM"
    mini="false"
    fixed="false"
    list-folded="true"
    autoplay="false"
    volume="0.4"
    theme="#FADFA3"
    order="list"
    loop="all"
    preload="auto"
    mutex="true">
</meting-js>
```
|选项	|默认	|描述|
|---|---|---|
|id(编号)	|require	|歌曲ID /播放列表ID /专辑ID /搜索关键字|
|server(平台)	|require	|音乐平台：netease，tencent，kugou，xiami，baidu|
|type（类型）	|require	|song，playlist，album，search，artist|
|auto（支持类种 类）	|options	|音乐链接，支持：netease，tencent，xiami|
|fixed（固定模式）	|false	|启用固定模式|
|mini（迷你模式）	|false	|启用迷你模式|
|autoplay（自动播放）	|false	|音频自动播放|
|theme(主题颜色)	|#2980b9	|默认#2980b9|
|loop（循环）	|all	|播放器循环播放，值：“all”，one”，“none”|
|order(顺序)	|list	|播放器播放顺序，值：“list”，“random”|
|preload(加载)	|auto	|值：“none”，“metadata”，“'auto”|
|volume（声量）	|0.7	|默认音量，请注意播放器会记住用户设置，用户自己设置音量后默认音量将不起作用|
|mutex（限制）	|true	|防止同时播放多个玩家，在该玩家开始播放时暂停其他玩家|
|lrc-type（歌词）	|0	|歌词显示|
|list-folded（列表折叠）	|false	|指示列表是否应该首先折叠|
|list-max-height（最大高度）	|340px	|列出最大高度|
|storage-name（储存名称）	|metingjs	|存储播放器设置的localStorage键|


这就是ID的获取方式，其他平台类似


ID 获取

插入代码到需要的位置，打开主题文件夹下的 `layout/_macro/sidebar.njk` 文件，插入到如图所示位置，不同位置会导致显示位置不同。


引入的效果
为了避免切换页面引起重新播放，需要进行全局设置，在主题文件夹下的 `layout/_layout.njk` 里面添加
```javascript
<!--pjax：防止跳转页面音乐暂停-->
 <script src="https://cdn.jsdelivr.net/npm/pjax@0.2.8/pjax.js"></script>
```


在主题配置文件中修改 `pjax: true`。
