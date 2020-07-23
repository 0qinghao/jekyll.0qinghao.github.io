---
layout: post
title: 在基于 Jekyll 的博客中加入文字的注音
categories: [日语]
description: 在使用 jekyll 构建的博客文章中加入注音, 同时涉及本地构建静态网站推送 github page / coding page 的方法
keywords: jekyll, 注音, ruby, 振假名, hurigana, furigana, 日语, 日文, github page, 静态网页
furigana: true
---

越来越多的人选择将博客托管在 [GitHub page](https://pages.github.com/) 或是国内的 [Coding page](https://help.coding.net/docs/devops/cd/static-website.html)，通过 Jekyll 将 markdown 排版的文章渲染为静态网页。
当一个 repo 被你设定为 GitHub page 时，GitHub 将检测 repo 的目录结构，如果符合 Jekyll 的特征，例如根目录下包含 _config.yml 配置时，GitHub 将在你 push 后自动进行在线构建。

![](/assets/images/2020-07-23-20-40-26.png)

查看 repo 的提交历史将能看到构建成功的消息，如果构建失败则会收到一封邮件报告。因此理论上我们在本地甚至不需要配置 Jekyll 环境，只需要保证 push 的内容具备让 GitHub 进行在线构建的条件即可。

### 在线构建的局限性

将构建的任务交给 GitHub 确实十分方便，也能满足绝大部分用户的需求。但我们注意到 GitHub 提供的在线构建服务并不支持所有 Jekyll 插件，支持列表可以在这里查询：[https://pages.github.com/versions/](https://pages.github.com/versions/)

而很不幸的是，有一个插件对我来说是必不可少的，而它却不在支持列表中。这个插件是 [jekyll-furigana](https://github.com/guentoan/jekyll-furigana)，用于给文本注音，主要应用场景有：

( 日本語:にほんご )の( 漢字:かんじ )に( 振:ふ )り( 仮名:がな )を( 編集:へんしゅう )する

( 中:zhōng )( 文:wén )拼音注音

( 奇奇怪怪的用法:😀😁🤣😑🤨😮😶 )
 
