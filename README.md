# 語日俱進 - growdaily

我們都是一起從零開始練習日文的小夥伴～  
目標是「共同語日俱進」！一起加油吧！

## ⚠️ CRITICAL FIX - DEPLOYMENT ERROR (2026-05-13)

The repeated error:

✘ [ERROR] The directory specified by the "assets.directory" field in your configuration file does not exist: /opt/buildhome/repo/dist

**Root cause**: Your Cloudflare Pages project settings still have **Deploy command = `npx wrangler deploy`**. This runs *before* the build step, so `dist/` is never created.

The commit a85f827 (and current main) fixed `wrangler.jsonc` to use the correct `pages.build` config. The log you showed is from an older build or cache.

### REQUIRED DASHBOARD CHANGES (do this now):

1. Go to: https://pages.cloudflare.com/
2. Select your **growdaily** project
3. Go to **Settings** → **Build and deployments**
4. Click **Edit** on both Production and Preview environments
5. Set exactly:
   - **Build command**: `npm run build`
   - **Build output directory**: `dist`
   - **Deploy command**: (completely empty - delete `npx wrangler deploy`)
   - **Root directory**: (leave empty)
6. Save changes
7. Click **Redeploy** (or push a new commit to trigger)

### Current Project State (verified)

- `wrangler.jsonc` now uses:
  ```json
  "pages": {
    "build": {
      "command": "npm run build",
      "output_dir": "dist"
    }
  }
  ```
- Build script correctly copies `index.html`, `data/`, and `prompts/` into `dist/`
- `.wranglerignore` and `.gitignore` prevent including heavy files like `node_modules/workerd`

### Test Locally

```bash
npm ci
npm run build
ls -R dist/          # verify structure
npm run preview      # open http://localhost:8080
```

After you make the dashboard changes above, future deploys will work.

If you see any new error after doing this, paste the full new build log.

**維護者**: Bernie + Hermes Agent (updated via skill: cloudflare-pages-static-site-setup)
**最後更新**: 2026-05-13
