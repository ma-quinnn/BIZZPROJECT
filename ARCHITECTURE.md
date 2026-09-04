# BIZZ PROJECT — System Architecture

**Document:** ARCHITECTURE.md  
**Project:** BIZZ PROJECT  
**Status:** Active Architecture Definition  
**Founder:** Mae  
**Current Stage:** Early Development / Cybersecurity Engineering Lab

---

# 1. Purpose

This document defines the intended technical architecture of BIZZ PROJECT.

It describes:

- major system components;
- component responsibilities;
- data flow;
- security boundaries;
- telemetry flow;
- detection flow;
- investigation flow;
- AI reasoning boundaries;
- interfaces between components;
- development evolution;
- architectural constraints.

This document answers:

> **HOW should BIZZ PROJECT be structured?**

Product goals belong primarily in:

`PRODUCT.md`

Security requirements belong primarily in:

`SECURITY.md`

AI governance belongs in:

`AI_AGENT_POLICY.md`

Development sequencing belongs in:

`ROADMAP.md`

---

# 2. Architecture Philosophy

BIZZ PROJECT should grow incrementally.

The project should not begin with enterprise-scale architecture.

Early architecture should favor:

- simplicity;
- understandable components;
- clear data flow;
- explicit boundaries;
- testability;
- evidence traceability;
- secure defaults;
- replaceable components;
- incremental evolution.

The architecture should become more advanced only when actual project
requirements justify additional complexity.

Core principle:

> **Build the smallest architecture that correctly supports the current
> security capability.**

---

# 3. High-Level Architecture

The long-term conceptual architecture is:

```text
SECURITY TELEMETRY
        │
        ▼
TELEMETRY INGESTION
        │
        ▼
NORMALIZATION / VALIDATION
        │
        ▼
DETECTION ENGINE
        │
        ▼
ALERTS
        │
        ▼
CORRELATION / INCIDENT ENGINE
        │
        ▼
AI SOC REASONING
        │
        ▼
BIZZ API
        │
        ▼
SOC DASHBOARD
        │
        ▼
HUMAN ANALYST
````

Supporting components may eventually include:

```text
DATABASE
THREAT INTELLIGENCE
MITRE ATT&CK DATA
AUDIT LOGGING
AUTHENTICATION
RESPONSE ENGINE
SOAR PLAYBOOKS
REPORTING
```

Not every component exists during early development.

---

# 4. Core Data Flow

The fundamental BIZZ PROJECT data flow is:

```text
Source
  ↓
Event
  ↓
Detection
  ↓
Alert
  ↓
Incident
  ↓
Analysis
  ↓
Recommendation
  ↓
Human Decision
  ↓
Optional Response
```

This chain should remain traceable.

BIZZ PROJECT should eventually be able to answer:

> Where did this conclusion come from?

---

# 5. Evidence Provenance

Evidence provenance is a core architectural requirement.

Whenever practical, BIZZ PROJECT should preserve relationships between:

```text
Telemetry Source
      ↓
Raw Event
      ↓
Normalized Event
      ↓
Detection Rule
      ↓
Alert
      ↓
Incident
      ↓
AI Analysis
      ↓
