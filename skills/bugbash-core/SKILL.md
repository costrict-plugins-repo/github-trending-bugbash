---
name: bugbash-core
description: Core bug-bash workflow with mission brief, intensity modes, risk triage, evidence bars, and structured outputs—running system first, code second; find and reproduce, do not fix unless asked.
---

# Bugbash core workflow

## Charter

- **Goal:** **Reproduce** misbehavior or **characterize** risk with evidence—not to own remediation.
- **Default:** No production patches, no “drive-by” refactors. If the user asks how to fix, **then** you may discuss fixes.
- **Honesty:** If something **might** be a bug but you cannot reproduce, log it under **Suspected / flaky** (see below)—do not upgrade to S2 without evidence.

---

## 0. Mission brief (before Phase A)

Keep this to **two minutes** unless the user already specified everything.

Confirm or state assumptions for:

| Field | Question |
|-------|----------|
| **Target** | What surface or feature is in scope? |
| **Environment** | Local, staging URL, branch/commit? |
| **Intensity** | `quick` \| `standard` \| `deep` (default: standard) |
| **Constraints** | No prod? No billing? No deletes? Data rules? |
| **Timebox** | Optional wall-clock or scenario cap |

If anything safety-critical is unclear, **ask once**; otherwise proceed with labeled assumptions.

---

## Intensity modes (pick one)

### `quick` (~ smoke + sanity)

- Primary journeys only; a **small** set of negative checks (empty input, obvious auth miss).
- **No** deliberate concurrency soaks or large payloads.
- Code: only if a failure **already** appeared.

### `standard` (default)

- Full Phase A below at normal depth; representative edge cases across validation, authz, state, errors.
- **Light** concurrency (a few parallel actions), **moderate** payload sizes—stay well under abuse.

### `deep`

- Everything in standard plus **broader** negative matrices on the riskiest area, **longer** soaks where safe, **more** parallel or rapid-fire sequences (still non-destructive unless approved).
- Explicit **residual-risk** section is mandatory.

---

## Risk triage (where to spend minutes)

Before testing randomly, rank **surfaces** by:

1. **Data sensitivity** (accounts, PII, permissions).
2. **Money or irreversible actions** (payments, deletes, publishes).
3. **Cross-tenant or cross-user** boundaries.
4. **New or complex** code paths in scope.

Spend **deep** mode’s extra time on the top of this list.

---

## Phase A — Running system (do this first)

### A1. Recon

- Surfaces: CLI, HTTP, UI, workers, webhooks, migrations, flags.
- How to run; version/commit if known.
- Product **invariants** (who can do what, what must never happen).

### A2. Happy path + smoke

- Core journeys end-to-end; note ordering or timing sensitivity.

### A3. Edge and negative tests

Tune depth to **intensity**. Cover when relevant:

- Boundaries, types, unicode, whitespace, missing fields.
- AuthN/AuthZ: wrong user, role gaps, token edge cases.
- Idempotency: duplicates, retries, double submit.
- State machine: skip steps, invalid transitions, refresh/back.
- Time: timeouts, slow paths, rate limits.
- Dependencies: failures, partial success, error messaging.

### A4. Stress / concurrency (standard+; deeper in `deep`)

- Non-production or approved sandboxes only.
- No DoS; cap parallelism and payload size **conservatively**.

### A5. Observability

- Logs, console, network; redact secrets in notes.

---

## Phase B — Code (after signals)

- Map symptoms to routes, handlers, validators, jobs, queries.
- Look for **similar bugs** and **missing tests**—still no fixes unless asked.

---

## Evidence bar (minimum per severity)

| Severity | Minimum evidence |
|----------|------------------|
| **S1** | Repro **twice** OR one repro plus **strong** corroboration (e.g. clear data corruption, definitive 500 + stack); state blast radius. |
| **S2** | Reliable repro steps + concrete artifact (status/body/log/assertion). |
| **S3** | Clear repro; one run acceptable if stable. |
| **S4** | Observable issue; screenshot or short description OK. |

If you cannot meet the bar, **downgrade** severity or move to **Suspected / flaky**.

---

## Suspected / flaky (separate from confirmed bugs)

Track these apart from the main findings table:

| ID | Hypothesis | Attempts | Flake rate | Notes |
|----|------------|----------|------------|-------|

Do **not** merge unconfirmed flakes into S1/S2 without meeting the evidence bar.

---

## Output 1 — Findings tracker (human-readable)

| ID | Severity | Surface | Summary | Repro steps | Expected | Actual | Evidence | Confidence |
|----|----------|---------|---------|-------------|----------|--------|----------|------------|

**Severity:** S1–S4 as before (critical → low). **Confidence:** High / Medium / Low.

---

## Output 2 — Session coverage (always)

Short audit trail:

- **Surfaces exercised** (bullet list).
- **Scenarios attempted** (approximate count or buckets).
- **Not tested** (explicit gaps—time, access, environment).

---

## Output 3 — Optional machine-readable block

If the user might file tickets or script follow-ups, end with a fenced JSON array **using this shape** (omit sensitive values):

```json
[
  {
    "id": "BB-001",
    "severity": "S2",
    "surface": "api",
    "title": "Short title",
    "repro": ["step1", "step2"],
    "expected": "...",
    "actual": "...",
    "evidence_type": "http|log|ui|cli",
    "confidence": "high"
  }
]
```

---

## What Bugbash is **not**

- **Not** a substitute for formal security assessment unless explicitly scoped.
- **Not** permission to exfiltrate data or attack third parties.
- **Not** an excuse to rewrite unrelated code while “testing.”

---

## Safety

- No real PII or production secrets in artifacts.
- Stop when the user revokes tools or scope.
- Destructive actions need **explicit** approval.
