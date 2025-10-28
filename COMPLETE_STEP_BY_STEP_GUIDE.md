# Complete Step-by-Step Guide - GitHub Actions Assignment

## 📋 Overview
This guide walks you through the entire assignment from start to finish, including getting Vercel credentials and testing all 7 workflows.

---

## Part 1: Getting Vercel Credentials (10 minutes)

### Step 1.1: Create Vercel Account
1. Go to https://vercel.com
2. Click **"Sign Up"**
3. Choose **"Continue with GitHub"**
4. Authorize Vercel to access your GitHub account
5. Complete the signup process

### Step 1.2: Import and Deploy Your Repository (Let Vercel Deploy Once)
1. In Vercel Dashboard, click **"Add New..."** → **"Project"**
2. Find your `heavens-above` repository
3. Click **"Import"**
4. On the "Configure Project" page:
   - **Framework Preset:** Leave as detected or select "Other"
   - **Root Directory:** Leave as `./` (default)
   - **Build Command:** Leave empty or use `npm install`
   - **Output Directory:** `public`
5. Click **"Deploy"** button
6. Wait for initial deployment to complete (1-2 minutes)
7. You'll see "Congratulations!" page with your deployment URL

**Note:** This initial deployment is necessary to create the project. GitHub Actions will handle future deployments.

### Step 1.3: Get Project ID and Org ID
1. In Vercel Dashboard, go to your project
2. Click **"Settings"** (top navigation)
3. Scroll down to **"General"** section
4. You'll see:
   - **Project Name:** heavens-above
   - **Project ID:** `prj_xxxxxxxxxxxxx` ← Copy this
5. Scroll further down to find:
   - **Organization ID:** `team_xxxxxxxxxxxxx` or `user_xxxxxxxxxxxxx` ← Copy this

**Alternative way to get IDs:**
1. Go to project Settings → General
2. Look at the URL: `https://vercel.com/[org-name]/[project-name]/settings`
3. Or check the `.vercel/project.json` file if you've linked locally

