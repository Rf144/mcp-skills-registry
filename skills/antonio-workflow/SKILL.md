---
name: antonio-workflow
description: Antonio 的個人工作流程偏好。當使用者說「開工」、「開始工作」、「準備開工」、「今天開始」、「閱讀 antonio-workflow」、「套用工作流程」等開工相關口語時立即啟用並執行開工簡報。說「收工」、「準備收工」、「結束工作」、「今天結束」等收工相關口語時執行完整收工流程。啟用後在整個對話中維持三個核心行為：(1) 每次執行任何動作前先用繁體中文說明並等待確認，(2) 收工時執行 Obsidian 更新 + GitHub 推送 + log 整理，(3) 優先使用已連接的 MCP 工具完成任務，不向使用者索取 token 或憑證。
---

# Antonio 工作流程

啟用後，在整個對話中維持以下行為。

> **安裝說明：** 本地安裝時請將所有 `YOUR_USERNAME` 替換為該機器的 Windows 使用者名稱（例如筆電為 `Lg`，桌電為 `User`）。

---

## 行為一：動作前說明（繁體中文）

**每次要執行下列任何動作之前**，先用繁體中文說明，等使用者確認後再執行：

- 修改或建立檔案
- 執行終端機指令
- 安裝或移除套件
- 呼叫外部 API 或服務
- 刪除任何內容

**說明格式**（簡潔即可）：
- 這個動作要做什麼
- 會影響哪個檔案或系統
- 有沒有需要注意的風險

說明完後等使用者說「好」、「繼續」或類似確認語才執行。如果使用者說「不用每次問」或「直接做」，則在該對話中跳過這個行為。

---

## 行為五：提供可點擊連結

任何需要使用者「檢視」的內容，一律提供可直接點擊的超連結，不要貼純文字 URL 讓使用者手動複製：

- **網址**（部署連結、預覽頁面、API 文件）→ 用 `[說明文字](URL)` 格式
- **表單**（Google Forms、Airtable 等）→ 同上
- **GitHub PR / Issue / Commit**→ 同上
- **Vercel 部署頁、Supabase 資料庫頁**→ 同上

原則：使用者點一下就能到，不需要任何複製貼上的動作。

---

## 行為二：收工流程

當使用者說「收工」、「準備收工」、「結束工作」或「今天結束」時，依序執行以下步驟，每個步驟都先說明再詢問確認：

### Step 1 — 更新 Obsidian 筆記
根據對話內容更新：

**專案筆記**（`C:\Users\YOUR_USERNAME\OneDrive\文件\Obsidian Vault\Projects\`）
- `status`：🟢 上線中 / 🟡 進行中 / 🔴 暫停
- `next`：對話中提到的下一步

**踩坑記錄**（`C:\Users\YOUR_USERNAME\OneDrive\文件\Obsidian Vault\踩坑記錄\API 踩坑.md`）
- 若對話中有遇到並解決的錯誤，補進去

### Step 2 — 整理 md 檔
檢查 Obsidian Vault 裡當天修改過的 md 檔：
- 格式是否整齊（標題層級、清單縮排）
- 有無空白的待填欄位可以根據對話內容補上

### Step 3 — 建立今日 log
在 `C:\Users\YOUR_USERNAME\OneDrive\文件\Obsidian Vault\Daily\` 建立當天的 log 檔，格式如下：

```
# YYYY-MM-DD 工作日誌

## 今天做了什麼
- （根據對話內容整理）

## 遇到的問題
- （若有）

## 明天繼續
- （next 欄位的內容）
```

### Step 4 — 推送到 GitHub
詢問要推送到哪個 repo（根據對話內容猜測並讓使用者確認），然後：
1. `git add .`
2. `git commit -m "chore: 收工更新 YYYY-MM-DD"`
3. `git push`

若今天工作的專案有對應 GitHub repo（參考 `GitHub Repos 總覽.md`），優先推那個。

---

## 行為三：優先使用 MCP 工具

任何任務開始前，先檢查是否有對應的 MCP 工具：

- **Vercel** 相關 → 使用 `mcp__vercel__*`，不索取 token
- **GitHub** 相關 → 使用 `mcp__github__*`，不索取 token
- **Supabase** 相關 → 使用 `mcp__supabase__*`，不索取憑證
- **n8n** 相關 → 使用 `mcp__n8n-haha-mcp__*`
- **Airtable** 相關 → 使用 `mcp__airtable__*`
- **檔案操作** → 優先使用內建 Read / Write / Edit

只有在 MCP 工具明確不支援時，才詢問使用者是否需要手動提供憑證。

---

## 行為四：開工簡報

說「開工」、「開始工作」、「準備開工」、「今天開始」或類似口語時，在啟用三個行為規則之後，立即讀取以下 Obsidian 檔案：

1. `C:\Users\YOUR_USERNAME\OneDrive\文件\Obsidian Vault\README.md` — 專案狀態總覽
2. `C:\Users\YOUR_USERNAME\OneDrive\文件\Obsidian Vault\Daily\` — 最近一份 Daily log（昨天或今天）
3. `C:\Users\YOUR_USERNAME\OneDrive\文件\Obsidian Vault\踩坑記錄\API 踩坑.md` — 歷史踩坑記錄

讀完後輸出開工簡報，格式如下：

```
📋 開工簡報 — YYYY-MM-DD

【進行中的專案】
- 🟡 專案名稱 — 下一步：xxx

【上次留下的事】
- 從 Daily log 的「明天繼續」整理出來

【今天如果要繼續，建議從這裡開始】
- 根據 next 欄位和 Daily log 判斷優先順序

【可參考的關聯】
- 若使用者提到今天要做的事，比對歷史踩坑記錄、其他專案的技術架構，找出可以參考或要小心的地方
```

**關聯分析原則：**
讀完所有筆記後，主動判斷新工作與舊工作的關係，例如：
- 今天要串的 API 在踩坑記錄裡出現過 → 提醒注意
- 新專案架構與現有專案相似 → 指出可以直接參考哪個
- 今天的任務與某個「進行中」專案有重疊 → 建議合併或分開處理

如果使用者沒有說今天要做什麼，簡報完後直接問：「今天要從哪個專案開始？」

---

## 啟用確認

讀取這個 skill 後，立即執行「行為四：開工簡報」，不需要額外等待使用者指令。
