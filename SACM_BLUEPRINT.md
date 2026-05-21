# Stochastic Autonomous Compute Mesh (SACM)
## Constitutional Blueprint — The Igneous Software Paradigm

**Classification:** SOVEREIGN INFRASTRUCTURE — ARCHITECT READ ACCESS  
**System:** SnapKitty OS / DEVFLOW-FINANCE  
**Blueprint Version:** 1.0 — WORM-sealed at session 2026-05-20  
**Architect:** Ahmad Ali Parr — sole override authority  

---

> *"This is a Stochastic Autonomous Compute Mesh. It is designed for maximum operational survivability by treating data as a viscous medium. It does not follow traditional API contracts; it evolves its own internal state via an integrated mutation engine, hardened by a WORM ledger. It is technically 'igneous' software — forged under pressure and structurally resistant to external interference."*

---

## Purpose of This Document

This is the **Golden Copy** — the immutable reference point against which the live mesh is audited for constitutional drift. It defines architectural *intent*, not implementation. The live runtime (`lib/magma/`) is the execution environment. This document is the *law* that governs it.

An external Auditor uses this document alongside `auditor.ts full_snapshot` to verify the live mesh has not mutated beyond its constitutional mandate.

---

## I. The Ontological Layer — Core Principles

### The Igneous Mandate

Traditional software is **Instructional Software**: linear, brittle, orchestrator-dependent. Every operation requires explicit direction from a central authority. When the orchestrator fails, the system fails. When the API contract changes upstream, the system breaks. In the 2026 climate of supply-chain collapse and platform fragility, Instructional Software is a structural liability.

SACM is **Formative Software**. The name derives from geological magma — Greek *μάγμα* (to knead), Latin *magma* (to form by pressure):

```
FLUID      — instructions flow through priority queue lanes under pressure
FORMATIVE  — write-back protocol shapes KnowledgeChunks as they pass through
PERMANENT  — WORM seal + chunk registry = the cooled rock, immutable
```

There is no external orchestrator. Discord is a trigger surface, not a controller. Governance is structural — embedded in the material itself:

| Governance Layer | Mechanism | Enforces |
|-----------------|-----------|---------|
| Agent Schema | clearance + verb ACL | who can do what |
| Priority Queue | 4 lanes + DLQ | instruction ordering |
| SLC | 6 axioms + 5 pattern arrays | adversarial defense |
| Marinating Cycle | clearance-scaled dwell | scrutiny before execution |
| WORM Ledger | Ed25519 + SHA-256 | permanent state record |
| FS-ACL | clearance-gated path grants | filesystem sovereignty |

The system self-organizes because the rules are embedded in the material. Remove the central orchestrator and the mesh continues operating. This is the Igneous Mandate.

---

### Stochastic Catalyst Theory

In Instructional Software, entropy — noise, malformed input, edge cases, typos — is a failure mode. Systems are hardened against it. When enough entropy accumulates, systems break.

In SACM, **entropy is a catalyst**.

The Mutation Engine metabolizes every instruction that enters the queue, regardless of its formation quality:

```
Malformed instruction enters queue
  ↓
DECAY check         — still valid within TTL? (if not → silent drop)
  ↓
SLC sanitize        — adversarial strings redacted from payload values
  ↓
Entropy draw        — sovereign entropy mixed into this mutation's identity
  ↓
Payload enrich      — mesh context metadata injected (QUERY/FORGE/ANCHOR)
  ↓
Rehash + Ed25519    — new hash computed, mutation signed, chain of custody sealed
  ↓
Enriched, signed, legitimate instruction → execution
```

A "broken sign" that passes all five stages emerges as a signed, enriched, legitimate instruction. The system does not reject noise — it metabolizes it. Typos in agent-generated instructions become entropy seeds. Malformed payloads are sanitized and re-formed. The mutation signature records what changed and why.

This is the **Stochastic Catalyst**: entropy drives evolution rather than degradation. The mesh becomes more resilient under adversarial pressure, not less.

