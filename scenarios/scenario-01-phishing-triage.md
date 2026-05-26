# Scenario 01 — Phishing Triage

**Difficulty:** Beginner  
**Estimated Time:** 45–60 minutes  
**MITRE ATT&CK:** T1566.001 (Spearphishing Attachment), T1059.005 (VBA), T1027 (Obfuscated Files)  
**Skills Practised:** Email analysis, IOC extraction, SIEM querying, escalation decision

---

## Learning Objectives

By the end of this scenario you will be able to:
1. Identify indicators of a phishing email using headers and attachment analysis
2. Extract IOCs (sender domain, URL, file hash) and enrich them
3. Query the SIEM to determine if the email was delivered and clicked
4. Make a correct triage decision: close, monitor, or escalate

---

## Scenario Background

It is 09:15 on a Tuesday. The SOC ticketing platform receives an auto-generated alert:

> **Alert:** Email Security Gateway — Suspicious Attachment Blocked  
> **Recipient:** j.harris@[organisation].internal  
> **Subject:** URGENT: Invoice #INV-2024-8821 Overdue — Action Required  
> **Attachment:** Invoice_8821.xlsm (Excel macro-enabled workbook)  
> **Sender:** accounts-payable@invoic3-portal[.]net  
> **Sending IP:** 198.51.100.47 (placeholder — fictional)  
> **Gateway Action:** Quarantined

Your job is to triage this alert.

---

## Phase 1 — Initial Assessment (10 min)

### Step 1.1 — Review the alert
Answer the following before looking anything up:

1. What type of threat does this appear to be? *(phishing / malware / BEC / credential theft)*
2. What is suspicious about the sender domain? *(compare to legitimate domain)*
3. What makes .xlsm files particularly dangerous?
4. Does URGENT and Action Required in the subject indicate anything?

> **Expected Answers:** Spearphishing with macro-enabled attachment; sender domain uses a lookalike (invoic3 vs invoice); xlsm files contain VBA macros which execute code; urgency language is social engineering.

---

## Phase 2 — IOC Extraction & Enrichment (15 min)

### Step 2.1 — Extract IOCs
List all IOCs from the alert:

| IOC Type | Value | Source |
|----------|-------|--------|
| Sender domain | accounts-payable@invoic3-portal[.]net | Email header |
| Sending IP | 198.51.100.47 | Email header |
| Attachment name | Invoice_8821.xlsm | Email gateway alert |
| File hash (SHA-256) | [obtain from gateway alert details] | Email gateway |

### Step 2.2 — Enrich each IOC
Using your TI platform and public sources:

- [ ] Domain registration date — recently registered? (red flag if < 30 days old)
- [ ] IP reputation — is the sending IP on any blocklists?
- [ ] File hash — submit to sandbox / check VirusTotal detection ratio
- [ ] Domain — check for SPF/DKIM/DMARC — did the email fail authentication?

> **Training Note:** In a real environment, use the IOC enrichment script at `soc-automation/ioc-enrichment/enrich_ioc.py` or your TI platform.

---

## Phase 3 — SIEM Investigation (15 min)

### Step 3.1 — Determine delivery status
Query the SIEM to answer: *Was the email delivered to any other recipients?*

```
index=email_gateway action=delivered sender_domain=invoic3-portal.net
| stats count by recipient, subject, timestamp
```

### Step 3.2 — Check for execution
Query: *Did any endpoint open an .xlsm file in the relevant timeframe?*

```
index=edr event_type=file_open file_extension=xlsm earliest=-2h
| stats count by hostname, user, file_name, timestamp
```

### Step 3.3 — Check for macro execution
Query: *Was Excel used to spawn a child process (macro execution indicator)?*

```
index=edr parent_process=excel.exe event_type=process_create earliest=-2h
| stats count by hostname, child_process, command_line, timestamp
```

---

## Phase 4 — Triage Decision (10 min)

| Outcome | Criteria |
|---------|---------|
| **Close — False Positive** | Sender is legitimate; attachment is safe; no suspicious enrichment results |
| **Monitor — Low Risk** | Email quarantined; no delivery; IOCs low-reputation but no active threat |
| **Escalate — Incident** | Email delivered AND opened; macro executed; C2 callback detected |

### Decision Framework
1. Was the email delivered to any recipient? If yes, continue
2. Did any user open the attachment? If yes, continue
3. Did Excel spawn a child process? If yes → **ESCALATE** (follow account-compromise or malware playbook)
4. If quarantined with no delivery → **Monitor**, add IOCs to watchlist

---

## Phase 5 — Documentation (5 min)

Regardless of outcome, document in the ticket:
- [ ] Summary of findings
- [ ] All IOCs extracted and enrichment results
- [ ] SIEM queries run and key results
- [ ] Triage decision and justification
- [ ] Actions taken (IOCs blocked, user notified, escalated to IR)

---

## Debrief Questions

1. What additional context would change your triage decision?
2. How would you handle this if 50 employees received the same email?
3. What detection rule would catch macro-spawned child processes? *(hint: see soc-detection-rules/sigma/execution/)*
4. If this were a targeted attack on the finance team, what is the likely threat actor objective?

---

## Trainer Notes

**Inject variations to increase difficulty:**
- Some recipients received and opened the email (require full IR activation)
- The attachment was renamed to a .pdf to bypass gateway filters
- The sending IP resolves to a known APT infrastructure (raise TI alert)

**Pass criteria:** Analyst correctly identifies triage outcome and documents all IOCs with enrichment.
