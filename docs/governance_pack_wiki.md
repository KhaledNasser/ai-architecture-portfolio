# Governance Pack: Marketing Automation with AI & Agents

Agents generate personalized campaigns, a scoring agent rates them, and low-score or high-risk ones go to a human (see diagram). **Goals:** G1 trust · G2 compliance (PDPL, PCI) · G3 brand protection · G4 speed · G5 cost/risk control.

## Architecture principles

| Principle | Statement | Goal |
| --- | --- | --- |
| P1 Transparency by design | Customers always see what data we use and why (consent, opt-out, “why you got this” note). | G1, G2 |
| P2 Data stays in our control | Identifiable data leaves us only when necessary and PDPL-permitted; PII masked before external AI. | G2, G1 |
| P3 Risk-based human oversight | Agents send alone only if score ≥ threshold and risk is low; all decisions logged. | G3, G4 |
| P4 Security by sensitivity | For sensitive data, security beats cost: encryption, separate identity store, PCI provider. | G2, G5 |

## Architecture decision records

| ADR | Options considered | Decision and trade-off accepted |
| --- | --- | --- |
| 001 LLM access | External API · Self-hosted · Hybrid | Hybrid: self-hosted for identifiable data, API on masked data; extra masking step (P2, P4) |
| 002 Vector DB | Managed cloud · Dedicated self-hosted · PostgreSQL + vector | PostgreSQL + vector in-region; fewer systems, revisit at scale (P2, P4) |
| 003 Real-time vs batch | All real-time · All batch · Hybrid | Chat real-time, campaigns batched with review queue; campaigns not instant (P3, P1) |

## ARB checklist (any “no” blocks go-live)

| Area | Checks (evidence required) |
| --- | --- |
| Security | Encrypted in transit/at rest? Identity store separated? Channels pen-tested? PCI provider? |
| Privacy (PDPL) | Consent recorded and withdrawable? PII masked? Why-note on campaigns? Cross-border transfers justified? |
| Evals | Scorer agrees with human marketers? Outputs tested for accuracy, tone, bias? Re-tests scheduled? |
| Cost | Monthly cost estimated? Budget alerts and limits set? |
| Human oversight | High-risk always reviewed? Threshold signed off? Decisions logged? Stop switch tested? |

**Guardrail (CI):** fails a config change if an external AI call skips PII masking (P2), auto-send lacks a threshold (P3), or a sensitive store is unencrypted (P4).
