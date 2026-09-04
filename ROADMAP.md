# BIZZ PROJECT — Development Roadmap

**Document:** ROADMAP.md  
**Project:** BIZZ PROJECT  
**Status:** Active Development Roadmap  
**Founder / Product Owner:** Mae  
**Current Stage:** Foundation → v0.01  
**Target:** AI-Assisted SOC Laboratory Platform

---

# 1. Purpose

This document defines the development sequence of BIZZ PROJECT.

It answers:

> **What do we build next?**

The roadmap connects four things:

**LEARNING → BUILDING → TESTING → EVIDENCE**

A milestone is not complete merely because code exists.

Mae should understand the cybersecurity concept, the implementation
should work, tests should demonstrate expected behavior, and useful
evidence should be documented.

---

# 2. Roadmap Philosophy

BIZZ PROJECT grows incrementally.

The project should not jump directly to:

- AI investigation;
- autonomous response;
- complex dashboards;
- enterprise infrastructure.

Each stage should create knowledge required by the next stage.

The general progression is:

```text
EVENTS
   ↓
DETECTION
   ↓
ALERTS
   ↓
REAL TELEMETRY
   ↓
SIEM
   ↓
DETECTION ENGINEERING
   ↓
AI REASONING
   ↓
CORRELATION
   ↓
INVESTIGATION
   ↓
THREAT HUNTING
   ↓
CONTROLLED RESPONSE
````

---

# 3. Development Rule

For significant cybersecurity capabilities:

```text
LEARN
  ↓
OBSERVE
  ↓
INVESTIGATE MANUALLY
  ↓
BUILD WITH AI
  ↓
READ THE IMPLEMENTATION
  ↓
TEST
  ↓
BREAK SAFELY
  ↓
FIX
  ↓
EXPLAIN
  ↓
DOCUMENT
```

The objective is:

> **BIZZ PROJECT becomes more capable → Mae becomes more capable.**

---

# 4. Milestone Definition of Done

A milestone is complete when appropriate requirements below are met:

* Mae understands the core concept.
* The capability actually works.
* Inputs and outputs are understandable.
* Relevant tests pass.
* Failure behavior has been examined.
* Security boundaries are respected.
* Demo/lab/live data is correctly labeled.
* Documentation reflects reality.
* Mae can explain what was built.
* Git contains evidence of the milestone.

Not every milestone requires every criterion, but completion must be
based on demonstrated capability rather than appearance.

---

# 5. Phase 0 — Foundation

**Status: Nearly Complete**

## Objective

Establish BIZZ PROJECT's identity, governance, architecture, security
boundaries, repository structure, and AI-team responsibilities.

## Foundation Documents

* README.md
* PRODUCT.md
* ARCHITECTURE.md
* SECURITY.md
* AI_AGENT_POLICY.md
* ROADMAP.md

## AI Agent Contracts

* agents/CLAUDE_CTO.md
* agents/GPT_SOC.md
* agents/GEMINI_PRODUCT.md

## Founder Learning

Mae should understand at a high level:

* what BIZZ PROJECT is;
* MONITOR → DETECT → ANALYZE → PROTECT;
* evidence versus AI reasoning;
* AI-agent separation of duties;
* least privilege;
* lab isolation;
* why the project starts small.

## Definition of Done

Phase 0 is complete when:

* core documentation exists;
* AI roles are defined;
* repository structure exists;
* security boundaries are documented;
* Mae understands the project's high-level architecture;
* development can begin without unresolved foundational blockers.

---

# 6. v0.01 — First Security Engine

## Theme

> **EVENT → DETECTION → ALERT**

## Objective

Build the smallest functioning BIZZ PROJECT cybersecurity capability.

No dashboard.

No AI SOC Analyst.

No Wazuh.

No complex database.

Just understandable security logic.

## BIZZ Lab #001

**Repeated Failed Authentication Detection**

## Mae Learns

* what authentication means;
* successful vs failed authentication;
* what a security event is;
* what fields an event contains;
* what suspicious repetition means;
* thresholds;
* time windows;
* detection logic;
* false positives;
* what an alert represents;
* why an alert is not proof of compromise.

## Build

Create controlled sample authentication events.

Python should:

1. read the events;
2. identify failed authentication attempts;
3. group relevant activity;
4. apply detection logic;
5. generate a structured alert.

Conceptual flow:

```text
sample-auth-events
        ↓
Python
        ↓
Failed Login Detection
        ↓
