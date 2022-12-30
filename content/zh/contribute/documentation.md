---
title: 帮助完善项目文档 Docs
linktitle: 完善文档
description: 文档是开源项目重要组成部分，流影文档还在不断丰富完善中，欢迎改善文档.
date: 2022-12-29
publishdate: 2022-12-29
categories: [contribute]
keywords: [docs,documentation,community, contribute]
menu:
  docs:
    parent: "contribute"
    weight: 20
weight: 20
sections_weight: 20
aliases: [/contribute/docs/]
toc: true
isCJKLanguage: true
---


## Fork 流影文档
最好在本地进行文档改善工作。
* 首先在Github中Fork流影文档 [ly_docs](https://github.com/Abyssal-Fish-Technology/ly_docs)
* Clone项目到本地，参见[GitHub's documentation on "forking"][ghforking]
* 本地创建分支进行文档完善工作

```txt
git checkout -b doc-revised
```

## 文档改善

流影文档使用 [hugoDocs](https://github.com/gohugoio/hugoDocs) 样式，使用Hugo生成项目文档，具体信息请参考其使用文档。

### 创建文档新内容

```txt
hugo new <DOCS-SECTION>/<new-content>.md
```
### 编辑文档
在`content`目录下找到对需要改善的文档md，修改内容即可。



## 反馈方式

* 联系邮箱：[opensource@abyssalfish.com.cn](mailto:opensource@abyssalfish.com.cn)
* 线上讨论区：[去Gitter沟通群](https://gitter.im/abyssalfish-os/community?utm_source=share-link&utm_medium=link&utm_campaign=share-link)


[ghforking]: https://help.github.com/articles/fork-a-repo/
