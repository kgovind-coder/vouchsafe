# Vouchsafe usage guide

Concrete recipes for common scenarios.

## Install

Vouchsafe ships as a single signed binary — no Python, no `pip`, no virtualenv to manage. Pick one:

### Linux

```bash
curl -L https://github.com/kgovind-coder/vouchsafe/releases/latest/download/vouchsafe-linux-x64 \
    -o /usr/local/bin/vouchsafe && chmod +x /usr/local/bin/vouchsafe
vouchsafe --version
```

### macOS (universal binary — Intel + Apple Silicon)

```bash
curl -L https://github.com/kgovind-coder/vouchsafe/releases/latest/download/vouchsafe-macos-universal \
    -o /usr/local/bin/vouchsafe && chmod +x /usr/local/bin/vouchsafe

# Clear the gatekeeper quarantine attribute if the OS blocks the first run:
xattr -d com.apple.quarantine /usr/local/bin/vouchsafe 2>/dev/null || true

vouchsafe --version
```

### Windows (PowerShell)

```powershell
$dest = "$env:USERPROFILE\AppData\Local\Programs\vouchsafe"
New-Item -ItemType Directory -Force -Path $dest | Out-Null
Invoke-WebRequest `
  -Uri "https://github.com/kgovind-coder/vouchsafe/releases/latest/download/vouchsafe-windows-x64.exe" `
  -OutFile "$dest\vouchsafe.exe"
# Add to PATH for this session:
$env:Path = "$dest;$env:Path"
# Add permanently via:  [Environment]::SetEnvironmentVariable("Path", "$env:Path;$dest", "User")
vouchsafe --version
```

### Docker (multi-arch: amd64 + arm64)

```bash
docker run --rm -v "$PWD":/scan ghcr.io/kgovind-coder/vouchsafe:latest scan /scan
```

### GitHub Action

In any GitHub Actions workflow:

```yaml
- uses: kgovind-coder/vouchsafe@v0.2.0
  with:
    path: .
    formats: json,sarif
```

### Verify your download

Every binary ships with a matching `.sha256` checksum file:

```bash
# Linux / macOS
shasum -a 256 -c vouchsafe-linux-x64.sha256

# Windows PowerShell
$expected = (Get-Content vouchsafe-windows-x64.sha256) -replace ' .+',''
$actual = (Get-FileHash vouchsafe-windows-x64.exe -Algorithm SHA256).Hash.ToLower()
if ($actual -eq $expected) { Write-Host "OK" } else { Write-Host "MISMATCH" }
```

---

## Common workflows

### Scan the current directory

```bash
vouchsafe scan .
```

Outputs `vouchsafe-report.json` and `vouchsafe-report.md` in the current directory by default.

### Scan a specific repo with all formats

```bash
vouchsafe scan /path/to/repo \
  --output ./reports/myapp-$(date +%Y%m%d) \
  --format json \
  --format markdown \
  --format sarif \
  --format cyclonedx \
  --format html \
  --format csv \
  --format junit
```

Produces seven files at the given prefix.

### Quiet mode (CI-friendly)

```bash
vouchsafe scan . --quiet --format json --output /tmp/scan
```

No terminal output, only the report files. Exit code is `0` whether or not findings exist.

### No file output (summary only)

```bash
vouchsafe scan . --no-files
```

Print the scan summary to the terminal but don't write any files. Useful for quick "what AI is in here?" checks.

---

## CI integration

### GitHub Actions

Drop this in `.github/workflows/ai-inventory.yml`:

```yaml
name: ai-inventory

on:
  pull_request:
  push:
    branches: [main]

permissions:
  contents: read
  security-events: write   # to upload SARIF

jobs:
  vouchsafe:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Download Vouchsafe
        run: |
          curl -L https://github.com/kgovind-coder/vouchsafe/releases/latest/download/vouchsafe-linux-x64 \
            -o /usr/local/bin/vouchsafe
          chmod +x /usr/local/bin/vouchsafe
      - name: Run scan
        run: vouchsafe scan . --quiet --format sarif --format json --output ./scan
      - uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: ./scan.sarif.json
      - uses: actions/upload-artifact@v4
        with:
          name: ai-inventory
          path: scan.*
```

### Azure DevOps Pipelines

