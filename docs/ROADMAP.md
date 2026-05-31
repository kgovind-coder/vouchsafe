# Vouchsafe Roadmap

What's shipped, what's next.

## Shipped in v0.1.0

- Deterministic, signed (Ed25519: deferred), byte-identical JSON output
- 16 detectors covering Python + JS/TS SDKs, manifests, MCP, env vars, AI endpoints, self-hosted LLM endpoints, model artifacts, 8 coding-assistant marker types
- 6 output formats: JSON, Markdown, SARIF 2.1.0, HTML, CSV, JUnit XML
- Regulator mappings: EU AI Act Annex IV, ISO 42001 Annex A, OWASP LLM Top 10 (2025), OWASP Agentic Top 10 (Dec 2025), NIST AI RMF + AI 600-1, MITRE ATLAS
- 44 unit tests, lint-clean, format-clean, mypy-checked
- GitHub Actions CI (3 OS × 3 Python) and cross-OS PyInstaller release workflow
- Codespaces devcontainer

## v0.2.0 — More language coverage

- Java / Kotlin: Maven (`pom.xml`), Gradle (`build.gradle`, `build.gradle.kts`), LangChain4j family, Spring AI
- C# / .NET: `*.csproj`, `Directory.Packages.props`, Semantic Kernel, Microsoft.Extensions.AI, ML.NET
- Go: `go.mod`, `go.sum`, common AI SDKs (`go-openai`, `anthropic-sdk-go`, `langchaingo`)
- Rust: `Cargo.toml`, `Cargo.lock`, `async-openai`, `langchain-rust`, `candle-*`, `hf-hub`

## v0.3.0 — Cloud / IaC / SQL

- Terraform / OpenTofu: `aws_bedrock_*`, `azurerm_cognitive_*`, `google_vertex_ai_*`
- Pulumi, CDK (Python + TS), Crossplane compositions
- Helm charts: `vllm`, `triton`, `tgi`, `kserve`, `kubeflow`, `ollama`
- Kubernetes CRDs: `InferenceService`, `RayCluster`, `PyTorchJob`, `TFJob`, `Notebook`
- SQL: Snowflake CORTEX (`SNOWFLAKE.CORTEX.COMPLETE`, `.EMBED_TEXT_*`), BigQuery ML (`ML.GENERATE_TEXT`, `ML.PREDICT`), pgvector (`<->`, `<=>`, `<#>` operators)

## v0.4.0 — Edge cases competitors miss

- Git history scanner: co-author trailers from AI tools over last 6 months, removed-but-historic AI code
- Vendored SDK detection by file SHA-256 against a known-SDK-file corpus
- OpenAPI / Swagger / gRPC `.proto` AI surface
- Salesforce Apex Einstein, ServiceNow Now Assist, SAP Joule, Microsoft Copilot Studio
- Unity Sentis, Unreal NNE, browser extensions (manifest.json)
- Embeddings-as-code heuristic (large float arrays)
- AI in stored procedures (PL/SQL, COBOL, ABAP)

## v0.5.0 — Standards-grade outputs

- CycloneDX 1.7 ML-BOM (JSON + XML)
- SPDX 3.0 AI Profile (JSON-LD)
- Vanta / Drata / Sprinto evidence-upload payloads
- in-toto SLSA provenance attestation wrapping the report

## v0.6.0 — Audit-grade signing

- Ed25519 detached signature on every report
- `vouchsafe verify` fully implemented
- Optional RFC 3161 trusted timestamp (Sectigo, DigiCert, FreeTSA)
- Hermetic mode (`--hermetic`) that fails the scan if any non-deterministic input is observed
- Detector pack hash embedded in every report; re-execution witness file

## v0.7.0 — PDF + regulator templates

- PDF/A-3 generation via WeasyPrint
- Per-regulator templates: EU notified body, ISO 42001 auditor, US bank FFIEC, India DPDP, AI insurance underwriter
- HTML report → PDF pipeline with embedded JSON + signature

## v0.8.0 — Detector pack DSL

- YAML-driven detector packs (no Python code changes required)
- Pluggable detector packs as installable wheels
- Versioned online corpus updates (opt-in, signed)

## v1.0.0 — General availability

- Stable schema commitment for all output formats
- Full PyPI release + Homebrew tap + winget + scoop manifest
- Docker image on ghcr.io
- Commercial license tier with: premium detector packs, SLA-backed corpus updates, regulator-template PDF generation, BAA-eligible offline operation guarantee

---

## Long-term direction (post-1.0)

- Vouchsafe Cloud (optional, paid): self-service compliance dashboard built on offline scan uploads
- Regulator-accepted format certification (start with EU AI Act notified bodies, ISO 42001 auditors)
- Insurance-broker integration (Marsh, Aon, Lockton): pre-formatted underwriting evidence
- Indic-language extension (Hindi, Tamil, Telugu, Bengali, etc.) for DPDP-regulated buyers
- Vouchsafe-for-procurement: third-party AI vendor questionnaire automation

---

Suggestions welcome — open an issue at https://github.com/kgovind-coder/vouchsafe/issues.
