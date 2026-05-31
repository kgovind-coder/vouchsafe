# Changelog

All notable changes to Vouchsafe will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

Tracking [the roadmap](docs/ROADMAP.md) for further additions.

## [0.2.0] — 2026-05-31

### Changed
- **License switched from PolyForm Noncommercial 1.0.0 to Apache License 2.0.**
  Vouchsafe is now free for any use including commercial. Apache 2.0 provides
  an explicit patent grant and is the standard license for serious developer
  tools (Kubernetes, Terraform, uv, bun). The previous PolyForm-NC license
  blocked the very buyers Vouchsafe is built to serve.

### Added — language coverage (from 2 → 10 languages)

- **Java / Kotlin** manifest detection: Maven (`pom.xml`) + Gradle
  (`build.gradle` and `.kts`). Covers OpenAI, Anthropic, Azure OpenAI,
  Vertex AI, AWS Bedrock, Cohere, `dev.langchain4j` family (prefix),
  `org.springframework.ai` family (prefix), MCP Java SDK, ONNX Runtime,
  DJL, Triton, Xebia Xef.
- **Go** module detection: 16 AI module paths including go-openai,
  anthropic-sdk-go, Azure azopenai, vertexai/aiplatform, google.golang.org/genai,
  AWS bedrockruntime, cohere-go, mistral-go, ollama, langchaingo, eino,
  MCP SDKs, Qdrant/Milvus/Weaviate/Pinecone Go clients.
- **Rust** Cargo.toml detection: async-openai, anthropic-sdk, candle, hf-hub,
  tokenizers, llama-cpp-rs, mistralrs, langchain-rust, rig-core, rmcp,
  qdrant-client, lancedb, plus the broader Rust ML ecosystem.
- **C# / .NET** project files (`.csproj`, `.fsproj`, `.vbproj`,
  `Directory.Packages.props`): full Microsoft.SemanticKernel + Connectors
  family, Microsoft.Extensions.AI family, OpenAI, Azure.AI.OpenAI,
  Anthropic.SDK, AWSSDK.Bedrock\*, Google.Cloud.AIPlatform.V1, Mscc.GenerativeAI,
  Microsoft.ML / OnnxRuntime, LLamaSharp, TorchSharp, ModelContextProtocol.
- **Ruby** Gemfile + gemspec: ruby-openai, anthropic, gemini-ai,
  google-cloud-ai_platform, aws-sdk-bedrockruntime, cohere-ruby, replicate-ruby,
  ollama-ai, langchainrb (+ rails), ruby_llm, model_context_protocol.
- **PHP** composer.json: openai-php/client (+ laravel),
  anthropic-php/anthropic-php (+ laravel), google/cloud-ai-platform,
  aws/aws-sdk-php, prism-php/prism, llphant/llphant, maestroerror/larva,
  php-mcp/server.
- **Swift** Package.swift + Podfile + .podspec: MacPaw/OpenAI,
  adamrushy/OpenAISwift, fumito-ito/AnthropicSwiftSDK, mlx-swift,
  AWS Amplify Swift SDK, AWSBedrockRuntime, GoogleGenerativeAI, FirebaseAI.
- **Dart / Flutter** pubspec.yaml: dart_openai, openai_dart,
  anthropic_sdk_dart, google_generative_ai, firebase_ai, firebase_vertexai,
  tflite_flutter, ollama_dart, langchain family, flutter_gemma, mcp_dart,
  google_mlkit_\* (prefix match).

### Added — new detector areas

- **Infrastructure-as-Code**: Terraform / OpenTofu resource type matching
  (`aws_bedrock_*`, `azurerm_cognitive_*`, `google_vertex_ai_*`, etc.),
  Helm `Chart.yaml` (vllm, triton, tgi, kserve, kubeflow, ollama, mlflow,
  ray-cluster, seldon, bentoml, nemo), Kubernetes CRDs (InferenceService,
  RayCluster, PyTorchJob, TFJob, MPIJob, Notebook).
