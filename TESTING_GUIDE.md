# Testing Guide for GitHub Actions Workflows

## Overview
This guide provides instructions for testing and validating the GitHub Actions workflows.

## Prerequisites
- GitHub repository with workflows configured
- Appropriate permissions (write access)
- GitHub CLI (optional, for advanced testing)

---

## Testing Individual Workflows

### 1. Continuous Integration (ci.yml)

#### Test Method 1: Push to Branch
```bash
# Create a test branch
git checkout -b test-ci

# Make a small change
echo "// Test comment" >> src/utils.js

# Commit and push
git add .
git commit -m "test: trigger CI workflow"
git push origin test-ci
```

#### Test Method 2: Create Pull Request
1. Push changes to a branch
2. Create PR to main/master
3. Observe CI checks running

#### Expected Results
- ✅ Tests run on multiple Node.js versions
- ✅ Linting checks complete
- ✅ Security audit runs
- ✅ Build status aggregates results

#### Artifacts Generated
- None (status checks only)

---

### 2. Deployment Pipeline (deploy.yml)

#### Test Method 1: Manual Dispatch
1. Go to Actions → Deployment Pipeline
2. Click "Run workflow"
3. Select environment (staging/production)
4. Click "Run workflow"

#### Test Method 2: Push to Main
```bash
git checkout main
git pull
# Make changes
git add .
git commit -m "feat: trigger deployment"
git push origin main
```

#### Expected Results
- ✅ Build artifact created
- ✅ Tests pass
- ✅ Deployment executes
- ✅ Health checks run

#### Artifacts Generated
- `build-artifact` (7-day retention)

#### Verification
Check deployment logs for:
- Build completion message
- Test results
- Deployment status
- Health check results

---

### 3. Scheduled Tasks (scheduled-tasks.yml)

#### Test Method: Manual Dispatch
1. Go to Actions → Scheduled Tasks
2. Click "Run workflow"
3. Select task type:
   - data-refresh
   - backup
   - cleanup
   - maintenance
4. Click "Run workflow"

#### Expected Results by Task

**Data Refresh:**
- ✅ Scraping script executes
- ✅ Data uploaded as artifact
- ✅ Optional: Changes committed

**Backup:**
- ✅ Backup archive created
- ✅ Uploaded to artifacts
- ✅ Backup notification sent

**Cleanup:**
- ✅ Old files removed
- ✅ Cache cleaned

**Maintenance:**
- ✅ Dependencies checked
- ✅ Health report generated

#### Artifacts Generated
- `satellite-data-{run_number}` (30 days)
- `weekly-backup-{run_number}` (90 days)

#### Verification
```bash
# Download and extract artifact
gh run download <run-id> -n satellite-data-123
```

---

### 4. Dependency Updates (dependency-updates.yml)

#### Test Method 1: Manual Trigger
1. Go to Actions → Dependency Updates
2. Click "Run workflow"
3. Click "Run workflow"

#### Test Method 2: Wait for Schedule
- Runs automatically every Monday at 9:00 AM UTC

#### Expected Results
- ✅ Outdated packages report generated
- ✅ Pull request created (if updates available)
- ✅ Security audit runs
- ✅ Security PR created (if vulnerabilities found)

#### Artifacts Generated
- `outdated-packages-report` (30 days)
- `security-audit-report` (90 days)

#### Verification
1. Check for new PRs:
   - "🔄 Automated Dependency Updates"
   - "🔒 Security Updates - Fix Vulnerabilities"
2. Review PR description
3. Check test results in PR

---

### 5. Code Review (code-review.yml)

#### Test Method: Create Pull Request
```bash
# Create feature branch
git checkout -b feature/test-code-review

# Make changes
echo "function test() { return true; }" >> src/utils.js

# Commit and push
git add .
git commit -m "feat: add test function"
git push origin feature/test-code-review

# Create PR via GitHub UI or CLI
gh pr create --title "Test Code Review" --body "Testing code review workflow"
```

#### Expected Results
- ✅ Code quality analysis runs
- ✅ Security scan completes
- ✅ Coding standards checked
- ✅ PR size label added
- ✅ Review checklist posted

#### Artifacts Generated
- `code-quality-reports` (30 days)
- `security-scan-reports` (90 days)