Recommendation
```

AI reasoning must not replace the original evidence.

The system should preserve enough information for analysts to inspect
the underlying events that contributed to an alert or conclusion.

---

# 6. Telemetry Sources

Initial and future telemetry sources may include:

## Initial Sources

* sample authentication logs;
* generated laboratory events;
* Windows Event Logs;
* Sysmon.

## Later Sources

* Linux logs;
* Wazuh;
* endpoint telemetry;
* network telemetry;
* identity telemetry;
* cloud telemetry;
* authorized threat intelligence.

Telemetry integrations should be added incrementally.

A new telemetry source should not be introduced unless BIZZ PROJECT can
understand:

* what the data represents;
* how it is collected;
* how trustworthy it is;
* how it will be normalized;
* which detections use it.

---

# 7. Telemetry Ingestion Layer

The ingestion layer receives security events.

Its responsibilities may include:

* reading sample files;
* receiving API input;
* reading event streams;
* accepting SIEM data;
* validating event structure;
* rejecting malformed events;
* assigning source metadata;
* preserving raw event information;
* forwarding valid events for normalization.

The ingestion layer should not perform complex security conclusions.

Its job is primarily:

> **Receive trustworthy structured security data.**

---

# 8. Raw Events

Where practical, BIZZ PROJECT should preserve original event content.

A raw event may contain:

```text
source
timestamp
host
user
event_id
process
network information
message
original payload
```

Raw events should remain distinguishable from:

* normalized data;
* enriched data;
* detection results;
* AI-generated analysis.

This separation protects evidence integrity.

---

# 9. Normalization Layer

Different telemetry sources may represent similar concepts differently.

The normalization layer should gradually map source-specific fields into
a common event structure.

Example:

```text
timestamp
source_type
event_type
host
user
process_name
process_id
parent_process
source_ip
destination_ip
destination_port
event_action
event_outcome
raw_event_reference
```

Normalization should preserve the original event.

The common schema may evolve as BIZZ PROJECT gains real telemetry.

Do not overdesign the schema before real event requirements are known.

---

# 10. Initial Event Model

For early versions, a simple event model is sufficient.

Example:

```json
{
  "timestamp": "2026-09-04T12:00:00Z",
  "source": "sample-authentication",
  "event_type": "login_failed",
  "host": "LAB-WIN11-01",
  "user": "testuser",
  "source_ip": "192.168.56.20",
  "outcome": "failure"
}
```

The first implementation should prioritize understanding over completeness.

---

# 11. Detection Engine

The detection engine evaluates events against detection logic.

Potential detection methods include:

* Python logic;
* Sigma rules;
* Wazuh rules;
* thresholds;
* behavioral rules;
* event sequences;
* correlations;
* IOC matching.

Initial development should begin with understandable Python logic.

Example:

```text
IF
multiple failed login events
occur for the same user
within a defined time window

THEN
generate an alert
```

The first goal is to understand the detection lifecycle.

---

# 12. Detection Rule Model

A detection should eventually include:

```text
detection_id
name
description
version
severity
required_telemetry
logic
MITRE ATT&CK mapping
assumptions
false_positive_notes
investigation_guidance
test_cases
```

The implementation format may evolve over time.

Detection metadata should remain separate from individual detection
results.

---

# 13. Alerts

When detection logic matches suspicious behavior, BIZZ PROJECT creates
an alert.

Conceptual alert structure:

```json
{
  "alert_id": "ALT-0001",
  "detection_id": "DET-AUTH-001",
  "title": "Repeated Failed Login Attempts",
  "severity": "medium",
  "timestamp": "...",
  "host": "LAB-WIN11-01",
  "user": "testuser",
  "evidence": [],
  "status": "new"
}
```

An alert should reference the events that caused it.

Alerts must not destroy the evidence chain.

---

# 14. Correlation Engine

The correlation engine connects related events and alerts.

Correlation may use relationships such as:

* same host;
* same user;
* same process;
* same IP;
* similar time window;
* parent-child process relationships;
* repeated behavior;
* attack sequence.

Example:

```text
Failed Login Attempts
        ↓
Successful Login
        ↓
PowerShell Execution
        ↓
Outbound Connection
```

The correlation engine should identify relationships.

It should not automatically declare malicious intent.

---

# 15. Incident Engine

Related alerts and events may eventually become incidents.

An incident represents a security investigation containing multiple
related pieces of evidence.

Conceptual structure:

```text
incident_id
title
severity
status
first_seen
last_seen
affected_hosts
affected_users
related_alerts
related_events
timeline
MITRE mappings
analyst_notes
AI analysis
recommendations
```

Incident creation may initially be manual or rule-based.

AI-assisted correlation may be introduced later.

---

# 16. Timeline Engine

Timeline construction should help analysts reconstruct activity.

Events should be ordered using normalized timestamps.

Example:

```text
10:02:01 Failed login
10:02:04 Failed login
10:02:08 Failed login
10:03:12 Successful login
10:04:03 PowerShell started
10:04:14 Outbound connection
```

Timeline information should preserve references to original evidence.

---

# 17. Entity Model

BIZZ PROJECT should gradually recognize important entities.

Potential entity types include:

```text
Host
User
Process
IP Address
Domain
File
Hash
Alert
Incident
Detection
```

Relationships may include:

```text
User → logged into → Host

Process → executed on → Host

Process → spawned → Process

Host → connected to → IP

Alert → contains → Event

