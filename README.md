# 📅 Calendar Management System

Comprehensive calendar management system for organizing, validating, and uploading academic schedules to Google Calendar. Built for Sec6 (Section 6) schedule management with automated color coding, validation, and Google Calendar integration.

## ✨ Features

### 🎨 Smart Color Organization
- Automatic color assignment based on course names
- 11 distinct Google Calendar colors for visual organization
- Handles typos and accent variations in French course names
- Customizable color mapping for different courses

### ✅ Advanced Validation
- Date and time format validation
- Week range checking (S14-S26)
- Duplicate event detection
- Missing field identification
- Detailed error reporting with row numbers

### 📊 Statistics & Reporting
- Event distribution by color, week, and course
- Visual bar charts in terminal output
- Date range analysis
- Course frequency reports
- Weekly event summaries

### 🔄 Google Calendar Integration
- Direct upload to Google Calendar
- Course-specific calendar creation
- Smart sync with duplicate detection
- Batch processing with rate limiting
- OAuth 2.0 authentication

### 🛠️ File Management
- Automatic backup creation
- Split calendars by color/course
- UTF-8 encoding support
- CSV export/import
- ICS calendar format support

## 🚀 Quick Start

### Prerequisites

- Python 3.10 or higher
- Google Calendar API access (credentials.json)
- Linux/macOS/Windows with terminal access

### Installation

```bash
# Clone the repository
git clone https://github.com/TheSilent01/Calendar.git
cd Calendar

# Create virtual environment
python3 -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
pip install google-auth google-auth-oauthlib google-auth-httplib2 google-api-python-client
```

### Google Calendar Setup

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create a new project or select existing one
3. Enable Google Calendar API
4. Create OAuth 2.0 credentials
5. Download credentials and save as `credentials.json` in project root

## 📖 Usage

### Calendar Organizer

The enhanced calendar organizer tool provides comprehensive calendar management.

#### Basic Usage

```bash
# Organize and color-code your calendar
python3 calendar_organizer.py sec6.csv

# Show detailed statistics
python3 calendar_organizer.py sec6.csv --stats

# Validate without making changes
python3 calendar_organizer.py sec6.csv --validate-only
```

#### Advanced Options

```bash
# Split calendar by color
python3 calendar_organizer.py sec6.csv --split-by-color output/

# Custom output filename
python3 calendar_organizer.py sec6.csv -o organized_schedule.csv

# Skip backup creation
python3 calendar_organizer.py sec6.csv --no-backup

# Don't sort by color
python3 calendar_organizer.py sec6.csv --no-sort

# View all options
python3 calendar_organizer.py --help
```

### Google Calendar CLI

Upload calendars to Google Calendar with the integrated CLI tool.

#### List Calendars

```bash
python3 src/gcal_cli.py list
```

#### Upload Calendar

```bash
# Upload all courses
python3 src/gcal_cli.py upload --csv sec6_colored.csv

# Upload specific course (with filter)
python3 src/gcal_cli.py upload --csv sec6_colored.csv --filter "Analyse"

# Dry run (preview without uploading)
python3 src/gcal_cli.py upload --csv sec6_colored.csv --dry-run
```

#### Smart Sync

```bash
# Sync only missing calendars
python3 src/gcal_cli.py sync --csv sec6_colored.csv

# Delete existing and re-sync
python3 src/gcal_cli.py sync --csv sec6_colored.csv --delete-existing

# Set custom pause between uploads (seconds)
python3 src/gcal_cli.py sync --csv sec6_colored.csv --pause 60
```

#### Manage Calendars

```bash
# Delete calendars matching pattern
python3 src/gcal_cli.py delete "Sec6" --yes

# Remove duplicate events
python3 src/gcal_cli.py dedupe --pattern "Sec6"

# Prune all non-essential calendars
python3 src/gcal_cli.py prune --yes

# Audit CSV for issues
python3 src/gcal_cli.py audit --csv sec6_colored.csv
```

## 🎨 Color Mapping

| Course | Color | Visual |
|--------|-------|--------|
| Analyse 4 | Tomato | 🔴 Red |
| Programmation Avancée | Flamingo | 🌸 Pink |
| English for International | Tangerine | 🟠 Orange |
| Développement Personnel | Banana | 🟡 Yellow |
| Éléments de Machines | Sage | 🟢 Green |
| Savoir être | Basil | 💚 Green |
| Électromagnétisme | Peacock | 🔵 Blue |
| Algèbre 2 | Blueberry | 💙 Blue |
| Optique | Lavender | 🟣 Purple |
| Méthodes Numériques | Grape | 🍇 Purple |
| Techniques d'écriture | Citron | 🍋 Yellow |

## 📁 Project Structure

```
Calendar/
├── README.md                   # This file
├── README_ORGANIZER.md         # Detailed organizer documentation
├── calendar_organizer.py       # Enhanced calendar organizer tool
├── requirements.txt            # Python dependencies
├── .gitignore                  # Git ignore rules
│
├── sec6.csv                    # Original calendar data
├── sec6_colored.csv           # Organized output (color-coded)
├── optimized_schedule.csv     # Optimized schedule data
│
├── src/
│   ├── gcal_cli.py            # Google Calendar CLI tool
│   └── final_extractor.py     # PDF extraction tool
│
├── credentials.json           # Google OAuth credentials (gitignored)
├── token.json                 # OAuth token (gitignored)
│
├── scripts/                    # Utility scripts
├── logs/                       # Operation logs (gitignored)
├── docs/                       # Additional documentation
└── artifacts/                  # Archived data (gitignored)
```

