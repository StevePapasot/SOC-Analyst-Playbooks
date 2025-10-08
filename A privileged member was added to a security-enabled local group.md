# A privileged member was added to a security-enabled local group

This alert indicates that a user account **with escalated privileges** (administrator or elevated account) **is added to a Windows local group that grants privileges**, such as:

- Administrators
- Remote Desktop User
- Backup Operators
- Power Users

### What indicates

1. Legitimate administrative action.
2. Privilege escalation attempt.

### Impact

- Expands the attacker’s access level on that host.
- Allows execution of commands, disabling defenses, persistence or lateral movement.
- High risk

## A. Legitimate administrative activity

- Username. Is the username that did it the administrator or just a random user.
- Source IP address: Is the source IP from a Domain Controller/ management server or an endpoint?
- Timeframe: Did all this happened on reasonable working hours or on strange timeframe?
- Destination IP. Are the changes (the destination IP) are locally or not?

### Investigate

1. **Filter the username**

Add time range 1-2 hours time range of the event.

**Filter also the destination IP to narrow down the results.**

1. **Identify who or what was added and to which group.**

Add as filters the event ids:

4728 -> Member added to a security-enabled global group.

4732 -> Member added to a security-enabled local group.

4756-> Member added to a security-enabled universal group.

1. **Extra: Revel the added member and group**

Click on the Event Details and search for the

- Username (Who performed the action)
- Group name
- Target username
- Source IP/ Dest IP

## B. Privilege Escalation

1. Check the administrator’s logon history

Add time range 1-2 hours time range of the event.

**Filter also the destination IP to narrow down the results. ⇒ *Look for: Logon type or session origin, Source IP of the login, Multiple logons before success, Any remote IPs***

1. Compare source IPS

Filter only the username to see if the same user logged in from other hosts or IPs recently.

1. Look for suspicious patterns

Multiple failed logins

Logon from non admin accounts

After hours activity

<img width="1313" height="600" alt="image" src="https://github.com/user-attachments/assets/83d02a3a-40a4-4c97-91ea-ba4f2a2a3428" />
