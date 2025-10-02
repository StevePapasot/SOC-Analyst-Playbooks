<img width="547" height="766" alt="image" src="https://github.com/user-attachments/assets/9bac00cf-f303-44b7-876a-c791036f8ac7" />

## Trigger

- Repeated authentication attempts for a user or many users within a short window (e.g., >10 attempts in 5 minutes).
- Can be failed-only (brute-force/credential stuffing) or a mix of failures and occasional successes.

## Capture Facts (L1 Steps)

- Record Offense ID, timestamps, username(s), source IP(s), destination/service, host(s), event IDs (4625, 4624), and failure count.
- Note whether any success occurred after failures and which IP it came from.
- Identify if attempts target one account or many accounts (horizontal vs vertical).

## Correlate in QRadar

- Pull related events: authentication logs, VPN logs, firewall, EDR alerts, IdP sign-in logs.
- Check for patterns: same source IP hitting multiple accounts, bursts across time, or single user attacked from many IPs.

## Enrich Source(s)

- IP reputation (AbuseIPDB, VirusTotal), ASN, geolocation, reverse DNS, TOR/anonymizer checks.
- Identify whether source IP belongs to cloud providers, known scanner ranges, or corporate VPN egress.

## Determine Intent & Scope

- Vertical attack: many failures on one account → targeted brute-force.
- Horizontal attack: same IP hitting many accounts → credential stuffing.
- Distributed: many IPs hitting many accounts → botnet/credential spray.

## Decision Logic

- Failures only, internal/trusted IP, user confirms → monitor/close (FP).
- Failures only, external malicious IP or many accounts targeted → escalate (brute-force/credential stuffing).
- Failures followed by success from suspicious IP → escalate immediately (possible compromise).
- Privileged account targeted or successful login → escalate immediately.

## Escalation Criteria

Escalate if any:

- Successful login after failures from suspicious IP.
- Multiple accounts targeted from same IP or botnet behavior.
- Privileged/service accounts targeted or accessed.
- Correlated malicious activity on endpoints or network (C2, lateral movement).

## L1 Actions

- Document time, users, IPs, hosts, counts; screenshot QRadar offense and timeline.
- Run AQL queries to list related attempts and history for users/IPs.
- Enrich and include reputation data in the report.
- Contact user(s) to verify legitimate activity where appropriate.
- If policy allows: request account lock, force password reset, block IPs, or request endpoint isolation via EDR team.
- Escalate to L2/IR with evidence package (offense ID, logs, AQL results, user confirmations).
