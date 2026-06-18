# PrimeSrc Pipeline

Automated pipeline for extracting streaming URLs from PrimeSrc embed pages.

## Features

- **Stage 1**: Fetches API keys from PrimeSrc embed URLs
- **Stage 2**: Extracts stream URLs using Chrome automation
- Generates JSON summaries with TMDB metadata
- Supports both local and GitHub Actions execution

## GitHub Actions Setup

### 1. Repository Setup

1. Create a GitHub repository
2. Push this code to your repository
3. Create a `multiple_primesrc.txt` file with your PrimeSrc embed URLs (one per line)

Example `multiple_primesrc.txt`:
```
https://primesrc.me/embed/movie?tmdb=12345
https://primesrc.me/embed/movie?tmdb=67890
550  # Just TMDB ID works too
```

### 2. Running the Workflow

#### Manual Trigger (Recommended)

1. Go to your repository on GitHub
2. Click on the "Actions" tab
3. Select "PrimeSrc Pipeline" workflow
4. Click "Run workflow"
5. Configure options:
   - **Skip Stage 1**: Use existing API URL list (for re-runs)
   - **Skip Stage 2**: Only collect API keys, skip Chrome extraction
   - **Media Type**: Choose `movie` or `tv`
6. Click "Run workflow"

#### Automatic Triggers

The workflow runs automatically on:
- **Daily schedule** at 00:00 UTC (can be customized in the workflow file)
- **Push to main branch** when `dolkana.py`, the workflow file, or `multiple_primesrc.txt` changes

### 3. Downloading Results

After the workflow completes:

1. Go to the workflow run page
2. Scroll down to "Artifacts"
3. Download available artifacts:
   - `api-url-list`: Contains all API URLs collected
   - `final-stream-urls`: Contains extracted stream URLs
   - `pipeline-summary`: JSON files with metadata and grouped results
   - `pipeline-report`: HTML report (if generated)

### 4. Customizing the Workflow

Edit `.github/workflows/primesrc-pipeline.yml` to customize:

- **Schedule**: Change the cron expression
  ```yaml
  schedule:
    - cron: '0 0 * * *'  # Daily at midnight UTC
  ```

- **Python version**: Change in `setup-python` step
  ```yaml
  python-version: '3.11'
  ```

- **Commit results**: The workflow can optionally commit results back to the repository (currently enabled)

## Local Usage

### Prerequisites

```bash
pip install nodriver
```

### Basic Usage

```bash
# Run full pipeline
python dolkana.py

# Specify input file
python dolkana.py --input my_urls.txt

# Skip stages
python dolkana.py --skip-stage1  # Use existing api_url_list.txt
python dolkana.py --skip-stage2  # Only collect keys, no Chrome

# Media type
python dolkana.py --type tv

# Custom timeout and batch settings
python dolkana.py --timeout 60 --batch-size 10
```

### Command-Line Options

```
--input FILE          Input file with embed URLs (default: multiple_primesrc.txt)
--api-list FILE       API URL list file (default: api_url_list.txt)
--output FILE         Output stream URLs file (default: final_stream_urls.txt)
--json-out FILE       JSON summary output (default: pipeline_summary.json)
--skip-stage1         Skip Stage 1 (fetch API keys)
--skip-stage2         Skip Stage 2 (extract URLs via Chrome)
--type {movie,tv}     Media type (default: movie)
--port PORT           Chrome debug port (default: 9222)
--timeout SECONDS     Page timeout per tab (default: 45)
--batch-size N        Concurrent tabs (default: 5)
--reloads N           Reload retries per tab (default: 2)
--final-retries N     Extra retry passes (default: 1)
--live-profile        Use live Chrome profile (requires --kill-chrome)
--kill-chrome         Kill existing Chrome before starting
--refresh-profile     Refresh cached Chrome profile
--keep-open           Keep browser open after completion
```

## Output Files

- `api_url_list.txt`: All API URLs for Stage 2
- `final_stream_urls.txt`: Extracted stream URLs
- `pipeline_summary.json`: Pretty JSON with metadata
- `pipeline_summary.gz.json`: Compressed JSON (gzip + base64)

## Platform Support

The script automatically detects and adapts to:
- **Windows**: Uses default Chrome installation paths
- **Linux**: Uses system Chrome/Chromium
- **macOS**: Uses Chrome.app bundle

Chrome path can be overridden with `CHROME_EXE` environment variable.

## Troubleshooting

### GitHub Actions Issues

**Chrome not found:**
```yaml
# The workflow uses browser-actions/setup-chrome
# This should install Chrome automatically
```

**Permission denied:**
```bash
# Make sure dolkana.py is executable (not usually needed)
chmod +x dolkana.py
```

**Out of memory:**
```yaml
# Reduce batch size in workflow:
python dolkana.py --batch-size 3
```

### Local Issues

**nodriver not installed:**
```bash
pip install nodriver
```

**Chrome profile locked:**
```bash
# Use --kill-chrome flag
python dolkana.py --kill-chrome
```

**Timeout errors:**
```bash
# Increase timeout and reduce batch size
python dolkana.py --timeout 90 --batch-size 3
```

## Security Notes

- The script uses Chrome automation which may trigger security warnings
- TMDB API key is embedded in the script (public key for basic lookups)
- Be cautious about committing sensitive data to GitHub
- Consider using GitHub Secrets for API keys if needed

## License

MIT License - feel free to modify and use as needed.
