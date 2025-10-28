# 🚀 AI 輔助 Google Apps Script 開發

## 📋 為什麼需要這套開發流程？

### **傳統開發痛點**
- 在 Google Apps Script 編輯器中寫程式碼沒有現代IDE的輔助
- 測試需要手動在Google Sheets中操作，效率低且容易出錯
- 程式碼沒有版本控制，協作困難
- AI無法看到Google Apps Script線上編輯器的程式碼

### **AI Coding 的機會**
- AI 可以幫你寫程式碼，但需要「看得到」你的完整專案
- AI 需要快速的feedback loop來驗證產生的程式碼
- 本地開發讓AI能完整理解你的專案結構

## 🎯 解決方案：本地開發 + clasp + Google API 權限

### **核心概念**
1. **本地開發**：使用現代IDE (VS Code/Claude Code) 讓AI能看到完整專案
2. **clasp同步**：本地程式碼自動推送到Google Apps Script
3. **Google API權限**：讓本地測試能存取真實的Google Sheets
4. **自動化hooks**：AI修改程式碼後自動部署

## 🛠️ 真人需要做的設定工作

### **1. Google Cloud Console 設定 (最重要！)**

**目的：** 讓Claude Code和本地測試能夠存取你的Google Sheets

#### **Step 1: 建立Google Cloud專案**
1. 前往 [Google Cloud Console](https://console.cloud.google.com/)
2. 建立新專案或選擇現有專案
3. 記下專案ID

#### **Step 2: 啟用必要的API**
1. 在Google Cloud Console中，前往「API和服務」→「程式庫」
2. 搜尋並啟用以下API：
   - **Google Sheets API** (必要)
   - **Google Drive API** (需要時啟用)

#### **Step 3: 建立Service Account**
1. 前往「API和服務」→「憑證」
2. 點擊「建立憑證」→「服務帳戶」
3. 填寫服務帳戶名稱，例如：`my-app-service-account`
4. 角色選擇「編輯者」或「Google Sheets API Admin」
5. 建立完成後，點擊該服務帳戶
6. 前往「金鑰」頁籤，點擊「新增金鑰」→「JSON」
7. **下載JSON檔案並妥善保存**

#### **Step 4: 共享Google Sheets給Service Account**
1. 打開你想要程式存取的Google Sheets
2. 點擊右上角「共用」按鈕
3. 將Service Account的email（格式如：`service-account-name@project-id.iam.gserviceaccount.com`）加入共用清單
4. 權限設為「編輯者」

### **2. 本地專案初始化**

#### **建立基本專案結構**
```
my-gas-project/
├── src/           # Apps Script 程式碼
├── .claude/       # Claude Code 設定
├── .clasp.json    # clasp 設定
├── package.json   # Node.js 相依
└── .env          # 認證資訊 (不要提交到git!)
```

#### **安裝必要工具**
```bash
# 安裝 clasp (Google Apps Script 命令列工具)
npm install -g @google/clasp

# 登入你的Google帳號
clasp login

# 建立或連結Apps Script專案
clasp create --type standalone --title "我的專案"
# 或連結現有專案: clasp clone <script-id>
```

### **3. 設定自動化 hooks**

**目的：** AI修改程式碼後自動部署到Google Apps Script

**建立檔案：`.claude/settings.local.json`**
```json
{
  "hooks": {
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command", 
            "command": "clasp push"
          }
        ]
      }
    ]
  }
}
```

**效果：**
- 每次與Claude對話結束時，自動執行 `clasp push`
- AI修改的程式碼立即同步到Google Apps Script
- 你可以馬上在Google Sheets中測試新功能

### **4. 設定認證資訊**

**建立 `.env` 檔案**（重要：不要提交到git）
```env
# 從Service Account JSON檔案複製以下資訊
GOOGLE_SERVICE_ACCOUNT_EMAIL=your-service-account@your-project.iam.gserviceaccount.com
GOOGLE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n你的私鑰內容\n-----END PRIVATE KEY-----"

# 你的Google Sheets ID (從網址取得)
TEST_MASTER_SHEET_ID=1fTQ3AZ93yP_q7oCncMASozScIJ36NlJwgc3vplr0nJI
TEST_EMPLOYEE_SHEET_ID=1g82f-yigavTtmL3YABxINFaXHerDppve9Ol0CM7aHVo
```

**如何取得Google Sheets ID：**
- 打開Google Sheets，網址格式：`https://docs.google.com/spreadsheets/d/SHEET_ID/edit`
- `SHEET_ID` 就是你需要的ID

### **5. 基本package.json設定**

```json
{
  "name": "my-gas-project",
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch"
  },
  "dependencies": {
    "googleapis": "^128.0.0",
    "google-auth-library": "^9.2.0", 
    "dotenv": "^16.3.1"
  },
  "devDependencies": {
    "jest": "^29.7.0"
  }
}
```

安裝相依套件：
```bash
npm install
```

## 🤖 如何請AI幫你寫程式

### **1. 讓AI看到你的完整專案**

**好的指令：**
```
請分析 @src/Code.js 的現有實作，
為新功能「員工請假申請系統」寫程式碼
```

**為什麼有效：**
- AI能理解現有程式碼結構和風格
- AI可以產生與現有程式碼一致的新功能
- 避免重複造輪子

### **2. 明確描述功能需求**

**指令範例：**
```
我需要開發員工請假申請功能：
1. 員工在個人試算表填寫請假申請 
2. 自動同步到主控台進行審核
3. 支援事假、病假、特休等假別
4. 請直接修改 src/Code.js 添加這些功能
```

**效果：**
- AI會產生符合需求的程式碼
- 功能需求被正確理解
- 直接可用的實作結果

### **3. 利用現有檔案結構**

**有用的檔案：**
- `src/Code.js` - 主要程式邏輯
- `src/Config.js` - 設定檔
- `.env` - 認證和試算表ID設定

**指令範例：**
```
請修改 @src/Code.js 加入新功能，
並更新 @src/Config.js 中的工作表名稱設定
```

## 🎯 實際應用情境

### **情境1：全新功能開發**

**你的需求：** 想要開發員工請假申請功能

**AI指令：**
```
我需要開發員工請假申請功能：
1. 員工在個人試算表填寫請假申請
2. 自動同步到主控台進行審核  
3. 支援事假、病假、特休等假別
4. 請修改 @src/Code.js 加入這些功能
```

**你會得到：**
- 完整的新功能程式碼
- 自動同步機制
- 錯誤處理邏輯
- 可立即使用的功能

### **情境2：修復程式問題**

**你發現：** 例假日加班還能申請補休，但這違反勞動法規

**AI指令：**
```
@src/Code.js 有問題：例假日加班記錄還能被用來申請補休，
但勞動法規定例假日加班只能發加班費。

請修改相關函數，讓例假日加班：
1. 標記為「不可補休」狀態
2. 在補休配對時被排除
```

**你會得到：**
- 精確的函數修改
- 符合法規的邏輯
- 不會影響其他功能

### **情境3：功能優化**

**你想要：** 提升程式效能和穩定性

**AI指令：**
```
請優化 @src/Code.js 的效能：
1. 減少Google Sheets API的呼叫次數
2. 加強錯誤處理機制  
3. 提取重複的邏輯到共用函數
```

**你會得到：**
- 更高效的程式碼
- 更好的錯誤處理
- 更易維護的結構

## 🧪 自動測試：為什麼重要？如何運作？

### **為什麼需要自動測試？**

**傳統測試方式的問題：**
- 手動在Google Sheets中測試，費時費力
- 容易遺漏邊界情況和錯誤處理
- 修改程式碼後需要重複測試所有功能
- AI產生的程式碼品質無法保證

**自動測試的優勢：**
- 幾秒鐘內測試所有功能
- 確保AI產生的程式碼真的能運作
- 修改程式碼時自動驗證沒有破壞現有功能
- 使用真實的Google Sheets API，不是模擬資料

### **如何請AI幫你設定測試**

**指令範例：**
```
請為 @src/Code.js 建立自動測試：
1. 測試Google Sheets API連接
2. 測試資料讀取和寫入功能
3. 測試錯誤處理機制
4. 使用我在 .env 設定的測試用Google Sheets
```

**AI會幫你建立：**
- `test/` 資料夾和測試檔案
- 連接真實Google Sheets的測試
- 各種情境的測試案例
- 自動化測試流程

### **測試運作方式**

#### **1. 連接測試**
- 驗證Service Account權限設定正確
- 確認能存取你的Google Sheets
- 測試基本的讀寫操作

#### **2. 功能測試**
- 測試員工資料同步功能
- 測試加班記錄處理
- 測試補休配對邏輯
- 測試錯誤情況處理

#### **3. 整合測試**
- 測試完整的業務流程
- 驗證多個功能間的協作
- 確保資料一致性

### **運行測試**

```bash
# 執行所有測試
npm test

# 持續監控（檔案變更時自動重跑測試）
npm run test:watch

# 測試特定功能
npm test -- test/connection.test.js
```

### **測試結果解讀**

**✅ 測試通過：**
```
✓ 應該能成功連接到 Google Sheets API
✓ 應該能讀取員工清單
✓ 應該能新增加班記錄
```

**❌ 測試失敗：**
```
✗ Google Sheets API has not been used in project before
→ 需要到Google Cloud Console啟用API
```

### **驗證程式碼品質**

#### **1. 自動測試**
- AI修改程式碼後，立即運行 `npm test`
- 確認所有功能正常運作
- 發現問題立即修正

#### **2. Google Apps Script驗證**
- AI修改程式碼後自動執行 `clasp push`
- 前往 [Google Apps Script](https://script.google.com/) 查看部署結果
- 測試線上環境功能

#### **3. 實際操作驗證**
- 在Google Sheets中測試新功能
- 檢查資料同步是否正確
- 驗證使用者操作流程

## 📊 成效評估

### **開發效率提升**
- **傳統方式**：手動寫程式碼 → 複製到GAS → 手動測試 → 修bug → 重複
- **AI輔助方式**：描述需求 → AI產生程式碼+測試 → 自動推送 → 自動測試

**時間節省：60-80%**

### **程式碼品質提升**
- **測試覆蓋率**：從0%提升到90%+
- **Bug減少**：TDD確保功能正確性
- **可維護性**：模組化設計，易於擴展

### **學習效果**
- **理解AI能力邊界**：什麼時候AI幫得上忙，什麼時候需要人工介入
- **掌握prompt技巧**：如何給AI有效的指令
- **建立開發流程**：可複製到其他專案的標準化流程

## 💡 關鍵成功要素

1. **給AI足夠的context**：spec、現有程式碼、測試資料
2. **使用TDD方法**：讓AI先寫測試，確保需求理解正確
3. **建立自動化**：clasp hooks減少手動操作
4. **真實環境測試**：用真實的Google Sheets API驗證
5. **迭代改進**：根據測試結果持續優化

---

**開始你的AI輔助GAS開發之旅吧！** 🎯

記住：AI是強大的助手，但你仍然是專案的主導者。清楚地表達需求，建立好的開發流程，AI就能幫你快速實現想法！