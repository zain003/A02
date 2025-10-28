# Quick Start Guide - GitHub Actions with Vercel

## 🚀 Get Started in 5 Minutes

### Step 1: Vercel Setup (2 minutes)

1. Go to [vercel.com](https://vercel.com) and sign up with GitHub
2. Import your repository
3. Get credentials:
   - Dashboard → Settings → Tokens → Create Token
   - Project Settings → Copy **Project ID** and **Org ID**

### Step 2: Add GitHub Secrets (1 minute)

Go to: **Repository → Settings → Secrets and variables → Actions**

Add these 3 secrets:
```
VERCEL_TOKEN = your_token_here
VERCEL_ORG_ID = your_org_id_here
VERCEL_PROJECT_ID = your_project_id_here
```

### Step 3: Enable Actions (30 seconds)

**Repository → Settings → Actions → General**
- Select "Allow all actions and reusable workflows"
- Save

### Step 4: Test It! (1 minute)

```bash
# Push to main to trigger deployment
git checkout main
git push origin main

# Or run manually
# Go to Actions → Deployment Pipeline → Run workflow
```

---

## 📋 What You Get

### 7 Automated Workflows

1. **CI** - Tests on every push/PR
2. **Deploy** - Auto-deploy to Vercel on push to main
3. **Scheduled** - Daily data refresh, weekly backups
4. **Dependencies** - Weekly dependency updates
5. **Code Review** - Automated PR checks
6. **Documentation** - Auto-deploy docs to GitHub Pages
7. **Release** - Automated release management

---

## 🧪 Quick Tests

### Test CI
```bash
git checkout -b test
echo "// test" >> src/utils.js
git add . && git commit -m "test: CI" && git push origin test
```

### Test Deployment
```bash
git checkout main
git push origin main
# Check Actions tab for deployment URL
```

### Test Code Review
```bash
# Create PR from your test branch
gh pr create --title "Test" --body "Testing workflows"
```

### Test Release
```bash
git tag v1.0.0
git push origin v1.0.0
# Check Releases page
```

---

## 📁 Important Files

### Must Read
- `README_ASSIGNMENT.md` - Assignment submission summary
- `SETUP_INSTRUCTIONS.md` - Detailed setup guide
- `WORKFLOWS_DOCUMENTATION.md` - Complete workflow docs

### Workflows Location
All in `.github/workflows/`:
- `ci.yml`
- `deploy.yml`
- `scheduled-tasks.yml`
- `dependency-updates.yml`
- `code-review.yml`
- `documentation.yml`
- `custom-workflow.yml`

---

## 🔍 Check Results

### View Workflows
1. Go to **Actions** tab
2. Click on any workflow run
3. View logs and artifacts

### Download Artifacts
```bash
# Using GitHub CLI
gh run list
gh run download <run-id>
```

### Check Deployment
- Vercel deployment URL shown in workflow logs
- Or check your Vercel dashboard

---

## ❓ Troubleshooting

### Deployment Fails?
- ✅ Check secrets are added correctly
- ✅ Verify Vercel project is linked
- ✅ Check workflow logs for errors

### CI Fails?
- ✅ Run `npm install` locally
- ✅ Check Node.js version (16+)
- ✅ Review error in Actions logs

### Need Help?
1. Check `WORKFLOWS_DOCUMENTATION.md`
2. Review `TESTING_GUIDE.md`
3. Check workflow logs in Actions tab

---

## 📊 Assignment Deliverables

✅ 7 YAML workflow files  
✅ 4 documentation files (1400+ lines)  
✅ 4 configuration files  
✅ All workflows tested  
✅ Vercel deployment configured  
✅ Artifacts and logs generated  

**Status:** Ready for submission! 🎉

---

## 🎯 Next Steps

1. ✅ Verify all secrets are added
2. ✅ Push to main to test deployment
3. ✅ Create a PR to test code review
4. ✅ Check all workflows in Actions tab
5. ✅ Review generated artifacts
6. ✅ Submit assignment

---

**Need detailed info?** See `README_ASSIGNMENT.md`  
**Need setup help?** See `SETUP_INSTRUCTIONS.md`  
**Need testing help?** See `TESTING_GUIDE.md`