- **SQL-AI**: Snowflake CORTEX function family + AI_FILTER/CLASSIFY/AGG,
  BigQuery ML.GENERATE_TEXT / GENERATE_EMBEDDING + AI.GENERATE / IF / SCORE,
  pgvector (CREATE EXTENSION + vector(N) column type + distance operators),
  ClickHouse vector functions, DuckDB vss, sqlite-vec, SurrealDB HNSW/MTREE.
- **Git history**: Co-authored-by trailers for 18 AI coding tools (Claude,
  Copilot, ChatGPT, Codex, Cursor, Devin, Aider, Cody, Tabnine, Gemini,
  Amazon Q, v0.dev, Lovable, Bolt.new, Codeium/Windsurf, Continue.dev,
  Roo Code, Cline) over last 200 commits; commits that *removed* AI library
  imports (historical-usage signal).

### Added — new output and UX

- **SPDX 3.0 AI Profile** output (JSON-LD): `--format spdx`.
- **Local web viewer**: `vouchsafe scan --serve` runs a localhost-only
  HTTP server on `127.0.0.1:8765+` that displays the HTML report,
  with `/report.json` available for programmatic access.
- **Diff mode**: `vouchsafe scan --since <git-ref>` limits scanning to
  files changed since the given commit / branch / tag. Falls back to a
  full scan gracefully when git isn't available.
- **`vouchsafe init`**: generates a starter `.vouchsafe.yml` config in
  the project root.
- **Pretty terminal output**: the scan summary now opens with a Rich
  panel showing finding count / vendor count / category count / elapsed
  time — designed for screenshot-friendly demo output.

### Added — distribution

- **Docker image** published to `ghcr.io/kgovind-coder/vouchsafe`,
  multi-arch (linux/amd64 + linux/arm64), built and pushed by the release
  workflow on every tag.
- **GitHub Action** at the repo root (`action.yml`): users can
  `uses: kgovind-coder/vouchsafe@v0.2.0` in their workflows to download
  the right binary, run the scan, expose `findings-count` / `vendor-count`
  outputs, and optionally fail the build on findings.

### Tested

- **84 unit tests passing** (up from 52 in v0.1).
- Full cross-OS CI matrix (3 OS × 3 Python) plus release smoke tests for
  every shipped binary.

## [0.1.0] — 2026-05-31

## [0.1.0] — 2026-05-31

The first public release. A scanner that detects AI / LLM / agent / MCP
touchpoints in a codebase and produces audit-ready evidence in seven output
formats, mapped to five regulatory frameworks.

### Detectors (16)

- **ai-endpoint** (`<vendor>`) — URL regex for 24 AI providers (OpenAI, Anthropic, Azure, Vertex, Bedrock, Cohere, Mistral, Replicate, Together, Groq, Fireworks, Perplexity, xAI, DeepSeek, Alibaba DashScope, ElevenLabs, AssemblyAI, Deepgram, OpenRouter, Voyage, Jina, HuggingFace). Catches vendored SDKs, minified bundles, shell scripts, OpenAPI specs.
- **self-hosted-llm-endpoint** — Ollama, LM Studio
- **env-var** — 65+ AI-related env var names; **presence only**, values never read
- **python-sdk-import** — 60+ Python AI SDKs (providers, agent frameworks, vector DBs, classical ML, eval/guardrails, AI gateways)
- **python-manifest** — `requirements*.txt`, `pyproject.toml`, `Pipfile`, `setup.py`, `environment.yml`
- **js-sdk** — 60+ JS/TS packages incl. Vercel AI SDK family + LangChain JS family, in source imports and `package.json`
- **mcp-config** — Config files for Claude Desktop, Cursor, VS Code Copilot, Continue, Windsurf, Cline + per-server enumeration with command fingerprint
- **model-artifact** — 14 extensions (`.pt`, `.safetensors`, `.gguf`, `.onnx`, `.tflite`, `.h5`, `.pkl`, etc.) + magic-byte sniffing for ambiguous `.bin`
- **coding-assistant** — 8 distinct detectors (Claude Code, Cursor, Aider, GitHub Copilot, Continue.dev, Codeium/Windsurf, OpenAI Codex CLI, Google Gemini CLI) using marker-file matching to avoid noise

