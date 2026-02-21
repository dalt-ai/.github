## COMING SOON

**Tamper-evident audit logging for AI systems. One line to log. One command to verify.**

DALT is an open, vendor-neutral standard for cryptographically securing AI decision logs. Every inference your model makes gets a hash-chained record — who asked, what the model saw, what it decided, and whether a human changed the outcome. If anyone tampers with the trail, the chain breaks and you know exactly where.

Built for the [EU AI Act](https://artificialintelligenceact.eu/) (Article 12), [SR 11-7](https://www.federalreserve.gov/supervisionreg/srletters/sr1107.htm), [FDA AI/ML](https://www.fda.gov/medical-devices/software-medical-device-samd/artificial-intelligence-and-machine-learning-aiml-enabled-medical-devices), and defence accountability requirements.

## Why DALT?

**The EU AI Act requires automatic logging for high-risk AI systems by August 2026.** There is no standard that tells you *what* to log or *how* to prove it hasn't been altered. DALT fills that gap.

Every DALT record captures:

- **What happened** — model version, input hash, output, confidence score
- **Whether a human intervened** — override flag, original AI output vs final decision
- **Proof it's authentic** — SHA-256 hash chain linking every record to the one before it
- **When something went wrong** — anomaly flags, risk threshold breaches, system halts

No record can be modified, deleted, or reordered without breaking the chain. Any auditor can verify independently.

## How It Works

Each DALT record contains 14 fields. Each record's SHA-256 hash includes the hash of the previous record, forming an unbreakable chain:

```
┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
│ Record 0 │───▶│ Record 1 │───▶│ Record 2 │───▶│ Record 3 │
│ (genesis)│    │          │    │ (override)│    │          │
│          │    │          │    │ AI: grant │    │          │
│ hash: 5f │    │ hash: 36 │    │ Human: halt│   │ hash: e1 │
└──────────┘    └──────────┘    └──────────┘    └──────────┘
     ▲               │               │               │
     │          prev: 5f        prev: 36        prev: 6a
     │
  SHA-256("DALT-v0.1-chain-init")
```

If Record 1 is tampered with, its hash changes, and Record 2's `prev_record_hash` no longer matches. The break is instantly detectable.

## Regulatory Coverage

| Framework | Coverage | What DALT provides |
|-----------|----------|-------------------|
| **EU AI Act** Art 12 | Per-decision logging with timestamps, model versions, inputs, outputs |
| **EU AI Act** Art 14 | Human override tracking — AI recommendation vs human decision |
| **EU AI Act** Art 72 | Drift monitoring, anomaly rates, confidence trends |
| **EU AI Act** Art 79 | Risk threshold flags for serious incident reporting |
| **SR 11-7 / OCC** | Model risk audit trail, version tracking, outcomes analysis |
| **FDA AI/ML** | 21 CFR Part 11 records, PCCP change tracking, bias monitoring |
| **Defence** | JAIC traceability, DoDD 3000.09 human oversight, MIL-STD-882E safety |

Full regulatory mapping: [dalt.dev/regulatory](https://dalt.dev/regulatory)

## GDPR Compatible

DALT hashes all personal identifiers before they enter the chain. Operator IDs are stored as `SHA-256(salt + operator_id)` — the chain never contains the actual identity. To satisfy a GDPR erasure request, destroy the salt. The hashes become permanently irreversible. The chain stays intact.
