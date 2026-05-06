# 📅 Day 08 – Advanced Attack Patterns

## 🎯 Learning Objective

Understand:

- Authentication attack patterns
- Attacker behavior after successful login
- Attack chains and post-compromise actions
- SOC investigation and response thinking
- How attackers change strategy during attacks

---

## 🧠 Core Attack Patterns

### 🔹 Brute Force

**Definition**

Attacker repeatedly guesses passwords for a single account.

**Log Pattern**
- Same user account
- Same IP address
- Multiple failed login attempts
- High-frequency behavior

**Key Indicators**
- Rapid failures
- One account heavily targeted
- Possible successful login after failures

**Example**

FAILED LOGIN - admin - IP: 10.10.10.5

FAILED LOGIN - admin - IP: 10.10.10.5

FAILED LOGIN - admin - IP: 10.10.10.5

SUCCESS LOGIN - admin - IP: 10.10.10.5

---

### 🔹 Password Spraying

**Definition**

Attacker tries common passwords across multiple user accounts.

**Log Pattern**
- Same IP address
- Multiple user accounts
- Few attempts per user
- Low-and-slow behavior

**Key Indicators**
- Distributed targeting
- Avoids account lockout
- Sometimes followed by successful login

**Example**

FAILED LOGIN - user1 - IP: 45.77.22.9

FAILED LOGIN - user2 - IP: 45.77.22.9

FAILED LOGIN - user3 - IP: 45.77.22.9

SUCCESS LOGIN - user2 - IP: 45.77.22.9

---

### 🔹 Credential Stuffing

**Definition**

Attacker uses stolen usernames and passwords from data breaches.

**Log Pattern**
- Successful logins
- Few or no failed attempts
- Login from unusual IP/location
- Activity appears legitimate

**Key Indicators**
- Success without brute-force pattern
- New country or unusual IP
- Immediate suspicious post-login activity

**Example**

SUCCESS LOGIN - user1 - IP: 103.22.11.5 (New Location)

PASSWORD CHANGE - user1

FILE DOWNLOAD - user1

---

## 🔐 Post-Compromise Behavior

After successful login, attackers may:

- Change password
- Add recovery email
- Disable MFA
- Download files
- Access sensitive data
- Continue attacking other accounts

---

## 🔥 Account Takeover (ATO)

**Definition**

Unauthorized access and control of a user account.

**Common Indicators**
- Login from unusual location
- Password change after login
- Recovery email changes
- Suspicious file access

**Why Dangerous**
- Attacker gains legitimate access
- Harder to detect
- Can lead to persistence and data theft

---

## 🔐 Persistence

**Definition**

Actions taken by attacker to maintain long-term access.

**Examples**
- Password change
- Recovery email addition
- MFA changes

**Why Important**

Persistence allows attacker to return later even if user notices suspicious activity.

---

## 📤 Data Exfiltration

**Definition**

Unauthorized access or download of sensitive data.

**Indicators**
- Large file downloads
- Sensitive file access
- Downloads after suspicious login

**Risk**
- Data theft
- Privacy impact
- Business impact

---

## 🧠 Attack Chain Thinking

SOC analysts must connect logs into a full attack sequence.

**Example Attack Chain**
Password Spraying
→ Successful Login
→ Account Takeover (ATO)
→ Persistence
→ Data Exfiltration
→ Continued Expansion

---

## ⚠️ Important SOC Mindsets

### 🔹 Success ≠ Safe
A successful login can still be malicious.

### 🔹 No Failures ≠ No Attack
Credential stuffing may show only successful logins.

### 🔹 Attack = Sequence, Not Event
Never analyze only one event. Always connect:
- Login
- Password change
- File access
- Continued attacks

### 🔹 Login = Beginning, Not End
SOC analysts must investigate:
- What happened after login
- Attacker behavior
- Persistence actions
- Data access

---

## 🛡️ SOC Response Thinking

**Immediate Actions**
- Lock compromised account
- Force password reset
- Revoke active sessions
- Block suspicious IP

**Investigation**
- Analyze login history
- Check geo/IP anomalies
- Identify accessed/downloaded data
- Look for additional compromised users

**Prevention**
- Enable MFA
- Create detection rules
- Monitor suspicious authentication behavior

---