```yaml
trigger: [main]

pool:
  vmImage: ubuntu-latest

steps:
  - script: |
      curl -L https://github.com/kgovind-coder/vouchsafe/releases/latest/download/vouchsafe-linux-x64 \
        -o vouchsafe
      chmod +x vouchsafe
      ./vouchsafe scan . --format sarif --format junit --output $(Build.ArtifactStagingDirectory)/scan
    displayName: "Vouchsafe AI inventory scan"

  - task: PublishTestResults@2
    inputs:
      testResultsFormat: JUnit
      testResultsFiles: $(Build.ArtifactStagingDirectory)/scan.junit.xml

  - task: PublishBuildArtifacts@1
    inputs:
      pathToPublish: $(Build.ArtifactStagingDirectory)
      artifactName: ai-inventory
```

### GitLab CI

```yaml
vouchsafe:
  stage: test
  image: alpine
  before_script:
    - apk add --no-cache curl
    - curl -L https://github.com/kgovind-coder/vouchsafe/releases/latest/download/vouchsafe-linux-x64 -o vouchsafe
    - chmod +x vouchsafe
  script:
    - ./vouchsafe scan . --format sarif --format junit --output scan
  artifacts:
    reports:
      junit: scan.junit.xml
    paths:
      - scan.*
```

---

## Output formats by use case

| Need | Format | Filename suffix |
|---|---|---|
| Programmatic consumption | JSON | `.json` |
| GitHub Code Scanning / Azure DevOps inline annotations | SARIF | `.sarif.json` |
| SBOM tooling, software-composition platforms | CycloneDX | `.cdx.json` |
| Human-readable report for PRs / docs | Markdown | `.md` |
| Standalone shareable report | HTML | `.html` |
| Spreadsheet ingestion, procurement reviews | CSV | `.findings.csv` + `.components.csv` |
| CI test-result publishers, fail-on-change gates | JUnit XML | `.junit.xml` |

---

## Reading the report

### Finding fields

Each finding records:

| Field | What it means |
|---|---|
| `detector_id` | Stable identifier for the rule that fired (`python-sdk-import.openai`, `mcp-config.server`, …) |
| `category` | High-level grouping: `ai-sdk`, `agent-framework`, `vector-db`, `mcp`, `model-artifact`, … |
| `vendor` | Who supplies it (`openai`, `anthropic`, `pinecone`, `langchain`, …) |
| `confidence` | `high` (import, exact filename), `medium` (suggestive heuristic), `low` (comment/marker) |
| `evidence.path` | Forward-slash NFC-normalized path, relative to scan root |
| `evidence.line` / `evidence.column` | 1-based location (when applicable) |
| `evidence.snippet_hash` | First 16 hex chars of SHA-256 of the matched snippet |
| `framework_mappings` | Sorted list of regulator references this finding maps to |
| `extra.*` | Detector-specific metadata (e.g. `package`, `module`, `server`, `command`) |

### Framework references

Every finding is tagged with one or more framework IDs. Examples:

- `eu_ai_act.annex_iv.s2` — "detailed description of elements & dev process"
- `iso_42001.A.10.3` — "Suppliers (third-party AI components)"
- `owasp_llm.LLM03` — "Supply Chain"
- `owasp_agentic.ASI02` — "Tool Misuse"
- `nist_ai_rmf.value_chain`
- `mitre_atlas.AML.T0011.000` — "Unsafe ML Artifacts"

A given finding may have multiple mappings — e.g. a Pinecone import maps to both `owasp_llm.LLM08` (Vector and Embedding Weaknesses) and `iso_42001.A.7.2` (Data for development).

---

## Determinism guarantee

For a given input + detector pack version, Vouchsafe produces **byte-identical** output. You can:

1. Run Vouchsafe in CI today
2. Save the report
3. Re-run Vouchsafe 6 months later on the same commit
4. Compare reports byte-for-byte to prove nothing changed

This is what regulators and insurers actually want. An "AI thinks the inventory looks fine" assessment does not.

Timestamps (`scan_started_at`, `scan_finished_at`, `elapsed_seconds`) are the only fields that legitimately vary between runs.

---

## Troubleshooting

**My binary won't run on macOS:** macOS gatekeeper blocks unsigned binaries. Run:

```bash
xattr -d com.apple.quarantine vouchsafe-macos-universal
```

(Future: we will codesign once the project ships a Developer ID.)

**False positives:** Open a [bug report](https://github.com/kgovind-coder/vouchsafe/issues/new?template=bug_report.md) with the snippet that fired incorrectly.

**Missing detector for a tool I use:** Open a [detector request](https://github.com/kgovind-coder/vouchsafe/issues/new?template=detector_request.md).

**Scan is too slow:** Add the relevant directory to `DEFAULT_SKIP_DIRS` in `src/vouchsafe/core/walker.py` (configurable skip list is on the v0.2.0 roadmap).