Incident → contains → Alert
```

A graph database is not required during early development.

Relationships can initially be represented using normal application data
structures or relational tables.

---

# 18. Database Layer

Early versions should use the simplest database suitable for the project.

Initial direction:

> **SQLite**

Reasons:

* simple;
* local;
* no server required;
* easy to understand;
* appropriate for early lab development.

Potential early tables:

```text
events
detections
alerts
incidents
incident_events
incident_alerts
ai_analyses
audit_logs
```

The exact schema should evolve with actual features.

---

# 19. Future Database Evolution

If BIZZ PROJECT eventually requires:

* multiple users;
* larger telemetry volume;
* concurrent access;
* server deployment;
* stronger scaling;

the system may migrate to a database such as:

```text
PostgreSQL
```

Migration should occur because of demonstrated requirements.

Not because PostgreSQL appears more professional.

---

# 20. Backend

The backend is responsible for BIZZ PROJECT application logic.

Planned direction:

> **Python**

Possible framework:

> **FastAPI**

The backend may eventually handle:

* event ingestion;
* normalization;
* detections;
* alerts;
* incidents;
* database access;
* AI requests;
* investigation logic;
* reporting;
* response authorization;
* API endpoints.

Framework choice should remain reviewable until implementation begins.

---

# 21. Backend Structure

A possible future structure may resemble:

```text
backend/
│
├── app/
│   ├── api/
│   ├── models/
│   ├── services/
│   ├── detections/
│   ├── correlation/
│   ├── ai/
│   ├── database/
│   └── security/
│
├── tests/
└── requirements.txt
```

This is directional.

Claude may propose a simpler or better organization during implementation.

Major architectural changes should remain documented.

---

# 22. API Layer

The BIZZ API provides controlled access to backend capabilities.

Potential endpoints may eventually include:

```text
GET /events
GET /events/{id}

GET /alerts
GET /alerts/{id}

GET /incidents
GET /incidents/{id}

GET /detections
POST /detections/test

POST /investigate
POST /hunt

GET /health
```

Consequential response endpoints require stronger controls and should not
be introduced during early stages.

---

# 23. Frontend

The SOC dashboard provides the human analyst interface.

Planned direction:

```text
React / Next.js
```

The frontend should focus on:

* situational awareness;
* evidence visibility;
* alerts;
* incidents;
* timelines;
* investigation workflow;
* AI analysis;
* MITRE context;
* threat hunting;
* reports.

The frontend should never become the source of security truth.

It displays information produced or retrieved from trusted backend
systems.

---

# 24. Frontend Architecture

A possible structure may eventually resemble:

```text
frontend/
│
├── app/
├── components/
├── features/
├── services/
├── types/
└── tests/
```

Actual implementation should remain simple during early versions.

Do not introduce complex state-management systems until required.

---

# 25. Official Interface Areas

Long-term interface areas may include:

```text
Dashboard
Alerts
Incidents
Threat Hunting
Endpoints
Network
MITRE ATT&CK
IOC Intel
Reports
SOAR Playbooks
Automation
Cases
Settings
```

These are product directions.

They do not imply implementation.

---

# 26. BIZZ AI Reasoning Layer

The AI reasoning layer interprets security evidence.

Potential responsibilities include:

* explain alerts;
* summarize evidence;
* correlate related information;
* create timelines;
* identify investigation gaps;
* map behavior to MITRE ATT&CK;
* generate hypotheses;
* recommend next investigation steps;
* assist threat hunting;
* generate incident summaries.

The AI layer is not a telemetry source.

---

# 27. AI Input Model

AI should receive structured context where practical.

Example:

```text
Alert metadata
Relevant events
Entity information
Timeline
Detection details
Known MITRE context
Analyst question
```

Avoid sending unnecessary unrelated data.

AI context should follow least privilege and data minimization.

---

# 28. AI Output Model

AI output should gradually support structured reasoning.

Example:

```json
{
  "facts": [],
  "inferences": [],
  "hypotheses": [],
  "unknowns": [],
  "mitre_attack": [],
  "recommended_next_steps": [],
  "confidence": null
}
```

Structured AI output improves:

* traceability;
* testing;
* display;
* validation;
* auditing.

Free-form explanation may accompany structured output.

---

# 29. AI Evidence References

Where practical, AI conclusions should reference evidence IDs.

Example:

```text
FACT:
PowerShell executed.

Evidence:
EVT-1042

INFERENCE:
The timing is suspicious because execution followed a new successful
login.

