# [SOC176] - [RDP Brute Force Detected]

**Date/Time:** Mar, 07, 2024, 11:44 AM
**Analyst:** Colin Osborn
**Severity:** Medium
**Status:** Closed - True Positive

---

## Alert Summary

> An RDP brute force was detected from 218.92.0.56. This was determined to be a true positive as process and terminal history confirms malicious post exploitation recon activity.

---

## Affected Assets

| Field        | Value         |
| ------------ | ------------- |
| Hostname     | Matthew       |
| IP Address   | 172.16.17.148 |
| User Account | Matthew       |
| OS / System  | Windows       |


---

## Indicators of Compromise (IOCs)

| Type | Value       | Source             | Malicious? |
| ---- | ----------- | ------------------ | ---------- |
| IP   | 218.92.0.56 | Firewall / OS logs | Yes        |


---

## Investigation Timeline

| Time (UTC) | Action            | Finding                                                |
| ---------- | ----------------- | ------------------------------------------------------ |
| 9:10 PM    | Alert Triggered   | Multiple failed logon attempts                         |
| 9:11 PM    | Reviewed Logs     | Confirmed successful logon (event 4624)                |
| 9:12 PM    | Contained Machine | Reconnaissance commands ran in the terminal            |
| 9:14 PM    | Escalated to T2   | Confirmed intrusion with active recon on internal host |


---

## Analysis

An RDP brute force was detected from an external IP that was confirmed to be malicious. Reviewing OS and firewall logs confirmed this attack was a true positive and required endpoint containment. Terminal history revealed post exploitation recon commands including whoami, net user, net localgroup administrators, and netstat -ano. This indicated that the attacker was enumerating users, privileges and network connections.

---

## MITRE ATT&CK Mapping

| Field         | Value                       |
| ------------- | --------------------------- |
| Tactic        | Initial Access              |
| Technique     | Brute Force T1110           |
| Sub-technique | Password Guessing T1110.001 |

---

## Verdict

- [x] True Positive
- [ ] False Positive
- [ ] Benign

**Reasoning:** The brute force attack from confirmed malicious IP 218.92.0.56 was successful. Evidence of post-exploitation recon commands executed on the victim host Matthew within one minute of the alert triggering.

---

## Actions Taken

- [x] IP/Domain blocked
- [ ] Email quarantined
- [x] User notified
- [x] Account disabled
- [x] Escalated to L2
- [ ] No action required

---
