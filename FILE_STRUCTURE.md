# 📁 File Structure Overview

Complete overview of the PrimeSrc Pipeline project structure.

## 📊 Directory Tree

```
c:\Users\AC\Desktop\primsrc\
│
├── 📄 dolkana.py                      # Main pipeline script (UPDATED)
├── 📄 multiple_primesrc.txt          # Input URLs (edit this!)
├── 📄 requirements.txt               # Python dependencies
├── 📄 .gitignore                     # Git ignore rules
│
├── 📚 Documentation Files
│   ├── START_HERE.md                 # 👈 Read this first!
│   ├── README.md                     # Complete documentation
│   ├── SETUP_SUMMARY.md              # What was changed
│   ├── GITHUB_ACTIONS_SETUP.md       # Detailed setup guide
│   ├── DEPLOYMENT_CHECKLIST.md       # Pre-deployment checklist
│   └── FILE_STRUCTURE.md             # This file
│
├── 📂 .github/
│   └── workflows/
│       ├── primesrc-pipeline.yml     # Main workflow
│       ├── validate.yml              # Code validation
│       └── README.md                 # Workflow documentation
│
└── 🔄 Generated Files (created when script runs)
    ├── api_url_list.txt              # Stage 1 output
    ├── final_stream_urls.txt         # Stage 2 output
    ├── pipeline_summary.json         # Results with metadata
    └── pipeline_summary.gz.json      # Compressed results
```

## 📝 File Descriptions

### 🔧 Core Files

#### `dolkana.py`
**Purpose**: Main Python script that runs the extraction pipeline
**Changes**: 
- ✅ Made cross-platform (Windows/Linux/macOS)
- ✅ Environment variable support
- ✅ Better Chrome detection
- ✅ Fixed hardcoded paths

**When to modify**: Advanced users only, for logic changes

---

#### `multiple_primesrc.txt`
**Purpose**: Input file containing PrimeSrc embed URLs
**Format**: One URL or TMDB ID per line
**Example**:
```txt
https://primesrc.me/embed/movie?tmdb=550
https://primesrc.me/embed/movie?tmdb=680
12345
```
**When to modify**: Every time you want to extract new URLs

---

#### `requirements.txt`
**Purpose**: Lists Python dependencies for the project
**Contents**: `nodriver>=0.28`
**When to modify**: When adding new Python libraries

---

#### `.gitignore`
**Purpose**: Tells Git which files to ignore
**Includes**: Python cache, logs, temporary files
**When to modify**: When you want to exclude/include specific files from Git

---

### 📚 Documentation Files

#### `START_HERE.md` 👈 **Read First!**
**Purpose**: Quick start guide (under 10 minutes)
**Contents**:
- Two paths: GitHub Actions or Local
- Step-by-step instructions
- Common scenarios
- Quick troubleshooting

**Who should read**: Everyone, especially first-time users

---

#### `README.md`
**Purpose**: Complete project documentation
**Contents**:
- Feature overview
- GitHub Actions setup
- Local usage
- All command-line options
- Troubleshooting guide
- Platform support

**Who should read**: Everyone, for reference

---

#### `SETUP_SUMMARY.md`
**Purpose**: Summary of what was changed for GitHub Actions
**Contents**:
- List of modifications
- New features
- File structure
- Quick start links

**Who should read**: Users who want to know what was changed

---

#### `GITHUB_ACTIONS_SETUP.md`
**Purpose**: Detailed GitHub Actions setup guide
**Contents**:
- Step-by-step GitHub setup
- Configuration options
- Schedule customization
- Advanced features
- Security best practices

**Who should read**: Users setting up GitHub Actions for the first time

---

#### `DEPLOYMENT_CHECKLIST.md`
**Purpose**: Comprehensive pre-deployment checklist
**Contents**:
- Pre-deployment checks
- GitHub setup steps
- Post-deployment verification
- Troubleshooting checklist
- Maintenance tasks

**Who should read**: Users preparing to deploy to production

---

#### `FILE_STRUCTURE.md` (this file)
**Purpose**: Overview of project file structure
**Contents**: Descriptions of all files and their purposes

**Who should read**: Users who want to understand the project organization