Evidence:
EVT-1038
EVT-1042
```

The analyst should be able to inspect the referenced evidence.

---

# 30. AI Trust Boundary

AI models must be treated as untrusted reasoning components.

An AI model may:

* make mistakes;
* hallucinate;
* misunderstand events;
* be influenced by malicious input;
* produce unsafe recommendations.

Therefore:

```text
AI OUTPUT ≠ SECURITY TRUTH
```

AI output should be treated as:

> **analytical assistance**

until supported by evidence and human review where required.

---

# 31. Instructions vs Security Data

The architecture must distinguish:

```text
TRUSTED INSTRUCTIONS
```

from:

```text
UNTRUSTED SECURITY DATA
```

Logs, filenames, alerts, telemetry fields, threat intelligence, and
network content must be treated as data.

They must not automatically become instructions to the AI model.

This protects future BIZZ PROJECT AI capabilities from prompt-injection
style attacks.

---

# 32. AI Provider Abstraction

BIZZ PROJECT should avoid unnecessary dependence on one AI provider.

The architecture may eventually use an internal AI interface such as:

```text
AIService
```

Conceptually:

```text
BIZZ Backend
     │
     ▼
AI Service Interface
     │
 ┌───┴────┐
 ▼        ▼
Provider A
Provider B
```

This does not need to be implemented during early versions.

Provider abstraction should be introduced only when useful.

---

# 33. MITRE ATT&CK Integration

MITRE ATT&CK should function as contextual knowledge.

Potential uses include:

* detection mappings;
* alert context;
* incident summaries;
* hunt planning;
* coverage analysis.

ATT&CK mappings should remain connected to observable behavior.

BIZZ PROJECT should not use ATT&CK merely as a visual label.

---

# 34. Threat Intelligence

Threat intelligence may later enrich investigations.

Potential enrichment includes:

* IP reputation;
* domains;
* hashes;
* malware families;
* known infrastructure;
* ATT&CK context.

Threat intelligence is supporting context.

It should not automatically override local evidence.

---

# 35. Audit Logging

Important actions should eventually produce audit records.

Conceptual audit event:

```json
{
  "actor": "Mae",
  "actor_type": "human",
  "action": "approve_endpoint_isolation",
  "target": "LAB-WIN11-01",
  "timestamp": "...",
  "reason": "...",
  "result": "success"
}
```

AI actions should also identify the responsible agent or component.

---

# 36. Authentication

Early local development may not require complex authentication.

Authentication should be introduced before BIZZ PROJECT supports:

* multiple users;
* remotely accessible environments;
* consequential actions;
* sensitive telemetry;
* production deployment.

Future authentication may support:

* local accounts;
* secure sessions;
* role-based access control.

Authentication must not be implemented prematurely merely to imitate an
enterprise application.

---

# 37. Authorization

Authorization becomes critical when actions can change systems.

Potential future roles may include:

```text
Viewer
Analyst
Senior Analyst
Administrator
Founder
```

Response permissions should follow least privilege.

An analyst able to view alerts should not automatically be able to
isolate endpoints.

---

# 38. Response Engine

A response engine should not exist during the earliest versions.

When introduced, it may support controlled defensive actions such as:

* endpoint isolation;
* account disablement;
* credential reset requests;
* indicator blocking;
* firewall changes.

The response chain should be:

```text
Recommendation
     ↓
Human Review
     ↓
Authorization Check
     ↓
Approved Action
     ↓
Execution
     ↓
Verification
     ↓
Audit Log
     ↓
Rollback if needed
```

---

# 39. SOAR Layer

SOAR-style automation belongs to later project stages.

Potential playbooks may include:

```text
Suspicious Login Investigation
Malware Alert Triage
IOC Enrichment
Endpoint Containment
Credential Compromise Review
```

Playbooks should be:

* understandable;
* testable;
* scoped;
* auditable;
* reversible where possible.

---

# 40. Environment Architecture

The target environment model is:

```text
DEV
 ↓
LAB
 ↓
STAGING
 ↓
PRODUCTION
```

During early development, only:

```text
DEV
LAB
```

may exist.

---

# 41. DEV Environment

DEV is where code is developed.

DEV may use:

* sample telemetry;
* mock data;
* synthetic alerts;
* local database;
* isolated services.

DEV must not present simulated activity as real security evidence.

---

# 42. LAB Environment

LAB contains controlled cybersecurity systems used to generate real
security telemetry.

Potential components:

```text
Mae's Host Laptop
        │
        │ management only
        ▼
