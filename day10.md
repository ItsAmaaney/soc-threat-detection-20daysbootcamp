# 📅 Day 12 – Investigation Flow & Alert Triage Exercises

---

## 🎯 Learning Objective

Understand:
- Alert triage
- Severity assessment
- Investigation flow
- Confidence-based analysis
- Persistence recognition
- SOC response prioritization

---

## 🧠 Key Concepts

### 🔹 Alert Triage

Triage means deciding:
- Which alerts are urgent
- Which require deeper investigation
- Which may be false positives

---

### 🔹 Severity Levels

#### 🟢 Low Severity
- Low confidence
- Low impact
- Little evidence of compromise

#### 🟡 Medium Severity
- Suspicious activity exists
- Compromise not fully confirmed
- Monitoring required

#### 🔴 High Severity
- Strong compromise indicators
- Persistence behavior
- Successful attacker actions
- High business impact

---

## 🔥 Important SOC Principle

```text
Successful compromise matters more than failed attempts.
```

---

## 🔥 Detection Confidence

Confidence increases when:
- Multiple suspicious signals correlate
- Persistence actions occur
- Post-login impact exists

**Strong Confidence Signals:**
- New country login
- Password change
- MFA disabled
- File download
- Suspicious persistence
- Unusual device behavior

---

## 🔥 Exercise 1 — Low Severity Login Activity

### 📜 Logs

```text
FAILED LOGIN - user1
FAILED LOGIN - user1
SUCCESS LOGIN - user1
```

### ❓ Questions

1. Severity level?
2. Why not automatically high severity?
3. What context needed?
4. What should analyst monitor next?

### ✅ Answers

**1️⃣ Severity level?**

Low severity (possibly low-medium).

**2️⃣ Why not automatically high severity?**

- Only a few failed attempts observed
- No strong indicators of compromise
- No persistence or suspicious post-login activity

**3️⃣ What context needed?**

- Login timing
- Location
- Source host
- IP reputation
- Device history
- Normal vs unusual behavior

**4️⃣ What should analyst monitor next?**

- Post-login activity
- Password changes
- MFA changes
- Suspicious downloads
- Unusual sessions

---

## 🔥 Exercise 2 — Medium Severity Authentication Activity

### 📜 Logs

```text
FAILED LOGIN - userA - IP: 66.1.1.1
FAILED LOGIN - userB - IP: 66.1.1.1
SUCCESS LOGIN - userB - IP: 66.1.1.1
```

### ❓ Questions

1. Severity level?
2. Why suspicious?
3. Why not highest severity yet?
4. What should analyst monitor next?
5. What would increase confidence further?

### ✅ Answers

**1️⃣ Severity level?**

Medium severity.

**2️⃣ Why suspicious?**

- Multiple user accounts targeted from same IP
- Possible password spraying behavior

**3️⃣ Why not highest severity yet?**

- No persistence actions observed yet
- No MFA disable or suspicious file access
- No confirmed impact beyond successful login

**4️⃣ What should analyst monitor next?**

- Post-login activity
- Password changes
- MFA disable
- Suspicious downloads
- Additional targeted accounts

**5️⃣ What would increase confidence further?**

- Unusual login timing
- Suspicious location
- Malicious IP reputation
- Unusual device behavior
- Persistence actions
- Suspicious post-login activity

---

## 🔥 Exercise 3 — High Severity Persistence

### 📜 Logs

```text
SUCCESS LOGIN - admin_user - New Country
PASSWORD CHANGE - admin_user
MFA DISABLED - admin_user
```

### ❓ Questions

1. Severity level?
2. Why high-confidence compromise?
3. Which action is strongest persistence indicator?
4. Recommended SOC response?

### ✅ Answers

**1️⃣ Severity level?**

High severity — possible account takeover (ATO) with persistence behavior.

**2️⃣ Why high-confidence compromise?**

- Successful login from unusual country
- Multiple correlated suspicious actions
- Persistence behavior observed immediately after login

**3️⃣ Which action is strongest persistence indicator?**

MFA disabled — indicates attacker weakening account security and maintaining access.

**4️⃣ Recommended SOC response?**

- Revoke active sessions
- Force password reset
- Re-enable MFA
- Investigate source IP and host
- Investigate additional account compromise
- Monitor for further suspicious activity
- Escalate incident if necessary

---

## 🎯 Key Takeaways

| Concept | Detail |
|---|---|
| **Severity depends on** | Confidence, impact, and criticality |
| **Confidence increases with** | Multiple correlated suspicious behaviors |
| **Persistence actions** | Greatly increase severity |
| **Successful compromise** | Matters more than failed attempts alone |

**Analysts monitor:**
- Post-login behavior
- Persistence actions
- Suspicious sequences
- Unusual user behavior

> NOT isolated events alone.