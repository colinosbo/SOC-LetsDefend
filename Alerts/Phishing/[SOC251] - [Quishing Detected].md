
**Date/Time:** Jan 01, 2024 – 12:37 PM UTC
**Analyst**: Colin Osborn
**Severity**: Medium
**Status**: Closed - True Positive

---

## 1. Alert Summary

> A phishing email containing a QR code was delivered to a corporate user. The QR code redirected to a malicious URL, triggering the SOC251 'Quishing Detected' rule.

---

## 2. Affected Assets

| Field        | Value                |
| ------------ | -------------------- |
| Hostname     | Claire               |
| IP Address   | 172.16.17.181        |
| User Account | Claire@letsdefend.io |
| OS / System  | Windows              |

---

## 3. Email Details

| Field               | Value                                                                                |
| ------------------- | ------------------------------------------------------------------------------------ |
| SMTP Address        | 158.69.201.47                                                                        |
| Source Address      | security@microsecmfa.com                                                             |
| Destination Address | Claire@letsdefend.io                                                                 |
| Subject             | New Year's Mandatory Security Update: Implementing Multi-Factor Authentication (MFA) |
| Device Action       | Allowed                                                                              |

---

## 4. Indicators of Compromise (IOCs)

| Type   | Value                                                                                                                                        | Source       | Malicious? |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | ---------- |
| IP     | 158.69.201.47                                                                                                                                | SMTP IP      | Yes        |
| Domain | microsecmfa.com                                                                                                                              |              | Yes        |
| URL    | [https://ipfs.io/ipfs/Qmbr8wmr41C35c3K2GfiP2F8YGzLhYpKpb4K66KU6mLmL4#](https://ipfs.io/ipfs/Qmbr8wmr41C35c3K2GfiP2F8YGzLhYpKpb4K66KU6mLmL4#) | QR Code Link | Yes        |


---

## 5. Investigation Timeline

| Time (UTC) | Action                               | Finding                                     |
| ---------- | ------------------------------------ | ------------------------------------------- |
| 12:37 PM   | Alert Triggered                      | SOC251 – QR Phishing Detected               |
| 12:40 PM   | Checked SMTP IP reputation           | Marked as malicious                         |
| 12:43 PM   | Decoded URL code using external tool | URL resolved to IPFS-hosted page            |
| 12:48 PM   | Reviewed sender domain               | Not affiliated with legitimate MFA provider |
| 12:55 PM   | Reviewed endpoint logs for Claire    | No evidence of URL access or malware        |

---

## 6. Analysis

> Explain your reasoning in 3-5 sentences. What confirmed or ruled out a threat? What tools did you use?
> The Quishing alert was determined to be a **true positive** through scanning artifacts using threat intelligence. The sender IP came back as malicious and has been reported for phishing. The email appeared to be from Microsoft, but was not from the @Microsoft.com domain. The email contained a QR code, which contained a malicious link to a site that has been removed due to malware. Investigation of the targeted user's endpoint confirmed that the user did **NOT** click the link and did not need isolation.

---

## 7. MITRE ATT&CK Mapping

| Field         | Value                          |
| ------------- | ------------------------------ |
| Tactic        | Initial Access                 |
| Technique     | T1566 - Phishing               |
| Sub-technique | T1566.002 - Spearphishing Link |

---

## 8. Verdict

- [x] True Positive
- [ ] False Positive
- [ ] Benign

**Reasoning:** The email contained a malicious URL code linking to an untrusted external page. The sender IP was flagged as malicious. 

---

## 9. Actions Taken

- [x] IP/Domain blocked
- [x] Email quarantined
- [x] User notified
- [ ] Account disabled
- [ ] Escalated to L2
- [ ] No action required

---

## 10. Recommendations

> Implement enhanced QR code scanning in the email gateway
> Conduct phishing awareness training focused on QR code attacks
> Consider DMARC enforcement policies

---

