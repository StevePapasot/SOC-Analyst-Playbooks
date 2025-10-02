### **Alert Example: “10 failed login attempts in 5 minutes from user X.”**

<img width="697" height="798" alt="image" src="https://github.com/user-attachments/assets/b854c840-473c-40c0-8588-d879205cf452" />


### Scope / When to use

Trigger this playbook for alerts such as:

- “X failed logins for user U within Y minutes.”
- “Multiple failed logins from same IP / across multiple accounts.”
- “Many failed logins across the estate from one IP” (credential stuffing).
- Repeated authentication failures followed by success.

### **What you check:**

- Was there a **successful login after the failures**?
- Source IP (internal or external, country, reputation).

### **Action:**

- If login eventually succeeded from suspicious IP → **escalate**.
- If just repeated failures with no success → monitor/possible FP.

## Fast triage checklist (L1 quick hits — do these first)

1. Note alert metadata: time window, username(s), source IP(s), count of failures, any successful follow-on login.
2. Check whether the alert shows a **successful** login after failures.
3. Check source IP geolocation / reputation.
4. Check whether the user is known to be traveling / using VPN.
5. Check if many accounts were targeted (mass attack) or just one account (targeted).
6. See if there are other correlated alerts (malware, lateral movement, suspicious process).
7. If suspicious → escalate immediately with details.

## Step-by-step investigation (detailed)

### Step A — Capture the facts

- Time of activity (UTC + local): record exact timestamps.
- Username(s) affected.
- Source IP(s) and geolocation.
- Number of failed attempts and time window.
- Whether a successful login occurred (and from which IP).
- Affected host(s) (workstation, DC, VPN appliance).

### Step B — Quick enrichment

- Lookup IP reputation (AbuseIPDB, internal TI).
- Reverse DNS for IP (if available).
- Check if IP is a known corporate VPN egress/remote office.
- Check past authentication history for the user (normal login hours, typical IPs).
- See if any other accounts had failures from same IP.

### Step C — Validate

- If no successful login and source IP = external & reputation bad → likely brute-force (escalate).
- If successful login from the same suspicious IP → escalate as potential compromise.
- If source IP is corporate VPN or known remote user → call/DM the user to confirm.
- If repeated failures from internal host or johnny local script → check with endpoint team (may be automated service account misconfigured).
- If multiple accounts targeted from one IP → treat as credential stuffing / automated attack → escalate.

### Step D - Escalate Criteria

Escalate if **any**:

- Successful login after failures (from external/malicious IP).
- Multiple accounts hit from one IP.
- Privileged/service accounts involved.
- Source IP = TOR, blacklisted, abnormal geo.

### Step E - L1 Actions

- Document **time, user, IP, host, count**.
- Screenshot / export QRadar offense & correlated events.
- Escalate with IOC list (IPs, usernames, event IDs).
- If policy allows: request **account lock / password reset / IP block**.

