# Vercel Setup Guide - Detailed Instructions with Screenshots Guide

## 🎯 Goal
Get 3 credentials from Vercel to use in GitHub Actions:
1. `VERCEL_TOKEN`
2. `VERCEL_ORG_ID`
3. `VERCEL_PROJECT_ID`

---

## Part 1: Create Vercel Account (3 minutes)

### Step 1: Sign Up
1. Open browser and go to: **https://vercel.com**
2. Click **"Sign Up"** button (top right)
3. Click **"Continue with GitHub"**
4. Authorize Vercel to access your GitHub account
5. Complete any additional signup steps

**✅ You now have a Vercel account!**

---

## Part 2: Import Project WITHOUT Deploying (5 minutes)

### Step 2: Import Repository

1. In Vercel Dashboard, click **"Add New..."** button (top right)
2. Select **"Project"** from dropdown
3. You'll see "Import Git Repository" page
4. Find your **"heavens-above"** repository in the list
   - If not visible, click "Adjust GitHub App Permissions"
   - Grant access to the repository
5. Click **"Import"** button next to your repository

### Step 3: Deploy the Project (Initial Setup)

**Updated approach - Let Vercel deploy once:**

When you see the "Configure Project" page:
1. **Framework Preset:** Leave as detected or select "Other"
2. **Root Directory:** Leave as `./` (default)
3. **Build Command:** Leave empty or use `npm install`
4. **Output Directory:** Enter `public`
5. Click **"Deploy"** button
6. Wait for deployment to complete (1-2 minutes)
7. You'll see "Congratulations!" with your live URL

**Why deploy now?**
- Vercel requires an initial deployment to create the project
- This gives you the Project ID we need
- GitHub Actions will handle all future deployments
- The initial deployment won't interfere with GitHub Actions

**✅ Your project is now set up in Vercel!**

---

## Part 3: Get Project ID and Org ID (3 minutes)

### Step 4: Navigate to Project Settings

1. In Vercel Dashboard, you should see your project listed
2. Click on the **"heavens-above"** project
3. Click **"Settings"** tab (top navigation bar)

### Step 5: Get Project ID

1. In Settings, you're on the **"General"** page by default
2. Scroll down until you see a section with project information
3. Look for **"Project ID"**
   - It looks like: `prj_xxxxxxxxxxxxxxxxxxxxx`
   - Example: `prj_abc123def456ghi789`
4. Click the **copy icon** next to it
5. **Paste it somewhere safe** (Notepad, Notes app, etc.)

**Label it:** `VERCEL_PROJECT_ID = prj_xxxxxxxxxxxxxxxxxxxxx`

### Step 6: Get Organization ID

1. Still in Settings → General
2. Scroll down further to find **"Organization"** or **"Team"** section
3. Look for **"Organization ID"** or **"Team ID"**
   - Personal account: `user_xxxxxxxxxxxxxxxxxxxxx`
   - Team account: `team_xxxxxxxxxxxxxxxxxxxxx`
   - Example: `team_abc123def456ghi789`
4. Click the **copy icon** next to it
5. **Paste it somewhere safe**

**Label it:** `VERCEL_ORG_ID = team_xxxxxxxxxxxxxxxxxxxxx`

**Can't find it?** Alternative method:
1. Click your profile picture (bottom left)
2. Click **"Settings"**
3. Look for your organization/team ID there

---

## Part 4: Create Vercel Token (3 minutes)

### Step 7: Navigate to Tokens Page

1. Click your **profile picture** (bottom left corner)
2. Click **"Settings"**
3. In the left sidebar, find and click **"Tokens"**
   - Full path: Settings → Tokens

### Step 8: Create New Token

1. Click **"Create Token"** button
2. Fill in the form:
   - **Token Name:** `GitHub Actions` (or any name you prefer)
   - **Scope:** Select **"Full Account"**
     - Or select specific projects if you prefer
   - **Expiration:** 
     - For assignment: Select **"No Expiration"**
     - For production: Choose appropriate duration
3. Click **"Create Token"** button

### Step 9: Copy Token (IMPORTANT!)

**⚠️ CRITICAL: You can only see this token ONCE!**

1. A token will appear: `vercel_xxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
2. **IMMEDIATELY copy it** (click copy button)
3. **Paste it somewhere safe** (Notepad, Notes app, etc.)
4. **DO NOT close the page** until you've saved it

**Label it:** `VERCEL_TOKEN = vercel_xxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

