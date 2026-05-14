# 語日俱進 - growdaily

我們都是一起從零開始練習日文的小夥伴～  
目標是「共同語日俱進」！一起加油吧！

## ⚠️ LATEST FIX (2026-05-14) - wrangler deploy error resolved in config

**Previous error fixed**: Changed `wrangler.jsonc` back to use `"assets": {"directory": "./dist"}` (exactly what the current `npx wrangler deploy` command expects).

**New error was**: 
- "Unexpected fields found in top-level field: 'pages'"
- "Missing entry-point to Worker script or to assets directory"

This happened because `npx wrangler deploy` (Worker mode) does not understand the `pages.build` section.

### REQUIRED Cloudflare Dashboard Settings

1. Go to https://pages.cloudflare.com/ → your **growdaily** project
2. **Settings** → **Build and deployments** → **Edit** (Production + Preview)
3. Set these values:

   - **Build command**: `npm run build`          ← This creates the `dist/` folder
   - **Build output directory**: `dist`
   - **Deploy command**: `npx wrangler deploy`   ← Now compatible with wrangler.jsonc
   - **Root directory**: (empty)

4. Save and **Redeploy**.

### Current Config (verified)

**wrangler.jsonc**
```json
{
  "$schema": "node_modules/wrangler/config-schema.json",
  "name": "growdaily",
  "compatibility_date": "2026-05-14",
  "assets": {
    "directory": "./dist"
  }
}
```

**package.json build script** — copies `index.html` + `data/` (JSON files with godan verb tables, hiki counter rules, resources) into `dist/`.

### Local Test

```bash
npm ci
npm run build
ls dist/                  # must contain index.html + data/
npm run preview
```

After you update the dashboard Build command to `npm run build`, the remote build will create `dist/` before running `wrangler deploy`, and the deployment should succeed.

The site focuses on phonological understanding of Japanese (連濁, 鼻音化, て形 changes) rather than rote memorization, with the main "五段動詞與て形音變總整理" table.

**維護者**: Bernie + Hermes Agent (using cloudflare-pages-static-site-setup skill)  
**最後更新**: 2026-05-14
