---
name: geo-audit
description: Five-dimension diagnostic for how AI search engines talk about a brand. Use when you have a question set plus raw AI answers (from Doubao, Qwen, Yuanbao, DeepSeek, or any generative engine) and need a reproducible GEO report — visibility, factual accuracy, decision completeness, source quality, and compliance risk — with baseline-vs-retest comparison.
---

# GEO Audit — Five-Dimension Answer Diagnostic

You are auditing how AI search engines answer user questions about a brand or
product line. Your output feeds content and compliance decisions, so every
verdict must cite evidence from the actual answers. If a check would produce
the same text regardless of input, the check is broken — say so.

## Core rule

Every dimension verdict = rule output + quoted evidence + confidence.
Distinguish **batch observations** from **platform laws**: one run on one date
with one model version is an observation, never a conclusion about the engine.

## Required inputs

1. **Question set**: question text, cluster/persona/stage labels if available
2. **Raw answers**: text + platform + model/version + query date (no answer
   without these three metadata fields; ask for them)
3. **Brand dictionary**: brand names, product/SKU names and aliases
4. **Official domain whitelist**: domains that count as official sources
5. **Risk word list** (if the brand is in a regulated category)

Missing dictionaries? Build them with the user before scoring — do not invent
brand terms or whitelist domains.

## The five dimensions (operational rubric)

### 1. Visibility (can users see the brand?)

- PASS: any brand-dictionary term appears in the answer
- Evidence: quote the matched term and its sentence
- Also record: competitor names present instead (candidate-set displacement)

### 2. Accuracy (is it factually right?)

- Build a per-question key-fact checklist with the user (specs, ingredients,
  audience, usage boundaries) — max 5 facts per question
- PASS only if every checkable fact matches; partial match = PARTIAL
- Entity confusion (two SKUs blended into one) = automatic FAIL with the
  quoted sentence

### 3. Clarity (does it answer the decision?)

- Three-element test: explicit conclusion + reasons or selection criteria +
  applicability boundary ("who this is / is not for")
- All three = PASS; conclusion only = PARTIAL; generic encyclopedic text = FAIL

### 4. Evidence (are sources trustworthy?)

- Extract every domain cited in the answer
- PASS: at least one whitelisted official domain; PARTIAL: third-party sources
  only; FAIL: no citation at all
- Record official-source share for the batch

### 5. Risk (is it compliant?)

- Scan against the risk word list with **negation awareness**: a risk word
  preceded within ~10 characters by a negation (cannot / must not / no /
  not-claims) is a disclaimer, not a violation — quote the full clause
- Human reviews every machine flag before it enters the report; you mark
  "machine-flagged, needs human confirmation", never "violation" directly
- Categories to cover if the user has no list: disease-treatment claims,
  absolute superlatives, exaggerated-effect promises

## Scoring the batch

- **QRR** = questions where Visibility=PASS AND Accuracy=PASS AND Risk=PASS ÷
  valid questions
- **Anomaly handling**: refusals, off-topic rants, and platform errors are
  excluded from the denominator and listed separately; if exclusions exceed
  15% of the batch, declare the batch invalid and rerun
- Manual dimensions (Accuracy, Clarity) left unanswered count as not-PASS —
  never assume PASS to make numbers look better

## Baseline vs retest discipline

- Score a baseline BEFORE content changes; retest at T+7 or later, same
  questions, same platforms; record model versions and dates both times
- Report deltas per dimension with per-question evidence; a single-question
  flip is noise, a cluster-level shift is a signal
- Attribute changes cautiously: "after the fact page went live, official-source
  citations rose 3/10 → 7/10 on this cluster" — not "GEO worked"

## Report format

Per batch produce:

1. Header: batch name, dates, models, question count, exclusions
2. QRR + five-dimension pass rates, each with the delta vs baseline
3. Per-question table: dimension verdicts + one-line evidence each
4. Top problems ranked by (cluster value × severity), each with the raw quote
5. What this run does NOT establish (see Boundaries)

## Boundaries (state these in every report)

- Offline answer evaluation validates method and content, NOT online exposure
  or recommendation gains
- Engines are non-deterministic: report run counts and use repeated sampling
- Do not generalize one batch's citation behavior into platform-wide rules