**If you lose it:** You'll need to create a new token (delete old one first)

---

## Part 5: Understand Deployment Behavior (1 minute)

### Step 10: How Deployments Work Now

**Current Setup:**
- ✅ Vercel is connected to your GitHub repository
- ✅ Vercel may auto-deploy on push (this is fine!)
- ✅ GitHub Actions will ALSO deploy (this is what we want!)

**What happens when you push to main:**
1. Vercel detects the push and may deploy automatically
2. GitHub Actions workflow also triggers and deploys
3. Both deployments succeed
4. The GitHub Actions deployment is what counts for your assignment

**For the assignment:**
- Both deployments working = ✅ Perfect!
- Only GitHub Actions deploying = ✅ Also perfect!
- The important part is that GitHub Actions workflow runs successfully

**Optional - Disable Vercel Auto-Deploy:**
If you want ONLY GitHub Actions to deploy:
1. Go to project Settings → Git
2. Find "Production Branch" or "Deploy Hooks"
3. Disable automatic deployments
4. This is optional and not required for the assignment

---

## Part 6: Add Secrets to GitHub (5 minutes)

### Step 11: Navigate to GitHub Repository Settings

1. Open your GitHub repository in browser
2. Click **"Settings"** tab (top navigation)
   - Make sure you're in repository settings, not your profile
3. In left sidebar, click **"Secrets and variables"**
4. Click **"Actions"**

### Step 12: Add VERCEL_TOKEN

1. Click **"New repository secret"** button (green button)
2. Fill in:
   - **Name:** `VERCEL_TOKEN` (must be exact, all caps)
   - **Secret:** Paste your token: `vercel_xxxxxxxxxxxxxxxxxxxxxxxxxxxxx`
3. Click **"Add secret"**

**✅ Secret 1 added!**

### Step 13: Add VERCEL_ORG_ID

1. Click **"New repository secret"** again
2. Fill in:
   - **Name:** `VERCEL_ORG_ID` (must be exact, all caps)
   - **Secret:** Paste your org ID: `team_xxxxxxxxxxxxxxxxxxxxx`
3. Click **"Add secret"**

**✅ Secret 2 added!**

### Step 14: Add VERCEL_PROJECT_ID

1. Click **"New repository secret"** again
2. Fill in:
   - **Name:** `VERCEL_PROJECT_ID` (must be exact, all caps)
   - **Secret:** Paste your project ID: `prj_xxxxxxxxxxxxxxxxxxxxx`
3. Click **"Add secret"**

**✅ Secret 3 added!**

### Step 15: Verify Secrets

You should now see 3 secrets listed:
- `VERCEL_TOKEN`
- `VERCEL_ORG_ID`
- `VERCEL_PROJECT_ID`

**Note:** You can't view secret values after adding them (security feature)

---

## Part 7: Test the Setup (5 minutes)

### Step 16: Trigger Deployment Workflow

**Method 1: Push to main**
```bash
git checkout main
git push origin main
```

**Method 2: Manual trigger**
1. Go to repository → **Actions** tab
2. Click **"Deployment Pipeline"** (left sidebar)
3. Click **"Run workflow"** button (right side)
4. Select branch: `main`
5. Select environment: `production`
6. Click **"Run workflow"** (green button)

### Step 17: Monitor Deployment

1. Stay on Actions tab
2. You'll see a new workflow run appear
3. Click on it to see details
4. Watch the jobs run:
   - Build Application
   - Run Tests
   - Deploy to Vercel Production

### Step 18: Get Deployment URL

1. Click on **"Deploy to Vercel Production"** job
2. Expand the logs
3. Look for line that says:
   ```
   ✅ Deployed to production: https://heavens-above-xxxxx.vercel.app
   ```
4. **Copy this URL**
5. Open it in a new browser tab
6. **Verify your site is live!**

---

## Troubleshooting

### Problem 1: Can't Find Project ID or Org ID

**Solution:**
1. Go to project Settings → General
2. Scroll through entire page
3. Look for any section with IDs
4. Or check URL: `https://vercel.com/[org-name]/[project-name]`
5. Or use Vercel CLI:
   ```bash
   npm i -g vercel
   vercel login
   vercel link
   # Check .vercel/project.json
   ```

### Problem 2: Token Not Working

**Symptoms:** Deployment fails with "Unauthorized" or "Invalid token"

