# Day 02 – True Positive vs False Positive

## 🎯 Objective
Learn how to distinguish between real attacks and normal user behavior, and decide the correct action (monitor or escalate).

---

## 🧠 Key Concepts

### 1. True Positive (TP)
- A real attack is happening  
- Indicates malicious activity  

**Example:**
- Multiple failed logins followed by a successful login  
- Suspicious pattern with no normal context  

---

### 2. False Positive (FP)
- Looks suspicious but is normal behavior  
- No malicious intent  

**Example:**
- User enters wrong password multiple times  
- Login from expected location and time  

---

## ⚠️ Important Insight

- Many failed attempts alone are **less dangerous**  
- Even a few attempts with a **successful login** are **more dangerous**


Failure → attempt
Success → compromise 🚨


---

## 🧠 Decision Logic

### Monitor
- Multiple failed attempts  
- No successful login  
- No strong evidence of compromise  

### Escalate
- Successful login after suspicious behavior  
- Unusual pattern or context  
- Possible account compromise  

---

## 🔍 Key Rule


Pattern + Context = Decision


- Pattern alone is not enough  
- Context (location, time, behavior) is critical  

---

## ⚔️ Practice Scenarios

### Scenario 1

admin → fail

admin → fail

admin → fail

admin → success


**Analysis:**
- Pattern: Repeated attempts + success  
- Decision: True Positive  
- Action: Escalate  
- Reason: Indicates brute force leading to compromise  

---

### Scenario 2

john → fail

john → fail

john → fail

john → success

(location same, office time)


**Analysis:**
- Pattern: Repeated attempts  
- Context: Normal location and time  
- Decision: False Positive  
- Action: Monitor  
- Reason: Likely user error (forgot password)  

---