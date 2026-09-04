# BIZZ PROJECT

> **An AI-native cybersecurity defense and SOC engineering project built
> to monitor, detect, analyze, and protect — while simultaneously
> developing the cybersecurity knowledge of its Founder.**

**Founder:** Mae  
**Project Type:** AI-Native Cybersecurity / SOC Engineering / Learning Lab  
**Current Stage:** Early Development  
**Environment:** Authorized Defensive Laboratory

---

## Mission

BIZZ PROJECT is an independent AI-native cybersecurity defense platform
and cybersecurity engineering project.

Its mission is to develop defensive capabilities that help security
analysts:

**MONITOR → DETECT → ANALYZE → PROTECT**

BIZZ PROJECT is being designed to:

- monitor authorized environments;
- detect suspicious behavior;
- investigate security events;
- correlate evidence across telemetry;
- assist threat hunting;
- map activity to MITRE ATT&CK;
- explain security findings;
- recommend defensive actions;
- support incident analysis;
- gradually introduce carefully controlled defensive automation.

But BIZZ PROJECT has a second mission:

> **As BIZZ PROJECT becomes more capable, its Founder should become more
> capable too.**

The project is therefore both a cybersecurity platform and a practical
SOC engineering learning environment.

---

# Why BIZZ PROJECT Exists

Modern security operations generate enormous amounts of telemetry.

Security analysts must determine:

- What happened?
- Is the activity actually suspicious?
- Which systems and users are involved?
- What evidence supports the alert?
- How are multiple events related?
- What should be investigated next?
- What defensive action is justified?

BIZZ PROJECT explores how AI can assist this process without replacing
evidence, analyst reasoning, or human authority.

The goal is not to build an AI that simply declares:

> "This is malicious."

The goal is to build a system capable of saying:

> "This activity is suspicious because these events occurred, these
> entities are related, this behavior may correspond to these techniques,
> this is what remains uncertain, and this is what should be investigated
> next."

---

# Core Principle

BIZZ PROJECT follows:

> **AI reasons about evidence. AI does not create the evidence.**

AI-generated statements are not security evidence.

Evidence should originate from authorized and verifiable sources such as:

- Windows Event Logs;
- Sysmon;
- Linux logs;
- Wazuh;
- SIEM telemetry;
- endpoint telemetry;
- network telemetry;
- authentication events;
- process events;
- authorized threat intelligence.

AI may interpret, correlate, summarize, explain, and generate hypotheses
from that evidence.

It must not manufacture it.

---

# SOC Philosophy

BIZZ PROJECT follows four foundational principles:

### AI-First Development

AI agents assist research, engineering, cybersecurity analysis, testing,
and documentation.

### Zero-Trust Security

Access, actions, data, and automation should not be trusted merely
because they originate from an AI or internal component.

### Evidence-Grounded Decisions

Security conclusions should be supported by observable evidence.

### Human-Controlled Impact

Consequential defensive actions remain under human control until higher
levels of automation have been explicitly tested and authorized.

---

# AI Reasoning Model

BIZZ PROJECT aims to distinguish security reasoning into four categories:

### FACT

Directly supported by available telemetry.

### INFERENCE

A conclusion reasonably derived from evidence.

### HYPOTHESIS

A plausible explanation requiring further investigation.

### UNKNOWN

Something not established by available evidence.

This distinction is intended to reduce hallucination, unsupported
conclusions, and analyst overconfidence.

---

# Progressive AI Autonomy

BIZZ PROJECT does not begin as an autonomous security system.

Autonomy progresses gradually:

### L0 — Observe

Read authorized telemetry.

### L1 — Investigate

Query, correlate, construct timelines, and gather evidence.

### L2 — Recommend

Recommend investigation, containment, remediation, or escalation.

### L3 — Human-Approved Response

Execute explicitly approved defensive actions.

### L4 — Controlled Autonomy

Perform narrowly scoped, pre-authorized, tested, logged, and preferably
reversible actions.

BIZZ PROJECT currently begins primarily at:

> **L0 / L1**

Autonomy must be earned through testing.

---

# BIZZ PROJECT AI Team

BIZZ PROJECT uses specialized AI agents with separated responsibilities.

## Mae

**Founder / Product Owner / Human Authority**

Mae owns:

- project vision;
- priorities;
- product direction;
- authorization boundaries;
- risk decisions;
- final acceptance;
- learning direction.

Mae is the final human authority.

---

## Claude

**AI CTO / Principal Engineering Agent**

Primary responsibility:

> **HOW should approved BIZZ PROJECT capabilities be engineered?**

Claude focuses on:

- software architecture;
- frontend;
- backend;
- APIs;
- databases;
- integrations;
- testing;
- debugging;
- DevOps;
- code quality;
- technical implementation.

Operational contract:

`agents/CLAUDE_CTO.md`

---

## GPT

**AI SOC Architect / Cybersecurity Defense Agent / Mentor**

Primary responsibility:

> **What does cybersecurity require, what does the evidence support, and
> how should it be investigated?**