**Solution:**
1. Verify token was copied correctly (no extra spaces)
2. Check token hasn't expired
3. Verify token has correct permissions (Full Account)
4. Create a new token:
   - Go to Vercel Settings → Tokens
   - Delete old token
   - Create new token
   - Update GitHub secret

### Problem 3: Project Not Found

**Symptoms:** "Project not found" error in workflow

**Solution:**
1. Verify VERCEL_PROJECT_ID is correct
2. Check project exists in Vercel dashboard
3. Ensure token has access to the project
4. Try re-importing project in Vercel

### Problem 4: Secrets Not Working in GitHub

**Symptoms:** Workflow can't access secrets

**Solution:**
1. Verify secrets are at **repository level** (not organization)
2. Check secret names are **exact** (case-sensitive):
   - `VERCEL_TOKEN` (not `vercel_token`)
   - `VERCEL_ORG_ID` (not `VERCEL_ORG_id`)
   - `VERCEL_PROJECT_ID` (not `VERCEL_PROJECT_id`)
3. No extra spaces in secret values
4. Delete and re-create secrets if needed

### Problem 5: Deployment Succeeds but Site Not Working

**Symptoms:** Workflow succeeds but URL shows error

**Solution:**
1. Check Vercel dashboard for deployment status
2. Verify `vercel.json` configuration is correct
3. Check if `public` folder exists with content
4. Look at Vercel deployment logs in Vercel dashboard
5. May need to adjust `vercel.json` routes

---

## Quick Reference Card

### Vercel Credentials Checklist

```
□ VERCEL_TOKEN
  Format: vercel_xxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  Get from: Vercel → Settings → Tokens → Create Token
  
□ VERCEL_ORG_ID
  Format: team_xxxxxxxxxxxxxxxxxxxxx or user_xxxxxxxxxxxxxxxxxxxxx
  Get from: Vercel → Project Settings → General → Organization ID
  
□ VERCEL_PROJECT_ID
  Format: prj_xxxxxxxxxxxxxxxxxxxxx
  Get from: Vercel → Project Settings → General → Project ID
```

### GitHub Secrets Checklist

```
□ Go to: Repository → Settings → Secrets and variables → Actions
□ Add: VERCEL_TOKEN (exact name, all caps)
□ Add: VERCEL_ORG_ID (exact name, all caps)
□ Add: VERCEL_PROJECT_ID (exact name, all caps)
□ Verify: All 3 secrets listed
```

### Test Deployment Checklist

```
□ Push to main branch OR run workflow manually
□ Go to Actions tab
□ Click on Deployment Pipeline run
□ Wait for "Deploy to Vercel Production" job
□ Copy deployment URL from logs
□ Open URL in browser
□ Verify site is live
```

---

## Visual Guide Summary

### Where to Find Each Credential:

**VERCEL_TOKEN:**
```
Vercel Dashboard
  └─ Click Profile Picture (bottom left)
      └─ Settings
          └─ Tokens (left sidebar)
              └─ Create Token
                  └─ Copy token (only shown once!)
```

**VERCEL_PROJECT_ID:**
```
Vercel Dashboard
  └─ Click Project (heavens-above)
      └─ Settings (top nav)
          └─ General (default page)
              └─ Scroll down
                  └─ Project ID: prj_xxxxx
                      └─ Click copy icon
```

**VERCEL_ORG_ID:**
```
Vercel Dashboard
  └─ Click Project (heavens-above)
      └─ Settings (top nav)
          └─ General (default page)
              └─ Scroll down
                  └─ Organization ID: team_xxxxx or user_xxxxx
                      └─ Click copy icon
```

---

## Success Indicators

You've successfully set up Vercel when:

- ✅ You have all 3 credentials saved
- ✅ All 3 secrets added to GitHub
- ✅ Deployment workflow runs without errors
- ✅ You get a Vercel URL in workflow logs
- ✅ Your site is accessible at the Vercel URL
- ✅ Site shows your project content

**You're ready to test all workflows!** 🚀

---

## Next Steps

After Vercel setup is complete:

1. ✅ Test all 7 workflows (see COMPLETE_STEP_BY_STEP_GUIDE.md)
2. ✅ Collect screenshots and evidence
3. ✅ Prepare assignment submission
4. ✅ Submit!

**Estimated Total Time:** 20-30 minutes for complete Vercel setup
