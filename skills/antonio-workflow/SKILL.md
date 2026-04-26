---
name: antonio-workflow
description: Antonio 的個人工作流程偏好。當使用者說「開工」、「閱讀 antonio-workflow」、「套用工作流程」時立即啟用。說「收工」時執行完整收工流程。啟用後在整個對話中維持三個核心行為：(1) 每次執行任何動作前先用繁體中文說明並等待確認，(2) 說「收工」時自動更新 Obsidian 筆記、整理 md、建立日誌並推上 GitHub，(3) 優先使用已連接的 MCP 工具完成任務，不向使用者索取 token 或憑證。
---

# Antonio 工作流程

啟用後，在整個對話中維持以下行為。

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

## 行為二：收工流程

當使用者說「收工」時，依序執行以下步驟，每個步驟都先說明再詢問確認：

### Step 1 — 更新 Obsidian 筆記
根據對話內容更新：

**專案筆記**（`C:\Users\Lg\OneDrive\文件\Obsidian Vault\Projects\`）
- `status`：🟢 上線中 / 🟡 進行中 / 🔴 暫停
- `next`：對話中提到的下一步

**踩坑記錄**（`C:\Users\Lg\OneDrive\文件\Obsidian Vault\踩坑記錄\API 踩坑.md`）
- 若對話中有遇到並解決的錯誤，補進去

### Step 2 — 整理 md 檔
檢查 Obsidian Vault 裡當天修改過的 md 檔：
- 格式是否整齊（標題層級、清單縮排）
- 有無空白的待填欄位可以根據對話內容補上

### Step 3 — 建立今日 log
在 `C:\Users\Lg\OneDrive\文件\Obsidian Vault\Daily\` 建立當天的 log 檔，格式如下：

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

## 啟用確認

讀取這個 skill 後，回覆：

> 「Antonio 工作流程已啟用 ✓ 每次動作前我會先說明、優先使用 MCP 工具。說「收工」時我會自動更新 Obsidian、整理 md、建立日誌並推上 GitHub。」
