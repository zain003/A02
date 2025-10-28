# Setup Instructions for GitHub Actions with Vercel Deployment

## Prerequisites
- GitHub account with repository access
- Vercel account (free tier works)
- Node.js installed locally (for testing)

---

## Step 1: Vercel Setup

### 1.1 Create Vercel Account
1. Go to [vercel.com](https://vercel.com)
2. Sign up with GitHub
3. Import your repository

### 1.2 Get Vercel Credentials
1. Go to Vercel Dashboard → Settings → Tokens
2. Create new token → Copy the token
3. Go to Project Settings → General
4. Copy **Project ID** and **Org ID**

### 1.3 Configure GitHub Secrets
Go to GitHub repository → Settings → Secrets and variables → Actions

Add these secrets:
- `VERCEL_TOKEN` - Your Vercel token
- `VERCEL_ORG_ID` - Your organization ID
- `VERCEL_PROJECT_ID` - Your project ID

---

## Step 2: Repository Configuration

### 2.1 Enable GitHub Actions
1. Go to repository Settings → Actions → General
2. Select "Allow all actions and reusable workflows"
3. Click Save

### 2.2 Set Up Branch Protection (Optional but Recommended)
1. Go to Settings → Branches
2. Add rule for `main` branch:
   - ✅ Require pull request reviews before merging
   - ✅ Require status checks to pass (select CI workflow)
   - ✅ Require branches to be up to date before merging

---

## Step 3: Test Workflows

### 3.1 Test CI Workflow
```bash
# Create test branch
git checkout -b test-workflows

# Make a small change
echo "// Test" >> src/utils.js

# Commit and push
git add .
git commit -m "test: trigger CI workflow"
git push origin test-workflows
```

Check Actions tab - CI workflow should run automatically.

### 3.2 Test Deployment
```bash
# Push to main branch
git checkout main
git merge test-workflows
git push origin main
```

This will trigger automatic deployment to Vercel production.

### 3.3 Test Manual Deployment
1. Go to Actions → Deployment Pipeline
2. Click "Run workflow"
3. Select environment (production/preview)
4. Click "Run workflow"

---

## Step 4: Verify Workflows

### Check Each Workflow:

#### ✅ Continuous Integration (ci.yml)
- Triggers: Push, Pull Request
- Expected: Tests pass on Node.js 16, 18, 20

#### ✅ Deployment Pipeline (deploy.yml)
- Triggers: Push to main, Manual
- Expected: Deploys to Vercel, returns deployment URL

#### ✅ Scheduled Tasks (scheduled-tasks.yml)
- Triggers: Daily (2 AM UTC), Weekly (Sunday 3 AM UTC), Manual
- Test: Run manually from Actions tab

#### ✅ Dependency Updates (dependency-updates.yml)
- Triggers: Weekly (Monday 9 AM UTC), Manual
- Test: Run manually, check for PR creation

#### ✅ Code Review (code-review.yml)
- Triggers: Pull Request
- Test: Create PR, check for automated comments

#### ✅ Documentation (documentation.yml)
- Triggers: Push to main (docs changes), Manual
- Test: Update README.md, push to main

#### ✅ Custom Workflow (custom-workflow.yml)
- Triggers: Tag push (v*.*.*), Manual
- Test: Create tag `git tag v1.0.0 && git push origin v1.0.0`

---

## Step 5: Access Deployment

### Production URL
After successful deployment, find your URL:
1. Check workflow logs in Actions tab
2. Look for "Deploy to Vercel Production" job
3. URL will be displayed in logs
4. Or visit your Vercel dashboard

### Preview Deployments
- Run workflow manually with "preview" environment
- Preview URL will be shown in workflow logs

---

## Troubleshooting

### Workflow Not Running
- Check if Actions are enabled in repository settings
- Verify workflow file syntax (no YAML errors)
- Check trigger conditions match your branch name

### Deployment Failing
**Error: Missing Vercel credentials**
- Solution: Add VERCEL_TOKEN, VERCEL_ORG_ID, VERCEL_PROJECT_ID to GitHub secrets

**Error: Vercel CLI not found**
- Solution: Workflow installs it automatically, check logs for errors

**Error: Build failed**
- Solution: Check if `npm ci` succeeds locally
- Verify package.json is correct

### Secrets Not Working
- Ensure secret names match exactly (case-sensitive)
- Secrets must be added at repository level, not organization
- Re-create secrets if they're not working

---

## Workflow Files Summary

| File | Purpose | Manual Test |
|------|---------|-------------|
| `ci.yml` | Testing & quality | Push to any branch |
| `deploy.yml` | Vercel deployment | Push to main or manual |
| `scheduled-tasks.yml` | Maintenance | Manual from Actions tab |
| `dependency-updates.yml` | Dependency management | Manual from Actions tab |
| `code-review.yml` | PR automation | Create pull request |
| `documentation.yml` | Docs deployment | Update README, push |
| `custom-workflow.yml` | Release management | Create git tag |

---

## Expected Artifacts

After running workflows, check Actions → Workflow run → Artifacts:

- **build-artifact** (7 days) - From deploy.yml
- **satellite-data-*** (30 days) - From scheduled-tasks.yml
- **weekly-backup-*** (90 days) - From scheduled-tasks.yml
- **outdated-packages-report** (30 days) - From dependency-updates.yml
- **security-audit-report** (90 days) - From dependency-updates.yml
- **code-quality-reports** (30 days) - From code-review.yml
- **security-scan-reports** (90 days) - From code-review.yml
- **documentation** (30 days) - From documentation.yml
- **release-notes** (30 days) - From custom-workflow.yml
- **performance-metrics** (90 days) - From custom-workflow.yml
- **deployment-log** (30 days) - From deploy.yml

---

## Quick Reference Commands

```bash
# Test CI
git checkout -b test-ci
echo "// test" >> src/utils.js
git add . && git commit -m "test: CI" && git push origin test-ci

# Deploy to production
git checkout main
git push origin main

# Create release
git tag v1.0.0
git push origin v1.0.0

# View workflow runs
gh run list

# Watch workflow
gh run watch

# Download artifacts
gh run download <run-id>
```

---

## Support

For issues:
1. Check workflow logs in Actions tab
2. Review WORKFLOWS_DOCUMENTATION.md
3. Check TESTING_GUIDE.md
4. Verify all secrets are configured correctly

---

## Assignment Deliverables Checklist

- ✅ 7 YAML workflow files in `.github/workflows/`
- ✅ WORKFLOWS_DOCUMENTATION.md (detailed documentation)
- ✅ TESTING_GUIDE.md (testing procedures)
- ✅ DELIVERABLES_SUMMARY.md (project summary)
- ✅ Configuration files (.eslintrc.json, .prettierrc, jsdoc.json)
- ✅ vercel.json (Vercel deployment config)
- ✅ All workflows tested and functional
- ✅ Artifacts generated and accessible
- ✅ Deployment logs available