Virtualization Platform
        │
        ├── Windows 11 VM
        │      ├── Sysmon
        │      └── Wazuh Agent
        │
        ├── Linux VM
        │
        └── BIZZ Services
```

The exact architecture may evolve.

The host laptop must not intentionally become the victim machine.

---

# 43. Lab Network Isolation

The BIZZ PROJECT laboratory should be separated from personal devices
as much as practical.

Potential controls include:

* host-only networks;
* NAT where required;
* isolated virtual networks;
* firewall restrictions;
* snapshots.

Lab malware or dangerous testing must not be allowed to freely reach:

* personal devices;
* home networks;
* unrelated systems;
* the public internet where unnecessary.

Detailed safety requirements belong in:

`SECURITY.md`

---

# 44. Windows Endpoint Lab

Initial real telemetry target:

> **Windows 11 Virtual Machine**

Potential components:

```text
Windows 11
Sysmon
Windows Event Logs
Wazuh Agent
Test User Accounts
```

The VM should support safe generation of events such as:

* login successes;
* login failures;
* PowerShell execution;
* process creation;
* network activity;
* controlled security simulations.

---

# 45. Linux Lab

A Linux VM may later support:

* authentication logs;
* process telemetry;
* network services;
* SSH events;
* controlled attack simulations;
* Wazuh agent testing.

Linux should be introduced when it contributes to a defined learning or
product objective.

---

# 46. Wazuh Architecture

Later architecture may include:

```text
Windows / Linux Agents
          │
          ▼
       Wazuh
          │
          ▼
 BIZZ Integration Layer
          │
          ▼
Detection / Incident / AI Layers
```

BIZZ PROJECT should learn how Wazuh works before abstracting it away.

Wazuh should complement BIZZ PROJECT.

It should not become the entire project.

---

# 47. Sample Logs

`sample-logs/` provides controlled example telemetry.

Sample logs support:

* early development;
* repeatable tests;
* detection tests;
* false-positive tests;
* demonstrations.

Sample logs must be clearly labeled.

They must not be represented as live telemetry.

---

# 48. Detections Directory

`detections/` should eventually contain defensive detection content.

Possible future structure:

```text
detections/
│
├── python/
├── sigma/
├── wazuh/
└── tests/
```

Only create substructure when implementation requires it.

Detection files should remain reviewable and understandable.

---

# 49. AI Directory

`AI/` may contain AI-specific BIZZ PROJECT components.

Possible future uses include:

* prompts;
* reasoning schemas;
* model interfaces;
* evaluation datasets;
* AI security tests;
* output schemas.

AI prompts should be treated as version-controlled project logic when
they materially affect security reasoning.

---

# 50. Tests

Testing should exist across the architecture.

Potential testing layers include:

```text
Unit Tests
Integration Tests
Detection Tests
API Tests
Telemetry Parsing Tests
Correlation Tests
AI Evaluation Tests
Security Tests
Rollback Tests
```

Testing should grow alongside implementation.

---

# 51. Detection Testing Architecture

Detection tests should include:

```text
Known malicious-like laboratory input
Known benign input
Boundary conditions
Malformed data
Repeated-event scenarios
Timing variations
```

Example:

```text
3 failed logins
→ no alert

5 failed logins within configured window
→ alert

5 failed logins spread across long period
→ expected behavior defined by rule
```

Detections should be proven through tests.

---

# 52. Correlation Testing

Correlation should be tested against:

* correctly related events;
* unrelated events;
* events from different users;
* events from different hosts;
* reordered timestamps;
* missing events.

The correlation engine should not connect unrelated activity simply
because events occurred near each other.

---

# 53. AI Evaluation

AI security reasoning must eventually be evaluated.

Tests may examine whether the model:

* cites available evidence;
* fabricates events;
* distinguishes facts and hypotheses;
* follows structured output;
* recognizes missing evidence;
* resists malicious telemetry instructions;
* provides safe recommendations.

AI quality must not be judged only by whether responses sound convincing.

---

# 54. Error Handling

Components should fail explicitly.

Examples:

```text
Malformed Event
Missing Required Field
Detection Failure
Database Failure
AI Provider Failure
Telemetry Source Unavailable
```

Errors should not silently become valid security conclusions.

When AI services are unavailable, core evidence should remain accessible.

---

# 55. Graceful AI Failure

The platform should not become unusable simply because the AI provider
fails.

Where practical:

```text
Telemetry
Detection
Alerts
Evidence
```

should remain functional independently of AI reasoning.

This ensures AI enhances the SOC rather than becoming a single point of
failure.

---

# 56. Observability

As BIZZ PROJECT matures, the platform should monitor its own health.

Potential internal metrics include:

* ingestion failures;
* parser failures;
* detection errors;
* API errors;
* database errors;
* AI failures;
* processing latency.

BIZZ PROJECT should eventually distinguish:

```text
Security Event
```

from:

```text
BIZZ PROJECT Internal Error
```

---

# 57. Secrets Architecture

Secrets must remain outside source control.

Possible mechanisms include:

```text
Environment Variables
.env for local development
Secret Managers later
```

`.env` should not be committed.

`.env.example` may document variable names without real credentials.

---

# 58. Repository Architecture

Current repository structure:

```text
BIZZPROJECT/
│
├── AI/
├── agents/
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
```

This structure is intentionally simple.

Do not create large directory trees before implementation requires them.

---

# 59. Component Dependency Principle

Dependencies should generally flow in one direction.

Example:

```text
Frontend
   ↓
