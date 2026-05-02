# Day 05 – User vs IP Anomaly & Attack Identification

## 🎯 Objective
Understand the difference between **User-based** and **IP-based** anomalies and identify attack types correctly.

---

## 🧠 Core Concept

```
User = Identity (who)  
IP = Source (where)
```

---

## 🔥 Key Rule

```
Ask: What is repeating?
```

- Same USER repeating → User anomaly  
- Same IP repeating → IP anomaly  

---

## 🧠 Types of Anomalies

### 1. User-Based Anomaly
```
Same user  
Multiple IPs / locations / devices
```

→ Suspicious if pattern strong

---

### 2. IP-Based Anomaly
```
Same IP  
Multiple users  
Fast attempts
```

→ Indicates attacker source

---

## 🔥 Attack Types

### 1. Brute Force
```
Same user  
Multiple failed attempts  
Same IP
```

---

### 2. Distributed Brute Force
```
Same user  
Multiple IPs  
Multiple attempts
```

---

### 3. Password Spraying
```
Same IP  
Multiple users  
Few attempts each
```

---

### 4. Credential Stuffing (basic idea)
```
Same user  
Different IPs  
Using known credentials
```

---

## ⚠️ Important Rules

### 1. Not everything is TP or FP
```
There is a gray zone → Suspicious
```

---

### 2. Pattern vs Context

```
Strong pattern → dominates  
Weak pattern → context decides
```

---

### 3. IP change alone is NOT attack

```
IP change = signal  
Pattern = decision
```

---

## 🔥 Common Mistakes

```
Assuming IP change = attack ❌  
Ignoring pattern ❌  
Forcing classification ❌  
```

---

# ⚔️ SCENARIOS (Practice + Answers)

---

## 🧪 Scenario 1
```
POST /login user=admin → fail  
POST /login user=admin → fail  
POST /login user=admin → success  
(IP rotates each attempt)
```

### ✅ Answer
```
User/IP: User anomaly  
Attack: Distributed brute force  
Class: TP  
Severity: High  
Why: Same user targeted from multiple IPs with failures followed by success → likely compromise.
```

---

## 🧪 Scenario 2
```
POST /login user=admin → fail  
POST /login user=john → fail  
POST /login user=alex → fail  
POST /login user=test → fail  
(Same IP, very fast)
```

### ✅ Answer
```
User/IP: IP anomaly  
Attack: Password spraying  
Class: TP  
Severity: Medium  
Why: One IP targeting multiple users rapidly indicates spraying attempt without success.
```

---

## 🧪 Scenario 3 (Gray Zone)
```
POST /login → fail  
POST /login → fail  
POST /login → success  
(IP same, normal location, normal time)
```

### ✅ Answer
```
User/IP: User anomaly  
Attack: Possible brute force  
Class: Suspicious  
Severity: Medium  
Why: Pattern suggests password guessing but context appears normal, so not confirmed attack.
```

---

## 🧪 Scenario 4
```
GET /admin → 403  
GET /config → 403  
GET /backup → 404  
(Same IP, fast)
```

### ✅ Answer
```
User/IP: IP anomaly  
Attack: Reconnaissance (endpoint scanning)  
Class: Suspicious  
Severity: Low  
Why: Single IP probing multiple endpoints to identify valid resources.
```

---

## 🧪 Scenario 5
```
user=admin → fail (IP1)  
user=admin → fail (IP2)  
user=admin → fail (IP3)  
user=admin → success (IP4)
```

### ✅ Answer
```
User/IP: User anomaly  
Attack: Distributed brute force  
Class: TP  
Severity: High  
Why: Same user targeted from multiple IPs with repeated failures followed by success.
```

---

## 🧪 Scenario 6 (Normal Behavior)
```
user=admin → success (IP1)  
user=admin → success (IP2)  
user=admin → success (IP3)  
(same city, normal time)
```

### ✅ Answer
```
User/IP: User anomaly  
Attack: None  
Class: FP  
Severity: Low  
Why: Multiple successful logins from nearby locations likely due to normal user activity.
```

---

# 🧠 Key Takeaways

```
User anomaly → identity issue  
IP anomaly → source issue
```

```
Same user + many IPs → distributed attack  
Same IP + many users → spraying
```

```
Detection = pattern + context  
```

```
Strong pattern = decisive  
Weak pattern = investigate
```