## 🔍 Detection Thinking

SOC detection should focus on:
- Authentication anomalies
- Sequence-based behavior
- Unusual location logins
- Post-login actions

**Example Detection Logic**
IF:

Login from new country
AND
Password change within 5 minutes

THEN:

Trigger high-risk alert


---

## 🔥 Scenario 1 — Hidden Compromise

**📜 Logs**
- [09:00] SUCCESS LOGIN - user1 - IP: 103.22.11.5 (New Location)
- [09:02] PASSWORD CHANGE - user1
- [09:05] FILE DOWNLOAD - user1 - files: 45

**🧠 Analysis**
- Likely Credential Stuffing
- Account Takeover (ATO)
- Persistence established
- Data exfiltration occurred

**🚨 Why Suspicious**
- New location login
- No failed attempts
- Immediate password change
- File download after login

**🛡️ SOC Response**
- Lock account
- Force password reset
- Revoke sessions
- Block suspicious IP
- Investigate accessed data

---

## 🔥 Scenario 2 — Expansion Attack

**📜 Logs**
- [11:00] FAILED LOGIN - user1 - IP: 88.12.44.2
- [1-1:00] FAILED LOGIN - user2 - IP: 88.12.44.2
- [11:01] FAILED LOGIN - user3 - IP: 88.12.44.2
- [11:03] SUCCESS LOGIN - user2 - IP: 88.12.44.2
- [11:04] PASSWORD CHANGE - user2
 - [11:06] FAILED LOGIN - user4 - IP: 88.12.44.2
- [11:06] FAILED LOGIN - user5 - IP: 88.12.44.2

**🧠 Analysis**
- Initial Password Spraying
- Successful compromise
- Persistence established
- Attacker continues targeting more users

> 🔥 **Key Insight:** Attack is still ongoing after first compromise.

---

## 🔥 Scenario 3 — Stealth Attack

**📜 Logs**
[13:00] SUCCESS LOGIN - userZ - IP: 77.21.9.4 (New Country)
[13:20] SUCCESS LOGIN - userZ - IP: 77.21.9.4
[13:50] FILE ACCESS - userZ

**🧠 Analysis**
- Likely Credential Stuffing
- Possible Account Takeover
- Slow and stealthy behavior

**🚨 Why Hard To Detect**
- No brute-force pattern
- Appears like legitimate login activity

**🔑 Key Indicators**
- New country login
- Suspicious IP consistency
- File access after login

---

## 🔥 Scenario 4 — Brute Force Attack

**📜 Logs**

 - [14:00] FAILED LOGIN - admin - IP: 66.21.8.5
 - [14:00] FAILED LOGIN - admin - IP: 66.21.8.5
 - [14:01] FAILED LOGIN - admin - IP: 66.21.8.5
- [14:01] FAILED LOGIN - admin - IP: 66.21.8.5
- [14:03] SUCCESS LOGIN - admin - IP: 66.21.8.5

**🧠 Analysis**
- Clear Brute Force pattern
- Single account heavily targeted
- Rapid repeated failures
- Successful compromise possible

**🛡️ SOC Response**
- Lock account immediately
- Reset password
- Block attacking IP
- Review admin activity
- Enable MFA

---

## 🔥 Scenario 5 — Persistence Without Exfiltration

**📜 Logs**
[15:00] SUCCESS LOGIN - userM - IP: 91.22.7.4 (New Country)
[15:02] PASSWORD CHANGE - userM
[15:03] RECOVERY EMAIL ADDED - userM

**🧠 Analysis**
- Likely Account Takeover
- Persistence established
- No data theft yet
- Attacker securing long-term access

**🛡️ SOC Response**
- Lock account
- Revoke recovery changes
- Reset password
- Terminate active sessions
- Investigate source IP

---

## 🎯 Key Takeaways

| Pattern | Description |
|---|---|
| **Brute Force** | One account, many attempts |
| **Password Spraying** | Many accounts, few attempts |
| **Credential Stuffing** | Stolen valid credentials |
| **Persistence** | Attacker maintaining access |
| **Data Exfiltration** | Attacker stealing data |

> ✅ **Success ≠ Safe**
> 
> ✅ **Context matters more than isolated logs**

**Always think:**
- What happened?
- Why?
- What next?