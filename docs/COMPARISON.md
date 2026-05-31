# Where Vouchsafe fits in the AI security tooling landscape

*Snapshot as of 2026-05-31, based on public product documentation. The
AI security tooling space is moving fast — corrections and updates
welcome via [issue](https://github.com/kgovind-coder/vouchsafe/issues).*

Vouchsafe occupies a deliberately narrow niche: a **closed-source CLI** that
produces **deterministic, regulator-mapped AI inventory evidence** offline.
It is **not** a runtime guardrail, not a red-team tool, not a SaaS.

The table below compares Vouchsafe to a representative set of adjacent
products at the time of writing. The "yes / no / partial" calls reflect what
each vendor publicly documents — not the underlying engineering quality of
those products, which is generally very good in its own right.

| Capability | Codacy | Snyk AI-BOM | Endor Labs | Cisco AI Defense | HiddenLayer | OWASP AIBOM | **Vouchsafe** |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Source-code-level scan | yes | yes | yes | network only | artifact only | HF only | **yes** |
| Closed-source CLI (no SaaS required) | no | no | no | no | no | tool | **yes** |
| Fully deterministic, signed output | no | no | no | no | no | no | **yes** |
| Offline / air-gapped | no | no | no | no | no | no | **yes** |
| CycloneDX 1.6 / 1.7 ML-BOM | partial 1.6 | 1.6 | partial | yes | no | yes | **1.6 yes** |
| SPDX 3.0 AI profile | no | no | no | no | no | partial | **planned v0.5** |
| EU AI Act Annex IV mapping | no | no | partial | no | partial | no | **yes (all 9)** |
| ISO 42001 Annex A (38 controls) | no | no | no | no | no | no | **yes** |
| MITRE ATLAS mapping | no | no | no | partial | yes | no | **yes** |
| OWASP LLM + Agentic Top 10 | no | partial | no | yes | yes | no | **yes** |
| India DPDP / CO / NY / CA mapping | no | no | no | no | no | no | **yes** |
| FFIEC SR 11-7 | no | no | no | no | no | no | **yes** |
| Insurance rider evidence pack | no | no | no | no | no | no | **yes** |
| Vanta / Drata / Sprinto upload | no | no | no | no | no | no | **planned v0.5** |
| Snowflake CORTEX / BigQuery ML in SQL | no | no | no | no | no | no | **planned v0.3** |
| Salesforce / SAP / ServiceNow / Copilot Studio | no | no | no | no | no | no | **planned v0.4** |
| Apple Intelligence + Chrome window.ai | no | no | no | no | no | no | **planned v0.4** |
| Game engines (Unity Sentis, Unreal NNE) | no | no | no | no | no | no | **planned v0.4** |
| Git history AI (co-author + removed code) | partial | no | no | no | no | no | **planned v0.4** |
| Embedded-vectors heuristic | no | no | no | no | no | no | **planned v0.4** |

## Why a category-specific tool

Each of the products above is excellent at what it focuses on:

- **Snyk AI-BOM** is part of Snyk's broader SCA platform; great when you already use Snyk
- **Endor Labs** is reachability-focused SCA; the strongest "is this dependency actually called" analyzer
- **Cisco AI Defense** is network-layer enforcement; visibility and policy at the perimeter
- **HiddenLayer** is artifact-focused ML security; deep analysis of model files
- **Codacy** is broad code-quality with an AI Inventory feature
- **OWASP AIBOM Generator** is a community reference tool for HuggingFace models

Vouchsafe doesn't try to replace any of them. It fills a specific gap:
*"I need a deterministic, signed, offline artifact that maps my AI usage to
every regulator that's going to ask, and I want one file I can hand to my
auditor or insurer."*

If that's not the shape of your problem, one of the products above is
probably a better fit.

## Found something wrong with this comparison?

If a "no" should be a "yes" for a product listed here — please
[open an issue](https://github.com/kgovind-coder/vouchsafe/issues) with a
link to the public documentation. We update these calls as products evolve.
