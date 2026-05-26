# AGENTS.md

## Setup

- **Node.js 22+ required** - see `.nvmrc`
- **Windows**: Run `npm install -g windows-build-tools` before `npm install`
- **Linux**: Requires `libx11-dev zlib1g-dev libpng-dev libxtst-dev`
- **postinstall runs automatically**: `patch-package` runs after `npm install` - don't skip it

## Commands

| Command | Purpose |
|---------|---------|
| `npm start` | Dev mode (webpack + electron + watch) |
| `npm run build` | Production build (main + renderer) |
| `npm run dist` | Full distribution (build + electron-builder) |
| `npm run lint` | Lint code |
| `npm run lint-fix` | Fix lint issues |
| `npm run watch` | Watch renderer only |

## Dev Mode

- Opens WebRTC internals window automatically
- To open DevTools: `SHOW_DEV_TOOLS=true npm start`

## Important Flags

- `ENABLE_REMOTE_CONTROL`: Must be enabled in **both** `main.js:34` and `Conference.js:18`

## Architecture Notes

- **Main process**: `main.js` - handles window, protocol, security, IPC
- **Renderer**: React app in `app/` - bundled via webpack
- **Feature-based structure**: `app/features/*/` - each feature has components, redux, styled
- **@jitsi/electron-sdk**: Integrated via preload script; to develop with local version use `file://` path + `--force` install

## No Tests

This repo has **no test suite**. Don't look for test commands or write tests.

## Windows Media Devices Fix

If camera/microphone don't work on Windows, the fix is already applied in `main.js`:
- Media permissions are auto-granted on Windows to fix device access issues

## Release Process

1. Create branch: `git checkout -b release-X-Y-Z`
2. Version bump: `npm version [patch|minor|major]`
3. PR to master, then create draft release
4. CI builds on merge to master

## Key Files

- `main.js` - Electron entry, window management, protocol handling
- `app/features/conference/components/Conference.js` - Core Jitsi iframe integration
- `app/features/settings/` - Settings persistence
- `app/i18n/lang/` - Translation files
