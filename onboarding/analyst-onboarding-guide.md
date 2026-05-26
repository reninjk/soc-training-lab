# SOC Analyst Onboarding Guide

**Welcome to the Security Operations Centre.** This guide walks you through your first 90 days and ensures you have the access, knowledge, and skills to operate effectively.

---

## Week 1 — Access & Orientation

### Day 1–2: Admin & Access Provisioning
- [ ] ID badge and physical access issued by HR
- [ ] Laptop built and encrypted by IT — confirm BitLocker/FileVault enabled
- [ ] SOC email account created and MFA enrolled
- [ ] Password manager account provisioned
- [ ] VPN client installed and tested
- [ ] Emergency contact added to SOC on-call roster

### Day 3–4: Tool Access
Request access to the following (raise a ticket per system):
- [ ] SIEM (read-only first; elevated after sign-off)
- [ ] EDR console
- [ ] Ticketing platform
- [ ] Vulnerability management platform
- [ ] Threat intelligence portal
- [ ] Chat platform (SOC channels)
- [ ] GitHub (this org — reninjk)

### Day 5: Orientation
- [ ] SOC layout walkthrough with team lead
- [ ] Shift patterns and handover process explained
- [ ] Escalation matrix reviewed (`soc-incident-response/templates/escalation-matrix.md`)
- [ ] Emergency procedures confirmed (fire exits, bomb threat, power outage)

---

## Week 2–3 — Core Tools Training

### SIEM
- [ ] Log source overview: what's ingested, normalisation schema
- [ ] Build and save 5 queries covering: auth failures, outbound DNS, lateral movement indicators
- [ ] Understand alert prioritisation and suppression rules
- [ ] Shadow senior analyst through 3 full triage cycles

### EDR
- [ ] Endpoint coverage map reviewed
- [ ] Alert categories and severity mapping
- [ ] Host isolation procedure practised in lab environment
- [ ] Evidence collection runbook reviewed (`soc-incident-response/runbooks/evidence-collection.md`)

### Ticketing Platform
- [ ] Ticket lifecycle: Open → In Progress → Pending → Resolved → Closed
- [ ] SLA definitions and breach alerts
- [ ] Required fields for incident vs. service request vs. vulnerability ticket

---

## Week 4 — Detection & Process

### Detection Rules
- [ ] Review all Sigma rules in `soc-detection-rules/sigma/`
- [ ] Understand MITRE ATT&CK framework basics (Tactics → Techniques → Sub-techniques)
- [ ] Run each Sigma rule through the SIEM and validate it produces expected results in the test tenant
- [ ] Identify one false-positive candidate and raise a tuning suggestion

### Incident Response
- [ ] Read all playbooks in `soc-incident-response/playbooks/`
- [ ] Read all runbooks in `soc-incident-response/runbooks/`
- [ ] Complete phishing triage scenario (`soc-training-lab/scenarios/scenario-01-phishing-triage.md`)
- [ ] Attend or review the most recent post-incident review

---

## Days 30–60 — Supervised Operations

- [ ] Handle 10 alerts independently (with senior analyst review)
- [ ] Raise 2 tickets from scratch (end-to-end: detect → triage → document → escalate)
- [ ] Participate in one tabletop exercise (`soc-training-lab/tabletop/`)
- [ ] Complete at least one certification module (`soc-training-lab/certifications/cert-roadmap.md`)
- [ ] SIEM access elevated to standard analyst level (manager sign-off)

---

## Days 60–90 — Independent Operations

- [ ] Handle shift independently with on-call escalation only
- [ ] Deliver one weekly metrics digest (`soc-automation/reporting/weekly_metrics.py`)
- [ ] Identify and document one detection gap (new Sigma rule or tuning suggestion)
- [ ] First formal performance check-in with SOC Manager

---

## Key Contacts

| Role | How to Reach |
|------|-------------|
| SOC Manager | Internal directory |
| On-Call Senior Analyst | On-call roster (ticketing platform) |
| IT Helpdesk | Internal ticket queue |
| CISO | Via SOC Manager escalation |
| Incident Response Retainer | See escalation matrix |

---

## Key Reference Documents

| Document | Location |
|----------|----------|
| Escalation Matrix | soc-incident-response/templates/escalation-matrix.md |
| Incident Playbooks | soc-incident-response/playbooks/ |
| Detection Rules | soc-detection-rules/sigma/ |
| SIEM Query Cheatsheet | soc-training-lab/quick-reference/siem-query-cheatsheet.md |
| Cert Roadmap | soc-training-lab/certifications/cert-roadmap.md |

---

## Sign-Off

| Milestone | Completed | Manager Sign-Off | Date |
|-----------|-----------|-----------------|------|
| Week 1 — Access & Orientation | ☐ | | |
| Week 2–3 — Tool Training | ☐ | | |
| Week 4 — Detection & Process | ☐ | | |
| Day 60 — Supervised Ops | ☐ | | |
| Day 90 — Independent Ops | ☐ | | |
