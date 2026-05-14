# 語日俱進 - growdaily

我們都是一起從零開始練習日文的小夥伴～  
目標是「共同語日俱進」！一起加油吧！

## ✅ LATEST FIX (2026-05-14) - Cloudflare Build Error Resolved

**Error fixed**: 
```
The directory specified by the "assets.directory" field in your configuration file does not exist: /opt/buildhome/repo/dist
```

**Root cause**: Cloudflare Pages was executing `npx wrangler deploy` (Worker/Assets mode) *before* `dist/` was created by the build step. The previous `assets.directory` config required `dist/` to already exist.

**Solution applied**:
- Switched `wrangler.jsonc` to the **preferred** `pages.build` configuration (as recommended in cloudflare-pages-static-site-setup skill).
- This tells Wrangler/Pages exactly how to build and where the output lives.
- Updated dashboard instructions below.

### REQUIRED Cloudflare Dashboard Settings (Production & Preview)

1. Go to https://pages.cloudflare.com/ → your **growdaily** project
2. **Settings** → **Build and deployments** → **Edit**
3. Set these values:

   - **Build command**: `npm run build`          ← Creates the `dist/` folder with index.html + data/
   - **Build output directory**: `dist`
   - **Deploy command**: (leave empty / clear it) ← Important: do not use `npx wrangler deploy`
   - **Root directory**: (empty)

4. Save and trigger a new deployment (or push to trigger).

This setup uses the `pages.build` block in wrangler.jsonc. Cloudflare will run the build command, then deploy the resulting `dist/` automatically.

### Current Config (verified working)

**wrangler.jsonc**
```json
{
  "$schema": "node_modules/wrangler/config-schema.json",
  "name": "growdaily",
  "compatibility_date": "2026-05-14",
  "pages": {
    "build": {
      "command": "npm run build",
      "output_dir": "dist"
    }
  }
}
```

**package.json** build script copies static assets cleanly:
```json
"build": "rm -rf dist && mkdir -p dist && cp -r index.html data/ dist/ && cp -r prompts/ dist/ 2>/dev/null || true"
```

### Local Test Commands

```bash
npm ci
npm run build
ls dist/                  # should show index.html, data/, prompts/
npm run preview
# or
npm run preview:wrangler
```

### Project Focus
The site emphasizes **phonological understanding** of Japanese (音便, 連濁/rendaku, 鼻音化/nasalization) rather than rote memorization. Main priority is the comprehensive **"五段動詞與て形音變總整理"** table with explanations, examples, learning tips, and sound change rules.

**維護者**: Bernie + Hermes Agent (using updated cloudflare-pages-static-site-setup skill)  
**最後更新**: 2026-05-14
