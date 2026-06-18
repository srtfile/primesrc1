# 🚀 START HERE - PrimeSrc Pipeline on GitHub Actions

Welcome! This guide will get you up and running in **under 10 minutes**.

## 📋 What You Have

Your `dolkana.py` script is now ready for GitHub Actions! Here's what was set up:

✅ **GitHub Actions Workflow** - Runs automatically or on-demand
✅ **Cross-Platform Support** - Works on Windows, Linux, and macOS
✅ **Complete Documentation** - Step-by-step guides included
✅ **Automated Uploads** - Results saved as artifacts
✅ **Validation Workflow** - Automatic code quality checks

## 🎯 Choose Your Path

### Path A: Quick Start (GitHub Actions) ⚡
**Best for**: Running in the cloud, automation, scheduling
**Time**: 10 minutes

### Path B: Local Testing 🖥️
**Best for**: Development, testing, immediate results
**Time**: 5 minutes

---

## Path A: Quick Start with GitHub Actions ⚡

### Step 1: Prepare Your Repository (2 min)

```bash
# Navigate to your project
cd c:\Users\AC\Desktop\primsrc

# Initialize git (if not already done)
git init

# Add all files
git add .

# Create first commit
git commit -m "Initial commit: PrimeSrc pipeline with GitHub Actions"
```

### Step 2: Create GitHub Repository (2 min)

1. Go to https://github.com/new
2. Enter repository name (e.g., `primesrc-pipeline`)
3. Choose **Public** or **Private**
4. **DO NOT** initialize with README (you already have one)
5. Click **Create repository**

### Step 3: Push to GitHub (1 min)

```bash
# Replace USERNAME and REPO-NAME with yours
git remote add origin https://github.com/USERNAME/REPO-NAME.git
git branch -M main
git push -u origin main
```

### Step 4: Add Your URLs (1 min)

Edit `multiple_primesrc.txt` on GitHub or locally:

```txt
https://primesrc.me/embed/movie?tmdb=550
https://primesrc.me/embed/movie?tmdb=680
12345
```

If editing locally:
```bash
git add multiple_primesrc.txt
git commit -m "Add embed URLs"
git push
```

### Step 5: Enable Actions (30 sec)

1. Go to your repository on GitHub
2. Click **Settings** → **Actions** → **General**
3. Under "Actions permissions", ensure **Allow all actions** is selected
4. Under "Workflow permissions", select **Read and write permissions**
5. Click **Save**

### Step 6: Run the Workflow (1 min)

1. Go to **Actions** tab
2. Click **PrimeSrc Pipeline** (left sidebar)
3. Click **Run workflow** (right side)
4. Leave all options at default
5. Click green **Run workflow** button

### Step 7: Wait & Download (5-15 min)

1. Click on the running workflow to watch progress
2. Wait for completion (green checkmark)
3. Scroll down to **Artifacts**
4. Download:
   - `final-stream-urls` - Your extracted URLs
   - `pipeline-summary` - JSON with metadata

**🎉 Done! You're running on GitHub Actions!**

---

## Path B: Local Testing 🖥️

### Step 1: Install Dependencies (1 min)

```bash
cd c:\Users\AC\Desktop\primsrc
pip install -r requirements.txt
```

### Step 2: Add URLs (1 min)

Edit `multiple_primesrc.txt`:
```txt
https://primesrc.me/embed/movie?tmdb=550
12345
```

### Step 3: Run (3-10 min)

```bash
python dolkana.py
```

### Step 4: Check Results (30 sec)

Open these files:
- `final_stream_urls.txt` - Your extracted URLs
- `pipeline_summary.json` - Results with metadata

**🎉 Done! Results are in your folder!**

---

## 📚 Documentation Guide

Here's what each document is for:

| Document | Purpose | When to Read |
|----------|---------|--------------|
| **START_HERE.md** (this file) | Quick start | Read first |
| **SETUP_SUMMARY.md** | Overview of changes | After setup |
| **README.md** | Complete documentation | Reference |
| **GITHUB_ACTIONS_SETUP.md** | Detailed GitHub guide | Setup problems |
| **DEPLOYMENT_CHECKLIST.md** | Pre-deployment checklist | Before going live |
| **.github/workflows/README.md** | Workflow documentation | Customizing |

## 🔧 Common Scenarios

### Scenario 1: First Time Running

**Recommended**: Use Path A (GitHub Actions)

1. Follow Quick Start above
2. Use default settings
3. Download artifacts when complete
4. Review results

### Scenario 2: Testing New URLs

**Recommended**: Use Path B (Local Testing)

1. Add URLs to `multiple_primesrc.txt`
2. Run `python dolkana.py` locally
3. Verify results look good
4. Then run on GitHub Actions

### Scenario 3: Daily Automation

**Recommended**: Use GitHub Actions schedule

1. Complete Quick Start (Path A)
2. Workflow runs daily at midnight UTC automatically
3. Check Actions tab for results
4. Download artifacts as needed

### Scenario 4: Large URL Lists

**Recommended**: Use GitHub Actions with adjustments

1. Edit workflow file:
   ```yaml
   python dolkana.py --batch-size 3 --timeout 90
   ```
2. Run workflow
3. May take 30-60 minutes for hundreds of URLs

## ⚙️ Quick Configuration

### Change Schedule

Edit `.github/workflows/primesrc-pipeline.yml`:

```yaml
schedule:
  # Every 6 hours
  - cron: '0 */6 * * *'
  
  # Every Monday at 9 AM
  - cron: '0 9 * * 1'
  
  # Twice daily (midnight and noon)
  - cron: '0 0,12 * * *'
```

