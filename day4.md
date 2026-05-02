# Day 04 – Correlation Traps & Context-Based Decisions

## 🎯 Objective
Understand how similar log patterns can lead to different conclusions based on context and avoid incorrect detection.

---

## 🧠 Key Concept

### Correlation Trap
- Same pattern ≠ Same attack  

```
Pattern → gives suspicion  
Context → decides reality
```

---

## ⚠️ Important Rule

```
Do NOT assume attack based only on pattern  
Always check context (IP, location, time, device, behavior)
```

---

## 🧠 What is Context?

Context = additional information that helps determine if activity is normal or malicious

Examples:
- IP address  
- Location  
- Time  
- Device  
- User behavior  

---

## ⚔️ Scenario Comparison

### Scenario A (Unknown Context)

```
admin → fail  
admin → fail  
admin → fail  
admin → success  
(IP same, no context)
```

**Analysis:**
- Pattern: Multiple failed attempts + success  
- Decision: True Positive  
- Action: Escalate  
- Reason: No normal context → possible brute force  

---

### Scenario B (Known Context)

```
admin → fail  
admin → fail  
admin → fail  
admin → success  
(IP same, office time, known location)
```

**Analysis:**
- Pattern: Same as Scenario A  
- Decision: False Positive  
- Action: Monitor  
- Reason: Likely normal user behavior  

---

## 🔍 What Changed?

```
Context changed the decision, not the pattern
```

---

## ⚔️ Correlation Trap Example

```
GET /login → 200  
POST /login → fail  
POST /login → fail  
POST /login → success  
```

---

## 🧠 Analysis

- Pattern: Login attempts + success  
- Possible meanings:
  - Normal user mistake  
  - Brute force attack  

---

## ⚠️ Key Insight

```
This pattern is NOT always an attack
```

---

## 🔥 Decision Logic

```
Check context:
- IP location  
- Time  
- Device  
- Behavior  

If abnormal → True Positive  
If normal → False Positive  
```

---

## ⚠️ Mistakes to Avoid

- Assuming every pattern is an attack  
- Ignoring context  
- Jumping to conclusions  

---

## ✅ Improvements

- Separate pattern from context  
- Think before deciding  
- Avoid false positives  

---

## 🔥 Key Takeaways

```
Pattern ≠ Attack  
Context = Decision
```

```
Same logs → different outcomes
```

```
SOC = Detect + Decide + Act
```