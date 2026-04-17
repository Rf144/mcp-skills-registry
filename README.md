# mcp-skills-registry

Claude Code 環境規範的統一參考庫，整理所有本機 MCP 伺服器設定與可用 Skill 說明，讓所有 Claude Code 實例保持一致的工具使用規範。

## 檔案說明

| 檔案 | 說明 |
|------|------|
| `CLAUDE.md` | **主規範文件**，Claude Code 實例的核心參考點 |
| `mcp.template.json` | MCP 伺服器設定範本（佔位符版本，無真實金鑰） |
| `.gitignore` | 排除含真實金鑰的 `.mcp.json` |

## 包含內容

### MCP 伺服器（8 個）

| 名稱 | 功能 |
|------|------|
| filesystem | 本機檔案讀寫（桌面資料夾） |
| github | GitHub 倉儲操作、Issues、PR |
| puppeteer | 瀏覽器自動化、截圖、填表 |
| fetch | 靜態網頁抓取轉 Markdown |
| brave-search | 網路搜尋 |
| vercel | Vercel 部署管理、日誌查閱 |
| supabase | Supabase 資料庫查詢、schema 管理、SQL 執行 |
| n8n-haha-mcp | n8n workflow 建立/驗證/執行 |

### Skills（25 個）

分為系統類、排程類、開發類、文件類、n8n 專項類，詳見 `CLAUDE.md`。

## 在 Claude Code 中引用

### 方式 A：全域套用（推薦）

在 `~/.claude/CLAUDE.md` 加入以下一行，讓本機所有工作目錄的 Claude Code 都自動套用：

```
@https://raw.githubusercontent.com/Rf144/mcp-skills-registry/main/CLAUDE.md
```

### 方式 B：單一專案引用

在專案的 `CLAUDE.md` 加入同樣的 `@URL` 引用。

### 方式 C：本地複製

直接複製 `CLAUDE.md` 至工作目錄（更新時需手動同步）。

## 安全提醒

- **切勿**將含有真實 API 金鑰的 `.mcp.json` 提交至 Git
- 只提交 `mcp.template.json`（含佔位符版本）
- 真實設定檔已透過 `.gitignore` 排除

## 更新規範

每次更新 `CLAUDE.md` 時，請同步修改文件底部的「最後更新」日期。

---

*最後更新：2026 年 4 月 18 日*