---

### ⚙️ GitHub Workflows

#### `.github/workflows/primesrc-pipeline.yml`
**Purpose**: Main GitHub Actions workflow
**What it does**:
- Sets up Python and Chrome
- Runs the pipeline
- Uploads results as artifacts
- Optionally commits results

**Triggers**:
- 🕐 Scheduled: Daily at midnight UTC
- 👆 Manual: "Run workflow" button
- 📝 Automatic: Push to main

**When to modify**: To change schedule, settings, or behavior

---

#### `.github/workflows/validate.yml`
**Purpose**: Code validation workflow
**What it does**:
- Checks Python syntax
- Validates code before running pipeline

**Triggers**:
- Pull requests to main
- Push to main (when .py files change)

**When to modify**: To add more validation checks

---

#### `.github/workflows/README.md`
**Purpose**: Workflow-specific documentation
**Contents**:
- Workflow descriptions
- Usage instructions
- Customization guide
- Troubleshooting

**Who should read**: Users customizing GitHub Actions workflows

---

### 📤 Output Files (Generated)

These files are created when the script runs:

#### `api_url_list.txt`
**Created by**: Stage 1
**Contains**: API URLs extracted from embed pages
**Format**: One URL per line
```txt
https://primesrc.me/api/v1/l?key=abc123
https://primesrc.me/api/v1/l?key=def456
```
**Size**: ~1-10 KB typically
**Git**: Optional to commit

---

#### `final_stream_urls.txt`
**Created by**: Stage 2
**Contains**: Final extracted stream/embed URLs
**Format**: One URL per line
```txt
https://dood.watch/e/abc123
https://streamtape.com/e/def456
```
**Size**: ~1-10 KB typically
**Git**: Optional to commit

---

#### `pipeline_summary.json`
**Created by**: Summary stage
**Contains**: Results with TMDB metadata
**Format**: Human-readable JSON
**Structure**:
```json
[
  {
    "serial": 1,
    "title": "Movie Title",
    "tmdb_id": 550,
    "imdb_id": "tt0137523",
    "extracted_at": "2024-01-01T00:00:00Z",
    "host-1": "dood.watch",
    "url-1": "https://dood.watch/e/abc123",
    "host-2": "streamtape.com",
    "url-2": "https://streamtape.com/e/def456"
  }
]
```
**Size**: ~1-100 KB typically
**Git**: Recommended to commit

---

#### `pipeline_summary.gz.json`
**Created by**: Summary stage
**Contains**: Compressed version of pipeline_summary.json
**Format**: Gzip + Base64 wrapped in JSON
**Structure**:
```json
{
  "encoding": "gzip+base64",
  "source_file": "pipeline_summary.json",
  "compressed": "H4sIAAAAAAAA..."
}
```
**Size**: ~10-50% of original
**Git**: Optional to commit

---

## 📦 GitHub Actions Artifacts

When running on GitHub Actions, these files are uploaded as downloadable artifacts:

| Artifact Name | Contains | Download As |
|--------------|----------|-------------|
| `api-url-list` | api_url_list.txt | ZIP file |
| `final-stream-urls` | final_stream_urls.txt | ZIP file |
| `pipeline-summary` | pipeline_summary.json<br>pipeline_summary.gz.json | ZIP file |
| `pipeline-report` | pipeline_report.html (if generated) | ZIP file |

**How to download**: 
1. Go to workflow run page
2. Scroll to "Artifacts" section
3. Click artifact name to download

---

## 🔄 Workflow Process Flow

```
multiple_primesrc.txt
         ↓
    [Stage 1]
    Fetch API keys
         ↓
  api_url_list.txt
         ↓
    [Stage 2]
    Extract URLs (Chrome)
         ↓
final_stream_urls.txt
         ↓
    [Summary]
    Generate JSON
         ↓
pipeline_summary.json
pipeline_summary.gz.json
```

---

## 📏 Typical File Sizes