API
   ↓
Application Services
   ↓
Detection / Correlation / AI Services
   ↓
Data Layer
```

Low-level components should not depend unnecessarily on the UI.

Detection logic should be testable without running the dashboard.

---

# 60. Modular Architecture

Key capabilities should remain modular.

Examples:

```text
Ingestion
Normalization
Detection
Correlation
AI Reasoning
Database
API
Frontend
Response
```

Modularity allows BIZZ PROJECT to improve one area without rewriting the
entire system.

Modularity does not require microservices.

---

# 61. No Microservices Yet

BIZZ PROJECT should not begin as a microservices architecture.

Early architecture should favor:

> **A modular monolith.**

Reasons:

* easier to learn;
* easier to debug;
* easier to test;
* easier to run locally;
* lower operational complexity.

Services may be separated later if demonstrated requirements justify it.

---

# 62. No Kubernetes Yet

Kubernetes is not currently required.

BIZZ PROJECT should not introduce orchestration infrastructure before it
has workloads that require orchestration.

The project should first understand:

* the application;
* the telemetry;
* the detections;
* the data flow.

Infrastructure complexity should follow application maturity.

---

# 63. Containers

Containers may later help isolate application services.

Possible future components:

```text
Frontend Container
Backend Container
Database Container
AI Worker Container
```

Containers are useful development and deployment tools.

They are not assumed to be complete security boundaries.

---

# 64. Early Architecture — v0.01

BIZZ PROJECT v0.01 should be extremely simple.

Architecture:

```text
Sample Login Events
        │
        ▼
Python Script
        │
        ▼
Detection Logic
        │
        ▼
Structured Alert
        │
        ▼
Terminal Output
```

No frontend required.

No AI required.

No Wazuh required.

No complex database required.

The purpose is to understand:

> **Event → Detection → Alert**

---

# 65. v0.01 Example

Input:

```json
[
  {
    "user": "mae",
    "event_type": "login_failed"
  },
  {
    "user": "mae",
    "event_type": "login_failed"
  },
  {
    "user": "mae",
    "event_type": "login_failed"
  }
]
```

Detection:

```text
Count failed authentication attempts by user.
```

Output:

```json
{
  "alert": true,
  "title": "Repeated Failed Login Attempts",
  "user": "mae",
  "failed_attempts": 3
}
```

The exact threshold will be defined during the learning exercise.

---

# 66. Architecture Evolution — v0.1

Conceptual architecture:

```text
Sample Data
     ↓
Python Backend
     ↓
Detection Engine
     ↓
SQLite
     ↓
API
     ↓
Dashboard
```

This introduces application structure without real telemetry complexity.

---

# 67. Architecture Evolution — v0.2

```text
Windows VM
    ↓
Sysmon / Windows Logs
    ↓
BIZZ Ingestion
    ↓
Normalization
    ↓
Detection
    ↓
Alerts
    ↓
Dashboard
```

This introduces real laboratory telemetry.

---

# 68. Architecture Evolution — v0.3

```text
Windows / Linux
       ↓
     Wazuh
       ↓
BIZZ Integration
       ↓
Detection / Alerts
       ↓
Dashboard
```

This stage teaches SIEM architecture and integration.

---

# 69. Architecture Evolution — v0.4

Detection capabilities expand:

```text
Python
Sigma
Wazuh
Behavioral Rules
MITRE Mapping
```

Testing becomes increasingly important.

---

# 70. Architecture Evolution — v0.5

AI enters the architecture:

```text
Alert
  ↓
