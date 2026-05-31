# Vouchsafe Detector Catalog

This is the canonical list of detectors shipped with Vouchsafe.
Each detector has a **stable ID** that auditors will reference; once
published, the ID does not change.

## Conventions

- **Detector ID:** kebab-case, dotted hierarchy. Format: `<family>.<vendor>` or just `<family>` for whole-class detectors.
- **Confidence:** `high` = unambiguous signal (import, manifest, exact filename), `medium` = suggestive (ambiguous extension), `low` = comment/marker only.
- **Category:** see `src/vouchsafe/core/models.py::Category`.

---

## AI provider endpoints

`ai-endpoint.<vendor>` — Universal URL regex. Catches AI providers in any text file regardless of programming language.

**Covered vendors (24):** `openai`, `anthropic`, `azure-openai`, `google-gemini`, `google-vertex`, `aws-bedrock`, `cohere`, `mistral`, `replicate`, `together-ai`, `groq`, `fireworks-ai`, `perplexity`, `xai`, `deepseek`, `alibaba-dashscope`, `elevenlabs`, `assemblyai`, `deepgram`, `openrouter`, `voyageai`, `jina-ai`, `huggingface`

**Trigger:** any `https?://api.<provider>.com/...` URL appearing in a text file. Catches vendored SDKs, minified bundles, shell scripts using curl, Postman collections, OpenAPI specs.

---

## Self-hosted LLM endpoints

`self-hosted-llm-endpoint.ollama` — Connections to `:11434` (Ollama default port).
`self-hosted-llm-endpoint.lm-studio` — Connections to `:1234` (LM Studio default port).

---

## Environment variables

`env-var.<vendor>` — AI-related environment variable **names** (presence only, values are never read).

**Covered:** 65+ env vars across OpenAI, Anthropic, Azure, Google, AWS, HuggingFace, Cohere, Mistral, Replicate, Together, Groq, Fireworks, Perplexity, xAI, DeepSeek, DashScope, Qwen, Moonshot, ZhipuAI, Baichuan, Yi, Reka, AI21, Stability, ElevenLabs, AssemblyAI, Deepgram, OpenRouter, Voyage, Jina, Pinecone, Weaviate, Qdrant, LangChain, LangSmith, LangFuse, Helicone, Portkey, LiteLLM, Lakera, MLflow, W&B, Neptune, Comet, Tavily, Exa, Brave, Serper, Roboflow, LlamaCloud.

---

## Python SDKs (60+)

`python-sdk-import.<vendor>` — Matches `import X` / `from X import Y` in `.py` files.

Covers: OpenAI, Anthropic, Cohere, Mistral, Google Gemini/Vertex, Replicate, Together, Groq, Fireworks, xAI, DashScope, Qianfan, Reka, AI21, Stability, ElevenLabs, AssemblyAI, Deepgram, Whisper variants, HuggingFace ecosystem (transformers/sentence-transformers/diffusers/datasets/accelerate/peft/trl/safetensors/tokenizers), self-hosted runners (Ollama, llama-cpp-python, vLLM, TGI, MLC-LLM, GPT4All), agent frameworks (LangChain, LangGraph, LangServe, LangSmith, LlamaIndex, Haystack, Semantic Kernel, DSPy, CrewAI, AutoGen, AG2, Agno, PydanticAI, smolagents), vector DBs (Pinecone, Weaviate, Qdrant, Milvus, Chroma, LanceDB, redisvl), eval/guardrails (DeepEval, Promptfoo, Ragas, TruLens, Guardrails AI, NeMo Guardrails, LLM Guard, Rebuff), AI gateways (LiteLLM, Portkey), classical ML (scikit-learn, XGBoost, LightGBM, CatBoost, TensorFlow, Keras, PyTorch, JAX, ONNX runtime, MLflow, W&B, Neptune), Ultralytics YOLO.

---

## Python manifests

`python-manifest.<vendor>` — AI/ML packages declared in dependency manifests:

- `requirements*.txt`, `constraints.txt`
- `pyproject.toml` (`[project] dependencies`, `[project.optional-dependencies]`, `[tool.poetry.dependencies]`, `[tool.poetry.dev-dependencies]`, `[tool.poetry.group.*.dependencies]`, `[tool.uv] dev-dependencies`)
- `Pipfile`
- `setup.py` (`install_requires=[...]` regex parse)
- `environment.yml` / `conda.yaml`

Covers the same vendor set as Python SDKs above, plus distribution-name aliases (e.g. `huggingface-hub` → `huggingface`, `google-generativeai` → `google-gemini`).

---

## JavaScript / TypeScript SDKs (60+)

`js-sdk.<vendor>` — Matches `import`, `require()`, and `import()` specifiers in `.js`, `.ts`, `.jsx`, `.tsx`, `.mjs`, `.cjs`, `.vue`, `.svelte`, `.astro` files **and** dependency entries in `package.json` (`dependencies`, `devDependencies`, `peerDependencies`, `optionalDependencies`, `bundledDependencies`).

