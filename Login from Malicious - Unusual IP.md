<img width="955" height="1135" alt="image" src="https://github.com/user-attachments/assets/0a42710a-dcba-41e4-835b-b55ed35ed59b" />

# 1. Trigger

- QRadar offense: Successful login from an IP flagged as **malicious / TOR / anonymizer / unusual ASN**.
- Login from a **geolocation** never seen before for this user.
- Threat intel feed match for IP (blacklisted, C2, phishing infra).

---

# 2. Quick Triage (L1 Steps)

- **Alert Details** → username, src IP, geo, timestamp, service (VPN, O365, endpoint).
- **Check in QRadar:**
    - Events: `Authentication Success` (EventID=4624 or IdP log).
    - Correlate with any **Authentication Failures** before success.
- **Enrich IP:**
    - QRadar TI, AbuseIPDB, VirusTotal, ASN, reverse DNS.
- **User Context:** traveling? remote worker? VPN use? privileged account?

---

# 3. Decision Logic

- **Known/benign IP** (internal range, corporate VPN, user confirms) → **monitor/close (FP)**.
- **Malicious IP + successful login** → **escalate immediately** (possible account compromise).
- **Malicious IP + privileged account** → escalate even faster (critical).
- **Unusual IP but benign reputation** → verify with user; escalate if unverified.

---

# 4. Escalation Criteria

Escalate if **any**:

- IP is on threat feeds (TOR, botnet, blacklisted).
- Successful login from malicious IP.
- Privileged/service account logged in from unusual IP.
- Correlated alerts: data access anomalies, suspicious processes, mailbox rules, exfiltration.

---

# 5. L1 Actions

- Document: time, username, src IP, ASN, geo, event IDs.
- Export QRadar offense and timeline.
- Enrich IP reputation; screenshot evidence.
- Contact user (securely) to confirm login legitimacy.
- If policy allows: request account lock / MFA reset / IP block.
