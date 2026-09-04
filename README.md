# AgentA Landing

Public download site for the AgentA Android app, hosted on Vercel.

- `index.html` — the landing page. The **Download APK** button points at the
  latest public release asset (`AgentA.apk`) of this repo, so tapping it on a
  phone downloads the installer directly.
- Deploy: `npx vercel --prod` from this folder.

## Publishing a new APK

1. Download the APK artifact from the private
   [AgentA](../../) repo (`gh run download --repo Shuash11/AgentA ...`).
2. Rename it to `AgentA.apk` and attach it to a release here:
   `gh release create vX.Y.Z AgentA.apk --repo Shuash11/AgentA-landing`
3. The button picks it up automatically via `/releases/latest/download/`.
