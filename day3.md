# Day 03 – Log Correlation & Attack Chains

## 🎯 Objective
Understand how to connect multiple logs to identify attack sequences and interpret endpoint responses correctly.

---

## 🧠 Key Concepts

### Log Correlation
- Single log → limited value  
- Multiple logs → meaningful pattern  

```
Events → Sequence → Attack Story
```

---

### Attack Chain
Attackers act in steps, not isolated actions.

```
Recon → Endpoint discovery → Login attempts → Compromise
```

---

### HTTP Response Understanding

**200 (OK)**
- Resource exists and is accessible  
- Usually public endpoint  

**403 (Forbidden)**
- Resource exists but access is restricted  
- Indicates potentially sensitive endpoint  

**404 (Not Found)**
- Resource does not exist  

---

## ⚠️ Key Insight

```
403 is more valuable than 200 for attackers
```

- 200 → public / expected  
- 403 → protected / high-value target  

---

## ⚔️ Scenario

```
GET /backup → 404  
GET /admin → 403  
GET /login → 200  
POST /login → fail  
POST /login → fail  
POST /login → success  
```

---

## 🔍 Analysis

### Pattern
- Multiple endpoint requests from same IP  
- Mixed responses (404, 403, 200)  
- Repeated login attempts  

---

### Attack Chain
```
Recon → Endpoint discovery → Brute force → Account compromise
```

---

### Final Result
- Successful login → Account compromise  

---

### Why
- 404 → endpoint not found  
- 403 → sensitive endpoint exists  
- 200 → valid login page found  
- Failed + success login → brute force success  

---

## ⚠️ Mistakes I Made
- Treated logs individually instead of as a sequence  
- Misunderstood importance of 403  
- Jumped to conclusions without full context  

---

## ✅ Improvements
- Connected logs step-by-step  
- Understood attacker perspective  
- Improved explanation clarity  

---

## 🔥 Key Takeaways

```
Logs alone are not useful  
Correlation creates meaning
```

```
403 → valuable target  
200 → normal endpoint
```

```
Attack = sequence, not single event
```