Structured BIZZ Alert
```

## Tests

Test at minimum:

**Benign case**

Normal authentication activity → no alert.

**Threshold case**

Repeated failures meeting the rule → alert.

**Below threshold**

Insufficient failures → no alert.

**Different users**

Unrelated users should not automatically be grouped together.

**Malformed event**

Bad input should fail safely.

## Mae Investigation

Before accepting the alert, Mae should answer:

* What happened?
* Which user was involved?
* How many failures occurred?
* During what period?
* Why did the rule alert?
* What evidence supports it?
* Could it be benign?
* What should an analyst check next?

## Definition of Done

v0.01 is complete when:

* sample events exist;
* Python reads them;
* detection logic works;
* a structured alert is generated;
* tests demonstrate expected behavior;
* Mae can explain the detection;
* BIZZ Lab #001 is documented.

---

# 7. v0.1 — Initial SOC Application

## Theme

> **Turn the security engine into an application.**

## Objective

Introduce a basic backend, API, persistence, and SOC interface.

## Mae Learns

* frontend vs backend;
* APIs;
* JSON;
* HTTP basics;
* database basics;
* application data flow;
* how alerts move through a system.

## Build

Potential architecture:

```text
Sample Events
     ↓
Python Backend
     ↓
Detection Engine
     ↓
SQLite
     ↓
API
     ↓
BIZZ Dashboard
```

Initial interface should focus on:

* Dashboard;
* Alerts;
* Alert details;
* Evidence.

## Technology Direction

Potential initial technologies:

* Python;
* FastAPI;
* SQLite;
* React / Next.js.

Final implementation decisions remain subject to engineering review.

## Definition of Done

* backend runs locally;
* events can be processed;
* alerts persist;
* API exposes required data;
* dashboard displays alerts;
* alert evidence can be inspected;
* demo data is clearly labeled;
* Mae can explain frontend → API → backend → database flow.

---

# 8. v0.2 — Real Windows Telemetry

## Theme

> **From sample events to laboratory evidence.**

## Objective

Introduce real security telemetry from an isolated Windows laboratory
endpoint.

## Mae Learns

* Windows Event Logs;
* Sysmon;
* event IDs;
* process creation;
* authentication telemetry;
* timestamps;
* endpoint telemetry;
* raw vs normalized events.

## Lab

Initial target:

```text
Windows 11 VM
   ├── Windows Event Logs
   └── Sysmon
```

Mae's physical laptop remains outside the victim role.

## Build

BIZZ should begin consuming selected laboratory telemetry.

## Initial Exercises

Possible labs:

* failed authentication;
* successful authentication;
* PowerShell execution;
* process creation;
* controlled network activity.

## Definition of Done

* isolated Windows VM exists;
* telemetry collection works;
* BIZZ receives selected real lab events;
* raw evidence is preserved;
* events are understandable;
* at least one detection operates on real laboratory telemetry;
* Mae can manually inspect the underlying evidence.

---

# 9. v0.3 — Wazuh / SIEM Integration

## Theme

> **Learn how a real SIEM pipeline works.**

## Objective

Integrate BIZZ PROJECT with Wazuh and understand security-event
collection and alerting.

## Mae Learns

* SIEM concepts;
* agents;
* log collection;
* rules;
* alerts;
* centralized telemetry;
* event searching;
* SIEM investigation workflow.

## Architecture

```text
Windows / Linux
       ↓
   Wazuh Agents
       ↓
      Wazuh
       ↓
BIZZ Integration
       ↓
