---
slug: laravel-newer
title: 初探 Laravel
authors: [admin]
tags: [歡迎, 介紹]
---

在 104, Cake 看了一輪，凡是在找 PHP 的工作，八成以上都要 Laravel 的經驗，所以看來還是要學一下 Laravel 了。

現在可以搭配 AI 來學習真的很方便，學習程式最快的做法就是實作，我可以請 AI 很快給我想要的功能，但把我想學的部份留空下來讓我自己進行，很快能學到一個主題知識所需具備的基礎技能。

---

Laravel 的一些基礎：

* Laravel 在 CMD 的指令中靠 artisan 指令來執行各種語法，包括資料庫升級 `migrate`、快速建立各個程式的模板`make`等等。
* 採 MVC 架構，Model 層採 ORM ，View 層使用容易上手快速結合 html 的 Blade ，C 用的看起來是自己的 Controller。
* 若要前後端分離搭配 Laravel Sanctum/Passport + Vue/React ，或使用 Inertia.js（SPA 但不用 API，謝謝 Claude 補充）
* 資料庫的 migrate 做法可以透過封裝好的 Schema 程式快速做到在 SQL 裡面 Create, Update 等語法，結構也清楚易讀。
* 通常應用程式的打造過程會用 Rest API （分成 CRUD）去實現後端 API（但到現在還沒內建 OpenAPI 文件系統的樣子，不敢相信）
* 登入的實作有官方的最佳實踐做法，可以用 Cookie、Token、JWT 等做法。通常用下列三組功能。
	- Laravel Breeze：簡單的 Cookie-based 認證
    - Laravel Sanctum：SPA/API Token 認證
    - Laravel Passport：完整 OAuth2 伺服器
- 另外現代化的框架另一個選擇：Livewrite，待研究（或是前面提到的 Inertia.js）

然後被 Claude 稱讚「你的基礎觀念很扎實！」，嘿嘿嘿（蠟筆小新臉紅）。

有什麼新發現再繼續寫上來。