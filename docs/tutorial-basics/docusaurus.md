---
sidebar_position: 1
title: Docusaurus寫文章相關注意事項
---

# Docusaurus 的相關規則

這篇留下我寫 Docusaurus 會用到的各種規則，以避免忘記~~可以有小抄偷看~~。

## Front Matter

Yaml 上方有一段
```
title: "小歪歪"
```

這個就是 Front Matter ，以下列一些重要的 Front matter

#### Blog 的 Front Matter

參考[這個手冊頁面](https://docusaurus.io/docs/api/plugins/@docusaurus/plugin-content-blog#markdown-front-matter)

* title 文章標題
* date 日期，用字串表示，不指派的話會取檔名或檔案路徑，例如`2021-04-15-blog-post.mdx`, `2021-04-15-blog-post/index.mdx`, `2021/04/15/blog-post.mdx` 
* tags 標籤，會用陣列
* draft 是否是草稿，用 boolean
* image 會放在 og image 的代表圖片
* slug 當作網址 path 的字串
* description
* last_update

## MDX 格式

Docusaurus 支援可以寫 react 和 jsx 的 MDX 文件檔，這讓網站可以透過 AI 餵很多互動式的內容，可以參考[官方這份文件](https://docusaurus.io/docs/markdown-features/react)。

MDX 目前實測有這些要點：
* 頁首用一個 Markdown 的大標題作頁面名稱
* 使用 js 撰寫的元件要有 import 或 export 前綴（請 claude 寫出來的沒有）
* 可用 `docusaurus start` 驗證 render 對不對（通常會掛在 `npmm run start`指令下）