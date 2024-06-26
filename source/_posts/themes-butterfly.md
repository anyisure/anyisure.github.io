---
title: themes-butterfly
tags:
  - Hexo
  - 教程
  - 主题
categories:
  - 教程
  - Hexo
keywords: 'Hexo,重新部署,恢复'
description: hexo主题butterfly
cover: 'https://i.loli.net/2021/02/24/5O1day2nriDzjSu.png'
abbrlink: 64f686f3
date: 2024-06-26 09:20:36
top_img:
---

[Butterfly主题官方文档](https://butterfly.js.org/)

### 安装 
在项目根目录

```bash
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```

### 应用主题
修改根目录下的`_config.yml`，把主题改为`butterfly`

```YAML
theme: butterfly
```

### 安裝插件
如果你沒有 `pug` 以及 `stylus` 的渲染器，请下载安装：

```bash
npm install hexo-renderer-pug hexo-renderer-stylus --save
```

### 升级

- 升级办法：
在hexo根目录下，执行`git pull`

- 升级建议：
升级完成后，到 Github 的 [Releases 界面](https://github.com/jerryc127/hexo-theme-butterfly/releases) 查看新版的更新內容。

- 为减少升级带来的影响，建议使用以下方法：
在 hexo 的根目录创建 `_config.butterfly.yml`，并把主题目录下的 `_config.yml` 内容 `_config.butterfly.yml` 去。( 注意: 复制的是主题下的 _config.yml ，而不是 hexo 的 _config.yml)