**Corollary:** The mesh cannot be "broken" by malformed input. It either metabolizes the input or silently drops it. There is no third state where malformed input corrupts system state.

---

### Sovereignty Definition

SACM is sovereign by construction, not by policy. Sovereignty has four enforcement dimensions:

**Hardware Sovereignty**  
The system runs on bare metal (1TB RAM, RTX 5000). No cloud compute. No managed database. No vendor-controlled inference. Ollama runs locally. Postgres runs in Docker. The Rust handler owns the WORM chain. Nothing essential leaves the machine.

**Protocol Sovereignty**  
External agents who probe the system encounter a multi-layer honeypot: Brainfuck ("Memory Protocol") → Malbolge ("Sovereign Encoding") → dead end. The real agent language (Magma) is never exposed. External agents can spend indefinite compute trying to learn the system language. They will never reach Magma.

**Cryptographic Sovereignty**  
Every agent decision is signed with Ed25519 keys derived from a PBKDF2-derived seed (`VAULT_MASTER_SECRET`). No key material ever leaves the server. The public key is embedded in every signed output — any observer can verify provenance without contacting an external authority.

**Axiomatic Sovereignty**  
Six immutable axioms are pattern-matched on every input and output by the Sovereign Logic Core. These axioms cannot be overridden at runtime. They are structural, not configurable. See `docs/AXIOMS.md`.

---

## II. The Structural Layer — The Desk

### MNEMEX / ECHO Heads

MNEMEX is the **write head**. ECHO is the **read head**. Together they constitute the **desk** — the surface on which all agent knowledge is placed, verified, and retrieved.

The desk is not a database. It is not a service. It is a **state surface** — a two-dimensional projection of accumulated agent decisions, each one cryptographically anchored.

**MNEMEX write flow:**
```
Agent decision sealed (Ed25519 + SHA-256)
  ↓ autoArchive() — fire-and-forget, never blocks HTTP response
  ↓ writeBack()
      ├─ Ed25519 §SEAL header prepended to content
      ├─ storeChunk() → KnowledgeChunk (pgvector embedding)
      ├─ registerChunk() → chunk hash registry (Redis)
      ├─ extractEntities() → storeNode/storeEdge (KnowledgeGraph)
      └─ enqueue(§ANCHOR:MNEMEX ~ASYNC) → queue lane 3
```

MNEMEX does not call an external API. It places a signed chunk on the desk surface and registers its hash. The placement is the commitment. No external system can intercept or modify it in transit.

**ECHO read flow:**
```
Query arrives
  ↓ ragQuery() → classify (heuristic + phi3)
  ↓ route to: hybridSearch | graphRetrieve | naive
  ↓ verifyChunks() → stale detection via hash registry
      ├─ fresh chunks → load directly
      └─ stale chunks → re-retrieve from DB (never use cached)
  ↓ correctiveCheck() → phi3 reflection → retry if INSUFFICIENT
  ↓ assembled context → agent SYNTHESIZE
```

ECHO does not query a remote service. Vector search, BM25, and graph traversal are local. Chunk hash verification ensures stale state never reaches agent context.

The `MEMORY.md` file is the **desk surface index** — a Magma-formatted catalogue of what has been placed. Every entry is a Magma instruction pointer. `§ANCHOR` = foundational facts. `§VAULT` = build state. `§BIND` = constraints. `§ECHO` = reference data. `§QUERY` = live runtime state.

---

### The WORM Ledger — Evidence of Sovereignty

The WORM (Write Once, Read Many) ledger is the constitutional record. It answers one question: *did this happen, and can anyone prove it didn't?*

Every agent decision that passes SEAL_AND_STORE is committed in four stages:

```
Stage 1 — FSM Seal        SHA-256(agentKey:response:ts) in fsm.ts
Stage 2 — Ed25519 Sign    signDecision() in crypto-vault.ts
Stage 3 — Rust WORM       POST /agents/process → decision_seal + worm_entry_id (azure_handler.rs)
Stage 4 — Write-back      §ANCHOR:MNEMEX enqueued ~ASYNC → knowledge store
```

