# 📅 Day 13 – False Positive Reduction & Detection Tuning

## 🧠 Core Idea

Weak detections → too many alerts → alert fatigue → real attacks get missed.
Good SOC teams continuously tune detections to improve quality over quantity.

---

## 🔥 Key Concepts

**False Positive (FP)** — Alert fires but activity is legitimate (e.g. mistyped password).

**Detection Tuning** — Improving rules to reduce noise and increase confidence.

**Threshold Evasion** — Attackers stay below alert limits using:
- Low-and-slow (spread attempts over hours)
- Distributed IPs (one attempt per IP)

---

## ⚖️ Detection Tradeoff

| Too Sensitive | Too Strict |
|---|---|
| Catches more attacks | Fewer false positives |
| Many false positives | May miss real attacks |

---

## ✅ Better Detection Logic.

Weak rule: `3 failed logins`

Strong rule:
```text
same IP + multiple users + successful login + password change
= high confidence alert
```

---
## 🔥 Exercise 1 — Weak Detection Rule

### 📜 Detection Rule

```text
Alert if:
3 failed logins occur
```

### ❓ Questions

1. Why weak?
2. Why noisy?
3. How attackers evade it?
4. How improve it?

### ✅ Answers

**1️⃣ Why weak?**
- Legitimate users may accidentally trigger it
- Lacks context
- Not enough evidence of compromise

**2️⃣ Why noisy?**

Many users may:
- Mistype passwords
- Use outdated credentials
- Repeatedly fail login normally

**3️⃣ How attackers evade it?**

Attackers may:
- Spread attempts over time
- Use multiple IP addresses
- Avoid same-IP thresholds

**4️⃣ How improve it?**

Better logic:
```text
same IP
+ multiple targeted users
+ successful login
+ password change
```

Possible classification: password spraying / possible account takeover.

---

## 🔥 Exercise 2 — Better Correlation

### 📜 Logs

```text
FAILED LOGIN - userA - IP: 55.1.1.1
FAILED LOGIN - userB - IP: 55.1.1.1
SUCCESS LOGIN - userB - IP: 55.1.1.1
PASSWORD CHANGE - userB
```

### ❓ Questions

1. Why stronger detection confidence?
2. Which signals matter most?
3. Why stronger than failed logins alone?

### ✅ Answers

**1️⃣ Why stronger detection confidence?**
- Multiple users targeted
- Successful login occurred
- Password changed immediately after login

**2️⃣ Which signals matter most?**
- Same IP targeting multiple users
- Successful login
- Password change after login

**3️⃣ Why stronger than failed logins alone?**
- Suspicious sequence observed
- Likely compromise succeeded
- Persistence behavior appeared
- Attacker behavior became visible

---

## 🔥 Exercise 3 — Low-and-Slow Evasion

### 📜 Logs

```text
09:00 FAILED LOGIN - admin - IP: 11.1.1.1
13:00 FAILED LOGIN - admin - IP: 11.1.1.1
18:00 FAILED LOGIN - admin - IP: 11.1.1.1
22:00 SUCCESS LOGIN - admin - IP: 11.1.1.1
```

### ❓ Questions

1. Why difficult to detect?
2. How attacker evaded thresholds?
3. What should better detections consider?

### ✅ Answers

**1️⃣ Why difficult to detect?**
- Login attempts spread across long intervals
- Activity appears less aggressive
- Threshold-based rules may not trigger

**2️⃣ How attacker evaded thresholds?**
- Avoided rapid failed attempts
- Bypassed time-window detections

**3️⃣ What should better detections consider?**
- Long-term behavior correlation
- Successful login after failures
- Unusual login timing
- Account targeting patterns

---

## 🔥 Exercise 4 — Distributed Evasion

### 📜 Logs

```text
FAILED LOGIN - admin - IP: 11.1.1.1
FAILED LOGIN - admin - IP: 22.2.2.2
FAILED LOGIN - admin - IP: 33.3.3.3
FAILED LOGIN - admin - IP: 44.4.4.4
SUCCESS LOGIN - admin - IP: 55.5.5.5
```

### ❓ Questions

1. Why same-IP detection fails here?
2. What attack technique used?
3. What should analyst correlate instead of IP alone?
4. What increases confidence here?

### ✅ Answers

**1️⃣ Why same-IP detection fails here?**
- Each IP performs very few attempts
- Same-IP thresholds never trigger

**2️⃣ What attack technique used?**

Distributed brute force attack.

**3️⃣ What should analyst correlate instead of IP alone?**
- Targeted account
- Timing
- Authentication sequence
- Successful login behavior

**4️⃣ What increases confidence here?**
- Multiple distributed failures
- Eventual successful login
- Suspicious sequence timing

---

## 🔥 Exercise 5 — High Confidence Detection

### 📜 Logs

```text
SUCCESS LOGIN - finance_user - New Country
PASSWORD CHANGE - finance_user
MFA DISABLED - finance_user
FILE DOWNLOAD - finance_user
```

### ❓ Questions

1. Why very high confidence compromise?
2. Which signals increase severity most?
3. Why prioritize immediately?
4. Recommended SOC response?

### ✅ Answers

**1️⃣ Why very high confidence compromise?**
- Successful login
- Persistence behavior
- Suspicious geo-location
- File download activity

**2️⃣ Which signals increase severity most?**
- MFA disabled
- Password change
- File download
- New country login

**3️⃣ Why prioritize immediately?**
- Likely account takeover
- Persistence established
- Possible data exfiltration
- Sensitive account involved

**4️⃣ Recommended SOC response?**
- Revoke sessions
- Reset password
- Re-enable MFA
- Investigate downloaded files
- Investigate source IP/device
- Monitor additional compromise activity

---


## 🎯 Key Takeaways

- Quality over quantity — fewer, better alerts
- Correlate behavior, not just events
- Persistence actions = high confidence
- IP alone is weak evidence
- Always think: sequence, timing, context