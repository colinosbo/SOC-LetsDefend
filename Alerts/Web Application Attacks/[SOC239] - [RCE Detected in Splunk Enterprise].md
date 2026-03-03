# [SOC239] - [RCE Detected in Splunk Enterprise]

**Date/Time:**   Nov, 21, 2023, 12:24 PM
**Analyst:** Colin Osborn
**Severity:** High
**Status:** Closed - True Positive 

---

## Alert Summary

> The attacker brute forced a default login on the splunk host. CVE-2023-46214 was successfully executed to upload a malicious 'shell.xsl' through the upload directory in splunk. This allowed the attacker to successfully initiate a reverse shell.

---

## Affected Assets

| Field        | Value             |
| ------------ | ----------------- |
| Hostname     | Splunk Enterprise |
| IP Address   | 18.219.80.54      |
| User Account | Admin             |
| OS / System  | Ubuntu            |


---

## Indicators of Compromise (IOCs)

| Type | Value                                                                                                                          | Source               | Malicious? |
| ---- | ------------------------------------------------------------------------------------------------------------------------------ | -------------------- | ---------- |
| IP   | 180.101.88.240                                                                                                                 | Proxy / Firewall log | Yes        |
| File | shell.xsl                                                                                                                      | Proxy Log            | Yes        |
| File | shell.sh                                                                                                                       | Firewall Log         | Yes        |
| URL  | http://18.219.80.54:8000/en-US/splunkd/__upload/indexing/preview?output_mode=json&props.NO_BINARY_CHECK=1&input.path=shell.xsl |                      |            |

---

## Investigation Timeline

| Time (UTC) | Action                  | Finding                                                                                                                                                 |
| ---------- | ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 6:04 PM    | Alert Triggered         | RCE Detected in Splunk Enterprise                                                                                                                       |
| 6:04 PM    | Reviewed HTTP login log | Attacker 180.101.88.240 successfully authenticated to Splunk web interface using default admin credentials via automated Python tool. HTTP 200 returned |
| 6:05 PM    | Reviewed SOC alert      | Malicious shell.xsl uploaded through Splunk upload directory. NO_BINARY_CHECK=1 used to bypass file validation. CVE-2023-46214 confirmed                |
| 6:07 PM    | Reviewed firewall log   | shell.sh executed on host, reverse shell established. Outbound connection from 18.219.80.54:54321 to attacker 180.101.88.240:1923 confirmed             |

---

## Analysis

The threat actor successfully logged into the Splunk web interface using an automated python tool. a malicious 'shell.xsl' file was uploaded through the upload directory using the **NO_BINARY_CHECK** parameter to successfully bypass file validation. The file uploaded created a shell.sh file on the server, successfully executing a reverse shell on the Splunk Enterprise host.

---

## MITRE ATT&CK Mapping

| Field         | Value                    |
| ------------- | ------------------------ |
| Tactic        | Initial Access           |
| Technique     | T1078 - Valid Accounts   |
| Sub-technique | T1078 - Default Accounts |

| Field         | Value                                                 |
| ------------- | ----------------------------------------------------- |
| Tactic        | Execution                                             |
| Technique     | T1059.004 - Application Layer Protocol: Web Protocols |
| Sub-technique | N/A                                                   |

---

## Verdict

- [x] True Positive
- [ ] False Positive
- [ ] Benign

**Reasoning:** Confirmed malicious activity. Attacker authenticated using default credentials via automated Python tool, exploited CVE-2023-46214 too upload and execute malicious XSL file.

---

## Actions Taken

- [x] IP/Domain blocked
- [ ] Email quarantined
- [ ] User notified
- [x] Account disabled
- [x] Escalated to L2
- [ ] No action required