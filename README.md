# Package Name

Package description

## Installation

```bash
npm install package-name
# or
pnpm add package-name
# or
yarn add package-name
```

## Usage

```typescript
import { YourClass } from "package-name";

// Usage example
const instance = new YourClass();
```

## API

### YourClass

Description of your main export.

#### Methods

- `method()` - Description of method

## Development

### 🔧 Quick Setup

To quickly set up branch protection rules, run the automated script:

```bash
# Make sure you have GitHub CLI installed and authenticated
gh auth login

# Run the setup script (choose one method)
./scripts/setup-branch-protection.sh
# or
npm run setup:branch-protection
```

This script will automatically:

- ✅ Detect your main branch (main/master)
- ✅ Configure branch protection rules
- ✅ Require pull requests before merging
- ✅ Require at least 1 approval
- ✅ Dismiss stale reviews on new commits
- ✅ Disable force pushes and deletions
- ✅ Apply rules to administrators

### Manual Repository Security Setup (Alternative)

If you prefer manual setup, configure branch protection rules:

#### Branch Protection Rules

1. Go to your GitHub repository
2. Navigate to Settings → Branches
3. Click "Add rule" for the `main`/`master` branch
4. Configure the following settings:

**Required settings:**

- ✅ Require a pull request before merging
- ✅ Require approvals: 1 (or more for team projects)
- ✅ Dismiss stale PR approvals when new commits are pushed
- ✅ Require status checks to pass before merging
- ✅ Require branches to be up to date before merging
- ✅ Require conversation resolution before merging
- ✅ Restrict pushes that create files larger than 100MB

**Optional but recommended:**

- ✅ Require signed commits
- ✅ Include administrators (applies rules to repo admins too)
- ✅ Allow force pushes: No one (or "Specify who can force push" for admins only)
- ✅ Allow deletions: No one

#### Status Checks (if you add CI/tests)

If you add testing workflows, also enable:

- ✅ Require status checks to pass before merging
- Add your test workflow names to required checks

This ensures:

- No direct commits to main/master
- All changes go through pull requests
- Code review is mandatory
- Automated releases work properly

### Setup for Automated Releases

To enable automated releases, configure the following secrets in your GitHub repository:

#### 1. NPM Token

1. Go to [npmjs.com](https://www.npmjs.com) and sign in
2. Click on your avatar → "Access Tokens"
3. Click "Generate New Token" → "Automation"
4. Copy the generated token
5. In your GitHub repo: Settings → Secrets and variables → Actions
6. Click "New repository secret"
7. Name: `NPM_TOKEN`
8. Value: paste your npm token

#### 2. GitHub Token

The `GITHUB_TOKEN` is automatically provided by GitHub Actions, no additional configuration needed.

#### 3. Repository Configuration

Make sure in Settings → Actions → General:

- "Allow GitHub Actions to create and approve pull requests" is checked
- Workflow permissions: "Read and write permissions" is selected

### Releases

This project uses [semantic-release](https://github.com/semantic-release/semantic-release) for automated releases. Use conventional commits:

- `feat:` - New feature (minor version)
- `fix:` - Bug fix (patch version)
- `feat!:` or `BREAKING CHANGE:` - Breaking change (major version)

Examples:

```
feat: add new authentication method
fix: resolve memory leak in parser
feat!: remove deprecated API endpoints
```

### Release Process

1. Make your commits following conventional commits
2. Create a pull request to `main`/`master`
3. Get code review and approval
4. Merge the pull request
5. GitHub Actions triggers automatically
6. A new release is created if significant changes are detected
7. The package is published to npm
8. A GitHub release with changelog is generated

## License

MIT
