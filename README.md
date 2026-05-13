# 語日俱進 - growdaily

我們都是一起從零開始練習日文的小夥伴～  
目標是「共同語日俱進」！一起加油吧！

## 網站特色
- 每日單字練習卡片（點擊即可切換）
- 社群共筆知識庫連結（HackMD）
- AI 智能分類學習筆記（未來由 Bernie 的 Agent 自動維護）
- 乾淨優雅的介面，適合長期使用

## Cloudflare Pages 部署說明（已修正）

### 目前問題已解決
- 已新增 `index.html` 包含完整的靜態資源（HTML + Tailwind + JS）
- Cloudflare 之前報錯 `Could not detect a directory containing static files` 是因為專案原本是空的

### 請在 Cloudflare Dashboard 設定以下參數：

1. 前往 [Cloudflare Pages](https://pages.cloudflare.com/)
2. 選擇你的 growdaily 專案
3. 點擊 **Settings** → **Build and deployments**
4. 修改以下設定：
   - **Build command**: 留空 或填 `echo "Build completed"`
   - **Output directory**: `/` （根目錄）
   - **Root directory**: 留空（預設整個 repository）

5. 儲存後點擊 **Save**，然後 **Deploy** 或等待 Git push 自動部署。

### 本地開發
```bash
# 安裝依賴（如果未來想用 Vite 開發）
npm create vite@latest . -- --template vanilla

# 或直接用瀏覽器開啟
open index.html
```

### 未來規劃
- 串接 HackMD API 自動更新共筆內容
- 加入學習打卡系統與進度追蹤
- 使用 AI Agent 動態維護單字庫與分類
- 升級為 Vite + React 或 Astro 專案

---

**維護者**：Bernie + AI Agent  
**部署平台**：Cloudflare Pages  
**最後更新**：2026-05-13

有任何問題或想新增功能，歡迎直接告訴我！