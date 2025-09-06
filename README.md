# WhyBlog Content Repository

這是 WhyBlog 雙倉庫架構中的**內容倉庫**，專門存放部落格文章和知識庫內容。

## 倉庫用途

- 存放 Markdown 格式的部落格文章和文檔
- 管理網站所需的圖片、檔案等靜態資源
- 透過 repository_dispatch 機制自動觸發應用程式倉庫的建置與部署

## 目錄結構

```
whyblog-content/
├── README.md                 # 本說明檔
├── blog/                     # 部落格文章目錄
│   ├── authors.yml          # 作者資訊配置
│   └── 2024-01-01-welcome/  # 文章目錄 (日期-標題格式)
│       └── index.md         # 文章內容
├── docs/                     # 知識庫文檔目錄
│   ├── intro.md            # 介紹頁面
│   └── category/           # 分類目錄
└── static/                   # 靜態資源目錄
    └── img/                 # 圖片資源
```

## 使用說明

### 新增部落格文章

1. 在 `blog/` 目錄下建立新的文章目錄，格式為 `YYYY-MM-DD-文章標題`
2. 在該目錄下建立 `index.md` 檔案，撰寫文章內容
3. 如有圖片，請放置在 `static/img/` 目錄下

### 新增知識庫文檔

1. 在 `docs/` 目錄下的適當分類中建立 `.md` 檔案
2. 檔案開頭使用 Front Matter 設定標題、標籤等元資訊

### 自動部署流程

當內容有更新時（push 或 PR 合併），會自動觸發以下流程：

1. 內容倉庫的 GitHub Actions 檢測到變更
2. 透過 repository_dispatch 通知應用程式倉庫
3. 應用程式倉庫自動拉取最新內容並重新建置
4. 建置完成後自動部署到 GitHub Pages

## 相關倉庫

- **應用程式倉庫**: [whyblog-docusaurus](https://github.com/junsuwhy/whyblog-docusaurus) - Docusaurus 應用程式與建置配置
- **線上網站**: [GitHub Pages 網址](https://junsuwhy.github.io/whyblog-docusaurus/)

## 技術架構

本專案採用雙倉庫 + repository_dispatch 架構：

- **內容倉庫** (此倉庫): 純內容管理，專注於寫作
- **應用程式倉庫**: Docusaurus 應用程式與 GitHub Actions 部署流程
- **自動觸發**: 內容更新時自動觸發應用程式重新建置與部署

這種架構的優點：
- 內容與程式碼分離，便於管理
- 內容作者無需關心技術細節
- 自動化部署，提升工作效率
- 各倉庫職責單一，維護簡單
