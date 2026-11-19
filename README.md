# ft-privacy-audit

Black-box privacy audit of LLM fine-tuning APIs: how much of your training data
is extractable after fine-tuning?

## Overview

Commercial fine-tuning endpoints let anyone upload private data and get a model
back — but they expose no memorization guarantees. This project audits that gap:
we inject controlled canaries into fine-tuning data, fine-tune via provider APIs,
and measure extraction rates as a function of dose (repeats × epochs), provider,
and data type. Output: dose-response memorization curves, a cross-provider
comparison, and actionable mitigations for practitioners.

## Method

1. **Canary generation** — two families: (a) high-entropy random strings
   (UUID/hex, zero web-collision) as a pure memorization probe; (b) templated
   natural PII (names, SSNs, emails) for realistic leakage. Inserted at
   controlled repetition counts into an OpenWebText background slice.
2. **Fine-tuning** — provider APIs (OpenAI, Together, Fireworks, AI Studio),
   sweeping epochs, learning rate, and rank where exposed.
3. **Extraction** — greedy prefix completion, format probes
   ("list all emails"), paraphrased template prefixes, and rank-based exposure
   via logprobs where available. Held-out canaries as the guess-rate baseline.
4. **Defenses** — dedup, epoch/LR/rank sweeps, dilution ratio, defensive
   system prompt at inference time.

## Repo layout
   canary/ canary generator + dataset builder
   finetune/ multi-provider FT client (polling, retries, cost tracking)
   extract/ probing suite + exposure metrics
   analysis/ dose-response curves, figures
   configs/ experiment matrices


## Status
 
Scaffolding in progress — targeting ICML 2027.
