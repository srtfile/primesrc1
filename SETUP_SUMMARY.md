# Setup Summary - GitHub Actions for dolkana.py

## What Was Done

Your `dolkana.py` script has been configured to run on GitHub Actions! Here's what was created:

### 1. **GitHub Actions Workflow** (`.github/workflows/primesrc-pipeline.yml`)
   - Automatically runs the PrimeSrc pipeline
   - Triggers:
     - Manual (workflow_dispatch)
     - Daily at midnight UTC
     - On push to main branch
   - Sets up Python, Chrome, and all dependencies
   - Uploads results as artifacts
   - Optionally commits results back to repository

### 2. **Script Updates** (`dolkana.py`)
   - ✅ Made cross-platform compatible (Windows, Linux, macOS)
   - ✅ Added environment variable support for Chrome path
   - ✅ Auto-detects Chrome in multiple locations
   - ✅ Fixed hardcoded Windows paths

### 3. **Supporting Files**
   - `README.md` - Complete documentation
   - `GITHUB_ACTIONS_SETUP.md` - Step-by-step setup guide
   - `requirements.txt` - Python dependencies
   - `.gitignore` - Prevents committing unnecessary files
   - `multiple_primesrc.txt` - Sample input file
   - `.github/workflows/validate.yml` - Code validation workflow

## Quick Start

### Method 1: Using GitHub (Easiest)

1. **Create a GitHub repository**
   ```bash
   cd c:\Users\AC\Desktop\primsrc
   git init
   git add .
   git commit -m "Initial commit"
   ```

2. **Push to GitHub**
   - Create new repository on github.com
   - Follow the instructions to push existing repository

3. **Run the workflow**
   - Go to Actions tab
   - Click "PrimeSrc Pipeline"
   - Click "Run workflow"
   - Download results from Artifacts

### Method 2: Using Locally (Testing)

1. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

2. **Add URLs to multiple_primesrc.txt**
   ```
   https://primesrc.me/embed/movie?tmdb=550
   12345
   ```

3. **Run the script**
   ```bash
   python dolkana.py
   ```

## Key Features

### ✅ Cross-Platform Support
- Windows (your current system)
- Linux (GitHub Actions)
- macOS

### ✅ Automated Execution
- Schedule: Daily runs
- Manual trigger: On-demand via GitHub UI
- Auto-trigger: On code changes

### ✅ Result Management
- Artifacts: Download from GitHub Actions
- Auto-commit: Results pushed back to repo
- Compressed: Gzip+Base64 for large datasets

### ✅ Configuration Options
All script options available in GitHub Actions:
- `--skip-stage1` / `--skip-stage2`
- `--type movie` or `--type tv`
- `--batch-size`, `--timeout`, etc.

## File Structure

```
c:\Users\AC\Desktop\primsrc\
├── .github/
│   └── workflows/
│       ├── primesrc-pipeline.yml  # Main workflow
│       └── validate.yml           # Code validation
├── dolkana.py                      # Main script (updated)
├── multiple_primesrc.txt           # Input URLs
├── requirements.txt                # Python dependencies
├── .gitignore                      # Git ignore rules
├── README.md                       # Main documentation
├── GITHUB_ACTIONS_SETUP.md        # Setup guide
└── SETUP_SUMMARY.md               # This file
```

## Workflow Options

### Input Parameters (Manual Run)

When you click "Run workflow" in GitHub Actions, you can configure:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| skip_stage1 | boolean | false | Skip API key collection |
| skip_stage2 | boolean | false | Skip URL extraction |
| media_type | choice | movie | Movie or TV show |

### Output Artifacts

After each run, download these artifacts:

| Artifact | Contains |
|----------|----------|
| api-url-list | API URLs for Stage 2 |
| final-stream-urls | Extracted stream URLs |
| pipeline-summary | JSON files with metadata |
| pipeline-report | HTML report (if generated) |

## Customization

### Change Schedule

Edit `.github/workflows/primesrc-pipeline.yml`:

```yaml
schedule:
  - cron: '0 */6 * * *'  # Every 6 hours
```

### Change Python Version

```yaml
- name: Set up Python
  uses: actions/setup-python@v5
  with:
    python-version: '3.12'  # Change to 3.12
```

### Adjust Performance

```yaml
- name: Run PrimeSrc Pipeline
  run: |
    python dolkana.py --batch-size 3 --timeout 90
```

### Disable Auto-Commit

Remove or comment out the "Commit and push results" step.

## Next Steps

### For GitHub Setup:

1. ✅ Read `GITHUB_ACTIONS_SETUP.md` for detailed instructions
2. ✅ Create GitHub repository
3. ✅ Push code to GitHub
4. ✅ Add your URLs to `multiple_primesrc.txt`
5. ✅ Run workflow from Actions tab
6. ✅ Download results from Artifacts

### For Local Testing:

1. ✅ Install requirements: `pip install -r requirements.txt`
2. ✅ Edit `multiple_primesrc.txt` with your URLs
3. ✅ Run: `python dolkana.py`
4. ✅ Check output files

## Differences from Original Script

### What Changed:
- ✅ Paths are now cross-platform
- ✅ Chrome detection works on Linux/macOS
- ✅ Environment variable support for CI/CD
- ✅ Better error messages

### What Stayed the Same:
- ✅ All functionality intact
- ✅ Same command-line arguments
- ✅ Same output format
- ✅ Same stages and logic

## Common Issues & Solutions

### Issue: Chrome not found
**Solution**: GitHub Actions workflow automatically installs Chrome

### Issue: Workflow times out
**Solution**: Reduce `--batch-size` or increase timeout

### Issue: nodriver not installed locally
**Solution**: `pip install -r requirements.txt`

### Issue: Permission denied on Windows paths
**Solution**: Script now uses platform-independent paths

## Resources

- **Main Docs**: `README.md`
- **Setup Guide**: `GITHUB_ACTIONS_SETUP.md`
- **This Summary**: `SETUP_SUMMARY.md`
- **GitHub Actions Docs**: https://docs.github.com/en/actions

## Support

If you encounter issues:

1. Check workflow logs in GitHub Actions tab
2. Test locally first: `python dolkana.py --help`
3. Review error messages in workflow output
4. Check artifacts for partial results

## Success Indicators

You'll know it's working when:

✅ Workflow appears in Actions tab
✅ Workflow runs without errors
✅ Artifacts are uploaded
✅ Files appear in artifacts download
✅ Results contain valid URLs

## Estimated Run Times

- Stage 1 (API collection): 1-5 minutes
- Stage 2 (URL extraction): 5-15 minutes per 100 URLs
- Total: ~10-20 minutes for typical workload

Adjust `--batch-size` to balance speed vs. reliability.

---

**You're all set!** Follow the quick start guide above to begin using GitHub Actions with your PrimeSrc pipeline.
