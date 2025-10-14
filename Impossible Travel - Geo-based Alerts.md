<img width="669" height="911" alt="image" src="https://github.com/user-attachments/assets/a2acbb75-55bc-4481-8c71-42523de9b00f" />

## WHAT IT MEANS?
It means that the user was successfully loggedin or tries to login from two IPs that are in two different locations and our user is impossible to be there. Two distant locations in a short period of time (minutes-seconds)


## Trigger

- Two (or more) successful logins for the **same user** from **distant geolocations** within a short window (e.g., Athens → New York in 2 hours).
- Successful login from a **new country/IP** outside the user’s normal profile.
- Geo-anomaly raised by an IdP (Okta/Azure AD) or QRadar rule.

---

# Investigation
## STEP 1. Gather information:
- Username
- Source IP
- Destination IP
- Timestamp
- Log source
- Auth Type (Success/Fail)


### WHAT AUTH TYPES MEAN?
1. Successful Login = Creds Worked -> A) Compromised Credentials 
                                      B) Legitimate User login
2. Failed Login -> A) Brute-force (5 attempts in 2 minutes)
                   B) Compromised credentials tested
                   C) Recon

## STEP 2. Behavior:
1) Check if the user have done it in the past.
2) Check all the source IPs.

<!> IMPORTANT: Check if the user is using VPN <!>

### TIP: ip analyze checklist.
1. OWNERSHIP (ASN) -> WHO OWNS THE IP?
2. GeoIP location
3. Anonymous Proxy (VT)
4. Reputation Check (VT, AbuseIPDB)
5. Passive DNS/Hostnames (Any linked domains? DNS/VT)
6. Check if used before (7 or 30 days TimeFrame)
7. Login Method Used (Raw Logs - Password or MFA use)
8. Log Source












