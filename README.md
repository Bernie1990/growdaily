# 語日俱進 - growdaily

我們都是一起從零開始練習日文的小夥伴～  
目標是「共同語日俱進」！一起加油吧！

## 最新修正 (2026-05-13)

**問題已定位**：Cloudflare Pages 仍在執行 `npx wrangler deploy`（Worker 模式），而不是 `wrangler pages deploy dist` 或純靜態 Pages 流程。

這導致 wrangler 把專案當成 Worker 部署，缺少 entry point 或 assets 配置。

### 立即修正步驟（Cloudflare Dashboard）

1. 前往 https://pages.cloudflare.com/
2. 選擇你的 **growdaily** 專案
3. 點擊 **Settings** → **Build and deployments**
4. 點擊 **Edit**（在 Production / Preview 分頁）
5. **重要修改**：

   - **Build command**：改成 `npm run build`
   - **Build output directory**：改成 `dist`
   - **Deploy command**：**清空或移除** `npx wrangler deploy`（這是最主要問題！）
   - **Root directory**：留空

6. 點擊 **Save**，然後點擊 **Redeploy**。

### 專案已更新的部分

- `wrangler.jsonc` 已改回使用 `"assets": {"directory": "./dist"}`（符合 wrangler 建議）
- `package.json` 的 `deploy` script 使用正確的 `wrangler pages deploy dist`
- `.gitignore` / `.wranglerignore` 已排除所有不該上傳的檔案（node_modules、workerd binary 等）
- Build script 確保 `dist/` 只包含乾淨的靜態檔案

### 本地測試

```bash
npm install
npm run build          # 檢查 dist/ 是否正確
npm run deploy         # 本地執行部署（需已登入 wrangler）
```

或直接用：

```bash
npx wrangler pages deploy dist
```

### 驗證方式

部署成功後，dist 資料夾應該只包含：
- index.html
- data/ 資料夾（所有 JSON）
- prompts/（如果需要）

**不會再出現** `node_modules/workerd/bin/workerd (119 MiB)` 或 "Missing entry-point to Worker script" 錯誤。

---

**維護者**：Bernie + Hermes Agent  
**最後更新**：2026-05-13

如果按照以上步驟修改後仍有錯誤，請把最新的 build log 貼給我，我會繼續幫您調整 wrangler 配置或改成純 Pages 模式（不依賴 wrangler）。