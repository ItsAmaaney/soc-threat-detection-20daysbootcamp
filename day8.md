# 📅 Day 08 – Time-Based Detection & Stealth Attacks

---

# 🎯 Objective

Learn:
- low-and-slow attacks
- time correlation
- threshold evasion
- delayed post-compromise behavior
- basic detection engineering thinking

---

# 🧠 Core Concepts

## 🔹 Low-and-Slow Attack

### Definition
Attackers spread activity across long time periods to avoid detection.

### Characteristics
- Long intervals between attempts
- Same IP targets multiple users
- Activity appears normal individually
- Difficult to correlate manually

### Example

08:00 FAILED LOGIN - user1 - IP: 55.1.1.1

11:00 FAILED LOGIN - user2 - IP: 55.1.1.1

15:00 FAILED LOGIN - user3 - IP: 55.1.1.1

19:00 SUCCESS LOGIN - user2 - IP: 55.1.1.1


### 🔑 **Key Insight:** Attack timing itself is behavior.

---

## 🔹 Time Correlation

**Definition**

Connecting events across long time periods to identify attack patterns.

**Weak Analyst Thinking**

One failed login = normal

**Strong Analyst Thinking**

Same IP

Multiple users

Spread across hours

Eventual success

> **Key Idea:** Single events may look harmless. Correlated events reveal attacks.

---

## 🔹 Threshold Evasion

**Definition**

Attackers intentionally stay below alert thresholds.

**Weak Detection Rule**
Alert if:

5 failed logins within 10 minutes

**Attacker Bypass**
09:00 FAILED LOGIN - user1

09:20 FAILED LOGIN - user2

09:40 FAILED LOGIN - user3

10:00 FAILED LOGIN - user4

**Why Detection Fails**
- Rule focuses only on short time windows
- Ignores long-term behavior
- Ignores distributed targeting

---

## 🔍 Improved Detection Thinking

**Better Detection Logic**
IF:

Same IP targets multiple users

Across extended time period

AND:

Distributed failed logins observed

THEN:

Trigger low-and-slow spraying alert


**Detection Focus**
- Behavior pattern
- Timing
- User distribution
- Correlation

> Not just failure count.

---

## 🔹 Delayed Post-Compromise Behavior

**Definition**

Attacker delays malicious actions after successful login.

**Example**
09:00 SUCCESS LOGIN - userA - New Country
15:00 FILE DOWNLOAD - userA
22:00 PASSWORD CHANGE - userA

**Why Attackers Delay**
- Avoid sequence-based detection
- Reduce suspicion
- Make events appear unrelated

**Attacker Gains**
- Stealth
- Longer access
- Lower detection probability

---

## 🔐 Persistence

**Definition**

Attacker actions used to maintain continued access.

**Examples**
- Password change
- Recovery email addition
- MFA modification

> 🔑 **Key Insight:** Persistence indicates long-term access intent.

---

## ⚠️ SOC Tradeoff Thinking

| Approach | Pros | Cons |
|---|---|---|
| **Narrow Detection Window** | Fewer false positives | Easier to bypass |
| **Wide Detection Window** | Better attack visibility | More false positives |

---

 # 🔥 Scenario 1 — Low-and-Slow Password Spraying

## 📜 Logs

```text
08:00 FAILED LOGIN - user1 - IP: 55.1.1.1
11:00 FAILED LOGIN - user2 - IP: 55.1.1.1
15:00 FAILED LOGIN - user3 - IP: 55.1.1.1
19:00 SUCCESS LOGIN - user2 - IP: 55.1.1.1
```

## 🎯 Attack Type

- Low-and-slow Password Spraying

## 🧠 Why Hard To Detect

- Login attempts spread across hours
- No rapid brute-force behavior
- Individual events appear harmless
- Detection rules may only monitor short time windows

## ⚠️ What Weak Analysts Miss

Weak analysts may:

- Treat each failed login separately
- Ignore long time gaps
- Miss correlation between users and IP
- Focus only on rapid attacks

## ✅ What Good Analysts Notice

Good analysts correlate:

- Same IP
- Multiple targeted users
- Long-term activity
- Eventual successful login

> 💡 Good analysts understand: **Attack timing itself is behavior.**

## 🛡️ SOC Investigation / Response

- Investigate source IP reputation
- Check user2 activity after login
- Identify additional targeted users
- Monitor for persistence or file access
- Review geo-location and device history

---

# 🔥 Scenario 2 — Delayed Post-Compromise

## 📜 Logs

```text
09:00 SUCCESS LOGIN - userA - IP: New Country
15:00 FILE DOWNLOAD - userA
22:00 PASSWORD CHANGE - userA
```

## 🎯 Attack Type

- Credential Stuffing / Account Takeover (ATO)
- Delayed Post-Compromise Activity

## 🧠 Why Hard To Detect

- Malicious actions delayed across many hours
- Events may appear unrelated
- No obvious attack chain in short time window

## ⚠️ What Weak Analysts Miss

Weak analysts may:

- Treat file access as normal activity
- Ignore delayed password change
- Fail to connect events back to suspicious login

## ✅ What Good Analysts Notice

Good analysts identify:

- New country login
- Delayed suspicious actions
- Persistence behavior
- Timing used for stealth

## 🛡️ SOC Investigation / Response

- Verify login legitimacy with user
- Investigate accessed/downloaded files
- Check for persistence changes
- Reset credentials
- Revoke active sessions

---

# 🔥 Scenario 3 — Threshold Evasion

## 📜 Logs

```text
09:00 FAILED LOGIN - user1 - IP: 44.1.1.1

09:20 FAILED LOGIN - user2 - IP: 44.1.1.1

09:40 FAILED LOGIN - user3 - IP: 44.1.1.1

10:00 FAILED LOGIN - user4 - IP: 44.1.1.1
```

## 🎯 Attack Type

- Low-and-slow Password Spraying
- Threshold Evasion Attack

## 🧠 Why Hard To Detect

- Activity stays below alert threshold
- No burst of rapid failures
- Weak detections focus only on short windows

## ⚠️ What Weak Analysts Miss

Weak analysts may:

- Ignore slow distributed failures
- Depend only on failure count thresholds
- Miss attacker timing strategy

## ✅ What Good Analysts Notice

Good analysts detect:

- Same IP targeting multiple users
- Long-term distributed behavior
- Intentional threshold evasion

## 🛡️ SOC Investigation / Response

- Correlate events across larger time window
- Investigate source IP
- Monitor targeted users
- Improve detection logic for long-term correlation

---

## 🎯 Key Takeaways

| Concept | Insight |
|---|---|
| **Attack timing** | Timing itself is attacker behavior |
| **Slow attacks** | Harder to detect than fast ones |
| **Detection rules** | Can be studied and bypassed |
| **Time correlation** | Critical for revealing hidden patterns |
| **Success ≠ safe** | Legitimate-looking logins can be malicious |
| **Persistence** | Always indicates high risk and long-term intent |

**Always think:**
- What happened?
- Why this timing?
- What is the attacker avoiding?
- What happens next?