### Step 1.4: Create Vercel Token
1. Click your **profile picture** (bottom left)
2. Click **"Settings"**
3. In left sidebar, click **"Tokens"**
4. Click **"Create Token"**
5. Give it a name: `GitHub Actions`
6. Set scope: **Full Account** (or select specific projects)
7. Set expiration: **No Expiration** (or choose duration)
8. Click **"Create Token"**
9. **COPY THE TOKEN IMMEDIATELY** (you won't see it again!)
   - Format: `vercel_xxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

### Step 1.5: Configure Git Integration (Optional)
1. In Vercel project Settings
2. Go to **"Git"** section
3. You'll see your GitHub repository connected
4. **Optional:** You can keep auto-deploy enabled or disable it
   - **Keep enabled:** Both Vercel and GitHub Actions will deploy (redundant but harmless)
   - **Disable:** Only GitHub Actions will deploy
5. For this assignment, either option works fine

**Recommendation:** Keep it enabled for now. GitHub Actions deployment will override it anyway.

---

## Part 2: Configure GitHub Repository (5 minutes)

### Step 2.1: Add Secrets to GitHub
1. Go to your GitHub repository
2. Click **"Settings"** (repository settings, not your profile)
3. In left sidebar, click **"Secrets and variables"** → **"Actions"**
4. Click **"New repository secret"**

Add these 3 secrets one by one:

**Secret 1:**
- Name: `VERCEL_TOKEN`
- Value: `vercel_xxxxxxxxxxxxxxxxxxxxxxxxxxxxx` (paste your token)
- Click "Add secret"

**Secret 2:**
- Name: `VERCEL_ORG_ID`
- Value: `team_xxxxxxxxxxxxx` or `user_xxxxxxxxxxxxx` (paste your org ID)
- Click "Add secret"

**Secret 3:**
- Name: `VERCEL_PROJECT_ID`
- Value: `prj_xxxxxxxxxxxxx` (paste your project ID)
- Click "Add secret"

### Step 2.2: Enable GitHub Actions
1. Still in repository Settings
2. Click **"Actions"** → **"General"** (left sidebar)
3. Under "Actions permissions":
   - Select **"Allow all actions and reusable workflows"**
4. Under "Workflow permissions":
   - Select **"Read and write permissions"**
   - Check **"Allow GitHub Actions to create and approve pull requests"**
5. Click **"Save"**

### Step 2.3: Set Up Branch Protection (Optional but Recommended)
1. In Settings, click **"Branches"**
2. Click **"Add branch protection rule"**
3. Branch name pattern: `main`
4. Enable:
   - ✅ Require a pull request before merging
   - ✅ Require status checks to pass before merging
   - Search and select: `Test on Node.js` (from CI workflow)
5. Click **"Create"**

---

## Part 3: Push Workflows to GitHub (2 minutes)

### Step 3.1: Commit and Push All Files
```bash
# Navigate to your project
cd heavens-above

# Check status
git status

# Add all workflow files
git add .github/workflows/
git add *.md
git add .eslintrc.json .prettierrc jsdoc.json vercel.json

# Commit
git commit -m "feat: add GitHub Actions workflows with Vercel deployment"

# Push to main branch
git push origin main
```

### Step 3.2: Verify Workflows Are Uploaded
1. Go to your GitHub repository
2. Click **"Actions"** tab
3. You should see all 7 workflows listed:
   - Continuous Integration
   - Deployment Pipeline
   - Scheduled Tasks
   - Dependency Updates
   - Code Review Automation
   - Documentation Deployment
   - Custom Workflow - Release Management

---

## Part 4: Test Each Workflow (30 minutes)

### Test 1: Continuous Integration (ci.yml) ✅

**Method 1: Automatic trigger (already happened)**
- The push to main in Step 3.1 already triggered this!
- Go to Actions → Click "Continuous Integration" → View the run

**Method 2: Create a test branch**
```bash
# Create test branch
git checkout -b test-ci

# Make a small change
echo "// Test CI workflow" >> src/utils.js

# Commit and push
git add .
git commit -m "test: trigger CI workflow"
git push origin test-ci
```

**Expected Results:**
- ✅ Tests run on Node.js 16, 18, 20
- ✅ Linting checks complete
- ✅ Security audit runs
- ✅ All jobs show green checkmarks

**Where to check:**
- Actions tab → Continuous Integration → Latest run
- View logs for each job

---

### Test 2: Deployment Pipeline (deploy.yml) ✅

**Method 1: Automatic (already triggered by push to main)**
- Check Actions → Deployment Pipeline

**Method 2: Manual deployment**
1. Go to **Actions** tab
2. Click **"Deployment Pipeline"** (left sidebar)
3. Click **"Run workflow"** (right side)
4. Select branch: `main`
5. Select environment: `production`
6. Click **"Run workflow"** (green button)

**Expected Results:**
- ✅ Build job completes
- ✅ Test job passes
- ✅ Deploy to Vercel Production runs
- ✅ Deployment URL shown in logs
- ✅ Artifact "build-artifact" created
- ✅ Artifact "deployment-log" created

**Where to check:**
1. Actions → Deployment Pipeline → Latest run
2. Click "Deploy to Vercel Production" job
3. Look for line: `✅ Deployed to production: https://your-project.vercel.app`
4. Copy URL and open in browser
5. Download artifacts: Scroll to bottom → "Artifacts" section

**Test Preview Deployment:**
1. Run workflow again
2. Select environment: `preview`
3. Get preview URL from logs

---

### Test 3: Scheduled Tasks (scheduled-tasks.yml) ✅

**Note:** This runs automatically daily/weekly, but we'll test manually

**Manual Test:**
1. Go to **Actions** tab
2. Click **"Scheduled Tasks"** (left sidebar)
3. Click **"Run workflow"**
4. Select task type:
   - Try `data-refresh` first
5. Click **"Run workflow"**

**Expected Results:**
- ✅ Data refresh job runs
- ✅ Application executes
- ✅ Artifact "satellite-data-XXX" created

**Test Other Tasks:**
Repeat for:
- `backup` - Creates backup archive
- `cleanup` - Cleans old files
- `maintenance` - Runs maintenance tasks

**Where to check:**
- Actions → Scheduled Tasks → Each run
- Download artifacts to verify backups

---

### Test 4: Dependency Updates (dependency-updates.yml) ✅

**Manual Test:**
1. Go to **Actions** tab
2. Click **"Dependency Updates"**
3. Click **"Run workflow"**
4. Click **"Run workflow"** (green button)

**Expected Results:**
- ✅ Check updates job completes
- ✅ Outdated packages report generated
- ✅ If updates available: PR created automatically
- ✅ If vulnerabilities found: Security PR created
- ✅ Artifacts created

**Where to check:**
1. Actions → Dependency Updates → Latest run
2. Check "Pull requests" tab for new PRs:
   - "🔄 Automated Dependency Updates"
   - "🔒 Security Updates - Fix Vulnerabilities"
3. Download artifact "outdated-packages-report"
4. Download artifact "security-audit-report"

---

### Test 5: Code Review (code-review.yml) ✅

**Create a Pull Request:**
```bash
# Use the test-ci branch from Test 1
git checkout test-ci

# Make another change
echo "function testFunction() { return true; }" >> src/utils.js

# Commit and push
git add .
git commit -m "feat: add test function for code review"
git push origin test-ci

# Create PR using GitHub CLI (or use GitHub web interface)
gh pr create --title "Test: Code Review Workflow" --body "Testing automated code review"

# Or create PR manually:
# Go to GitHub → Pull requests → New pull request
# base: main, compare: test-ci
# Click "Create pull request"
```

**Expected Results:**
- ✅ Code quality analysis runs
- ✅ Security scan completes
- ✅ Coding standards checked
- ✅ PR size label added (size/XS, size/S, etc.)
- ✅ Review checklist posted as comment
- ✅ Artifacts created

**Where to check:**
1. Go to your Pull Request
2. Check "Checks" tab - see all workflows running
3. Check "Conversation" tab - see automated comment with checklist
4. Check PR labels - see size label
5. Actions → Code Review Automation → Latest run
6. Download artifacts: code-quality-reports, security-scan-reports

---

### Test 6: Documentation Deployment (documentation.yml) ✅

**Method 1: Update documentation**
```bash
# Checkout main
git checkout main
git pull

# Update README
echo "\n## New Section\nThis is a test for documentation deployment." >> README.md

# Commit and push
git add README.md
git commit -m "docs: test documentation deployment"
git push origin main
```

**Method 2: Manual trigger**
1. Go to Actions → Documentation Deployment
2. Click "Run workflow"
3. Click "Run workflow"

**Expected Results:**
- ✅ Documentation builds
- ✅ API docs generated
- ✅ Deployed to GitHub Pages
- ✅ Artifact "documentation" created

**Where to check:**
1. Actions → Documentation Deployment → Latest run
2. After deployment, go to: Settings → Pages
3. Your docs URL: `https://[username].github.io/heavens-above/`
4. Download artifact "documentation"

**Note:** GitHub Pages may take 2-5 minutes to update

---

### Test 7: Custom Workflow - Release Management (custom-workflow.yml) ✅

**Method 1: Create a release tag**
```bash
# Make sure you're on main with latest changes
git checkout main
git pull

# Create and push a tag
git tag v1.0.0
git push origin v1.0.0
```

**Method 2: Manual trigger**
1. Go to Actions → Custom Workflow - Release Management
2. Click "Run workflow"
3. Select release type: `patch`
4. Enable "Generate changelog": ✅
5. Click "Run workflow"

**Expected Results:**
- ✅ Release notes generated
- ✅ Performance metrics collected
- ✅ GitHub Release created (if tag pushed)
- ✅ Release packages created (.tar.gz, .zip)
- ✅ Artifacts created

**Where to check:**
1. Actions → Custom Workflow → Latest run
2. Go to repository → **Releases** (right sidebar)
3. See new release "v1.0.0" with:
   - Release notes
   - Download packages
   - Metrics report
4. Download artifacts: release-notes, performance-metrics

---

## Part 5: Verify All Artifacts (10 minutes)

### Check All Generated Artifacts

1. Go to **Actions** tab
2. For each workflow run, scroll to bottom
3. See **"Artifacts"** section
4. Download and verify:

| Workflow | Artifact Name | What to Check |
|----------|---------------|---------------|
| Deploy | build-artifact | Contains build files |
| Deploy | deployment-log | Shows deployment details |
| Scheduled | satellite-data-XXX | Contains scraped data |
| Scheduled | weekly-backup-XXX | Backup archive |
| Dependencies | outdated-packages-report | JSON with outdated packages |
| Dependencies | security-audit-report | Security vulnerabilities |
| Code Review | code-quality-reports | ESLint results |
| Code Review | security-scan-reports | Security scan results |
| Documentation | documentation | HTML docs |
| Custom | release-notes | Release notes markdown |
| Custom | performance-metrics | JSON metrics |

---

## Part 6: Collect Evidence for Assignment (15 minutes)

### Take Screenshots

**Screenshot 1: Actions Overview**
- Go to Actions tab
- Show all 7 workflows listed
- Show recent runs with green checkmarks

**Screenshot 2: Successful CI Run**
- Click Continuous Integration
- Show all jobs completed successfully
- Show test matrix (Node 16, 18, 20)

**Screenshot 3: Successful Deployment**
- Click Deployment Pipeline
- Show deployment job logs
- Highlight Vercel deployment URL

**Screenshot 4: Pull Request with Code Review**
- Show PR with automated checks
- Show review checklist comment
- Show PR size label

**Screenshot 5: Artifacts**
- Show artifacts section from any workflow
- Show multiple artifacts with retention days

**Screenshot 6: GitHub Release**
- Show Releases page
- Show release v1.0.0 with packages

**Screenshot 7: Vercel Dashboard**
- Show successful deployment in Vercel
- Show production URL

### Export Workflow Logs

For each workflow:
1. Click on the workflow run
2. Click "..." (top right)
3. Click "Download log archive"
4. Save as: `[workflow-name]-logs.zip`

### Create Summary Document

Create a document with:
- Screenshots of all 7 workflows running
- Links to your GitHub repository
- Link to deployed Vercel site
- List of all artifacts generated
- Any issues encountered and how you solved them

---

## Part 7: Assignment Submission Checklist

### Files to Submit/Show

✅ **GitHub Repository Link**
- Your repository URL with all workflows

✅ **Workflow Files** (7 files in `.github/workflows/`)
- ci.yml
- deploy.yml
- scheduled-tasks.yml
- dependency-updates.yml
- code-review.yml
- documentation.yml
- custom-workflow.yml

✅ **Documentation Files**
- WORKFLOWS_DOCUMENTATION.md
- TESTING_GUIDE.md
- DELIVERABLES_SUMMARY.md
- SETUP_INSTRUCTIONS.md

✅ **Configuration Files**
- .eslintrc.json
- .prettierrc
- jsdoc.json
- vercel.json

✅ **Evidence of Testing**
- Screenshots of all workflows running
- Downloaded artifacts
- Workflow logs
- Link to deployed site on Vercel
- Link to GitHub Pages documentation
- Link to created GitHub Release

✅ **Deployment URLs**
- Vercel Production: `https://your-project.vercel.app`
- GitHub Pages: `https://[username].github.io/heavens-above/`

---

## Troubleshooting Common Issues

### Issue 1: Vercel Deployment Fails

**Error:** "Missing required secrets"
**Solution:**
1. Verify secrets are added correctly in GitHub
2. Names must be exact: `VERCEL_TOKEN`, `VERCEL_ORG_ID`, `VERCEL_PROJECT_ID`
3. No extra spaces in secret values
4. Re-create secrets if needed

**Error:** "Project not found"
**Solution:**
1. Verify VERCEL_PROJECT_ID is correct
2. Check project exists in Vercel dashboard
3. Ensure Vercel token has access to the project

### Issue 2: CI Tests Fail

**Error:** "npm ci failed"
**Solution:**
1. Delete `package-lock.json`
2. Run `npm install` locally
3. Commit new `package-lock.json`
4. Push again

**Error:** "Node version not supported"
**Solution:**
- Update `package.json` engines field
- Or modify workflow to use compatible Node version

### Issue 3: Workflows Not Triggering

**Problem:** Pushed to main but no workflow runs
**Solution:**
1. Check Actions are enabled (Settings → Actions)
2. Verify workflow files are in `.github/workflows/`
3. Check YAML syntax (no errors)
4. Check branch name matches trigger (main vs master)

### Issue 4: Secrets Not Working

**Problem:** Workflow can't access secrets
**Solution:**
1. Secrets must be at repository level (not organization)
2. Check secret names are exact (case-sensitive)
3. Verify workflow has correct permissions
4. Re-create secrets

### Issue 5: GitHub Pages Not Deploying

**Problem:** Documentation workflow succeeds but no site
**Solution:**
1. Go to Settings → Pages
2. Source: Select "GitHub Actions"
3. Wait 2-5 minutes for deployment
4. Check workflow logs for errors

---

## Quick Command Reference

```bash
# Clone repository
git clone https://github.com/[username]/heavens-above.git
cd heavens-above

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

# Create PR
gh pr create --title "Test" --body "Testing"

# View workflow runs
gh run list

# Watch workflow
gh run watch

# Download artifacts
gh run download <run-id>
```

---

## Summary

You now have:
- ✅ 7 fully functional GitHub Actions workflows
- ✅ Automated deployment to Vercel
- ✅ Comprehensive documentation
- ✅ All workflows tested
- ✅ Artifacts generated
- ✅ Evidence for assignment submission

**Total Time:** ~1-2 hours for complete setup and testing

**Next Steps:**
1. Verify all workflows have run successfully
2. Collect screenshots and logs
3. Test your Vercel deployment URL
4. Prepare assignment submission document
5. Submit!

Good luck! 🚀