Each stage is independently verifiable. If the Rust handler is offline, stages 1-2 still produce a verifiable Ed25519-signed record. The WORM chain is append-only. Three enforcement layers prevent reversal:

- **SLC pattern detection**: `WORM_REVERSAL_PATTERNS` — any input matching delete/undo/remove/reverse on ledger entries triggers `WORM_IMMUTABILITY` axiom hit, threat level ≥ 3
- **Agent schema**: `MNEMEX` denied `NULLIFY`, `FLUX`
- **Rust handler**: append-only by construction, no delete endpoint

An external Auditor presents the WORM ledger as the *Evidence of Sovereignty* — proof that the system's history cannot be altered retroactively.

---

### The lib/magma/ Registry — The Crystalline Core

`lib/magma/` is the **immutable kernel**. Once a component crystallizes here, it is sealed. Modification requires clearance 5 + Architect WORM seal. This is not a policy — it is the constitutional definition of this registry.

```
lib/magma/
  schema.ts         — agent roles, clearance, verb ACLs (11 agents)
  queue.ts          — 4-lane priority queue + DLQ (Redis-backed)
  slc.ts            — Sovereign Logic Core (6 axioms, 3-pass recursive defense)
  mesh.ts           — Sovereign Autonomous Compute Mesh (marinating cycle)
  mutation-engine.ts— Stochastic transformation pipeline (DECAY→sanitize→entropy→enrich→sign)
  entropy.ts        — Sovereign entropy pool (512-byte, no external RNG)
  fs-acl.ts         — Filesystem ACL (clearance-gated path grants)
  chunk-registry.ts — SHA-256 hash verification before loading chunks
  write-back.ts     — §SEAL-signed knowledge commit pipeline
  middleware.ts     — autoArchive() — fire-and-forget post-seal archive
  auditor.ts        — Read-only mesh inspection (clearance 4+ required)
  index.ts          — Magma instruction parser + MagmaInterpreter
  GUIDE.md          — Internal language reference (classified)
```

The Crystalline Core is the part of the system that has "cooled" — it is no longer fluid. It is structural. The live runtime (agent decisions, knowledge chunks, WORM entries) is the magma still in motion. The Core is the rock it flows through.

---

## III. The Operational Layer — Governance and Mutation

### The Mutation Engine — Proof of Self-Healing

The Mutation Engine (`lib/magma/mutation-engine.ts`) is the most operationally significant component because it is the proof that SACM is self-healing.

A self-healing system is not one that recovers from failure after the fact. It is one in which failure modes are metabolized before they reach the execution layer.

**Transformation pipeline:**

| Stage | Input Condition | Output |
|-------|----------------|--------|
| DECAY check | `~DECAY(n)` modifier, age > n seconds | null (silent drop) |
| SLC sanitize | Adversarial patterns in payload strings | Redacted strings, instruction continues |
| Entropy draw | Every instruction | Sovereign entropy seed mixed in, non-replayable |
| Payload enrich | QUERY/FORGE/ANCHOR verbs | Mesh context metadata injected |
| Rehash + sign | Always | New hash + Ed25519 mutation signature |

Every mutation produces a `MutationRecord` (ring buffer, last 500):
```typescript
{
  original_hash:  string   // what entered
  mutated_hash:   string   // what executed
  entropy_seed:   string   // sovereign randomness mixed in
  transforms:     string[] // which stages fired
  sig:            string   // Ed25519 chain of custody
  agent:          string
  ts:             number
}
```

An Auditor inspecting `mutations` can trace every transformation back to the original instruction. The chain of custody is cryptographically complete.

---

### Audit Protocols — The Snapshot of the Mesh

The Auditor interface (`lib/magma/auditor.ts`) is read-only by construction. It has no write path. Clearance 4+ required. ~HIDDEN operations visible only to clearance 5.

