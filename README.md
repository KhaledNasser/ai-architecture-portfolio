# AI Architecture Portfolio

My coursework and capstone for the **AI Architecture Design** course with **Eng. Sameh Amin**.

This repo follows the course session by session. It records the design decisions I made, the reasons behind them, and how they add up to a final system architecture.

## Scenario

**Marketing Automation with AI & Agents.** A company engages customers through WhatsApp and its website. AI agents answer customer queries in real time and generate personalized marketing campaigns based on each customer's history and preferences. A scoring agent rates every campaign: high-scoring, low-risk campaigns are sent automatically, while low-scoring or high-risk ones are escalated to a human marketer for review. Human decisions are fed back to improve the agents.

## Assumptions

- Customers interact through WhatsApp and the website; first-time users sign up and give consent.
- Personal data is subject to PDPL, and payments are subject to PCI requirements.
- Identifiable customer data must stay in-region; external AI services only receive masked data.
- Identity/payment data and analytics data are stored separately.
- Chat replies must be real-time; campaign generation can run in scheduled batches.
- Audience volume is moderate, so a single PostgreSQL database with a vector extension is sufficient at launch.

## Session Progress

| Session | Mini assignment | Deliverables |
|---------|-----------------|--------------|
| 1 | **Governance pack**: architecture principles, ADRs, ARB checklist, and an automated guardrail | [Wiki page](docs/01-governance-pack.md) · [Architecture diagram](diagrams/01-marketing-ai-governance.drawio) |

### Session 1: Governance Pack

Defines the rules the system must follow before it is built.

- **4 architecture principles**, each linked to a business goal: transparency by design, data stays in our control, risk-based human oversight, and security proportional to data sensitivity.
- **3 ADRs**: hybrid LLM access (self-hosted for identifiable data, API for masked data), PostgreSQL with a vector extension, and real-time chat with batched campaigns.
- **ARB checklist** covering security, privacy (PDPL), evals, cost, and human oversight.
- **Automated guardrail**: a CI check that blocks configuration changes violating the principles.

## Repository Structure

| Folder | Contents |
|--------|----------|
| [`docs/`](docs/) | Session notes, one file per session (`01-governance-pack.md`, …) |
| [`diagrams/`](diagrams/) | Architecture diagrams, as exported PNG/SVG plus the editable source files |
| [`adr/`](adr/) | Architecture Decision Records, one per decision (`0001-model-hosting.md`, …) |
| [`capstone/`](capstone/) | Final architecture document (PDF) and presentation slides |

## How to Read This Repo

1. **Start with the scenario and assumptions** above for context.
2. **Follow `docs/` in order** to see how the design developed over the sessions.
3. **Check `adr/`** for the reasoning and trade-offs behind each major decision.
4. **Look at `diagrams/`** for the visual architecture.
5. **Finish with `capstone/`** for the complete final design.