### Adjust Performance

In the workflow file:

```yaml
# Faster (less reliable)
python dolkana.py --batch-size 10 --timeout 30

# Slower (more reliable)
python dolkana.py --batch-size 2 --timeout 90
```

### Disable Auto-Commit

In the workflow file, comment out or remove:

```yaml
- name: Commit and push results (optional)
  # if: success()
  # ... rest of step
```

## 🆘 Troubleshooting

### Problem: Workflow doesn't appear

**Solution**: Check Actions are enabled in Settings → Actions

### Problem: Workflow fails immediately

**Solution**: 
1. Check workflow logs
2. Validate YAML: https://www.yamllint.com/
3. Test locally first: `python dolkana.py`

### Problem: Chrome errors in GitHub Actions

**Solution**: Already handled! The workflow installs Chrome automatically.

### Problem: Timeout errors

**Solution**: Reduce batch size in workflow:
```yaml
python dolkana.py --batch-size 2 --timeout 90
```

### Problem: No artifacts uploaded

**Solution**: 
1. Check if input file has URLs
2. Check workflow logs for errors
3. Verify stages completed successfully

### Problem: Empty output files

**Solution**:
1. Verify URLs in `multiple_primesrc.txt` are valid
2. Check Stage 1 completed
3. Check Stage 2 logs for extraction errors

## 📊 Understanding Results

### Output Files

**api_url_list.txt**
- Contains API URLs for Chrome extraction
- One URL per line
- Used by Stage 2

**final_stream_urls.txt**
- Final extracted stream/embed URLs
- Ready to use

**pipeline_summary.json**
- Human-readable JSON
- Includes TMDB metadata
- Grouped by movie
- Multiple sources per movie

**pipeline_summary.gz.json**
- Compressed version (gzip + base64)
- Same data as above
- Smaller file size

### Sample Output

```json
[
  {
    "serial": 1,
    "title": "Fight Club",
    "tmdb_id": 550,
    "imdb_id": "tt0137523",
    "extracted_at": "2024-01-01T00:00:00Z",
    "host-1": "example.com",
    "url-1": "https://example.com/stream/123",
    "host-2": "another.com",
    "url-2": "https://another.com/embed/456"
  }
]
```

## 🎓 Next Steps

### After First Success:

1. ✅ Review artifacts and verify results
2. ✅ Adjust settings if needed (batch size, timeout)
3. ✅ Add more URLs to `multiple_primesrc.txt`
4. ✅ Set up schedule or use manual triggers
5. ✅ Share repository with team (if applicable)

### Optimization:

1. ✅ Monitor run times
2. ✅ Adjust batch size and timeouts
3. ✅ Review failure patterns
4. ✅ Update URL list regularly
5. ✅ Keep dependencies updated

### Advanced:

1. ✅ Set up status notifications
2. ✅ Add status badges to README
3. ✅ Create custom workflows
4. ✅ Use matrix strategy for multiple types
5. ✅ Set up self-hosted runners

## 📞 Getting Help

### Documentation:

1. **Setup issues**: Read `GITHUB_ACTIONS_SETUP.md`
2. **Workflow issues**: Read `.github/workflows/README.md`
3. **Deployment**: Use `DEPLOYMENT_CHECKLIST.md`
4. **General usage**: Read `README.md`

### Resources:

- GitHub Actions Docs: https://docs.github.com/en/actions
- YAML Validator: https://www.yamllint.com/
- Cron Schedule Helper: https://crontab.guru/

### Debugging:

1. **Check workflow logs** in Actions tab
2. **Test locally first** to isolate issues
3. **Review error messages** in detail
4. **Check output files** are created
5. **Verify Chrome installation** in logs

## ✨ Pro Tips

💡 **Test locally first** before relying on GitHub Actions
💡 **Start with small URL lists** to verify everything works
💡 **Use manual triggers** until you're confident in automation
💡 **Monitor Actions usage** in Settings → Billing
💡 **Enable notifications** for workflow failures
💡 **Document your changes** when customizing
💡 **Keep backups** of important results
💡 **Use version control** for workflow changes

## 🎯 Success Checklist

You'll know everything is working when:

- ✅ Workflow appears in Actions tab
- ✅ Workflow runs without errors
- ✅ All steps show green checkmarks
- ✅ Artifacts are uploaded
- ✅ Downloaded artifacts contain valid data
- ✅ URLs are successfully extracted
- ✅ JSON includes TMDB metadata

## 📅 Typical Timeline

| Task | Time | Complexity |
|------|------|------------|
| GitHub setup | 5 min | Easy |
| First workflow run | 10-20 min | Easy |
| Result validation | 2 min | Easy |
| Performance tuning | 10-30 min | Medium |
| Advanced customization | 30-60 min | Medium-Hard |

## 🚦 Status Indicators

### Green (Working)
✅ All steps complete
✅ Artifacts uploaded
✅ Results look good
✅ Schedule working

### Yellow (Needs Attention)
⚠️ Some URLs failed
⚠️ Timeouts occurring
⚠️ Longer than expected

### Red (Problem)
❌ Workflow failed
❌ No artifacts
❌ Syntax errors
❌ Permission denied

---

## 🎉 You're Ready!

Choose your path above and get started. If you run into issues, check the troubleshooting section or refer to the detailed documentation.

**Quick Start Reminder:**
1. Push to GitHub (or test locally)
2. Run workflow (or `python dolkana.py`)
3. Download results (or check local files)
4. Celebrate! 🎉

**Questions?** Check the documentation files listed in "📚 Documentation Guide" above.

**Happy extracting!** 🚀