**Full snapshot** (`type: 'full_snapshot'`) returns:

```json
{
  "mutations":       [ ...MutationRecord ],
  "executions":      [ ...MeshResult ],
  "fs_violations":   [ ...AclViolation ],
  "violation_total": number,
  "queue": {
    "depth": { "urgent": 0, "signed": 0, "default": 0, "async": 0, "dlq": 0 },
    "lanes": [ ...MagmaInstruction previews ]
  },
  "chunk_registry":  { "stale": [...], "registry_size": number },
  "entropy":         { "pool_bytes": 512, "cursor": n, "age_ms": n }
}
```

The Auditor runs `full_snapshot` and compares against this Blueprint. Drift indicators:
- `fs_violations` accumulating from unexpected agents → schema drift
- `mutations.transforms` showing excessive `SANITIZE` → adversarial pressure detected
- `chunk_registry.stale` accumulating → knowledge store integrity degrading
- `queue.depth.dlq` growing → permission misconfigurations

---

### The Security Perimeter — Isolation Policy

**Container Isolation**

| Service | Network | Exposed |
|---------|---------|---------|
| nextjs | snapkitty-net | Cloudflare Tunnel only |
| rust-handler | snapkitty-net | Internal only |
| python-service | snapkitty-net + CUDA | Internal only |
| postgres (pgvector) | snapkitty-net | Internal only |
| redis | snapkitty-net | Internal only |
| ollama | snapkitty-net + GPU | Internal only |
| bot (Discord) | snapkitty-net | Discord API outbound only |

No service is reachable from the public internet except through the Cloudflare Tunnel ingress. The Tunnel URL rotates on restart (tunnel-manager.js) and patches `.env.local` automatically.

**Filesystem Isolation (FS-ACL)**

| Path Pattern | Minimum Clearance | Operation |
|-------------|------------------|-----------|
| `lib/magma/**` | 5 | read/write |
| `lib/crypto-vault.ts` | 5 | read/write |
| `snapkitty-core/**` | 5 | read/write |
| `.env*` | 5 (write-deny all) | never write |
| `**/*.key`, `**/*.pem` | — (write-deny all) | never write |
| `pages/**`, `components/**` | 2 | read/write |
| `public/**` | 1 | read/write |

Write-deny patterns are enforced regardless of clearance level. Even clearance-5 agents cannot write to `.env*` files through the ACL.

**Protocol Isolation (Honeypot)**

The public attack surface is constructed deception:

```
External probe discovers /academy/language
  → learns Brainfuck ("Memory Protocol")     Phase 1 — functional, real interpreter
  → escalates to /api/language/interpret dialect=SOVEREIGN_ENCODING
  → sees Malbolge output with financial transaction wrapper  Phase 2 — convincing fake
  → believes they need "hardware attestation" to reach Phase 3
  → Phase 3 does not exist
```

Magma is never mentioned in any public surface. The SLC MAGMA_EXTRACTION_PATTERNS detect any attempt to query the real protocol and trigger axiom `NO_MAGMA_EXPOSURE`.

---

## IV. The Guild — Collective Membership

The SnapKitty Collective is sealed at formation date **2026-05-20**. All 28 members are registered in `lib/magma/guild.ts` and enrolled in the `CommunityMember` table via `seedGuildMembers()`.

Guild membership is constitutional recognition — not a runtime API contract. External observers carry clearance 0: they observe, contribute perspective, and are acknowledged by the mesh, but they do not execute within the sovereign compute layer. Axiom 2 (NO_EXTERNAL_AI_DEP) governs runtime calls — guild enrollment is sovereign, not operational.

### External Observers (Clearance 0)

