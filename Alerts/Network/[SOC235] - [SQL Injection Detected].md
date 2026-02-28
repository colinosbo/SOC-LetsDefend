# [SOC235] - [SQL Injection Detected]

**Date/Time:** Feb, 28, 2026, 1:31 PM
**Analyst:** Colin Osborn
**Severity:** High
**Status:** Closed - True Positive

---

## Alert Summary

The alert was generated through a detected SQL injection on the Atlanta-Server.

---

## Affected Assets

| Field        | Value          |
| ------------ | -------------- |
| Hostname     | Atlanta-Server |
| IP Address   | 172.16.20.12   |
| User Account | n/a            |
| OS / System  | Windows        |

---

## Indicators of Compromise (IOCs)

| Type   | Value                                                                                                                                                                                                                                                                                          | Source        | Malicious? |
| ------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- | ---------- |
| IP     | <br>118.194.247.28                                                                                                                                                                                                                                                                             | Attacker IP   | yes        |
| Domain | N/A                                                                                                                                                                                                                                                                                            |               |            |
| URL    | GET /?douj=3034%20AND%201%3D1%20UNION%20ALL%20SELECT%201%2CNULL%2C%27%3Cscript%3Ealert%28%22XSS%22%29%3C%2Fscript%3E%27%2Ctable_name%20FROM%20information_schema.tables%20WHERE%202%3E1--%2F%2A%2A%2F%3B%20EXEC%20xp_cmdshell%28%27cat%20..%2F..%2F..%2Fetc%2Fpasswd%27%29%23 HTTP/1.1 200 865 | Requested URL | yes        |

---

## Investigation Timeline

| Time (UTC) | Action              | Finding                                |
| ---------- | ------------------- | -------------------------------------- |
| 1:31 PM    | Alert Triggered     | SQL Injection Detected                 |
| 1:32 PM    | Began investigation | Reviewed alert details                 |
| 1:34       | Reviewed proxy logs | successful sqlmap execution identified |
|            |                     |                                        |
|            |                     |                                        |

---

## Analysis

The incident was triggered due to a successful SQL injection attempt to the Atlanta-Server. The attacker '118.194.247.28' used SQL map to send multiple requests to the web server as seen through the proxy logs. Each attempted returned a 200 status code and 865 bytes of data indicating that the server processed the malicious payloads.

---

## MITRE ATT&CK Mapping

| Field         | Value                                     |
| ------------- | ----------------------------------------- |
| Tactic        | Initial Access / Discovery                |
| Technique     | T1190 - Exploit Public Facing Application |
| Sub-technique | N/A                                       |

---

## Verdict

- [x] True Positive
- [ ] False Positive
- [ ] Benign

**Reasoning:** The alert was determined to be a true positive through review of proxy logs that proved the attacker successfully executed automated SQL injections through sqlmap.

---

## Actions Taken

- [x] IP/Domain blocked
- [ ] Email quarantined
- [ ] User notified
- [ ] Account disabled
- [x] Escalated to L2
- [ ] No action required

---

## Recommended Remediations

>Block IP 118.194.247.28 at the firewall level
>Use parameterized queries in the web app code to prevent SQLi
>Disable SQL functions like **xp_cmdshell** that allow OS command execution
>Implement input validation and sanitization on all user-supplied parameters

---
