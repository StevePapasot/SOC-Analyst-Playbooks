### ⚙️ **Alert meaning**

**“Azure AD Add User to Privileged Roles”** means that a user account in Azure Active Directory has been **granted administrative permissions**.

Someone either:

- added an account to a **built-in privileged role** (e.g., *Global Administrator, User Administrator, Exchange Administrator*), or
- modified an existing role assignment.

### 🎯 **Why it matters**

Privilege-granting events are critical because they can:

- give a normal user **full tenant control**,
- allow **mailbox access, MFA reset, or app registration**,
- or be used for **persistence** by an attacker after compromise.

So this alert sits in the **High / Critical** tier.

---

### 🧩 **What it affects**

- **Azure AD / Microsoft Enter ID** roles
- All integrated **Office 365**, **SharePoint**, **Teams**, **Intune**, **Exchange** services
- Downstream privileges inside connected apps and cloud resources

<img width="939" height="565" alt="image" src="https://github.com/user-attachments/assets/a5f0ae2d-1395-4ecb-a0fd-a2f9b849b64b" />

**Why it triggered?**

Someone executed a privileged role modification in Azure → Admin right ✅  // Privilege Escalation, Persistence, Account Compromised ⛔

## Investigation

### Step 1. Open & Review the Offense

Identify who added whom, to which rule, and from where?
✅ **Key Fields to Collect from QRadar**

- Username/ Actor → Who performed the role addition.
- Target User → Who received the privileged role.
- Role ID → Which role was granted
- Source IP (Internal or External) → Where the action came from (internal / external / API call)
- Log Source → See if done via portal, PowerShell, Graph API
- Event Time → Correlate with change windows

<img width="858" height="136" alt="image" src="https://github.com/user-attachments/assets/d183798b-3728-441c-84f2-ddefb5e2b709" />

### Step 2. Behavior & Legitimacy Check

Go see the info of the event.

**Check if the OAuth Actor has the rights to perform such actions.**

→ Check the username.

→ 30day time frame.

→ Event Name similar to the alert.

*<!> ALWAYS CHECK THE INFO INSIDE EVERY EVENT <!>*

### Step 3. Make Decision 
🧭 **Step-by-Step: Confirming if It’s a Planned Action**

1️⃣ Check the actor’s normal role

- If the actor is from **Sales** (not IT/Admin) → 🚩 unusual.
- If the actor has previous “role-management” or “Azure AD admin” activity → ✅ likely legitimate.

---

### **2️⃣ Check internal change-management**

- Look for a **ticket or request** in your company’s **ITSM** (e.g., ServiceNow, Jira, etc.).
- If there’s a matching approved request → ✅ **planned**.
- If no record → 🚩 **suspicious**.

---

### **3️⃣ Check with the Identity / Azure team**

- Contact the **IAM or Azure AD admin team**:
    
    > “Do you recognize this role assignment made by salesagent@barbastathis.com at 12:29 PM?”
    > 
- They can verify if it was an internal administrative test or external consultant onboarding.

---

### **4️⃣ Validate the target user**

- The target is **external (`#EXT#`)**, so confirm with the **project owner** or **IT manager** if this guest should have Security Admin privileges.
- If the external user was helping configure Azure security → ✅ FP.
- If not known → 🚩 possible compromise.

---

### **5️⃣ Confirm login location of the actor**

- Check **source IP**:
    - If Microsoft/Azure infrastructure (and matches usual login pattern) → normal.
    - If new region or VPN/proxy → suspicious.

<img width="813" height="718" alt="image" src="https://github.com/user-attachments/assets/875ec3ba-4744-47c4-8e64-95a93b864773" />






