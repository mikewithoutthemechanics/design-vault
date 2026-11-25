# ThreeUI Community — Agent Guidance

## Project Structure
- **Monorepo**: Two npm packages under one root
  - Root: `@designcodeio/threeui` — React component library (main package)
  - `packages/cli/`: `@designcodeio/threeui-cli` — Pro installer CLI
- **Entry points**: `src/index.ts` (lib), `src/main.tsx` (dev site), `packages/cli/bin/threeui.mjs` (CLI)
- **Build output**: `lib-dist/` (generated, not committed)

## Commands
| Task | Command |
|------|---------|
| Dev server | `npm run dev` |
| Typecheck | `npm run typecheck` |
| Run tests | `npm run test` |
| Build site (prod) | `npm run build:site` |
| Build library | `npm run build:lib` |
| **Full verification** (CI) | `npm run build` |
| Smoke test package | `npm run smoke:package` |
| Sync from private ThreeUI | `npm run sync:community -- /path/to/main-threeui` |

**Order matters**: `npm run build` runs `test → build:site → build:lib`. Run `npm run build` before PRs.

## Key Scripts (in `scripts/`)
- `audit-public.mjs` — Verifies no Pro/Beta code leaks into public build
- `public-boundary.test.mjs` — Checks import graph stays Community-only
- `library-package.test.mjs` — Validates package.json exports and files
- `audit-build.mjs` — Post-build artifact verification
- `generate-library-entry.mjs` — Creates `src/index.ts` from component catalog
- `sync-community-from-main.mjs` — One-way sync from private repo (maintainers only)

## CI / Release Flow
- **CI** (`.github/workflows/ci.yml`): Runs on PR/push to `main` and `automation/community-sync`. Executes `npm ci && npm run build && npm run smoke:package`.
- **Publish** (`.github/workflows/publish-npm.yml`): Triggers on `package.json` version change pushed to `main`. Publishes both packages via npm provenance.
- **Community sync PR** (`.github/workflows/community-sync-pr.yml`): Opens PR when `automation/community-sync` branch updates.

## Development Quirks
- **Two Three.js versions**: `three128` (legacy shaders) and `three165` (current) aliased in `package.json` and `vite.config.js`.
- **Component subpath exports**: Use `@designcodeio/threeui/components/ComponentName` for minimal imports.
- **Assets**: Components referencing runtime files expect them at root-relative URLs. Copy from `lib-dist/assets/` to your `public/` or override `sourceUrl`/`assetBaseUrl` props.
- **No auth/account code**: Community build strips Pro/Beta implementation, checkout runtime, and restricted fonts.
- **TypeScript**: Strict mode, `noEmit` for dev, `declaration: true` for lib build (`tsconfig.lib.json`).

## Testing
- Unit tests: `node --test` on `scripts/*.test.mjs` and `packages/cli/test/*.test.mjs`
- No external test runner; uses Node's built-in test runner
- Tests cover: public boundary, library package integrity, community sanitizers, release prep

## Adding/Updating Components
1. Modify `src/package-components/*.ts` (component entry points)
2. Run `npm run generate:lib` to update `src/index.ts`
3. Run `npm run build:lib` to verify library build
4. Run `npm run audit:public` to verify no Pro leaks

## Common Pitfalls
- Forgetting `npm run generate:lib` after adding components → stale exports
- Skipping `npm run audit:public` → Pro code accidentally published
- Not running full `npm run build` before PR → CI fails
- Editing `lib-dist/` directly → overwritten on next build