## 🔧 Configuration

### Color Customization

Edit `COLOR_MAP` in `calendar_organizer.py`:

```python
COLOR_MAP = {
    'Your Course Name': 'Tomato',
    'Another Course': 'Blueberry',
    # Add more mappings
}
```

### Week Range

Edit `EXPECTED_WEEKS` in `calendar_organizer.py`:

```python
EXPECTED_WEEKS = ['S14', 'S15', 'S16', ...]  # Academic weeks
```

### Google Calendar Settings

Configure in `src/gcal_cli.py`:

```python
DEFAULT_CSV = Path('your_default.csv')
TOKEN_PATH = Path('token.json')
CREDS_PATH = Path('credentials.json')
```

## 📊 Examples

### Example 1: Complete Workflow

```bash
# 1. Organize and validate calendar
python3 calendar_organizer.py sec6.csv --stats

# 2. Review the output
cat sec6_colored.csv

# 3. Upload to Google Calendar
python3 src/gcal_cli.py sync --csv sec6_colored.csv

# 4. Verify in Google Calendar web interface
python3 src/gcal_cli.py list
```

### Example 2: Split and Upload by Color

```bash
# Split into separate files by color
python3 calendar_organizer.py sec6.csv --split-by-color by_color/

# Upload specific color groups
python3 src/gcal_cli.py upload --csv by_color/sec6_Tomato.csv
python3 src/gcal_cli.py upload --csv by_color/sec6_Blueberry.csv
```

### Example 3: Maintenance and Cleanup

```bash
# Remove duplicate events
python3 src/gcal_cli.py dedupe --pattern "Sec6"

# Delete old calendars
python3 src/gcal_cli.py delete "old" --yes

# Re-sync with fresh data
python3 src/gcal_cli.py sync --csv sec6_colored.csv --delete-existing
```

## 🐛 Troubleshooting

### Authentication Issues

**Problem:** `credentials.json not found`
```bash
# Ensure you have downloaded OAuth credentials from Google Cloud Console
# Place the file in the project root directory
```

**Problem:** `Token expired`
```bash
# Remove old token and re-authenticate
rm token.json
python3 src/gcal_cli.py list  # This will open browser for re-auth
```

### Validation Errors

**Problem:** `Invalid date format`
```bash
# Dates must be in MM/DD/YYYY format
# Fix: 15/02/2026 → 02/15/2026
```

**Problem:** `Unexpected week S21`
```bash
# Check EXPECTED_WEEKS in calendar_organizer.py
# Either fix the CSV or update the expected weeks list
```

### Upload Issues

**Problem:** `Quota exceeded`
```bash
# Google Calendar API has rate limits
# Use --pause parameter to slow down uploads
python3 src/gcal_cli.py sync --csv sec6_colored.csv --pause 300
```

**Problem:** `Calendar already exists`
```bash
# Use sync instead of upload to handle existing calendars
python3 src/gcal_cli.py sync --csv sec6_colored.csv
```

## 🤝 Contributing

Contributions are welcome! Feel free to:
- Report bugs
- Suggest new features
- Submit pull requests
- Improve documentation

## 📝 CSV Format

Your calendar CSV should have these columns:

```csv
Subject,Start Date,Start Time,End Date,End Time,All Day Event,Description,Location,Private
```

**Example:**
```csv
Analyse 4 MOUZOUN,02/12/2026,04:30 PM,02/12/2026,06:30 PM,False,Section 6 - Week 14,,False
```

### Required Fields
- `Subject`: Course name and professor
- `Start Date`: MM/DD/YYYY format
- `Start Time`: HH:MM AM/PM format
- `End Date`: MM/DD/YYYY format
- `End Time`: HH:MM AM/PM format
- `All Day Event`: True or False
- `Description`: Week info and section

### Optional Fields
- `Location`: Room or building
- `Private`: Privacy flag
- `Color`: Google Calendar color (added by organizer)

## 🔐 Security

- **Never commit** `credentials.json` or `token.json` to git
- These files are in `.gitignore` for your protection
- OAuth tokens are stored locally and refreshed automatically
- Use environment variables for sensitive configuration

## 📄 License

This project is for academic schedule management. Use responsibly and in accordance with your institution's policies.

## 🙏 Acknowledgments

- Built for efficient academic calendar management
- Utilizes Google Calendar API
- Designed for Moroccan engineering school schedules

## 📞 Support

For issues or questions:
1. Check the [Troubleshooting](#-troubleshooting) section
2. Review the detailed [organizer documentation](README_ORGANIZER.md)
3. Open an issue on GitHub

---

**Made with ❤️ for better schedule management**
Commit notes: improved README with usage and layout.

## Additional features added

- Improved logging: uses a rotating log file under `logs/` and a console output with adjustable `--log-level` and `--log-file` flags.
- CI: a lightweight GitHub Actions workflow runs syntax checks and a smoke test on push/PR (`.github/workflows/ci.yml`).
- Shell completion: `docs/calendar-completion.sh` provides a small bash completion helper — source it from your shell or copy to `/etc/bash_completion.d/`.

