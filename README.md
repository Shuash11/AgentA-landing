# AgentA Landing

Public download site for the AgentA Android app, hosted on Vercel.

- `index.html` — the landing page. The **Download APK** button points at the
  latest public release asset (`AgentA.apk`) of this repo, so tapping it on a
  phone downloads the installer directly.
- Deploy: auto-deploys from `main` via Vercel project
  `prj_Jhz3Bovys9MoHleRH4I0J1PZHyAE` (no manual `npx vercel --prod` needed for
  routine releases).

## Publishing a new APK (automatic)

Pushes to `main` in the private [AgentA](https://github.com/Shuash11/AgentA)
repo trigger `Build APK`, then `Promote to Landing (latest)`:

1. Downloads the `AgentA-<SHA>.apk` artifact from that run.
2. Renames it to the stable `AgentA.apk` (+ a dated `AgentA-<SHA7>.apk` copy)
   and uploads both to the rolling `latest` release here with
   `gh release upload --clobber` (3x retry for the ~283MB file, dated copies
   pruned to the newest 2).
3. Verifies `releases/latest/download/AgentA.apk` with `curl -L -I -f`.
4. Patches the version pill in `index.html` to `latest • <MM-DD> • <SHA7>`
   (static HTML only, CSP-safe — `script-src 'none'` untouched) and pushes to
   `main`, which triggers the Vercel auto-deploy.

The button picks up the new build automatically via
`/releases/latest/download/`. Do not commit APK binaries to git.

## Rollback

- Re-upload a known-good asset to the same rolling release:
  `gh release upload latest AgentA-<SHA7>.apk --clobber` then copy it over
  `AgentA.apk` and re-upload, or
  `gh release download latest --pattern 'AgentA-<SHA7>.apk'`
  from the retained dated copies (newest 2 are kept).
- Revert the pill commit (`git revert <sha>` + push) to redeploy the previous
  landing page via Vercel.
