# Day 01 – Authentication Attacks & Patterns

## 🎯 Objective
Understand different types of authentication attacks and how to detect them using patterns.

---

## 🧠 Key Concepts

### 1. Brute Force
- Target: Single user  
- Behavior: Multiple login attempts  
- Pattern: High frequency attempts on same account  

**Example:**

admin → fail

admin → fail

admin → fail

admin → success


---

### 2. Password Spraying
- Target: Multiple users  
- Behavior: Few attempts per account  
- Pattern: Same source tries common password across users  

**Example:**

admin → fail

john → fail

alice → fail


---

### 3. Slow Brute Force (Low & Slow)
- Target: Single user  
- Behavior: Attempts spread over time  
- Pattern: Time gaps between login attempts  

**Example:**

10:00 → admin fail

10:10 → admin fail

10:20 → admin fail


---

### 4. Distributed Brute Force
- Target: Single user  
- Behavior: Attempts from multiple IPs  
- Pattern: Same user targeted from different IP addresses  

**Example:**

IP1 → admin fail

IP2 → admin fail

IP3 → admin fail

IP4 → admin success


---

## 🚨 Evasion Techniques

Attackers try to avoid detection by:

- Using time gaps (slow attacks)  
- Using multiple IPs (distributed attacks)  

**Key Idea:**

Fast attack → easy to detect

Slow + distributed → hard to detect


---

## 🧠 Detection Logic

Always analyze using:

- **User (target)**  
- **IP (source)**  
- **Time (behavior)**  

**Formula:**

Pattern = User + IP + Time


---

## ⚔️ Practice Scenarios

### Scenario 1

admin → fail

admin → fail

admin → fail

admin → success


**Analysis:**
- Pattern: Same user, repeated attempts  
- Attack: Brute force  
- Why: Multiple attempts on single account until success  

---

### Scenario 2

admin → fail

john → fail

alice → fail


**Analysis:**
- Pattern: Multiple users, few attempts  
- Attack: Password spraying  
- Why: Same source targeting multiple accounts  

---

### Scenario 3

IP1 → admin fail

IP2 → admin fail

IP3 → admin fail

IP4 → admin success


**Analysis:**
- Pattern: Same user, multiple IPs  
- Attack: Distributed brute force  
- Why: Different IPs used to bypass detection  

---

### Scenario 4

10:00 → admin fail

10:10 → admin fail

10:20 → admin fail


**Analysis:**
- Pattern: Same user, time gaps  
- Attack: Slow brute force  
- Why: Spaced attempts to avoid detection  

---

## ⚠️ Mistakes I Made

- Used vague explanations instead of precise patterns  
- Didn’t clearly mention user, IP, and behavior  
- Mixed concepts while explaining  

---

## ✅ Improvements

- Focused on structured explanation  
- Learned to identify patterns clearly  
- Understood importance of precise wording  

---

## 🔥 Key Takeaway


Individual logs are not important

Patterns are everything