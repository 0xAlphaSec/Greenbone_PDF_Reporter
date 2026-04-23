# openvas-pdf-reporter

A Python CLI tool that parses XML reports exported from OpenVAS/Greenbone and generates clean, professional PDF vulnerability reports.

## Features

- Parses raw OpenVAS XML exports automatically
- Filters vulnerabilities by minimum CVSS severity
- Generates a structured PDF with:
  - Scan summary table (input file, generation date, scan date, severity filter, total vulnerabilities)
  - Severity distribution table (Critical / High / Medium / Low / Info)
  - Scanned hosts table with hostname resolution and vulnerability count per host
  - Detailed vulnerability cards grouped by host, with description and recommended solution
- Handles XML namespaces and ignores sub-results without severity
- Color-coded severity badges following CVSS standard ranges
- Custom output path support — pass a directory or a full file path with `-o`

## Requirements

- Python 3.8+
- [reportlab](https://pypi.org/project/reportlab/)

## Setup

```bash
# Clone the repository
git clone https://github.com/0xAlphaSec/openvas-pdf-reporter.git
cd openvas-pdf-reporter

# Create and activate virtual environment
python -m venv openvas_env
openvas_env\Scripts\activate        # Windows
source openvas_env/bin/activate     # Linux/macOS

# Install dependencies
pip install -r requirements.txt
```

## Usage

```bash
# Basic usage (includes all severities)
python openvas_report.py report.xml

# Filter by minimum severity
python openvas_report.py report.xml --min-severity 7.0

# Custom output filename
python openvas_report.py report.xml --output my_report.pdf

# Save to a specific directory (filename is auto-generated)
python openvas_report.py report.xml --output /path/to/directory/

# Combined
python openvas_report.py report.xml -s 4.0 -o /path/to/client_report.pdf
```

## Output path behaviour

| `-o` value | Result |
|---|---|
| Not specified | PDF saved in current directory with auto-generated name |
| Filename (`my_report.pdf`) | Saved with that name in the current directory |
| Directory (`/reports/Italy/`) | Auto-generated filename saved inside that directory |
| Full path (`/reports/Italy/scan.pdf`) | Saved exactly at that path |

## CVSS Severity Ranges

| Level    | CVSS Score |
|----------|------------|
| CRITICAL | 9.0 – 10.0 |
| HIGH     | 7.0 – 8.9  |
| MEDIUM   | 4.0 – 6.9  |
| LOW      | 0.1 – 3.9  |
| INFO     | 0.0        |

## Optional: zsh shell integration

If you use zsh, you can call the tool from anywhere without activating the virtual environment manually. Add this to your `.zshrc`:

```zsh
OPENVAS_SCRIPT="$HOME/path/to/openvas_report.py"
OPENVAS_VENV="$HOME/path/to/openvas_env"

openvas_report() {
    if [[ $# -eq 0 ]]; then
        echo "Usage: openvas_report <report.xml> [-s severity] [-o output]"
        return 1
    fi
    source "$OPENVAS_VENV/bin/activate"
    python "$OPENVAS_SCRIPT" "$@"
    deactivate
}
```

Then reload: `source ~/.zshrc`

## Author

**Jesús Fernández** — [jfg.sec](https://www.instagram.com/jfg.sec) — [LinkedIn](https://www.linkedin.com/in/jesus-fernandez-gervasi)
