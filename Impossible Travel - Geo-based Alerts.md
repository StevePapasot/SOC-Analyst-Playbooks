<img width="669" height="911" alt="image" src="https://github.com/user-attachments/assets/a2acbb75-55bc-4481-8c71-42523de9b00f" />

# 1. Trigger

- Two (or more) successful logins for the **same user** from **distant geolocations** within a short window (e.g., Athens → New York in 2 hours).
- Successful login from a **new country/IP** outside the user’s normal profile.
- Geo-anomaly raised by an IdP (Okta/Azure AD) or QRadar rule.

---

# 2. Quick Triage (L1 Steps)

- **Alert Details** → username, IPs, geo (city/country), service/app, timestamps.
- **Check in QRadar:**
    - Events: **Authentication Success** (e.g., Windows `4624`, IdP sign-in success).
    - Compare with **Authentication Failure** just before success (password spraying → success).
- **Enrich IPs:**
    - QRadar TI / AQL lookups for reputation, ASN, reverse DNS.
    - External (AbuseIPDB, VirusTotal) if needed.
- **User Context:** traveling? corporate **VPN** egress? remote office? **MFA** prompted/approved? device known?

---

# 3. Decision Logic

- **Explained travel** (corporate VPN exit, known remote office, user confirms trip, matching MFA prompt) → **monitor/close (FP)**.
- **Unexplained** geo change **with success**, esp. from **malicious/TOR** IP → **escalate immediately** (possible account compromise).
- Any **privileged/service account** involved → **escalate**, even if “maybe” explained.

---

# 4. Escalation Criteria

Escalate if **any**:

- Successful login from **two distant countries** within a short window and **no VPN explanation**.
- Source IP has **bad reputation**, anonymizer/TOR, or unusual ASN.
- **Correlated alerts** (malware, new processes, password reset abuse, mailbox rules, data access spikes).
- **Privileged** or business-critical account impacted.

---

# 5. L1 Actions

- Document **time, user, IPs, geo, device/app**, and MFA outcome.
- **Screenshot/export** QRadar offense + event timeline.
- **Contact user** (securely) to verify travel/MFA; note the response.
- If policy allows: request **session revoke / MFA reset / temporary account lock / IP block**.