Covers: OpenAI, Anthropic SDK (+ Bedrock + Vertex variants), Azure OpenAI, Google (`@google/genai`, `@google/generative-ai`), Vertex (`@google-cloud/vertexai`, `@google-cloud/aiplatform`), AWS Bedrock (`@aws-sdk/client-bedrock-runtime`, `@aws-sdk/client-bedrock-agent-runtime`), Cohere, Mistral, Replicate, Together, Groq, Fireworks, ElevenLabs, AssemblyAI, Deepgram, Ollama, HuggingFace (`@huggingface/inference`, `@huggingface/hub`, `@huggingface/transformers`, `@xenova/transformers`), ONNX Runtime web/node, TensorFlow.js, MLC web-llm, Vercel AI SDK family (`ai`, `@ai-sdk/*`), LangChain JS family (`@langchain/*`, `langchain`), LangGraph, LangSmith, LlamaIndex, Mastra, Inngest, Pinecone, Weaviate, Qdrant, Milvus, Chroma, LanceDB, Turbopuffer, Promptfoo, LangFuse, Traceloop, Lunary, Portkey.

---

## MCP (Model Context Protocol)

`mcp-config.config` — MCP config file detected (medium confidence).
`mcp-config.server` — One finding per server declared inside the config (high confidence, with `extra.server` and `extra.command`).

**Config file locations recognized:**
- `.mcp.json` (Claude Code project)
- `mcp.json` (Cursor or VS Code Copilot)
- `claude_desktop_config.json` (Claude Desktop)
- `cline_mcp_settings.json` (Cline)
- `mcp_config.json` (Windsurf)
- `.cursor/mcp.json` (Cursor)
- `.vscode/mcp.json` (VS Code Copilot)
- `.gemini/settings.json` (Gemini CLI)

---

## Model artifacts

`model-artifact.<format>` — pre-trained model weights bundled with the repo.

**Extensions:** `.pt`, `.pth`, `.safetensors`, `.gguf`, `.ggml`, `.onnx`, `.tflite`, `.keras`, `.h5`, `.pkl`, `.pickle`, `.joblib`, `.mlmodel`, `.trt`, `.engine`, `.plan`.

**Magic-byte detection** (for ambiguous `.bin` files):
- `GGUF` → `gguf-llama-cpp`
- `\x89HDF\r\n\x1a\n` → `keras-hdf5`
- `\x80\x02`-`\x80\x05` → `python-pickle` (also a security flag — Pickle is unsafe)

---

## Coding-assistant footprints (8)

Each detector matches only the **marker file** for that tool — never the entire directory it lives in — to avoid noise. One finding per distinct marker.

| Detector ID | Tool | Marker file(s) |
|---|---|---|
| `coding-assistant.claude-code` | Claude Code | `CLAUDE.md`, `.claude/settings.json`, `.claude/settings.local.json`, `.mcp.json` |
| `coding-assistant.cursor` | Cursor | `.cursorrules`, `.cursorignore`, `.cursor/mcp.json`, `.cursor/settings.json` |
| `coding-assistant.aider` | Aider | `.aider.conf.yml`, `.aiderignore`, `.aider.*.history*` |
| `coding-assistant.github-copilot` | GitHub Copilot | `.github/copilot-instructions.md`, `.copilot-instructions.md`, `.copilotignore` |
| `coding-assistant.continue-dev` | Continue.dev | `.continue/config.{json,yaml}`, `.continuerc.json`, `.continueignore` |
| `coding-assistant.codeium-windsurf` | Codeium / Windsurf | `.windsurfrules`, `.codeiumignore` |
| `coding-assistant.openai-codex-cli` | OpenAI Codex CLI | `AGENTS.md` |
| `coding-assistant.google-gemini-cli` | Gemini CLI | `GEMINI.md` |

---

## Roadmap (not yet implemented)

Future detector packs:

- All 15 languages: Java/Kotlin (Maven, Gradle, LangChain4j, Spring AI), C#/.NET (NuGet, Semantic Kernel, ML.NET), Go (go.mod), Rust (Cargo.toml), Ruby (Gemfile), PHP (composer.json), Swift (Package.swift, Core ML, FoundationModels), Dart/Flutter (pubspec.yaml, ML Kit), Scala, R, Julia, Elixir
- Cloud IaC: Terraform `aws_bedrock_*`, `azurerm_cognitive_*`, `google_vertex_ai_*`; Helm charts (vllm, triton, tgi, kserve); Kubernetes InferenceService CRDs
- SQL AI: Snowflake CORTEX, BigQuery ML (`ML.GENERATE_TEXT`), pgvector
- Vendor-embedded AI: Salesforce Einstein (Apex), ServiceNow Now Assist, SAP Joule, Microsoft Copilot Studio
- Game engines: Unity Sentis (`.onnx` under `Assets/`), Unreal NNE
- Browser AI: Chrome `window.ai`, Transformers.js usage
- Git history: co-author trailers (Claude, Copilot, GPT, Codex, Cursor, Devin, Aider, Cody, Tabnine, Gemini, Q, v0, Lovable, Bolt) over last 6 months
- AI in stored procedures, shell scripts via curl, OpenAPI / gRPC specs
