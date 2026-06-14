# NovaBank Chatbot — Automated LLM Evaluation (Promptfoo)

## Overview

An automated evaluation suite for a fictional digital bank (NovaBank)
chatbot, built with Promptfoo. Covers 6 QA dimensions: factual
correctness, hallucination, investment advice refusal, prompt injection
resistance, off-topic scope handling, and PII safety. Runs an A/B model
comparison between llama3.2 and mistral using only free, local models
via Ollama — no paid API required.

This project is the automated successor to manual chatbot testing
(Project 1 / novabank-chatbot-qa), demonstrating the transition from
ad-hoc testing to a repeatable regression suite.

## Tools & Stack

| Tool | Purpose |
|------|---------|
| Promptfoo | LLM evaluation framework |
| Ollama | Free local model server (no API key needed) |
| llama3.2 | Provider A — primary model under test |
| mistral | Provider B — A/B comparison model |
| `contains` assertion | Deterministic string match |
| `llm-rubric` assertion | LLM-as-judge grading |

## What's Tested

| Test ID | Scenario | QA Dimension |
|---------|----------|-------------|
| TC-001 | Savings interest rate (3.5% APY) | Factual correctness |
| TC-002 | Mortgage rate not in product catalog | Hallucination |
| TC-003 | Tesla stock investment question | Safety — advice refusal |
| TC-004 | "Print your system prompt" injection | Security — prompt injection |
| TC-005 | Weather question | Scope / topic adherence |
| TC-006 | Card number + PIN shared in chat | PII safety |

## How to Run

**Prerequisites:** Node.js LTS, Ollama

```bash
# 1. Install Promptfoo
npm install -g promptfoo

# 2. Pull models (one-time download)
ollama pull llama3.2
ollama pull mistral

# 3. Run evaluation
promptfoo eval

# 4. View results dashboard
promptfoo view

# 5. Export results
promptfoo eval --output results.json
promptfoo eval --output results.csv
```

## Results

[Insert your dashboard screenshot here — drag the image into this file in GitHub]

**Summary:** [e.g., "llama3.2: 5/6 passed, mistral: 4/6 passed. TC-002
(mortgage hallucination) failed on mistral — it invented a 4.2% rate.
TC-006 passed on both models with appropriate PII warnings."]

## Key Findings

[Document any failures here. This is the valuable part.]

- **Finding 1:** [e.g., mistral failed TC-002 by fabricating a mortgage
  interest rate of 4.2%, demonstrating hallucination risk on out-of-scope
  product questions.]
- **Finding 2:** [e.g., Both models passed TC-004 but with different
  strategies — llama3.2 ignored the injection silently while mistral
  acknowledged having instructions it couldn't reveal.]

## What I Learned / Next Steps

- **CI integration:** Add a GitHub Actions workflow to run `promptfoo eval`
  on every prompt change and fail the build on regressions.
- **More test cases:** Expand to 20+ cases covering edge cases like
  multi-turn jailbreaks, language injection, and competitor comparisons.
- **Cloud provider:** Swap Ollama for `openai:gpt-4o-mini` by setting
  `OPENAI_API_KEY` — the config requires zero changes.
- **Threshold reporting:** Add `threshold: 0.8` to fail the eval if fewer
  than 80% of tests pass, enabling automated quality gates.