### Output formats (7)

- **JSON** with versioned schema (`$schema`), deterministic key order, byte-identical for the same input
- **Markdown** human-readable report with executive summary, findings by category, component inventory
- **SARIF 2.1.0** for GitHub Code Scanning / Azure DevOps Pipelines native ingestion
- **CycloneDX 1.6 ML-BOM** (JSON) — components typed as `library` / `machine-learning-model` / `service` / `application` with framework-mapping properties
- **HTML** single-file report (inline CSS, no external resources, deterministic)
- **CSV** — `findings.csv` (one row per finding) + `components.csv` (one row per unique vendor)
- **JUnit XML** for CI gating

### Regulatory framework mappings

Every finding is auto-tagged with framework reference IDs:

- **EU AI Act Annex IV** (sections s1–s9 of the technical file required by Article 11)
- **ISO/IEC 42001 Annex A** (A.4.2, A.4.3, A.4.4, A.6.1.3, A.6.2.x, A.7.x, A.8.2, A.10.3)
- **OWASP Top 10 for LLM Applications 2025** (LLM01–LLM10)
- **OWASP Top 10 for Agentic Applications Dec 2025** (ASI02 Tool Misuse, ASI03 Identity & Privilege Abuse)
- **NIST AI RMF + AI 600-1 GenAI Profile** (value_chain, information_security, data_privacy, information_integrity, human_ai_configuration)
- **MITRE ATLAS** technique IDs (AML.T0002, T0010, T0011.000, T0024.x, T0034, T0040)

### Core infrastructure

- Deterministic filesystem walker (lexicographic sort, gitignore-aware skip list, symlink cycle detection)
- SHA-256 hashing utilities (file content, snippet identity)
- Path / Unicode normalization (forward-slash, NFC, locale-independent)
- Frozen dataclass models (`Finding`, `Evidence`, `ScanReport`, `ScanStats`) with stable sort keys
- RFC 3339 UTC timestamps with millisecond precision
- `StrEnum`-based `Category` and `Confidence` (Python 3.11+)

### CLI

- `vouchsafe scan PATH` with `--format` (multi-valued), `--output` prefix, `--quiet`, `--no-files`
- `vouchsafe verify REPORT` (placeholder for v0.6.0 Ed25519 signature verification)
- `vouchsafe --version`
- Rich-rendered terminal summary table

### Distribution

- Pre-built standalone binaries on GitHub Releases for Linux x64, macOS universal, Windows x64
- Each binary ships with a SHA-256 checksum file
- Apache License 2.0 — free for any use including commercial; explicit patent grant. (v0.1.0 originally shipped under PolyForm Noncommercial 1.0.0; relicensed in the [Unreleased] section above before any commercial uptake.)
- Cross-platform builds via PyInstaller in GitHub Actions
- Codespaces devcontainer (Python 3.12 + uv)

### Tested

- **52 unit tests passing**
- Determinism verified: two scans of identical input produce byte-identical output (modulo timestamps)
- Cross-platform CI matrix: 3 OS × 3 Python versions

### Known limitations (planned for v0.2+)

- Language coverage limited to Python + JS/TS in v0.1; Java/Kotlin/C#/Go/Rust/etc. coming
- No Ed25519 signing yet (planned for v0.6.0)
- No PDF generation yet (planned for v0.7.0)
- No SPDX 3.0 output yet (planned for v0.5.0)
- No Snowflake CORTEX / BigQuery ML / Salesforce Einstein / SAP Joule yet (planned for v0.3.0–v0.4.0)
- No git history scanning yet (planned for v0.4.0)

See [docs/ROADMAP.md](docs/ROADMAP.md) for the full plan.
