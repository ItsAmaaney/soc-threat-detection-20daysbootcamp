# Day 06 – Post-Compromise Analysis (What Happens After Login)

## 🎯 Objective
Move beyond detecting login events and understand attacker actions after gaining access.

---

## 🧠 Core Concept

```
Login ≠ End of attack  
Login = Beginning of attack
```

SOC analysts must trace what happens AFTER login.

---

## 🔥 Post-Login Attacker Goals

### 1. Data Access (Exfiltration)
```
GET /account  
GET /files  
GET /download
```
→ Attacker reads or steals data

---

### 2. Persistence (Stay Inside System)
```
POST /change-password  
POST /add-email  
POST /add-user
```
→ Attacker ensures continued access

---

### 3. Privilege Abuse
```
Access admin panel  
Modify roles  
```
→ Attacker gains higher control

---

### 4. Lateral Movement (Advanced)
```
Access other accounts  
Move across systems
```

---

## ⚠️ Key Insight

```
Suspicious login alone is NOT enough  
Post-login activity determines impact
```

---

## 🔥 Dangerous Patterns

### Account Takeover (ATO)
```
Login → Change password → Add email
```
→ Full control + persistence

---

### Silent Compromise
```
Login (no failures, abnormal context)
```
→ Attacker already has credentials

---

### Data Exfiltration
```
Login → Access files → Download
```
→ Sensitive data stolen

---

## 🧠 Decision Rules

```
Login + data access = High risk  
Login + account change = High risk  
Login only + normal context = Low / Suspicious  
```

---

## ⚠️ Important Concepts

### 1. Pattern vs Context
```
Strong pattern → dominates decision  
Weak pattern → context decides
```

---

### 2. Failures vs Success
```
Failures → visible attack  
Success → potential compromise
```

---

### 3. Persistence Indicators
```
Password change  
Email change  
Account modification
```
→ High-risk signals

---

## 🔥 SOC Response Actions

### Immediate Actions
```
Lock account  
Force password reset  
Remove unauthorized changes  
```

---

### Investigation
```
Check accessed data  
Check downloads  
Check further activity  
```

---

### Protection
```
Enable MFA  
Review account security  
```

---

# ⚔️ SCENARIOS (Practice + Answers)

---

## 🧪 Scenario 1
```
POST /login → success (new IP, 3 AM)  
GET /account → 200  
GET /files → 200  
GET /download?id=123 → 200  
```

### ✅ Answer
```
What happened:
Suspicious login followed by access to account and file data.

Attacker action:
Accessed and downloaded data → likely data exfiltration.

Risk:
High

SOC response:
Block IP, force password reset, enable MFA, investigate accessed data.
```

---

## 🧪 Scenario 2
```
POST /login → success  
POST /change-password → success  
POST /add-email → success  
```

### ✅ Answer
```
What happened:
Login followed by credential and recovery changes.

Attacker action:
Established persistence and took control of account.

Risk:
High

SOC response:
Lock account, revert changes, reset password, remove added email, investigate activity.
```

---

## 🧪 Scenario 3
```
POST /login → success  
GET /dashboard → 200  
(no further activity)
```

### ✅ Answer
```
What happened:
Successful login with no suspicious follow-up actions.

Attacker action:
No clear malicious activity observed.

Risk:
Low / Suspicious (depends on context)

SOC response:
Monitor activity, verify login legitimacy.
```

---

## 🧪 Scenario 4
```
POST /login → success  
POST /add-email → success  
```

### ✅ Answer
```
What happened:
Login followed by addition of new email.

Attacker action:
Attempted persistence via recovery method.

Risk:
Medium (High if abnormal context)

SOC response:
Verify action, remove added email, force password reset, enable MFA.
```

---

## 🧪 Scenario 5
```
POST /login → success  
POST /change-password → success  
```

### ✅ Answer
```
What happened:
Login followed by password change.

Attacker action:
Gained control and may lock out legitimate user.

Risk:
High

SOC response:
Lock account, reset credentials, investigate access.
```

---

# 🧠 Key Takeaways

```
Detection = Entry level  
Investigation = Real SOC skill
```

```
Attack = Sequence, not event
```

```
Login + action = real threat
```

```
Persistence = biggest danger after compromise
```