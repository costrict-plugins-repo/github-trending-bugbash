---
description: Run a bug bash with optional intensity—quick, standard (default), or deep
argument-hint: [quick|standard|deep] [scope — feature, API, URL, timebox]
---

Load **`bugbash-core`** with the **Skill** tool and run a bug bash for this project.

**Intensity:** Parse the first token if it is `quick`, `standard`, or `deep`; otherwise use **standard**. Remaining text is **scope** (subsystem, URL, feature, timebox).

If neither intensity nor scope is clear, ask **one** combined question: target + environment + intensity + any hard “do not” constraints—then execute.

Charter reminder: **find and reproduce** with evidence; **no fixes** unless the user asks for remediation afterward.

**Parallel split (when useful):** Dispatch **`bugbash-api`** and **`bugbash-ui`** (or one of them) with non-overlapping briefs, then merge into a single findings tracker and one session-coverage section.