BIZZ Analysis
```

## Definition of Done

* Wazuh lab operates;
* endpoint telemetry reaches Wazuh;
* Mae can locate and interpret selected events;
* BIZZ can consume relevant Wazuh information;
* responsibilities of Wazuh vs BIZZ are understood.

---

# 10. v0.4 — Detection Engineering

## Theme

> **Build detections deliberately.**

## Objective

Expand from one simple rule into a tested detection-engineering
practice.

## Mae Learns

* detection engineering;
* Sigma;
* Wazuh rules;
* Python detections;
* MITRE ATT&CK;
* behavioral detection;
* false positives;
* false negatives;
* detection tuning;
* detection coverage.

## Detection Standard

Each mature detection should document:

* objective;
* required telemetry;
* logic;
* severity;
* MITRE mapping where applicable;
* assumptions;
* expected true positives;
* possible false positives;
* investigation guidance;
* limitations;
* tests.

## Portfolio Goal

Begin creating multiple documented BIZZ Lab investigations.

## Definition of Done

* multiple detections exist;
* detections use repeatable tests;
* benign and suspicious cases are evaluated;
* MITRE mappings are evidence-grounded;
* Mae can explain why each major detection works.

---

# 11. v0.5 — AI SOC Analyst

## Theme

> **AI enters after the evidence pipeline works.**

## Objective

Introduce evidence-grounded AI assistance.

## Mae Learns

* LLM fundamentals;
* structured prompts;
* structured output;
* hallucination;
* prompt injection;
* AI security;
* confidence vs evidence;
* context management.

## AI Capabilities

BIZZ AI may initially:

* explain alerts;
* summarize evidence;
* identify missing information;
* provide MITRE context;
* recommend investigation steps.

## Required Reasoning Model

```text
FACT
INFERENCE
HYPOTHESIS
UNKNOWN
```

## Security Requirement

> **AI reasons about evidence. AI does not create the evidence.**

## Definition of Done

* AI receives controlled evidence;
* outputs reference available evidence;
* facts and hypotheses are distinguished;
* malicious telemetry instructions are tested;
* AI failure does not break core detection functionality;
* Mae can challenge and verify AI conclusions.

---

# 12. v0.6 — Event Correlation & Incidents

## Theme

> **Connect individual signals into security stories.**

## Objective

Correlate related events and alerts into incidents.

Example:

```text
Failed Logins
      ↓
Successful Login
      ↓
PowerShell Execution
      ↓
Outbound Connection
      ↓
Incident
```

## Mae Learns

* correlation;
* temporal relationships;
* entity relationships;
* attack sequences;
* incident creation;
* timeline analysis.

## Build

Introduce:

* correlation logic;
* incident model;
* related alerts;
* related events;
* timelines.

## Definition of Done

* related activity can be correlated;
* unrelated activity is not incorrectly grouped in basic tests;
* incident timelines preserve evidence references;
* Mae can reconstruct what happened chronologically.

---

# 13. v0.7 — AI-Assisted Investigation

## Theme

> **Move from explanation to structured investigation.**

## Objective

Allow BIZZ AI to assist with gathering and organizing relevant evidence.

## Mae Learns

* investigation scoping;
* evidence collection;
* process relationships;
* entity pivoting;
* hypothesis testing;
* incident reasoning.

## AI May Assist With

* related-event discovery;
* timeline construction;
* process relationships;
* authentication history;
* network context;
* IOC extraction;
* investigation gaps;
* hypotheses;
* next-step recommendations.

## Definition of Done

* AI can investigate within explicitly allowed data;
* conclusions reference evidence;
* missing evidence is identified;
* hypotheses remain labeled;
* analyst review remains visible;
* Mae can manually reproduce important reasoning steps.

---

# 14. v0.8 — Threat Hunting

## Theme

> **Search proactively instead of waiting for alerts.**

## Objective

Support hypothesis-driven threat hunting.

## Mae Learns

* hunt hypotheses;
* telemetry selection;
* query construction;
* baselining;
* anomaly interpretation;
* investigation pivots.

## Workflow

```text
Hypothesis
    ↓
Telemetry Selection
    ↓
Query
    ↓
Results
    ↓
Evidence
    ↓
Interpretation
    ↓
Next Hunt
```

## AI Assistance

AI may help:

* translate controlled natural language into queries;
* explain queries;
* refine searches;
* summarize results.

Mae should still understand what the query does.

## Definition of Done

* hunts can be defined;
* queries are inspectable;
* results are evidence-backed;
* AI-generated queries can be reviewed;
* documented threat hunts exist in the portfolio.

---

# 15. v0.9 — Controlled Defensive Response

## Theme

> **From recommendation to carefully governed action.**

## Objective

Introduce defensive-response workflows without surrendering human
control.

## Mae Learns

* SOAR concepts;
* containment;
* remediation;
* authorization;
* least privilege;
* rollback;
* audit logging;
* automation risk.

## Initial Model

```text
Investigation
     ↓
Recommendation
     ↓
Mae Reviews
     ↓
Authorization
     ↓
Controlled Action
     ↓
Verification
     ↓
