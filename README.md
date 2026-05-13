# 語日俱進 - growdaily

我們都是一起從零開始練習日文的小夥伴～  
目標是「共同語日俱進」！一起加油吧！

## 最新修正 (2026-05-13)

**問題已定位**：Cloudflare Pages 仍在執行 `npx wrangler deploy` 而非使用 Pages build 設定，導致 `assets.directory` 或配置找不到 `dist/`。

**最新修正**：已更新 `wrangler.jsonc` 使用標準 `pages.build` 配置（符合 cloudflare-pages-static-site-setup skill 建議）。

### 立即修正步驟（Cloudflare Dashboard）

1. 前往 https://pages.cloudflare.com/
2. 選擇你的 **growdaily** 專案
3. 點擊 **Settings** → **Build and deployments**
4. 點擊 **Edit**（Production 與 Preview 分別設定）
5. **重要修改**：

   - **Build command**：`npm run build`
   - **Build output directory**：`dist`
   - **Deploy command**：**完全清空**（移除 `npx wrangler deploy`）
   - **Root directory**：留空

6. 點擊 **Save** 並 **Redeploy**。

### 目前專案配置（已更新）

- `wrangler.jsonc`：使用 `"pages": {"build": {"command": "npm run build", "output_dir": "dist"}}`
- `package.json`：包含正確的 `build` 與 `deploy` scripts
- `.wranglerignore` + `.gitignore`：已排除 node_modules、.wrangler、dist 等，避免 asset too large 或污染
- Build script：乾淨複製 `index.html` + `data/` 到 `dist/`

### 本地測試指令

```bash
npm install
npm run build          # 產生 dist/ 並驗證
npm run preview        # 用 serve 測試靜態站台
# 或
npm run deploy         # 直接部署（需 wrangler login）
```

或純 wrangler：
```bash
npx wrangler pages deploy dist
```

### 驗證

- `ls dist/` 應看到 `index.html` + `data/` 資料夾
- 部署後網站應正常載入 JSON 並顯示「五段動詞與て形音變總整理」等內容
- 不應再出現 "assets.directory does not exist" 或 workerd binary 過大錯誤

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