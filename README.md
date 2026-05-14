# 語日俱進 - growdaily

我們都是一起從零開始練習日文的小夥伴～  
目標是「共同語日俱進」！一起加油吧！

## ⚠️ DEPLOYMENT STATUS (2026-05-14)

We have been ping-ponging between two wrangler configs due to Cloudflare dashboard settings:

- `pages.build` config → "Unexpected fields found in top-level field: 'pages'" + "Missing entry-point to Worker script or to assets directory"
- `assets.directory` config → "The directory specified by the 'assets.directory' field ... does not exist: .../dist"

**Current working approach (after checking git history)**: Use `"assets": {"directory": "./dist"}` in `wrangler.jsonc` + **ensure dashboard Build command runs `npm run build` first**.

This matches commit 49a5fb7 ("revert wrangler.jsonc to assets.directory to match npx wrangler deploy").

### REQUIRED Cloudflare Dashboard Settings (Critical)

1. Go to https://pages.cloudflare.com/ → **growdaily** project
2. **Settings** → **Build and deployments** → **Edit** (both Production and Preview branches)
3. Set **exactly**:
   - **Build command**: `npm run build`          ← This is what creates the `dist/` folder *before* deploy runs
   - **Build output directory**: `dist`
   - **Deploy command**: `npx wrangler deploy`   ← Matches the current wrangler.jsonc (assets mode)
   - **Root directory**: (leave empty)

4. **Save**, then click **"Clear cache and deploy"** or push a new commit to trigger fresh build (caches were restoring old state).

### Current Config (working)

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

**package.json** (build script ensures clean static output):
```json
"scripts": {
  "build": "rm -rf dist && mkdir -p dist && cp -r index.html data/ dist/ && cp -r prompts/ dist/ 2>/dev/null || true",
  "deploy": "npm run build && wrangler pages deploy dist"
}
```

### Local Verification

```bash
npm ci
npm run build
ls -R dist/              # Must contain index.html + data/ + prompts/
npm run preview
```

After dashboard is updated to run the Build command, the Cloudflare log should show the build step creating `dist/` before `npx wrangler deploy` executes.

### Project Focus
Priority remains on phonological Japanese learning ("五段動詞與て形音變總整理" page) with explanations of 音便, 連濁 (rendaku), 鼻音化 rather than rote lists. All content lives in `data/*.json` and is copied to `dist/` on build.

**維護者**: Bernie + Hermes Agent (cloudflare-pages-static-site-setup skill + git history review)  
**最後更新**: 2026-05-14