GPT focuses on:

- SOC architecture;
- detection engineering;
- telemetry analysis;
- alert triage;
- incident investigation;
- correlation;
- MITRE ATT&CK;
- threat hunting;
- defensive response design;
- AI security;
- security review;
- cybersecurity mentoring.

Operational contract:

`agents/GPT_SOC.md`

---

## Gemini

**AI Product Strategy / UX / Research Agent**

Primary responsibility:

> **WHAT should BIZZ PROJECT build and WHY?**

Gemini focuses on:

- product research;
- SOC workflow research;
- UX/UI;
- user needs;
- requirements;
- prioritization;
- user flows;
- competitive research;
- product concepts.

Operational contract:

`agents/GEMINI_PRODUCT.md`

---

# Separation of Duties

No AI agent should control the entire lifecycle of a significant change.

The preferred workflow is:

**Gemini — Product / Research**

↓

**Mae — Approve / Modify Direction**

↓

**GPT — SOC / Security Requirements**

↓

**Claude — Architecture / Implementation**

↓

**Automated Tests**

↓

**GPT — SOC / Security Review**

↓

**Mae — Understand / Test / Approve**

↓

**Git Commit**

↓

**BIZZ PROJECT Lab**

This structure helps prevent an AI agent from becoming both the builder
and sole approver of its own security-sensitive work.

---

# Learning Philosophy

BIZZ PROJECT is intentionally designed as a learning system for its
Founder.

The project follows:

> **Never automate something Mae has not at least conceptually learned.**

For significant cybersecurity capabilities, the preferred cycle is:

**LEARN**

↓

**INVESTIGATE MANUALLY**

↓

**BUILD WITH AI**

↓

**READ THE IMPLEMENTATION**

↓

**TEST**

↓

**BREAK SAFELY**

↓

**FIX**

↓

**EXPLAIN**

The objective is:

> **BIZZ PROJECT becomes more capable → Mae becomes more capable.**

---

# Planned Architecture

The long-term architecture follows the general flow:

**Security Telemetry**

↓

**Telemetry Ingestion**

↓

**Detection Engine**

↓

**Correlation / Incident Engine**

↓

**AI SOC Reasoning**

↓

**BIZZ PROJECT API**

↓

**SOC Dashboard**

↓

**Human Analyst**

Initial telemetry will come from controlled laboratory environments.

Planned sources include:

- Windows;
- Sysmon;
- Linux;
- Wazuh;
- authentication telemetry;
- process telemetry;
- network telemetry.

Architecture will evolve incrementally as the project matures.

See:

`ARCHITECTURE.md`

---

# Detection Engineering

BIZZ PROJECT aims to develop understandable and testable detections using
technologies and approaches such as:

- Sigma;
- Wazuh rules;
- Python detection logic;
- behavioral detection;
- event correlation;
- MITRE ATT&CK mapping.

Detections should eventually document:

- objective;
- telemetry requirements;
- logic;
- severity;
- assumptions;
- true-positive expectations;
- possible false positives;
- investigation guidance;
- limitations;
- test evidence.

A detection is not trustworthy merely because it executes.

It must be tested.

---

# AI-Assisted Investigation

Future BIZZ PROJECT AI capabilities may assist analysts with:

- alert explanation;
- evidence summarization;
- related-event discovery;
- event correlation;
- timeline construction;
- process relationships;
- IOC extraction;
- MITRE ATT&CK mapping;
- hypothesis generation;
- threat hunting;
- investigation recommendations;
- incident reporting.

AI conclusions should remain traceable to supporting evidence.

---

# Security Laboratory

BIZZ PROJECT cybersecurity experiments must operate within authorized
environments.

Mae's physical laptop is not intended to serve as the attack victim.

Potentially risky security experiments should use appropriate isolation,
including:

- virtual machines;
- snapshots;
- containers where appropriate;
- isolated lab networks;
- disposable systems;
- dedicated test accounts.

BIZZ PROJECT must not be used for unauthorized testing of third-party
systems.

See:

`SECURITY.md`

---

# Development Environments

The target environment model is:

**DEV → LAB → STAGING → PRODUCTION**

Not every environment exists during early development.

BIZZ PROJECT must clearly distinguish between:

- simulated data;
- laboratory telemetry;
- test environments;
- real operational telemetry.

Interfaces should use labels such as:

**DEMO DATA**

**LAB ENVIRONMENT**

**LIVE**

where appropriate.

Experimental capability must not be represented as production-ready
security protection.

---

# Repository Structure

```text
BIZZPROJECT/
│
├── AI/
├── agents/
│   ├── CLAUDE_CTO.md
│   ├── GPT_SOC.md
│   └── GEMINI_PRODUCT.md
│
├── backend/
├── detections/
├── docs/
│   └── design/
├── frontend/
├── lab/
├── sample-logs/
├── tests/
│
├── AI_AGENT_POLICY.md
├── ARCHITECTURE.md
├── PRODUCT.md
├── README.md
├── ROADMAP.md
└── SECURITY.md
