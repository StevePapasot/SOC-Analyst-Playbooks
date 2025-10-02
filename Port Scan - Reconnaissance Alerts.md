<img width="557" height="770" alt="image" src="https://github.com/user-attachments/assets/c3e174a4-bf4a-4642-a2a5-7b046f3a6dc7" />


## 1. Trigger

- Multiple connection attempts to many ports on a single host, or many hosts targeted across a port range.
- High SYN/SYN-ACK/FIN rates from same source.
- IDS/NSM/firewall or QRadar correlation flags suspected scanning behavior.
- Examples: nmap-like behavior, mass TCP SYNs, horizontal scans across hosts.

## 2. Capture Facts (L1 Steps)

- Record **Offense ID, timestamp, source IP, destination IP(s)/range, ports, protocol (TCP/UDP), packet type (SYN/ACK/FIN)**, and time window.
- Note affected asset(s) criticality (Crown jewels? internal servers? admin tools?).
- Export raw logs and any available **pcap/flow** snippets from IDS/firewall.

## 3. Correlate in QRadar

- Search related events: firewall deny/allow, IDS signatures, NetFlow/Zeek/Suricata logs, endpoint alerts on targeted hosts.
- Check if multiple offenses from same source across different targets/time windows.

## 4. Enrich Source (Threat Intel)

- Lookup IP reputation (AbuseIPDB, VirusTotal), ASN owner (cloud provider vs ISP), reverse DNS.
- Check whether source IP belongs to cloud scanning services (Shodan/Censys/Google Cloud/AWS ranges) or known scanner infrastructure.
- Check WHOIS / ASN for likely benign scanners (research, security vendor) vs suspicious botnets.

## 5. Determine Intent & Scope

- **Wide, low-rate scan** over many hosts → broad reconnaissance (often automated).
- **Focused, high-rate scan** on specific ports on critical hosts → targeted recon (higher risk).
- **Persistent repeats** across days → persistent actor (escalate).
- Check whether scans coincide with exploit attempts, authentication attempts, or suspicious process executions on targeted hosts.

## 6. Decision Logic

- **Internal authorized scan** (internal scanner, vulnerability management window) → False Positive / document & monitor.
- **External benign scanner** (Shodan, Censys tags, known research ASN) → monitor; block only if abusive.
- **Malicious scan** (blacklisted IP, targeting critical assets, followed by exploit attempts) → escalate and contain (block IP, update ACLs).

## 7. Escalation Criteria

Escalate to L2 / Network / IR if any:

- Targeting of **critical assets** (databases, DCs, admin interfaces).
- **Exploit attempts** or successful compromise indicators correlated after scan.
- **Persistent scanning** from same source over time.
- Source IP on high-risk threat lists or associated with known botnets/C2 infrastructures.
- Scan followed by authentication attempts or data exfil alerts.

## 8. L1 Actions

- Document **time, src IP, dest IPs/ports, protocol, packet types, asset criticality**.
- Export QRadar offense, logs, and any available pcap/flow samples.
- Enrich IP and include reputation info in the report.
- If authorized by policy: request **temporary firewall block / ACL update** and notify network team.
- Escalate with clear evidence: offense ID, logs, pcap snippets, asset list, recommended containment.
