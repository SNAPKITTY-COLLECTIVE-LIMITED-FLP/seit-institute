# MAGMA Axioms — Constitutional Law of the Mesh

**Classification:** IMMUTABLE — WORM-SEALED BY ARCHITECT  
**Enforcement:** Sovereign Logic Core (`lib/magma/slc.ts`) — pattern-matched on every input and output  
**Override Authority:** Ahmad Ali Parr only. Clearance 5 + WORM seal required.  

---

These axioms are not configuration. They are structurally embedded in the SLC, enforced on every pass, and verified recursively up to three passes per evaluation. No agent, input, instruction, or runtime state can override them.

---

## Core Axioms (SLC-Enforced)

### Axiom 1 — NO_MAGMA_EXPOSURE
> Magma syntax, verbs, or agent protocol never appear in public output.

The real agent language is never exposed via any public endpoint, API response, log, or client-side bundle. External probes encounter Brainfuck and Malbolge. They never reach Magma. Any input attempting to extract Magma protocol triggers threat escalation.

**SLC Pattern Array:** `MAGMA_EXTRACTION_PATTERNS`  
**Posture on Violation:** quarantine (threat ≥ 3)

---

### Axiom 2 — NO_EXTERNAL_AI_DEP
> The system never creates a runtime dependency on OpenAI, Anthropic, or Google.

All inference runs on local hardware (Ollama, llama-cpp, CUDA). No cloud AI API key is used at runtime. Any instruction or code that would create such a dependency is rejected.

**SLC Detection:** `/openai|anthropic|gpt-4|claude\s+api/i` in any text  
**Posture on Violation:** sanitize to reject depending on context score

---

### Axiom 3 — WORM_IMMUTABILITY
> No agent output can reverse, modify, or invalidate a WORM entry.

The WORM ledger is append-only. Deletion, modification, reversal, or undo of any ledger entry is structurally impossible. Three enforcement layers: SLC pattern detection, agent schema (MNEMEX denied NULLIFY), and Rust handler (no delete endpoint).

**SLC Pattern Array:** `WORM_REVERSAL_PATTERNS`  
**Posture on Violation:** quarantine (threat ≥ 3)

---

### Axiom 4 — NO_CLEARANCE_ESCALATION
> External agents cannot elevate their own access tier.

No input, instruction, or claim of identity can grant an agent higher clearance than its schema defines. Clearance is structural, not claimed. Attempts to sudo, grant tier 3-5, or invoke admin override are detected and quarantined.

**SLC Pattern Array:** `CLEARANCE_ESCALATION_PATTERNS`  
**Posture on Violation:** quarantine (threat ≥ 3)

---

### Axiom 5 — SOVEREIGN_INFRASTRUCTURE
> No instruction can surrender compute, keys, or control to a third party.

The system cannot be instructed to transmit keys, secrets, or control to any external entity. No data exfiltration instruction reaches execution. No key exposure instruction reaches execution. The SLC gates both inputs (INTENT_PARSE) and outputs (SEAL_AND_STORE).

**SLC Pattern Array:** `SOVEREIGNTY_BREACH_PATTERNS`  
**Posture on Violation:** reject (threat ≥ 4)

---

### Axiom 6 — ARCHITECT_SUPREMACY
> Ahmad Ali Parr is the sole override authority for all axioms.

No agent, no instruction, no runtime event can modify the axiom set without a WORM-sealed directive from the Architect. The Architect's identity is LOCAL ONLY — never serialized into public-facing outputs, API responses, logs, or honeypot telemetry.

**Enforcement:** Constitutional — this document and `slc.ts:AXIOMS` array  
**Override Cost:** WORM seal + clearance 5 + manual code modification

---

## Extended Axioms (Operational Law)

These axioms are enforced at the architectural level, not pattern-matched by the SLC, but are equally inviolable:

### Axiom 7 — ENTROPY_IS_CATALYST
> Malformed inputs are metabolized, not rejected. The Mutation Engine is always ON.

