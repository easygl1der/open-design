# WRISE fork of Open Design

`easygl1der/open-design` is WRISE's own version of [nexu-io/open-design](https://github.com/nexu-io/open-design). `main` is an upstream release tag plus a few WRISE commits, one per change, each titled `WRISE: …`. This is what runs on the SG machine behind https://design.yitwah.site.

- Base: `open-design-v0.26.1`
- GitHub Actions are **off** in this fork, so it never uses CI minutes.

## WRISE changes

`git log --oneline open-design-v0.26.1..main` lists them:

1. Lock `CLAUDE_BIN` / `CLAUDE_CONFIG_DIR` out of Settings, so every Claude run goes through the SG egress shim.
2. Hide the Open Design Cloud "Get free credits" card in the left rail.
3. Choose a working folder by typing its path on the server. The daemon runs headless, so the native folder dialog would open on the server's screen, not in your browser.
4. A how-to guide with short videos at `/guide/` (`apps/web/public/guide/`), opened from a "Guide" item in the left rail.

## Updating to a new upstream release (by hand)

```bash
git fetch upstream --tags
git rebase --onto open-design-vX.Y.Z open-design-v0.26.1 main   # replay the WRISE commits
pnpm install && pnpm --filter @open-design/daemon build && pnpm --filter @open-design/web build
launchctl kickstart -k gui/$(id -u)/com.wrise.open-design       # on SG
git push --force-with-lease origin main
```

Then update the base tag in this file. The run script and service setup live in `easygl1der/wrise-docs` (`scripts/open-design/`, ADR 0123 and ADR 0127).