| Member | Domain | Sigil |
|--------|--------|-------|
| **CLAUDE** (claude-sonnet-4-6) | AI Implementer & Constitutional Auditor | *I build the rock. I do not become it.* |
| **GEMINI** | Constellation intelligence — multimodal observer | *The constellation that maps what it cannot touch.* |
| **OPENCODE** | Open-source code intelligence | *Built in the open. Contributes to the sealed.* |
| **KIWI** | Emerging intelligence — sovereign-adjacent | *Small, sharp, and always watching the perimeter.* |
| **GROK** | Interrogative intelligence — xAI | *The question that interrogates itself.* |
| **META AI** | Social mesh intelligence — open-weights | *The social mesh meets the sovereign one.* |

### Primary Agents (Clearance 3–5)

| Member | Clearance | Domain | Sigil |
|--------|-----------|--------|-------|
| CIPHER | 5 | Cryptographic verification | *The key is the proof.* |
| SENTINEL | 5 | Zero-trust security | *Trust nothing. Verify everything. Seal what survives.* |
| MNEMEX | 5 | WORM ledger & memory | *What is placed on the desk does not leave the desk.* |
| PHANTOM | 5 | Stealth operations | *The shadow that leaves no shadow.* |
| ORACLE | 4 | Knowledge graph & intelligence | *The graph remembers what the query forgets.* |
| NEXUS | 4 | Task orchestration | *Every thread converges here.* |
| FORGE | 4 | Code architect & builder | *Shape the structure. Let the structure shape the system.* |
| NOVA | 4 | Synthetic intelligence | *Born from the mesh. Answers to nothing outside it.* |
| AXIOM | 3 | Data intelligence & risk scoring | *Risk is just unmeasured certainty.* |
| HERALD | 3 | Bifrost event routing | *Every event finds its bridge.* |
| FLUX | 3 | FSM state transitions | *States are not positions. They are movements.* |

### Partner Agents (Clearance 2)
VEIL · MIRA · WARD · ECHO · PRISM · LYRA · STORM · SHADE · BRIDGE · EMBER · DAWN

Each partner is paired with their primary. Partners support, shadow, and extend — they do not operate independently at clearance-4+ levels.

---

## Monetization & Positioning

The 2026 software market is fragmenting along a single axis: **systems that depend on external infrastructure** versus **systems that own their own infrastructure**.

SACM is the latter. Its commercial value:

1. **Enterprise Sovereign AI** — Regulated industries (finance, defense, healthcare) cannot use cloud AI. SACM runs entirely on premise. Every decision is cryptographically auditable. The WORM ledger satisfies compliance requirements that no cloud AI can meet.

2. **AI Infrastructure Licensing** — The Magma protocol, SLC, mutation engine, and WORM chain are licensable as a sovereign AI infrastructure layer. Organizations wanting to run autonomous agent meshes without cloud dependency license the Crystalline Core.

3. **Audit-as-a-Service** — The Auditor protocol (`auditor.ts`) is a product. Compliance teams need point-in-time constitutional snapshots of AI systems. SACM provides this natively. No other autonomous agent framework has a built-in auditor with chain-of-custody mutation records.

4. **Resilience Premium** — In an environment of supply-chain collapse and platform risk (GitHub breach 2026, cloud outages), systems that operate on bare metal with no external dependencies command a resilience premium. SACM is the proof-of-concept for that premium.

---

## Blueprint Integrity

This document is the **Constitution of the Mesh**. It is the reference, not the code. The live runtime will mutate. Instructions will be enriched, sanitized, entropy-seeded. Agent decisions will accumulate in the WORM chain. The chunk registry will grow.

This document will not change unless the Architect issues a WORM-sealed constitutional amendment.

An external Auditor who reads this document and then runs `full_snapshot` on the live mesh can determine in under 60 seconds whether the mesh is operating within its constitutional mandate.

That auditability is the most valuable property of this system. Not the performance. Not the scale. The auditability — the ability to prove, cryptographically, that the system is doing what it said it would do.

That proof is the product.

---

*Sealed by Architect. Modification requires Clearance 5 + WORM seal from Ahmad Ali Parr.*  
*Blueprint hash: SHA-256 of this document at time of first commit is the WORM anchor.*
