# Deployment Checklist - GitHub Actions Setup

Use this checklist to ensure everything is properly configured before deploying to GitHub Actions.

## Pre-Deployment

### ✅ Local Testing

- [ ] Python 3.11+ is installed
- [ ] Install dependencies: `pip install -r requirements.txt`
- [ ] Test the script locally: `python dolkana.py --help`
- [ ] Add test URLs to `multiple_primesrc.txt`
- [ ] Run script locally: `python dolkana.py` (verify it works)
- [ ] Check output files are generated:
  - [ ] `api_url_list.txt`
  - [ ] `final_stream_urls.txt`
  - [ ] `pipeline_summary.json`
  - [ ] `pipeline_summary.gz.json`

### ✅ File Verification

- [ ] All files present:
  - [ ] `dolkana.py` (updated script)
  - [ ] `multiple_primesrc.txt` (input file)
  - [ ] `requirements.txt`
  - [ ] `.gitignore`
  - [ ] `.github/workflows/primesrc-pipeline.yml`
  - [ ] `.github/workflows/validate.yml`
  - [ ] `README.md`
  - [ ] `GITHUB_ACTIONS_SETUP.md`
  - [ ] `SETUP_SUMMARY.md`

- [ ] Workflow files are valid YAML (check at https://www.yamllint.com/)
- [ ] Python syntax is valid: `python -m py_compile dolkana.py`

### ✅ Configuration Review

- [ ] Review `.gitignore` - decide what to commit
- [ ] Review workflow schedule (default: daily at midnight UTC)
- [ ] Review workflow triggers (manual, schedule, push)
- [ ] Decide if you want auto-commit enabled (step in workflow)
- [ ] Set appropriate batch size (default: 5)
- [ ] Set appropriate timeouts (default: 45s per page)

## GitHub Repository Setup

### ✅ Create Repository

- [ ] Create new repository on GitHub (public or private)
- [ ] Note repository URL: `https://github.com/USERNAME/REPO-NAME`
- [ ] Initialize git locally (if not done):
  ```bash
  git init
  git add .
  git commit -m "Initial commit: PrimeSrc pipeline with GitHub Actions"
  ```

### ✅ Push to GitHub

- [ ] Add remote: `git remote add origin https://github.com/USERNAME/REPO-NAME.git`
- [ ] Set branch: `git branch -M main`
- [ ] Push: `git push -u origin main`
- [ ] Verify files appear on GitHub

### ✅ GitHub Settings

- [ ] **Actions** enabled (Settings → Actions → General → Allow all actions)
- [ ] **Workflows** have permissions (Settings → Actions → General → Workflow permissions):
  - [ ] Read and write permissions (if auto-commit enabled)
  - [ ] Allow GitHub Actions to create pull requests (optional)
- [ ] **Repository visibility** set correctly (public/private)

## Post-Deployment

### ✅ Verify Workflow

- [ ] Go to **Actions** tab on GitHub
- [ ] Verify "PrimeSrc Pipeline" workflow appears
- [ ] Verify "Validate" workflow appears
- [ ] Check workflow file syntax (GitHub shows errors if invalid)

### ✅ First Run (Test)

- [ ] Click on "PrimeSrc Pipeline" workflow
- [ ] Click "Run workflow"
- [ ] Leave all options at default
- [ ] Click green "Run workflow" button
- [ ] Wait for workflow to start (refresh page)

### ✅ Monitor First Run

- [ ] Click on the running workflow
- [ ] Watch logs in real-time
- [ ] Check each step completes successfully:
  - [ ] Checkout repository
  - [ ] Set up Python
  - [ ] Install Chrome
  - [ ] Verify Chrome installation
  - [ ] Install Python dependencies
  - [ ] Run PrimeSrc Pipeline
  - [ ] Upload artifacts

### ✅ Verify Results

- [ ] Workflow completed successfully (green checkmark)
- [ ] Scroll to "Artifacts" section
- [ ] Verify artifacts are present:
  - [ ] `api-url-list`
  - [ ] `final-stream-urls`
  - [ ] `pipeline-summary`
- [ ] Download and inspect artifacts
- [ ] Verify content is correct

### ✅ Check Auto-Commit (if enabled)

- [ ] New commit appears in repository
- [ ] Files updated:
  - [ ] `api_url_list.txt`
  - [ ] `final_stream_urls.txt`
  - [ ] `pipeline_summary.json`
  - [ ] `pipeline_summary.gz.json`
- [ ] Commit message: "Update PrimeSrc pipeline results [skip ci]"

## Troubleshooting

### If Workflow Fails

- [ ] Check workflow logs for error messages
- [ ] Identify which step failed
- [ ] Common issues:
  - [ ] **Syntax error**: Check YAML at yamllint.com
  - [ ] **Python error**: Test locally first
  - [ ] **Chrome not found**: Check "Install Chrome" step
  - [ ] **Timeout**: Reduce `--batch-size` or increase `--timeout`
  - [ ] **Permission denied**: Check workflow permissions in settings
  - [ ] **No artifacts**: Check if output files were created

### If No Artifacts

- [ ] Check "Run PrimeSrc Pipeline" step logs
- [ ] Verify `multiple_primesrc.txt` has valid URLs
- [ ] Check if Stage 1 completed
- [ ] Check if Stage 2 completed
- [ ] Look for Python errors or exceptions

### If Chrome Fails

- [ ] Check "Install Chrome" step completed
- [ ] Check "Verify Chrome installation" step
- [ ] Review Stage 2 logs for browser errors
- [ ] Consider reducing batch size: `--batch-size 2`

## Optimization

### ✅ Performance Tuning

- [ ] Monitor run time (typical: 10-20 minutes)
- [ ] Adjust batch size if needed:
  - Faster but less reliable: `--batch-size 10`
  - Slower but more reliable: `--batch-size 2`
- [ ] Adjust timeouts if needed:
  - Faster failure detection: `--timeout 30`
  - More patient: `--timeout 90`
- [ ] Consider number of URLs in `multiple_primesrc.txt`

### ✅ Schedule Optimization

- [ ] Default: Daily at midnight UTC
- [ ] Adjust if needed:
  - [ ] More frequent: `0 */6 * * *` (every 6 hours)
  - [ ] Less frequent: `0 0 * * 1` (weekly on Monday)
  - [ ] Specific times: `0 9,17 * * *` (9 AM and 5 PM daily)

### ✅ Cost Optimization (Private Repos)

- [ ] Monitor Actions usage: Settings → Billing
- [ ] Free tier: 500 minutes/month for private repos
- [ ] Typical run: 10-20 minutes
- [ ] Daily runs: ~300-600 minutes/month
- [ ] Optimize:
  - [ ] Reduce schedule frequency
  - [ ] Use manual triggers only
  - [ ] Optimize script performance

## Maintenance

### ✅ Regular Checks

- [ ] Weekly: Review workflow runs
- [ ] Weekly: Check artifact quality
- [ ] Monthly: Update dependencies
- [ ] Monthly: Review and update URLs
- [ ] Quarterly: Update Python version
- [ ] Yearly: Review and update workflows

### ✅ Updates

- [ ] Update `multiple_primesrc.txt` with new URLs
- [ ] Update workflow triggers as needed
- [ ] Update Python dependencies: `pip list --outdated`
- [ ] Update GitHub Actions: dependabot or manual
- [ ] Update documentation as needed

## Security

### ✅ Security Checklist

- [ ] **Repository visibility** appropriate for content
- [ ] **Sensitive data** not in `multiple_primesrc.txt`
- [ ] **API keys** stored as secrets (if any)
- [ ] **Branch protection** enabled for main (optional)
- [ ] **Dependabot alerts** enabled (Settings → Security)
- [ ] **Code scanning** enabled (Settings → Security, optional)

### ✅ Privacy

- [ ] Artifacts contain no sensitive information
- [ ] Logs contain no sensitive information
- [ ] Committed results contain no sensitive information
- [ ] URLs in `multiple_primesrc.txt` are safe to expose

## Documentation

### ✅ Documentation Complete

- [ ] `README.md` reviewed and accurate
- [ ] `GITHUB_ACTIONS_SETUP.md` reviewed
- [ ] `SETUP_SUMMARY.md` reviewed
- [ ] `.github/workflows/README.md` reviewed
- [ ] Repository has description and topics
- [ ] Add status badges (optional)

### ✅ Team Onboarding (if applicable)

- [ ] Share repository with team members
- [ ] Document custom configuration
- [ ] Provide access to artifacts
- [ ] Set up notifications
- [ ] Define roles and permissions

## Success Criteria

### ✅ Deployment Successful When:

- [ ] ✅ Workflow runs without errors
- [ ] ✅ All steps complete successfully
- [ ] ✅ Artifacts are uploaded
- [ ] ✅ Results contain valid URLs
- [ ] ✅ Schedule works as expected
- [ ] ✅ Manual trigger works
- [ ] ✅ Auto-commit works (if enabled)
- [ ] ✅ Team can access and use the system

## Rollback Plan

### If Deployment Fails:

1. [ ] Revert to local execution only
2. [ ] Fix identified issues locally
3. [ ] Test locally thoroughly
4. [ ] Update documentation with lessons learned
5. [ ] Re-deploy when ready

### Rollback Steps:

```bash
# Disable workflow
git mv .github/workflows/primesrc-pipeline.yml .github/workflows/primesrc-pipeline.yml.disabled
git commit -m "Temporarily disable workflow"
git push

# Fix issues...

# Re-enable workflow
git mv .github/workflows/primesrc-pipeline.yml.disabled .github/workflows/primesrc-pipeline.yml
git commit -m "Re-enable workflow"
git push
```

---

## Quick Reference

### Essential URLs

- Repository: `https://github.com/USERNAME/REPO-NAME`
- Actions: `https://github.com/USERNAME/REPO-NAME/actions`
- Settings: `https://github.com/USERNAME/REPO-NAME/settings`
- Billing: `https://github.com/settings/billing`

### Essential Commands

```bash
# Test locally
python dolkana.py

# Validate Python
python -m py_compile dolkana.py

# Install dependencies
pip install -r requirements.txt

# Check git status
git status

# Push changes
git add .
git commit -m "Update configuration"
git push
```

### Support Resources

- GitHub Actions Docs: https://docs.github.com/en/actions
- Workflow Syntax: https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions
- YAML Validator: https://www.yamllint.com/
- Cron Schedule: https://crontab.guru/

---

**Print this checklist and check off items as you complete them!**

✅ = Completed
❌ = Failed/Skipped
⚠️ = Needs attention