Audit
```

## Potential Lab Actions

Only after security review, examples may include:

* temporary endpoint isolation;
* temporary blocking;
* controlled test-account disablement.

## Definition of Done

* authentication and authorization exist where required;
* response targets are verified;
* actions require appropriate approval;
* actions are logged;
* outcomes are verified;
* rollback exists where practical;
* actions are tested in LAB;
* no unrestricted autonomous response exists.

---

# 16. v1.0 — AI-Assisted SOC Laboratory Platform

## Theme

> **Integrated defensive cybersecurity workflow.**

## Objective

Demonstrate a coherent laboratory platform capable of:

**MONITOR → DETECT → ANALYZE → PROTECT**

## Target Capabilities

BIZZ PROJECT should be able to demonstrate:

* real laboratory telemetry ingestion;
* tested security detections;
* structured alerts;
* event correlation;
* incidents;
* timelines;
* evidence provenance;
* MITRE ATT&CK context;
* AI-assisted analysis;
* threat hunting;
* incident reporting;
* controlled defensive-response workflows;
* auditability.

## v1.0 Does Not Mean

v1.0 does not automatically mean:

* commercial readiness;
* enterprise readiness;
* production certification;
* autonomous security operations;
* replacement for established SIEM/XDR platforms.

It means BIZZ PROJECT has matured into a coherent, demonstrable,
AI-assisted SOC laboratory platform.

---

# 17. Portfolio Roadmap

Development should produce evidence of Mae's cybersecurity growth.

A long-term target is approximately:

> **20–30 meaningful BIZZ Lab investigations**

Quality matters more than reaching an exact number.

Possible investigations may eventually cover:

* authentication attacks;
* suspicious PowerShell;
* process execution;
* persistence;
* suspicious network behavior;
* credential activity;
* privilege-related events;
* malware-like simulations;
* lateral-movement concepts;
* data-access anomalies.

Only authorized laboratory activity is permitted.

---

# 18. BIZZ Lab Documentation Standard

A substantial investigation should ideally contain:

```text
BIZZ Lab Number
Title
Objective
Concepts Learned
Lab Architecture
Telemetry Source
Scenario
Detection
Evidence
Investigation
Timeline
MITRE ATT&CK
Conclusion
False Positives
Limitations
BIZZ Implementation
Lessons Learned
```

Screenshots may be included where useful.

---

# 19. Portfolio Honesty

BIZZ PROJECT should distinguish between:

* implemented;
* tested;
* experimental;
* planned.

Do not represent roadmap capabilities as existing features.

Do not present demo data as live telemetry.

Do not claim production readiness based on laboratory success.

The repository should show genuine progression.

---

# 20. AI Team Workflow During Development

For significant features:

```text
Gemini
Product / Research
      ↓
Mae
Direction Approval
      ↓
GPT
SOC / Security Requirements
      ↓
Claude
Engineering Implementation
      ↓
Automated Tests
      ↓
GPT
SOC / Security Review
      ↓
Mae
Understand / Test / Approve
      ↓
Git
```

No AI agent should become both sole builder and sole approver of a
security-sensitive capability.

---

# 21. Current Priority

The current project priority is:

> **BIZZ PROJECT v0.01**

Specifically:

> **BIZZ Lab #001 — Repeated Failed Authentication Detection**

The immediate objective is not to build the dashboard.

It is to understand and implement:

```text
SECURITY EVENT
      ↓
DETECTION LOGIC
      ↓
ALERT
      ↓
ANALYST INVESTIGATION
```

---

# 22. Things We Intentionally Do Not Need Yet

During v0.01, BIZZ PROJECT does not need:

* React;
* Next.js;
* FastAPI;
* PostgreSQL;
* Wazuh;
* Docker;
* Kubernetes;
* cloud infrastructure;
* autonomous AI agents;
* threat-intelligence integrations;
* SOAR;
* real malware.

The initial requirements are intentionally small:

> **Python + sample events + detection logic + tests + Mae's analysis.**

---

# 23. Roadmap Change Control

This roadmap is directional, not immutable.

A milestone may change when:

* learning reveals a better sequence;
* technical dependencies change;
* security requirements change;
* testing exposes missing foundations;
* product research provides strong evidence.

Changes should have a reason.

New technology should not be added merely because it is fashionable.

Material roadmap changes require Mae's approval.

---

# 24. Final Roadmap Authority

Gemini may recommend product priorities.

GPT may recommend cybersecurity learning or security sequencing.

Claude may identify engineering dependencies.

Mae determines the final development direction.

---

# 25. Roadmap Principle

The goal is not to reach v1.0 as quickly as possible.

The goal is to reach each milestone with:

> **understanding + evidence + implementation + testing.**

That creates something more valuable than a large AI-generated
codebase that its Founder cannot explain.

---

# 26. BIZZ PROJECT Spirit

> **Zero Trust. Maximum Protection.**
>
> **Evidence Before Conclusions.**
>
> **Autonomous Investigation. Human-Authorized Impact.**
>
> **To Integrity and Beyond!**

---

**End of BIZZ PROJECT — Development Roadmap**