| File | Typical Size | Notes |
|------|-------------|-------|
| `dolkana.py` | ~33 KB | Script file |
| `multiple_primesrc.txt` | ~1-10 KB | Depends on # of URLs |
| `api_url_list.txt` | ~1-50 KB | 20-30 bytes per URL |
| `final_stream_urls.txt` | ~1-50 KB | 50-100 bytes per URL |
| `pipeline_summary.json` | ~1-500 KB | Varies with results |
| `pipeline_summary.gz.json` | ~0.5-250 KB | ~50% compression |

---

## 🔐 Git Tracking

### Always Tracked (Committed)
- ✅ `dolkana.py`
- ✅ `multiple_primesrc.txt`
- ✅ `requirements.txt`
- ✅ All documentation files
- ✅ Workflow files (`.github/workflows/`)

### Optional Tracking (Your Choice)
- ⚠️ `api_url_list.txt`
- ⚠️ `final_stream_urls.txt`
- ⚠️ `pipeline_summary.json`
- ⚠️ `pipeline_summary.gz.json`

### Never Tracked (Ignored)
- ❌ `__pycache__/`
- ❌ `*.pyc`
- ❌ `.vscode/`
- ❌ Temporary files

**Configure in**: `.gitignore`

---

## 🎯 File Modification Guide

### Files You SHOULD Edit:

| File | When | Why |
|------|------|-----|
| `multiple_primesrc.txt` | Always | Add your URLs |
| `.github/workflows/primesrc-pipeline.yml` | Optional | Change schedule/settings |
| `.gitignore` | Optional | Change what Git tracks |
| Documentation | Optional | Add notes/customizations |

### Files You SHOULDN'T Edit:

| File | Why |
|------|-----|
| `dolkana.py` | Complex logic, breaks easily |
| `.github/workflows/validate.yml` | Standard validation |
| `requirements.txt` | Required dependencies |

### Files That Are Auto-Generated:

| File | When |
|------|------|
| `api_url_list.txt` | During Stage 1 |
| `final_stream_urls.txt` | During Stage 2 |
| `pipeline_summary.json` | After completion |
| `pipeline_summary.gz.json` | After completion |

**Don't manually edit** auto-generated files - they'll be overwritten!

---

## 📊 Documentation Reading Order

### For First-Time Users:
1. **START_HERE.md** - Quick start (10 min)
2. **README.md** - Full documentation
3. **GITHUB_ACTIONS_SETUP.md** - If using GitHub
4. **DEPLOYMENT_CHECKLIST.md** - Before going live

### For Advanced Users:
1. **README.md** - Reference
2. **.github/workflows/README.md** - Workflow customization
3. **dolkana.py** - Source code review

### For Troubleshooting:
1. **README.md** - Troubleshooting section
2. **GITHUB_ACTIONS_SETUP.md** - GitHub-specific issues
3. **.github/workflows/README.md** - Workflow problems

---

## 🗂️ Backup Recommendations

### Essential (Always Backup):
- ✅ `multiple_primesrc.txt`
- ✅ `pipeline_summary.json`
- ✅ Modified configuration files

### Optional (Good to Backup):
- ⚠️ All output files
- ⚠️ Documentation customizations
- ⚠️ Workflow modifications

### No Need to Backup:
- ❌ `dolkana.py` (in Git)
- ❌ `requirements.txt` (in Git)
- ❌ Documentation files (in Git)

---

## 🔍 Quick Reference

### Find Information About:

| Topic | Check This File |
|-------|----------------|
| Quick start | START_HERE.md |
| Complete guide | README.md |
| GitHub setup | GITHUB_ACTIONS_SETUP.md |
| Pre-deployment | DEPLOYMENT_CHECKLIST.md |
| File structure | FILE_STRUCTURE.md (this file) |
| Workflows | .github/workflows/README.md |
| Troubleshooting | README.md + GITHUB_ACTIONS_SETUP.md |
| Command options | README.md |
| Customization | .github/workflows/README.md |

---

## ✨ Summary

- **Core**: `dolkana.py` + `multiple_primesrc.txt` + `requirements.txt`
- **Docs**: Multiple guides for different purposes
- **Workflows**: Automated GitHub Actions
- **Output**: JSON + text files with results
- **Git**: Selective tracking via `.gitignore`

**Start with**: `START_HERE.md` and go from there!

---

**Last Updated**: 2024
