# basic-package-template

Minimal npm package template with automated releases via OIDC trusted publishing. No tokens needed.

## New Package Setup

1. **Create repo from template** on GitHub
2. **Update `package.json`** — set `name`, `description`, `repository`, `bugs`, `homepage`
3. **Run `pnpm install`**
4. **Run `pnpm bootstrap` manually in your terminal** — registers the package on npm using your local auth. This must be run interactively (not from a script or CI) because npm requires 2FA/OTP confirmation.
5. **Configure trusted publisher** in npm UI:
   - Go to your package's access settings on npmjs.com
   - Add GitHub Actions as a trusted publisher
   - Repository: `<owner>/<repo>`, Workflow: `release.yml`
6. **Run `pnpm setup:branch-protection`** — secures the repo
7. **Push code** — CI publishes automatically with OIDC provenance

## How It Works

### Trusted Publishing (OIDC)

Instead of storing an `NPM_TOKEN` secret, this template uses [npm's trusted publishing](https://docs.npmjs.com/generating-provenance-statements). GitHub Actions authenticates directly with npm via OpenID Connect — no secrets to rotate, no tokens to leak.

The release workflow requests an `id-token: write` permission, and npm verifies the token came from your authorized repository and workflow.

> **Note:** The GitHub Actions runner's bundled npm may be too old to support OIDC token exchange. The release workflow includes `npm install -g npm@latest` to ensure OIDC trusted publishing works correctly.

### Versioning

semantic-release determines versions from **git tags**, not `package.json`. The `version` field in `package.json` is set to `"0.0.0-development"` and is updated automatically during publish.

**Important:** semantic-release defaults to `1.0.0` for the first release. If you want your first release to be `0.x.x`, create a git tag before your first push to `main`:

```bash
# To start at 0.1.0:
git tag v0.1.0
git push --tags
```

Without a pre-existing tag, all `feat:` commits in your history will be analyzed and the first release will be `1.0.0`.

### Release Flow

1. Push to `main` with [conventional commits](https://www.conventionalcommits.org/)
2. `semantic-release` determines the next version from git tags
3. Publishes to npm with provenance
4. Creates a GitHub release with changelog

### Conventional Commits

| Prefix | Version Bump | Example |
|--------|-------------|---------|
| `fix:` | Patch | `fix: handle null input` |
| `feat:` | Minor | `feat: add retry logic` |
| `feat!:` or `BREAKING CHANGE:` | Major | `feat!: redesign API` |

## Adding Your Build

This template ships with no build tooling — add what you need:

```bash
# Example: add tsup + typescript
pnpm add -D tsup typescript
```

Then update the `build` script in `package.json`.

## Scripts

| Script | Description |
|--------|-------------|
| `pnpm build` | Build the package (placeholder — replace with your build) |
| `pnpm bootstrap` | Register package on npm via local auth |
| `pnpm setup:branch-protection` | Apply branch protection rules via GitHub CLI |