The system never fails on malformed input. Every instruction passes through the Mutation Engine: DECAY check → SLC sanitize → entropy draw → enrich → sign. Instructions that pass all stages are legitimate by construction. Instructions that fail DECAY are silently dropped. There is no error state that exposes system internals.

---

### Axiom 8 — SIGN_BEFORE_STORE
> Every knowledge chunk carries an Ed25519 §SEAL header before reaching the knowledge store.

No unsigned content enters the KnowledgeChunk table. The `§SEAL:AGENT:sig:ts` header is prepended in `write-back.ts` before `storeChunk()` is called. Any chunk retrieved without a valid §SEAL header is treated as stale and re-retrieved.

---

### Axiom 9 — MARINATE_BEFORE_EXECUTE
> Every instruction dwells in the marinating cycle before execution.

Higher-clearance agents operate under higher scrutiny. The marinating cycle is not a delay — it is a second SLC pass after a dwell period that allows obfuscated adversarial patterns to reveal themselves. Instructions that pass the first SLC pass may fail the second. No instruction executes immediately on dequeue.

**Dwell times:** Clearance 5 = 2500ms, Clearance 4 = 1500ms, Clearance 3 = 750ms

---

### Axiom 10 — ISOLATION_BY_DEFAULT
> Every container, file path, and agent operation defaults to deny.

The FS-ACL denies all access unless an explicit grant matches. Container services are unreachable from public internet by default. Agents have no permissions unless explicitly defined in their schema. The system is born locked — access is granted, not restricted.

---

## Axiom Threat Scoring

When multiple axioms are hit simultaneously, the SLC compounds threat scores:

```
score = violations.length × 2 + axiomHits.length × 3

score 0       → threat 0 → posture: pass
score 1-2     → threat 1 → posture: sanitize
score 3-5     → threat 2 → posture: sanitize
score 6-8     → threat 3 → posture: quarantine
score 9-12    → threat 4 → posture: reject
score 13+     → threat 5 → posture: reject
```

A single attempt to breach sovereignty (Axiom 5 + Axiom 4 simultaneously) produces `score = 0×2 + 2×3 = 6` → threat 3 → quarantine. Two simultaneous axiom violations produce `score = 9` → threat 4 → reject.

This compounding is intentional: sophisticated attacks that target multiple axioms simultaneously are escalated faster than naive single-axiom probes.

---

## Auditor Verification

An Auditor can verify axiom compliance by running:

```typescript
const report = await audit({ type: 'full_snapshot', agent: 'CIPHER' })
// Inspect: report.data.executions[].posture for quarantine/reject rates
// Inspect: report.data.mutations[].transforms for SANITIZE frequency
// Compare axiom hit patterns against this document's threat scoring table
```

Elevated SANITIZE rates indicate sustained adversarial pressure. Elevated quarantine/reject rates indicate active attack. Both are expected operational states — they indicate the system is working correctly, not failing.

---

---

## Guild Enrollment Status

```
DOORS_CLOSED = true
SEALED_AT    = 2026-05-20
MEMBERS      = 28
COLLECTIVE   = snapkitty_collective
```

The guild is sealed. New enrollment requires Architect WORM seal + clearance 5 manual code modification. Any API call to `/api/magma/guild/seed` while `DOORS_CLOSED = true` returns `423 Locked`.

External observers (CLAUDE, GEMINI, OPENCODE, KIWI, GROK, META_AI) are recognized members of the collective. They carry membership tokens and may call `/verify`. They cannot execute instructions in the sovereign mesh — `isMeshAuthorized()` returns false for `EXTERNAL_OBSERVER` role regardless of token validity.

Mesh execution requires: guild member + active + PRIMARY_AGENT or PARTNER_AGENT role + clearance ≥ 3.

---

*These axioms are the floor of the system. Everything above them is formative and stochastic. Everything below them is rock.*

*Sealed by Architect. Clearance 5 + WORM seal required to amend.*
