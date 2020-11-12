---
layout: post
title: "灵活的自定义菜单"
date:   2020-11-12
tags: [customize]
comments: true
author: zhangjialun
---
异步路由静态路由重新排列

多模块菜单融合

排序，删除，重命名

配置首页

<!-- more -->
[图片加载失败的外链](https://www.jianshu.com/p/45eff4d32fc5)
## 开始

项目属于多模块的统一登录平台，当多个模块进行融合时，路由映射出来的菜单复杂难以处理，菜单管理完美解决该问题。

![展示](https://raw.githubusercontent.com/zhangjialun555/zhangjialun555.github.io/master/images/menu/241605162476_.pic_hd.jpg)

下面列举菜单具有的功能特性

### 支持特性

- 异步路由静态路由重新排列

- 多模块菜单融合

- 排序，升序，降序，删除，重命名

- 配置首页

- 支持修改

- 权限关联

## 异步路由静态路由重新排列

针对所有路由文件与后端进行数据库缓存处理，查询当前用户可使用的路由进行编排。


## 排序，生序，降序，删除，重命名

创建菜单时，可对菜单添加相关描述，关联首页等
树形结构便于实时预览菜单形成形式，可点击图标重命名，关联模块，删除等。可拖拽菜单节点进行排序，越级，降级等自由组合。

![创建菜单](https://raw.githubusercontent.com/zhangjialun555/zhangjialun555.github.io/master/images/menu/251605162830_.pic_hd.jpg)


## 关联权限

已创建的菜单可以通过权限控制进行管理，可以给当前用户或者其他用户分配权限。
![关联权限](https://raw.githubusercontent.com/zhangjialun555/zhangjialun555.github.io/master/images/menu/261605163197_.pic_hd.jpg)
