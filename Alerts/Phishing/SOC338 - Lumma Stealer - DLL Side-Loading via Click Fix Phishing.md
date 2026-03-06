
**Date/Time:** Apr, 19, 2024, 08:23 AM
**Analyst:** Colin Osborn
**Severity:** Critical
**Status:** Closed - True Positive

---

## Alert Summary

> A phishing email impersonating Microsoft was delivered to dylan@letsdefend.io, luring the user to visit a fake Windows 11 upgrade page at windows-update.site. The page used a Click Fix social engineering technique, it instructed Dylan to paste an obfuscated PowerShell command into the Run dialog in Windows The command abused mshta.exe to fetch and execute a remote HTA payload from overcoatpassably.shop, delivering Lumma Stealer via DLL side-loading. The Malware was quarantined.

---

## Affected Assets

| Field        | Value               |
| ------------ | ------------------- |
| Hostname     | dylan               |
| IP Address   | 172.16.17.216       |
| User Account | dylan@letsdefend.io |
| OS / System  | Windows             |

---

## Email Details

| Field               | Value                                          |
| ------------------- | ---------------------------------------------- |
| SMTP Address        | 132.232.40.201                                 |
| Source Address      | update@windows-update.site                     |
| Destination Address | dylan@letsdefend.io                            |
| Subject             | Upgrade your system to Windows 11 Pro for FREE |
| Device Action       | Allowed                                        |

---

## Indicators of Compromise (IOCs)

| Type   | Value                 | Source              | Malicious? |
| ------ | --------------------- | ------------------- | ---------- |
| IP     | 132.232.40.201        | Threat Intel        | Yes        |
| Domain | windows-update.site   | Email Security Logs | Yes        |
| Domain | overcoatpassably.shop | Terminal History    | Yes        |

---

## Investigation Timeline

| Time (UTC) | Action                                | Finding                                                                     |
| ---------- | ------------------------------------- | --------------------------------------------------------------------------- |
| 9:44 AM    | Alert Triggered                       | Lumma Stealer ClickFix phishing email delivered to Dylan                    |
| 9:45 AM    | Checked SMTP IP reputation            | malicious                                                                   |
| 9:47 AM    | Reviewed sender domain                | confirmed not affiliated with microsoft                                     |
| 9:50 AM    | Reviewed email body                   | URL embedded in email                                                       |
| 9:53 AM    | Reviewed browser and terminal history | Dylan visited https://windows-update.site/ — confirmed user clicked the URL |

---

## Analysis

> A phishing alert was determined to be a true Positive. A Phishing email from update@windows-update.site was delivered to dylan@letsdefend.io. This email was impersonating a legitimate Microsoft Windows 11 upgrade offer. Dylan clicked the embedded URL, landing on a ClickFix lure page that instructed him to paste an obfuscated PowerShell command into his Run dialog. The command used string obfuscation by inserting ]]] characters through the string, which were later striped at runtime via -replace. This payload was responsible for site-loading the Lumma Stealer DLL. 

---

## MITRE ATT&CK Mapping

| Field     | Value                                      |
| --------- | ------------------------------------------ |
| Tactic    | Initial Access                             |
| Technique | T1566.002 - Spearphishing Link             |
| Tactic    | Execution                                  |
| Technique | T1574.002 - DLL Side-Loading               |
| Technique | T1204.001 - User Execution: Malicious Link |

---

## Verdict

- [x] True Positive
- [ ] False Positive
- [ ] Benign

**Reasoning:** This alert was confirmed to be a true positive as Dylan visited the maliciour URL embedded in the phishing email. A clickFix lure tricked him into executing an obfuscated PowerShell command that abused mshta.exe to getch and run Lumma Stealer payload.

---

## Actions Taken

- [x] IP/Domain blocked
- [x] Email quarantined
- [x] User notified
- [ ] Account disabled
- [x] Escalated to T2
- [ ] No action required

---

## Recommendations

> - Block windows-update.site at proxy and firewall level
> - Enforce email gateway URL scanning 
> - Conduct phishing awareness training focused on fake CAPTCHA lures.

---