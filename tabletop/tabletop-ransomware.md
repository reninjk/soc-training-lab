# Tabletop Exercise — Ransomware Incident

**Exercise Type:** Tabletop (Discussion-Based)  
**Duration:** 2.5–3 hours  
**Participants:** SOC Manager, SOC Analysts (x2), IR Lead, IT Operations, Legal/Compliance, Communications  
**Facilitator:** SOC Manager or external IR consultant  
**MITRE ATT&CK:** T1486 (Data Encrypted for Impact), T1490 (Inhibit System Recovery), T1489 (Service Stop)

---

## Exercise Objectives

1. Validate the organisation's ability to detect and respond to a ransomware incident
2. Test decision-making under time pressure — particularly the isolate-vs-investigate trade-off
3. Identify gaps in backup/recovery procedures and communication protocols
4. Confirm legal and regulatory notification obligations are understood

---

## Ground Rules

- This is a no-fault exercise — there are no wrong answers, only learning opportunities
- Participants speak from their actual role and authority
- Injects are presented one at a time; discussion continues until the facilitator moves on
- Classified/sensitive details are excluded; participants use generic system names
- All findings are documented for the post-exercise After Action Review (AAR)

---

## Scenario Narrative

### T+0:00 — Initial Detection

> It is 02:47 on a Friday morning. An on-call SOC analyst receives a PagerDuty alert.
> Multiple endpoints in the Finance department are showing a spike in disk I/O and CPU.
> The EDR console shows a new process: `msupdate.exe` spawned from `winword.exe`.
> File rename events are flooding in — hundreds of files renamed to `.enc` extension per minute.

**Discussion Questions:**
1. Who does the on-call analyst notify first?
2. At what point do you declare this a P1 incident vs. continue investigating?
3. What is the immediate containment decision — isolate endpoints now or gather more evidence?
4. Who has authority to approve mass endpoint isolation?

---

### T+0:30 — Scope Expansion

> Re-scan of the environment shows 47 endpoints affected across Finance, HR, and the file server.
> The domain controller shows unusual authentication events from a service account.
> Backups are stored on a NAS share that appears to be reachable from the affected segment.

**Discussion Questions:**
1. How do you protect the backup server — can you isolate the NAS immediately?
2. Is this human-operated ransomware or automated? What evidence would distinguish them?
3. Who notifies the CISO and board? What is the communication script?
4. Do you engage your IR retainer now? What is the trigger?

---

### T+1:00 — Ransom Note Discovered

> A ransom note is found on affected systems: `READ_ME_DECRYPT.txt`.
> The note demands payment in cryptocurrency within 72 hours.
> It claims the attacker has exfiltrated 200GB of data and will publish it if payment is not made.

**Discussion Questions:**
1. What is your policy on ransom payment? Who makes this decision?
2. Does the exfiltration claim change the incident classification?
3. When and how do you notify your cyber insurance provider?
4. What are the regulatory notification requirements? (GDPR 72-hour window, sector-specific rules)
5. How do you communicate to staff without tipping off the attacker?

---

### T+2:00 — Recovery Decision

> IT Operations confirms backups are intact — last clean backup is from 18:00 the previous evening.
> Estimated recovery time for full restore: 36–48 hours.
> Legal advises that the 72-hour GDPR notification clock started at T+0.
> The attacker has posted a sample of the stolen data on a leak site.

**Discussion Questions:**
1. Do you begin recovery immediately or continue forensic investigation first?
2. What must be preserved before reimaging — and who is responsible?
3. What do you say in the GDPR notification? Who drafts and approves it?
4. What is the public/media communication strategy if the leak is discovered by the press?
5. How do you confirm the environment is clean before reconnecting restored systems?

---

## Key Decision Points Summary

| Time | Decision | Decision Maker | Policy Reference |
|------|----------|----------------|-----------------|
| T+0:05 | Isolate endpoints | SOC Manager | IR Playbook — Isolation |
| T+0:15 | Declare P1 incident | SOC Manager | Incident Classification |
| T+0:20 | Engage IR retainer | CISO | Retainer contract |
| T+0:45 | Notify CISO/board | SOC Manager | Escalation Matrix |
| T+1:10 | Ransom payment decision | Board + Legal | Payment Policy |
| T+1:15 | Insurance notification | Legal/Compliance | Insurance policy |
| T+1:20 | GDPR notification decision | DPO + Legal | GDPR Art. 33 |
| T+2:00 | Begin recovery | CISO + IT Ops | BCP/DR Plan |

---

## After Action Review (AAR)

To be completed within 5 business days of the exercise.

### What Went Well
<!-- Facilitator documents during exercise -->

### Gaps Identified
<!-- Facilitator documents during exercise -->

### Action Items

| Action | Owner | Due Date | Status |
|--------|-------|----------|--------|
| [ACTION-PLACEHOLDER] | | | Pending |

---

## References
- soc-incident-response/playbooks/ — Incident response playbooks
- soc-incident-response/templates/escalation-matrix.md — Who to call and when
- NIST SP 800-184 — Guide for Cybersecurity Event Recovery
- CISA Ransomware Guide — https://www.cisa.gov/stopransomware
