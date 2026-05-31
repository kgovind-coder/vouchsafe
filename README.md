# Vouchsafe

> A signed, deterministic inventory of every AI / LLM / agent / MCP touchpoint in your codebase, mapped to every regulator that is going to ask.

[![License: Apache 2.0](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-pre--alpha-orange.svg)](#)
[![Python](https://img.shields.io/badge/python-3.11%2B-blue.svg)](#)

Point Vouchsafe at your repo. Ten minutes later you have a signed PDF and machine-readable evidence pack that an EU notified body, an ISO 42001 auditor, a US bank regulator, or a cyber-insurance underwriter will accept — formatted exactly the way each one demands.

**No LLM inside.** No network calls during scan. Same input gives byte-identical output every time. Signed with Ed25519. Air-gapped by default.

---

## Why this exists

Every company is shipping AI features. Almost none of them can answer the question *"where is AI in our code?"*, let alone produce the audit evidence regulators now demand.

- **EU AI Act Article 11** (effective Aug 2, 2026) — fines up to EUR 35M or 7% global turnover
- **ISO/IEC 42001 Annex A** — 38 controls requiring code-derived evidence
- **NIST AI RMF / AI 600-1** — 12 GenAI risk categories
- **India DPDP + AI Governance Guidelines 2025** — effective May 2027
- **Colorado AI Act, NYC LL 144, California AB 2013** — state-level AI laws live in the US
- **FFIEC SR 11-7** — US bank model risk management
- **AI cyber-insurance riders** — 2026 renewals require an AI inventory

The same artifact serves all of these. Vouchsafe produces it in fifteen output formats and lets one scan serve every audit.

---

## What it detects

- **15 programming languages**: Python, JavaScript, TypeScript, Java, Kotlin, C#, Go, Rust, Ruby, PHP, Swift, Dart, Scala, R, Julia, Elixir
- **45+ AI provider SDKs**: OpenAI, Anthropic, Azure OpenAI, Google Gemini / Vertex, AWS Bedrock, Cohere, Mistral, Llama, Hugging Face, Replicate, Together, Groq, Fireworks, Perplexity, xAI, DeepSeek, Qwen, Baidu, Yi, Reka, AI21, Stability, ElevenLabs, AssemblyAI, Deepgram, Whisper, Roboflow, Ultralytics
- **Self-hosted runners**: Ollama, LM Studio, llama.cpp, vLLM, TGI, MLC-LLM, llamafile, GPT4All, Triton, TorchServe, KServe, BentoML, Ray Serve
- **Agent frameworks**: LangChain, LangGraph, LlamaIndex, Haystack, Semantic Kernel, DSPy, CrewAI, AutoGen, PydanticAI, Mastra, Vercel AI SDK, smolagents, LangFlow
- **25+ coding assistants**: Claude Code, Cursor, Aider, GitHub Copilot, Continue, Codeium / Windsurf, JetBrains AI, Tabnine, Cody, Replit, Amazon Q, Gemini Code Assist, Devin, Bolt.new, v0.dev, Lovable, Cline, Roo Code, Goose, Codex CLI, Gemini CLI
- **MCP**: every well-known server, plus per-IDE config files (Claude Desktop, Cursor, VS Code, Continue, Zed, Windsurf, Cline, Codex CLI, Gemini CLI)
- **Vector / RAG**: Pinecone, Weaviate, Qdrant, Milvus, Chroma, LanceDB, pgvector, sqlite-vec, MongoDB Atlas Vector, Redis Stack, Elastic kNN, OpenSearch kNN, ClickHouse, DuckDB vss, Turbopuffer
- **Classical ML**: scikit-learn, XGBoost, LightGBM, CatBoost, TensorFlow, Keras, PyTorch, JAX, ONNX, MLflow, W&B, Kubeflow, SageMaker, Vertex AI, Azure ML
- **Model artifacts**: `.pt`, `.safetensors`, `.gguf`, `.onnx`, `.tflite`, `.h5`, `.pkl`, `.mlpackage`, LoRA / PEFT adapters, sharded HF weights, LFS pointers — detected by magic bytes, not just extension
- **Cloud AI services**: AWS Bedrock / SageMaker / Comprehend / Rekognition / Textract / Lex / Kendra, Azure OpenAI / Cognitive Services / AI Foundry, GCP Vertex / Gemini / Document AI, Snowflake CORTEX, BigQuery ML, Oracle, IBM Watson, Databricks
- **Edge cases competitors miss**: vendored / minified SDKs, curl in shell scripts, OpenAPI / gRPC specs, AI in stored procedures, serverless AI, WASM modules, OpenAI-compatible self-hosts, AI in i18n files, COBOL / ABAP / PL-SQL, game engines (Unity Sentis, Unreal NNE), Apple Intelligence, Chrome `window.ai`, Salesforce Einstein, ServiceNow Now Assist, SAP Joule, browser extensions, feature flags, git history (last 6 months)

---

## What it outputs

- **PDF/A-3** — signed, regulator-templated (EU / UK / US / IN variants)
- **JSON** — versioned schema
- **SARIF 2.1.0** — GitHub Code Scanning / Azure DevOps native
- **CycloneDX 1.7 ML-BOM** — JSON + XML
- **SPDX 3.0 AI Profile** — JSON-LD
- **HTML** — single-file, interactive, deterministic
- **JUnit XML** — for CI gating
- **CSV** — findings, components, evidence
- **Markdown** — drop into your repo README
- **Vanta / Drata / Sprinto** — direct evidence-upload formats
- **in-toto SLSA attestation** — provenance of the report itself

Every report is **byte-identical** for the same input and detector pack, signed with **Ed25519**, optionally counter-signed with an **RFC 3161 trusted timestamp**.

---

## Regulator mappings (every finding tagged automatically)

EU AI Act Annex IV (all 9 sections), ISO/IEC 42001 Annex A (all 38 controls), NIST AI RMF + AI 600-1, MITRE ATLAS, OWASP LLM Top 10 (2025), OWASP Top 10 for Agentic Applications (Dec 2025), India DPDP + AI Governance Guidelines 2025, Colorado AI Act, NYC Local Law 144, California AB 2013, UK AI principles, FFIEC SR 11-7, HIPAA + AI, SOC 2 + AI, PCI-DSS 4.0, AI cyber-insurance rider patterns.

---

## Where Vouchsafe fits

Vouchsafe occupies a deliberately narrow niche: a **closed-source CLI** that
produces **deterministic, regulator-mapped AI inventory evidence** offline.
It is **not** a runtime guardrail, not a red-team tool, not a SaaS.

For a side-by-side view of how Vouchsafe compares to adjacent tools at the
time of writing — and **why a category-specific tool** rather than replacing
your existing AI security stack — see **[docs/COMPARISON.md](docs/COMPARISON.md)**.

---

## Install

Vouchsafe ships as a **single signed binary** for Linux, macOS, and Windows. No Python, no `pip`, no virtualenv needed. Pick whichever option matches your environment:

### Linux

```bash
curl -L https://github.com/kgovind-coder/vouchsafe/releases/latest/download/vouchsafe-linux-x64 \
    -o /usr/local/bin/vouchsafe && chmod +x /usr/local/bin/vouchsafe
vouchsafe --version
```

### macOS (Intel + Apple Silicon, universal binary)

```bash
curl -L https://github.com/kgovind-coder/vouchsafe/releases/latest/download/vouchsafe-macos-universal \
    -o /usr/local/bin/vouchsafe && chmod +x /usr/local/bin/vouchsafe

# First run on macOS may need to clear the gatekeeper quarantine attribute:
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
# Add to PATH for this session (add it permanently via System Properties):
$env:Path = "$dest;$env:Path"
vouchsafe --version
```

### Docker (multi-arch: amd64 + arm64)

```bash
docker run --rm -v "$PWD":/scan ghcr.io/kgovind-coder/vouchsafe:latest scan /scan
```

### GitHub Action

```yaml
- uses: kgovind-coder/vouchsafe@v0.2.0
  with:
    path: .
    formats: json,sarif
```

### Verify your download

Every binary ships with a `.sha256` checksum file at the same URL with `.sha256` appended. Verify before use:

```bash
# Linux / macOS:
shasum -a 256 -c vouchsafe-linux-x64.sha256

# Windows PowerShell:
$expected = (Get-Content vouchsafe-windows-x64.sha256) -replace ' .+',''
$actual = (Get-FileHash vouchsafe-windows-x64.exe -Algorithm SHA256).Hash.ToLower()
if ($actual -eq $expected) { "OK" } else { "MISMATCH" }
```

## Quickstart

```bash
# Scan the current directory, write JSON + Markdown report
vouchsafe scan .

# Multiple output formats at once
vouchsafe scan /path/to/repo --output ./report \
    --format json --format sarif --format cyclonedx --format spdx --format markdown

# Open the report interactively in your browser
vouchsafe scan . --serve

# PR-mode: only scan files changed since main
vouchsafe scan . --since main --format sarif --output pr-scan

# Drop a starter config in your repo for consistent team scans
vouchsafe init
```

## Status

**v0.2.0 shipped 2026-05-31** — 32 detectors across 10 programming languages,
8 output formats, 5-framework regulatory mappings, Docker image, GitHub Action,
local web viewer, diff mode. 84 unit tests, full cross-OS CI matrix.

See the [CHANGELOG](CHANGELOG.md) for what's in this release, and the [ROADMAP](docs/ROADMAP.md) for v0.2+.

### What ships in v0.2.0

**10 language detectors:** Python · JavaScript/TypeScript · Java/Kotlin (Maven + Gradle) · Go · Rust · C#/.NET · Ruby · PHP · Swift · Dart/Flutter.

**8 output formats:** JSON · Markdown · SARIF 2.1.0 · CycloneDX 1.6 ML-BOM · SPDX 3.0 AI Profile · HTML · CSV · JUnit XML.

**Cloud + IaC:** Terraform/OpenTofu (`aws_bedrock_*`, `azurerm_cognitive_*`, `google_vertex_ai_*`, etc.) · Helm charts (vllm, triton, tgi, kserve, kubeflow) · Kubernetes CRDs (InferenceService, RayCluster, PyTorchJob, TFJob).

**SQL-AI:** Snowflake CORTEX · BigQuery ML.GENERATE_TEXT / AI.GENERATE · pgvector · DuckDB vss · sqlite-vec.

**Git history:** Co-authored-by trailers from 18 AI tools (Claude, Copilot, Cursor, Devin, Aider, Codex, Gemini, etc.) · commits that removed AI libraries (historical-usage signal).

**Distribution:** signed binaries on GitHub Releases · Docker multi-arch image on GHCR · official GitHub Action · `--serve` local web viewer · diff mode (`--since`) for PR scans · `vouchsafe init` for project configs.

### What's coming in v0.3+

Ed25519 signing for reports · PDF generation with regulator templates · Vanta/Drata/Sprinto direct upload formats · Salesforce Einstein / SAP Joule / ServiceNow Now Assist detection · embedded-vectors heuristic · PyPI publishing.

---

## License

[Apache License 2.0](LICENSE) — free for any use, including commercial. Includes an explicit patent grant. Modifications, redistributions, and integrations are all permitted under the terms of the license.

If you'd like to support the project, please [star it on GitHub](https://github.com/kgovind-coder/vouchsafe), share what you build with it, or open an issue with feedback.

---

## Disclaimer

Vouchsafe generates inventory and evidence to support compliance workflows.
It **does not constitute legal, regulatory, or audit advice**. Whether the
output satisfies a specific obligation (e.g. EU AI Act Article 11 conformity
assessment, ISO/IEC 42001 certification, FFIEC SR 11-7 model inventory, India
DPDP audit, a cyber-insurance underwriting question) is determined by your
auditor, regulator, broker, or legal counsel — not by this tool.

References to regulatory frameworks and standards (EU AI Act, ISO/IEC 42001,
NIST AI RMF, OWASP LLM Top 10, OWASP Agentic Top 10, MITRE ATLAS, India DPDP
and AI Governance Guidelines, FFIEC SR 11-7, HIPAA, SOC 2, PCI-DSS, Colorado
AI Act, NYC Local Law 144, California AB 2013, UK DSIT AI principles) and to
artifacts produced by other organisations (CycloneDX, SPDX, SARIF, Vanta,
Drata, Sprinto, in-toto, SLSA) are **descriptive of what Vouchsafe produces**.
No endorsement by any regulator, standards body, insurer, broker, or named
company is implied or claimed. Vouchsafe is independently developed and is
not affiliated with, sponsored by, or certified by any of the above.

All trademarks, service marks, and registered names referenced in this
project are the property of their respective owners.

---

## Author

Built by [Kurri Govinda Reddy](https://github.com/kgovind-coder).