#### Verification
1. Check PR comments for review checklist
2. Verify PR labels (size/*)
3. Download and review quality reports
4. Check security scan results

---

### 6. Documentation Deployment (documentation.yml)

#### Test Method 1: Update Documentation
```bash
# Update README
echo "\n## New Section\nTest content" >> README.md

# Commit and push
git add README.md
git commit -m "docs: update README"
git push origin main
```

#### Test Method 2: Manual Dispatch
1. Go to Actions → Documentation Deployment
2. Click "Run workflow"
3. Click "Run workflow"

#### Expected Results
- ✅ Documentation built
- ✅ API docs generated
- ✅ Deployed to GitHub Pages
- ✅ Validation checks pass

#### Artifacts Generated
- `documentation` (30 days)

#### Verification
1. Visit GitHub Pages URL
2. Check API documentation
3. Verify links work
4. Test responsive design

---

### 7. Custom Workflow (custom-workflow.yml)

#### Test Method 1: Create Release Tag
```bash
# Create and push tag
git tag v1.0.0
git push origin v1.0.0
```

#### Test Method 2: Manual Dispatch
1. Go to Actions → Custom Workflow
2. Click "Run workflow"
3. Select release type (major/minor/patch)
4. Enable/disable changelog
5. Click "Run workflow"

#### Expected Results
- ✅ Release notes generated
- ✅ Performance metrics collected
- ✅ GitHub release created
- ✅ Release packages attached
- ✅ Notifications sent

#### Artifacts Generated
- `release-notes` (30 days)
- `performance-metrics` (90 days)
- Release packages (attached to release)

#### Verification
1. Check Releases page
2. Download release packages
3. Review release notes
4. Check metrics report

---

## Comprehensive Testing Checklist

### Pre-Testing Setup
- [ ] Fork or clone repository
- [ ] Configure repository secrets
- [ ] Enable GitHub Actions
- [ ] Set up branch protection rules
- [ ] Configure GitHub Pages (if needed)

### Workflow Testing
- [ ] Test CI workflow with push
- [ ] Test CI workflow with PR
- [ ] Test deployment to staging
- [ ] Test deployment to production
- [ ] Test scheduled task (manual)
- [ ] Test dependency updates
- [ ] Test code review on PR
- [ ] Test documentation deployment
- [ ] Test custom workflow with tag
- [ ] Test custom workflow manual dispatch

### Verification Steps
- [ ] All workflows complete successfully
- [ ] Artifacts are generated correctly
- [ ] Pull requests are created properly
- [ ] Deployments work as expected
- [ ] Notifications are sent
- [ ] Documentation is accessible
- [ ] Releases are created correctly

---

## Monitoring Workflows

### GitHub UI
1. Go to repository → Actions tab
2. View workflow runs
3. Click on specific run for details
4. Check job logs
5. Download artifacts

### GitHub CLI
```bash
# List workflow runs
gh run list

# View specific run
gh run view <run-id>

# Watch run in real-time
gh run watch <run-id>

# Download artifacts
gh run download <run-id>
```

---

## Troubleshooting Tests

### Workflow Not Starting
1. Check workflow file syntax
2. Verify trigger conditions
3. Check branch names
4. Review repository permissions

### Workflow Failing
1. Review job logs
2. Check error messages
3. Verify secrets are set
4. Test locally if possible

### Artifacts Not Generated
1. Check upload-artifact step
2. Verify file paths
3. Check retention settings
4. Review permissions

---

## Test Reports Location

### CI Test Reports
- Location: Workflow run logs
- Format: Console output
- Retention: 90 days (GitHub default)

### Deployment Logs
- Location: Workflow run logs
- Artifacts: build-artifact
- Retention: 7 days

### Code Quality Reports
- Artifacts: code-quality-reports
- Files: eslint-report.json, complexity-report.json
- Retention: 30 days

### Security Reports
- Artifacts: security-scan-reports
- Files: security-audit.json
- Retention: 90 days

### Documentation
- Location: GitHub Pages
- Artifacts: documentation
- Retention: 30 days

---

## Performance Testing

### Workflow Execution Times
Monitor and optimize:
- CI: Target < 5 minutes
- Deployment: Target < 10 minutes
- Code Review: Target < 3 minutes
- Documentation: Target < 5 minutes

### Optimization Tips
1. Use caching for dependencies
2. Parallelize independent jobs
3. Set appropriate timeouts
4. Use matrix strategies efficiently

---

## Continuous Monitoring

### Weekly Checks
- [ ] Review failed workflow runs
- [ ] Check artifact storage usage
- [ ] Monitor workflow execution times
- [ ] Review security scan results

### Monthly Checks
- [ ] Update action versions
- [ ] Review and optimize workflows
- [ ] Check secret expiration
- [ ] Audit workflow permissions

### Quarterly Checks
- [ ] Comprehensive workflow review
- [ ] Update documentation
- [ ] Review retention policies
- [ ] Optimize costs

---

## Success Criteria

A workflow test is successful when:
1. ✅ Workflow completes without errors
2. ✅ All jobs pass (or expected failures)
3. ✅ Artifacts are generated correctly
4. ✅ Notifications are sent (if configured)
5. ✅ Expected side effects occur (PRs, deployments, etc.)

---

## Getting Help

### Resources
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax Reference](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [GitHub Community Forum](https://github.community/)

### Support Channels
- GitHub Issues (for workflow problems)
- Team documentation
- Internal support channels
