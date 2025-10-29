# 常用的三個 AI CLI 工具指令

本文介紹三個主流 AI 命令列工具的使用方式與常用指令參數。

---

## Claude Code

Claude Code 是 Anthropic 提供的 AI 命令列工具，支援程式碼生成、專案分析與自動化任務執行。

### 基本語法

```bash
claude [選項] [提示詞]
```

### 常用參數說明

| 參數 | 說明 |
|------|------|
| `-p` 或 `--print` | 透過 SDK 執行單次查詢後立即退出（非互動模式） |
| `--verbose` | 啟用詳細日誌記錄，顯示完整的回合輸出（適用於除錯） |
| `--allowedTools` | 指定允許使用的工具，例如：`Edit`、`WriteFile`、`Bash` 等 |

### 工具權限設定

`--allowedTools` 支援以下格式：

- **指定特定工具**：`--allowedTools="Edit,WriteFile"`
- **限制命令範圍**：`--allowedTools="Bash(git:*)"`（只允許 git 相關命令）
- **完全控制**：`--allowedTools="*"`（允許所有工具，不建議）

### 指令範例

#### 範例 1：使用 Git 分析專案變更

```bash
claude -p --allowedTools="Edit,WriteFile,Bash(git:*)" "幫我透過 git 說明本專案的更動"
```

**說明**：
- 使用 `-p` 模式執行單次查詢
- 限制只能使用編輯、寫檔與 git 相關的 bash 命令
- AI 會分析 git 歷史並生成變更說明

#### 範例 2：啟用詳細日誌進行除錯

```bash
claude --verbose -p "分析專案架構並產生說明文件"
```

**說明**：
- 使用 `--verbose` 顯示完整執行過程
- 適合追蹤 AI 的決策流程與工具呼叫

### 參考資源

- [官方文件](https://docs.claude.com/en/docs/claude-code/cli-reference)
- [進階配置](https://docs.claude.com/en/docs/claude-code/advanced-configuration)

---

## Gemini CLI

Gemini CLI 是 Google 提供的 AI 命令列工具，整合 Gemini 模型能力。

### 基本語法

```bash
gemini [選項] [提示詞]
```

### 常用模式

#### 互動模式

```bash
gemini chat
```

#### Headless 模式（非互動）

```bash
gemini query "你的問題"
```

### 指令範例

#### 範例 1：直接查詢

```bash
gemini query "分析這個專案的架構並給出建議"
```

#### 範例 2：使用管道輸入

```bash
cat README.md | gemini query "總結這份文件的重點"
```

#### 範例 3：指定模型版本

```bash
gemini --model=gemini-pro query "優化這段程式碼"
```

### 參考資源

- [Headless 模式文件](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/headless.md)
- [GitHub 專案頁面](https://github.com/google-gemini/gemini-cli)

---

## GitHub Copilot CLI

GitHub Copilot CLI 是 GitHub 提供的命令列 AI 助手，專注於終端操作與指令建議。

### 基本語法

```bash
copilot [選項] [提示詞]
```

### 常用參數說明

| 參數 | 說明 |
|------|------|
| `-p` 或 `--print` | 非互動模式，執行後直接輸出結果 |
| `--allow-all-tools` | 允許使用所有可用工具 |
| `--json` | 以 JSON 格式輸出結果 |

### 指令範例

#### 範例 1：分析 Git 變更

```bash
copilot --allow-all-tools -p "幫我透過 git 說明本專案的更動"
```

**說明**：
- 使用 `-p` 非互動模式
- `--allow-all-tools` 允許執行必要的系統命令
- AI 會自動執行 `git log`、`git diff` 等命令並分析

#### 範例 2：生成複雜指令

```bash
copilot -p "找出專案中所有超過 100 行的 TypeScript 檔案"
```

#### 範例 3：互動模式

```bash
copilot
```

進入互動式對話，可持續提問與執行命令。

### 參考資源

- [官方文件](https://docs.github.com/en/copilot/github-copilot-in-the-cli)
- [最佳實踐](https://docs.github.com/en/copilot/using-github-copilot/using-github-copilot-in-the-command-line)

---

## 工具比較

| 特性 | Claude Code | Gemini CLI | Copilot CLI |
|------|-------------|------------|-------------|
| 提供者 | Anthropic | Google | GitHub |
| 程式碼編輯 | ✅ 優秀 | ⚠️ 基本 | ✅ 良好 |
| 專案分析 | ✅ 優秀 | ✅ 良好 | ✅ 良好 |
| 指令建議 | ⚠️ 基本 | ⚠️ 基本 | ✅ 優秀 |
| 工具權限控制 | ✅ 細緻 | ⚠️ 基本 | ✅ 良好 |
| 互動體驗 | ✅ 優秀 | ✅ 良好 | ✅ 優秀 |

---

## 使用建議

1. **程式碼重構與編輯**：優先使用 **Claude Code**
2. **終端指令協助**：優先使用 **Copilot CLI**
3. **快速查詢與對話**：可使用 **Gemini CLI**
4. **複雜專案分析**：**Claude Code** 配合適當的工具權限設定

---

## 安全注意事項

⚠️ **重要提醒**：

- 使用 `--allow-all-tools` 或類似參數時需特別小心
- 建議在沙盒環境或測試專案中先行試驗
- 避免在生產環境直接執行未經驗證的 AI 生成命令
- 定期檢查執行日誌確保無異常操作
