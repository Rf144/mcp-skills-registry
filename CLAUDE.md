# Claude Code 環境規範

> 本文件定義此工作區所有可用的 MCP 伺服器與 Skill，供所有 Claude Code 實例統一參考。
> 最後更新：2026 年 4 月 22 日

---

## 快速導覽

- [MCP 伺服器清單](#mcp-伺服器清單)
- [可用 Skills](#可用-skills)
- [環境變數設定](#環境變數設定)
- [使用守則](#使用守則)

---

## MCP 伺服器清單

### 1. filesystem

| 項目 | 說明 |
|------|------|
| 套件 | `mcp-server-filesystem` |
| 類型 | stdio（本機指令） |
| 掛載路徑 | `C:/Users/Lg/OneDrive/桌面` |
| 環境變數 | 無 |

**功能說明：** 提供本機檔案系統的讀寫操作，包含列出目錄、讀取檔案、寫入檔案、搜尋檔案等完整 CRUD 能力。

**主要工具：**
- `list_directory` / `directory_tree` — 瀏覽目錄結構
- `read_file` / `read_multiple_files` — 讀取文字或媒體檔案
- `write_file` / `edit_file` — 寫入或修改檔案
- `search_files` — 在掛載路徑下搜尋檔案
- `get_file_info` — 取得檔案元資訊

**使用場景：** 需要讀寫桌面資料夾下的任何檔案；儲存 workflow spec、報告、設定檔等。

---

### 2. github

| 項目 | 說明 |
|------|------|
| 套件 | `@modelcontextprotocol/server-github` |
| 類型 | stdio（npx） |
| 環境變數 | `GITHUB_PERSONAL_ACCESS_TOKEN=YOUR_GITHUB_PAT` |

**功能說明：** 操作 GitHub 倉儲，涵蓋 Issues、Pull Requests、分支管理、檔案提交等。

**主要工具：**
- `create_repository` / `fork_repository` — 建立或 Fork Repo
- `get_file_contents` / `create_or_update_file` — 讀取或更新檔案
- `push_files` — 批次推送多個檔案
- `create_issue` / `create_pull_request` — 建立 Issue 或 PR
- `list_commits` / `search_code` — 瀏覽提交記錄與程式碼搜尋

**使用場景：** 自動化 GitHub 工作流程；建立新 repo、更新設定文件、管理 Issues。

---

### 3. puppeteer

| 項目 | 說明 |
|------|------|
| 套件 | `mcp-server-puppeteer` |
| 類型 | stdio（本機指令） |
| 環境變數 | 無 |

**功能說明：** 控制 Chromium 瀏覽器進行網頁自動化操作，支援截圖、填表、點擊、JS 執行等。

**主要工具：**
- `puppeteer_navigate` — 導航至指定 URL
- `puppeteer_screenshot` — 截取當前頁面截圖
- `puppeteer_click` / `puppeteer_fill` — 點擊元素或填入表單
- `puppeteer_evaluate` — 在頁面內執行 JavaScript
- `puppeteer_select` / `puppeteer_hover` — 下拉選單與滑鼠懸停

**使用場景：** 自動化登入流程、抓取動態網頁內容、產生網頁截圖報告、自動化表單提交。

---

### 4. fetch

| 項目 | 說明 |
|------|------|
| 套件 | `mcp-server-fetch`（透過 uvx 執行） |
| 類型 | stdio（uvx） |
| 環境變數 | 無 |

**功能說明：** 抓取網頁或 HTTP 端點的內容，支援將 HTML 轉為 Markdown 以便 LLM 處理。

**主要工具：**
- `fetch` — 取得指定 URL 的內容（HTML 自動轉 Markdown）

**使用場景：** 讀取公開網頁文件、API 文件參考、抓取靜態網站資料（不需 JS 渲染時使用；需 JS 渲染請改用 puppeteer）。

---

### 5. brave-search

| 項目 | 說明 |
|------|------|
| 套件 | `@modelcontextprotocol/server-brave-search` |
| 類型 | stdio（npx） |
| 環境變數 | `BRAVE_API_KEY=YOUR_BRAVE_API_KEY` |

**功能說明：** 使用 Brave Search API 執行網路搜尋，支援一般網頁搜尋與本地商家搜尋。

**主要工具：**
- `brave_web_search` — 一般網路關鍵字搜尋
- `brave_local_search` — 本地商家與地點搜尋

**使用場景：** 查詢最新資訊、技術文件搜尋、取得不在訓練資料中的即時內容。

---

### 6. vercel

| 項目 | 說明 |
|------|------|
| 類型 | HTTP（遠端 MCP 伺服器） |
| URL | `https://mcp.vercel.com` |
| 認證 | `Authorization: Bearer YOUR_VERCEL_TOKEN` |

**功能說明：** 管理 Vercel 部署平台，涵蓋專案部署、日誌查閱、網域管理等。

**主要工具：**
- `deploy_to_vercel` — 部署專案至 Vercel
- `list_projects` / `get_project` — 列出或查看專案
- `list_deployments` / `get_deployment` — 管理部署記錄
- `get_deployment_build_logs` / `get_runtime_logs` — 查閱建置與執行日誌
- `check_domain_availability_and_price` — 查詢網域可用性
- `search_vercel_documentation` — 搜尋 Vercel 官方文件

**使用場景：** 部署前端應用程式、監控部署狀態、查閱執行錯誤日誌。

---

### 7. supabase

| 項目 | 說明 |
|------|------|
| 套件 | `@supabase/mcp-server-supabase` |
| 類型 | stdio（npx） |
| 環境變數 | `--access-token YOUR_SUPABASE_ACCESS_TOKEN` |

**功能說明：** 直接操作 Supabase 專案，涵蓋資料庫查詢、schema 管理、SQL 執行、Edge Functions 管理等。

**主要工具：**
- `list_projects` / `get_project` — 列出或查看 Supabase 專案
- `execute_sql` — 直接執行 SQL 語句
- `list_tables` / `apply_migration` — 管理資料表與 schema 遷移
- `list_migrations` — 查看遷移記錄
- `get_logs` — 查閱專案執行日誌
- `list_edge_functions` / `deploy_edge_function` — 管理 Edge Functions
- `generate_typescript_types` — 根據 schema 產生 TypeScript 型別
- `get_project_url` / `get_publishable_keys` — 取得專案連線資訊
- `create_project` / `pause_project` / `restore_project` — 管理專案生命週期
- `get_advisors` — 取得效能與安全建議

**使用場景：** 查詢或修改 Supabase 資料庫、管理 table schema、除錯 Edge Functions、取得專案設定資訊。

---

### 8. n8n-haha-mcp

| 項目 | 說明 |
|------|------|
| 套件 | `n8n-mcp`（透過 npx 執行） |
| 類型 | stdio（npx） |
| API URL | `https://xlab-haha.zeabur.app/api/v1` |
| 環境變數 | `N8N_API_URL`、`N8N_API_KEY` |

**功能說明：** 連接 n8n 工作流程自動化平台，可建立、修改、執行、驗證 workflow，以及管理節點與範本。

**主要工具：**
- `n8n_list_workflows` / `n8n_get_workflow` — 瀏覽工作流程
- `n8n_create_workflow` / `n8n_update_full_workflow` — 建立或更新工作流程
- `n8n_validate_workflow` / `n8n_autofix_workflow` — 驗證與自動修復
- `n8n_test_workflow` / `n8n_executions` — 測試執行與查看執行記錄
- `search_nodes` / `get_node` — 搜尋與查閱節點規格
- `search_templates` / `n8n_deploy_template` — 範本搜尋與部署
- `n8n_manage_datatable` — 管理 n8n 資料表

**使用場景：** 設計與部署 n8n 自動化流程、除錯 workflow 錯誤、查找可用節點、管理自動化排程。

---

### 9. airtable

| 項目 | 說明 |
|------|------|
| 套件 | `airtable-mcp-server` |
| 類型 | stdio（npx） |
| 環境變數 | `AIRTABLE_API_KEY=YOUR_AIRTABLE_API_KEY` |

**功能說明：** 直接操作 Airtable 資料庫，涵蓋讀取、新增、更新、刪除資料記錄，以及管理 Base 與 Table schema。

**主要工具：**
- `list_bases` — 列出所有 Base
- `list_tables` — 列出指定 Base 的所有 Table
- `list_records` / `get_record` — 讀取資料記錄
- `create_record` / `update_record` / `delete_record` — 新增、更新、刪除記錄
- `search_records` — 依條件搜尋記錄

**使用場景：** 讀寫 Airtable 資料庫、自動化資料同步、搭配 n8n 或其他 MCP 做跨系統整合。

---

## 可用 Skills

Skills 以 `/skill-name` 斜線指令呼叫，或根據觸發場景自動啟用。

### 系統與設定類

#### update-config
- **觸發時機：** 需要設定 Claude Code 自動行為、鉤子（hooks）、永久性設定時
- **功能說明：** 修改 `settings.json` 設定 Claude Code harness，用於「每次 X 都要做 Y」類型的自動化行為

#### keybindings-help
- **觸發時機：** 需要自訂鍵盤快捷鍵、重新綁定按鍵時
- **功能說明：** 修改 `~/.claude/keybindings.json`，支援和弦快捷鍵設定

#### explain-actions
- **觸發時機：** 每次執行任何需要使用者確認的操作前（**自動觸發，必須執行**）
- **功能說明：** 在確認提示前以繁體中文說明操作內容與影響

#### simplify
- **觸發時機：** 完成程式碼修改後，需要審查與精簡時
- **功能說明：** 審查已修改的程式碼，尋找可重用、可優化的部分並修正

---

### 任務排程類

#### loop
- **觸發時機：** 需要定期重複執行某個任務、輪詢狀態時
- **功能說明：** 以指定時間間隔循環執行指令或 Skill（例：`/loop 5m /foo`）

#### schedule
- **觸發時機：** 需要建立 cron 排程遠端 agent 時
- **功能說明：** 建立、更新、列出、執行以 cron 時間表運行的排程 agent

---

### 開發類

#### claude-api
- **觸發時機：** 程式碼中出現 `anthropic` / `@anthropic-ai/sdk`；需要使用 Claude API 開發應用時
- **功能說明：** 建置、除錯、優化 Claude API 應用程式，預設包含 prompt caching 最佳化

#### mcp-builder
- **觸發時機：** 需要建立新的 MCP 伺服器、整合外部 API 至 MCP 協定時
- **功能說明：** 建立高品質 MCP 伺服器的完整指南，支援 Python（FastMCP）與 Node/TypeScript 兩種實作
- **子參考：** `reference/evaluation`、`reference/mcp_best_practices`、`reference/node_mcp_server`、`reference/python_mcp_server`

#### skill-creator
- **觸發時機：** 需要建立新 Skill、改善現有 Skill、執行效能評測時
- **功能說明：** 完整的 Skill 生命週期管理：草稿 → 測試 → 評估 → 優化
- **子 Agent：** `agents/analyzer`、`agents/comparator`、`agents/grader`

#### web-artifacts-builder
- **觸發時機：** 需要建立複雜的多元件 HTML artifact，使用 React / Tailwind CSS / shadcn/ui 時
- **功能說明：** 建立精緻前端 artifact 的工具套件

---

### 文件類

#### doc-coauthoring
- **觸發時機：** 需要撰寫文件、提案、技術規格、決策文件時
- **功能說明：** 三階段協作撰寫流程：情境蒐集 → 結構精煉 → 讀者測試

---

### n8n 專項類

#### n8n-sdd
- **觸發時機：** 描述自動化需求、說「我想自動化...」、「幫我建一個...」時
- **功能說明：** n8n workflow 需求分析，三階段：需求訪談 → 可行性驗證 → 規格書產出。所有輸出使用繁體中文

#### n8n-workflow-patterns
- **觸發時機：** 設計新 workflow 架構、選擇模式、規劃 webhook 處理時
- **功能說明：** 來自真實 n8n workflow 的架構模式參考

#### n8n-node-configuration
- **觸發時機：** 設定 n8n 節點、確認必填欄位、了解操作相依性時
- **功能說明：** 依操作類型提供節點設定指引

#### n8n-mcp-tools-expert
- **觸發時機：** 使用 n8n-mcp MCP 工具、搜尋節點、驗證設定時
- **功能說明：** n8n-mcp 工具使用專家指南，提供工具選擇策略

#### n8n-code-javascript
- **觸發時機：** 在 n8n Code node 撰寫 JavaScript，使用 `$input`/`$json`/`$node` 語法時
- **功能說明：** n8n Code node 的 JavaScript 寫法指南

#### n8n-code-python
- **觸發時機：** 在 n8n Code node 撰寫 Python 時
- **功能說明：** n8n Code node 的 Python 寫法指南，含 `_input`/`_json`/`_node` 語法

#### n8n-expression-syntax
- **觸發時機：** 撰寫 n8n `{{}}` 表達式、修復表達式錯誤時
- **功能說明：** 驗證 n8n 表達式語法並修復常見錯誤

#### n8n-validation-expert
- **觸發時機：** 遭遇 n8n 驗證錯誤、operator 結構問題時
- **功能說明：** 解讀 n8n 驗證錯誤訊息並引導修復

---

## 環境變數設定

在 `.mcp.json` 中需設定以下佔位符（請替換為真實值，**勿提交至版本控制**）：

| 變數名稱 | 對應 MCP | 取得方式 |
|---------|---------|---------|
| `YOUR_GITHUB_PAT` | github | GitHub Settings > Developer settings > Personal access tokens |
| `YOUR_BRAVE_API_KEY` | brave-search | Brave Search API 官方網站 |
| `YOUR_VERCEL_TOKEN` | vercel | Vercel Dashboard > Settings > Tokens |
| `YOUR_SUPABASE_ACCESS_TOKEN` | supabase | Supabase Dashboard > Account > Access Tokens |
| `YOUR_N8N_API_KEY` | n8n-haha-mcp | n8n 實例 > Settings > API |
| `YOUR_AIRTABLE_API_KEY` | airtable | Airtable > 帳號設定 > Developer Hub > Personal access tokens |

---

## 使用守則

1. **API 金鑰安全：** 含有真實金鑰的 `.mcp.json` 請加入 `.gitignore`，僅提交範本檔案
2. **Skill 優先：** 遇到已知場景（n8n 開發、文件撰寫、MCP 建立）時，優先呼叫對應 Skill
3. **confirm 前說明：** 每次執行會影響系統的操作前，透過 `explain-actions` Skill 以繁體中文說明
4. **n8n workflow 前先 SDD：** 建立 n8n workflow 前，必須先透過 `n8n-sdd` 完成規格書並取得確認
5. **回應語言：** 一律使用**繁體中文**回應使用者

---

*最後更新：2026 年 4 月 22 日*