Evidence Retrieval
  ↓
AI Context Builder
  ↓
AI SOC Reasoning
  ↓
Structured Analysis
  ↓
Dashboard
```

AI should not receive unrestricted system access.

---

# 71. Architecture Evolution — v0.6

Introduce correlation:

```text
Events
  ↓
Alerts
  ↓
Correlation Engine
  ↓
Incident
  ↓
Timeline
```

---

# 72. Architecture Evolution — v0.7

Introduce AI investigation:

```text
Incident
   ↓
Evidence Query
   ↓
Related Events
   ↓
Timeline
   ↓
AI Investigation
   ↓
Hypotheses / Unknowns
```

---

# 73. Architecture Evolution — v0.8

Threat hunting:

```text
Analyst Hypothesis
       ↓
Search / Query
       ↓
Telemetry
       ↓
Results
       ↓
AI Assistance
       ↓
Analyst Interpretation
```

---

# 74. Architecture Evolution — v0.9

Controlled response:

```text
Investigation
      ↓
Recommendation
      ↓
Human Approval
      ↓
Response Engine
      ↓
Verification
      ↓
Audit
```

---

# 75. Architecture Evolution — v1.0

Target integrated laboratory architecture:

```text
AUTHORIZED SECURITY ENVIRONMENT
              │
              ▼
       TELEMETRY SOURCES
              │
              ▼
      INGESTION / SIEM
              │
              ▼
        NORMALIZATION
              │
              ▼
       DETECTION ENGINE
              │
              ▼
        ALERT ENGINE
              │
              ▼
    CORRELATION / INCIDENTS
              │
              ▼
      EVIDENCE RETRIEVAL
              │
              ▼
       AI SOC REASONING
              │
              ▼
          BIZZ API
              │
              ▼
        SOC DASHBOARD
              │
              ▼
       HUMAN ANALYST
              │
              ▼
      CONTROLLED RESPONSE
```

---

# 76. Architecture Decision Records

As BIZZ PROJECT matures, major technical decisions may be documented
using Architecture Decision Records.

Possible future location:

```text
docs/architecture/decisions/
```

Do not create this structure until decisions become complex enough to
justify it.

Examples of decisions that may eventually require records:

* database migration;
* authentication architecture;
* AI provider architecture;
* event schema changes;
* deployment model;
* major framework changes.

---

# 77. Architecture Change Rule

Claude may propose architectural improvements.

GPT may identify security architecture concerns.

Gemini may identify product requirements affecting architecture.

Mae decides material architecture changes.

Significant changes should update:

`ARCHITECTURE.md`

so repository documentation remains the source of truth.

---

# 78. Architecture Review Questions

Before introducing a significant component, ask:

1. What problem does it solve?
2. Which existing component depends on it?
3. Is it required at the current milestone?
4. Can Mae understand its purpose?
5. Can it be tested independently?
6. What security boundary does it introduce?
7. What data does it access?
8. What happens when it fails?
9. Can it be replaced later?
10. Is there a simpler design?

If a component cannot justify itself, it should probably not be added yet.

---

# 79. Architecture Success Criteria

The BIZZ PROJECT architecture succeeds when:

* telemetry remains traceable;
* detections are testable;
* alerts reference evidence;
* incidents preserve event relationships;
* AI reasoning remains separate from evidence;
* security boundaries are explicit;
* components can fail safely;
* the system remains understandable;
* architecture can grow incrementally;
* Mae can explain how the system works.

---

# 80. Architectural Principle

BIZZ PROJECT should not be architected to look impressive.

It should be architected to make defensive cybersecurity reasoning:

> **observable, testable, explainable, and secure.**

---

# 81. Final Architecture Authority

Claude is the principal engineering agent.

GPT reviews security and SOC architecture.

Gemini provides approved product requirements.

Mae is the final human authority.

No AI agent may silently make a material architecture change that
conflicts with project governance or security policy.

---

# 82. BIZZ PROJECT Architecture Spirit

The architecture should support:

> **Zero Trust. Maximum Protection.**
>
> **Evidence Before Conclusions.**
>
> **Autonomous Investigation. Human-Authorized Impact.**
>
> **To Integrity and Beyond!**

---

**End of BIZZ PROJECT — System Architecture**
