A user account has logged into Office 365 (Azure AD / Microsoft Entry ID) from a location, device, or IP address that deviates from their normal behavior.

<img width="910" height="377" alt="image" src="https://github.com/user-attachments/assets/ea2c7d11-ce4d-4e13-a668-d6b680df02c2" />

!!! A variation of that is the Suspicious Office365 Geography Change. It specifically highlights “impossible travel” or “location drift”.

<img width="936" height="342" alt="image" src="https://github.com/user-attachments/assets/f91fb808-7d27-4e94-b086-4c301c22a7cf" />

## **Investigation workflow**

### Step 1. Open and Review the Offense

→ Confirm which account, where and how suspicious login happened. 

*Find: Username, Source IP, Destination IP, Event Name, Timeframe and log source.*

What we want to see?

1. Is the same user successfully logged in?
2. Is the source IP internal or external?
3. Are the logs successful or failed?

<img width="886" height="176" alt="image" src="https://github.com/user-attachments/assets/a98575da-f1a5-4a82-83da-9c7f6be9cc0d" />

### **Step 2. Focus on user behavior**

→ When and where he usually connects?

- Check the destination IP. Is it legitimate MS infrastructure?
- Is the username the same? Consistent.

<!!!> Make authentication Analysis

Observe MFA - and device- related alerts.

ex. 

- `LogonError-UserStrongAuthClientAuthNRequiredInterrupt`
- `LogonError-DeviceNotCompliantDeviceManagementRequired`
- `Login Error Device Authentication Required`

The above indicated that MFA or conditional access checks were applied and paused.

*** On the behavior check where did he logged in. Did he do something not normal?

<!> Pay attention on the change of the event on the Timeframe. When did the alert hit?

<img width="846" height="360" alt="image" src="https://github.com/user-attachments/assets/a860949c-6036-4dae-9f54-b8a66ba419fb" />

example investigation steps.

## 🧭 Investigation Steps – *Suspicious Office365 Login*

1. **Opened the offense** → Collected alert details (source IP, username, destination IP, magnitude, rule name).
2. **Excluded CRE filter** → Focused only on raw portal_azure events.
3. **Checked source IP** → Found `194.116.208.241` (HostRoyale VPN, India-registered, Greece-based).
4. **Verified MFA/device events** → Saw MFA and compliance prompts → authentication valid.
5. **Checked user history (30 days)** → Normal activity (Teams, SharePoint, file access).
6. **Confirmed username consistency** → Same user across all events.
7. **Analyzed IP behavior** → VPN/cloud IPs, all European, not malicious.
8. **Correlated all data** → No signs of compromise or privilege actions.
9. **Classified alert** → **False Positive (legit user using VPN).**
10. **Recommended actions** → Mark FP in QRadar, note VPN ISP, continue monitoring.

<img width="856" height="854" alt="image" src="https://github.com/user-attachments/assets/af20fbe5-6be9-4f78-ad37-1c2aace35d22" />


