# GitHub Actions Quick Setup Guide

## Step-by-Step Instructions

### 1. Create GitHub Repository

```bash
# Initialize git if not already done
cd c:\Users\AC\Desktop\primsrc
git init

# Add all files
git add .

# Create initial commit
git commit -m "Initial commit: PrimeSrc pipeline with GitHub Actions"

# Create repository on GitHub, then:
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPO-NAME.git
git branch -M main
git push -u origin main
```

### 2. Verify Workflow File

After pushing, check that `.github/workflows/primesrc-pipeline.yml` is visible in your repository under the "Actions" tab.

### 3. Add Your URLs

Edit `multiple_primesrc.txt` and add your PrimeSrc embed URLs:

```txt
https://primesrc.me/embed/movie?tmdb=550
https://primesrc.me/embed/movie?tmdb=680
12345
```

Commit and push:
```bash
git add multiple_primesrc.txt
git commit -m "Add embed URLs"
git push
```

### 4. Run the Workflow

#### Option A: Manual Run (Recommended for First Time)

1. Go to `https://github.com/YOUR-USERNAME/YOUR-REPO-NAME/actions`
2. Click "PrimeSrc Pipeline" in the left sidebar
3. Click "Run workflow" button (top right)
4. Configure options:
   - Leave defaults for first run
   - Click green "Run workflow" button
5. Wait for workflow to complete (5-15 minutes depending on number of URLs)

#### Option B: Automatic Run

The workflow will run automatically:
- Daily at midnight UTC
- On every push to `main` that modifies relevant files

### 5. Download Results

1. Click on the completed workflow run
2. Scroll to "Artifacts" section at the bottom
3. Download the artifacts you need:
   - `pipeline-summary` - Contains the JSON files
   - `final-stream-urls` - Contains the extracted URLs
   - `api-url-list` - Contains the API URLs

### 6. View in Repository (Optional)

If you want results committed back to the repository:

1. The workflow automatically commits results back to main
2. Pull the changes locally:
   ```bash
   git pull
   ```
3. Check the generated files:
   - `pipeline_summary.json`
   - `final_stream_urls.txt`

## Workflow Configuration

### Modify Schedule

Edit `.github/workflows/primesrc-pipeline.yml`:

```yaml
schedule:
  # Run every 6 hours
  - cron: '0 */6 * * *'
  
  # Run every Monday at 9 AM UTC
  - cron: '0 9 * * 1'
  
  # Run twice daily (midnight and noon UTC)
  - cron: '0 0,12 * * *'
```

### Adjust Performance Settings

For better performance or to avoid timeouts:

```yaml
- name: Run PrimeSrc Pipeline
  run: |
    python dolkana.py \
      --batch-size 3 \
      --timeout 90 \
      --reloads 3
```

### Disable Auto-Commit

If you don't want results committed back:

Comment out or remove the "Commit and push results" step:

```yaml
# - name: Commit and push results (optional)
#   if: success()
#   ...
```

## Troubleshooting

### Workflow Fails to Start

- Check the workflow file syntax: https://www.yamllint.com/
- Verify Actions are enabled in repository settings

### Chrome/Browser Issues

The workflow uses `browser-actions/setup-chrome` which should handle Chrome installation automatically. If issues persist:

```yaml
- name: Install Chrome (alternative)
  run: |
    wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
    sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
    sudo apt-get update
    sudo apt-get install -y google-chrome-stable
```

### Timeout Issues

Increase timeouts in the workflow:

```yaml
- name: Run PrimeSrc Pipeline
  timeout-minutes: 60  # Increase from default 360
```

### Out of Memory

Reduce batch size:

```yaml
python dolkana.py --batch-size 2 --reloads 1
```

## Security Best Practices

### Private Repository

If your URLs contain sensitive information:

1. Make repository private (Settings → Danger Zone → Change visibility)
2. Artifacts are only accessible to repository members

### Secrets

If you need to use API keys:

1. Go to Settings → Secrets and variables → Actions
2. Add repository secret (e.g., `TMDB_API_KEY`)
3. Use in workflow:
   ```yaml
   env:
     TMDB_API_KEY: ${{ secrets.TMDB_API_KEY }}
   ```

### Branch Protection

Protect main branch:

1. Settings → Branches → Add rule
2. Require pull request reviews
3. Require status checks to pass

## Advanced: Self-Hosted Runners

For better performance or specific Chrome profiles:

1. Settings → Actions → Runners → New self-hosted runner
2. Follow setup instructions
3. Modify workflow:
   ```yaml
   runs-on: self-hosted
   ```

## Monitoring

### Email Notifications

GitHub sends email notifications for:
- Workflow failures
- First workflow run

Configure in: Settings → Notifications

### Status Badge

Add to README.md:

```markdown
![PrimeSrc Pipeline](https://github.com/YOUR-USERNAME/YOUR-REPO-NAME/workflows/PrimeSrc%20Pipeline/badge.svg)
```

## Cost Considerations

- **Public repositories**: GitHub Actions is free (2,000 minutes/month)
- **Private repositories**: Free tier includes 500 minutes/month
- This workflow typically uses 5-15 minutes per run
- Daily runs = ~300-450 minutes/month (within free tier)

## Need Help?

1. Check the Actions tab for error logs
2. Review the workflow run details
3. Check artifact uploads for partial results
4. Verify Chrome installation step succeeded
5. Test locally first: `python dolkana.py --help`
