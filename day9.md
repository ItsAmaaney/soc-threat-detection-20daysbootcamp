# 📅 Day 9 – Advanced IP-Based Analysis

---

## 🎯 Learning Objective

Understand:
- Public vs private IPs
- NAT (Network Address Translation)
- Why IP alone is weak evidence
- Shared infrastructure challenges
- Distributed attacks
- VPNs, VPS, residential proxies
- Attacker infrastructure and OPSEC

---

## 🧠 Public vs Private IP

### 🔹 Private IP

**Definition**

IP address used inside local/internal networks.

**Common Private IP Ranges**

```text
10.x.x.x
192.168.x.x
172.16.x.x – 172.31.x.x
```

**Used In**
- Home WiFi
- Office networks
- Café WiFi
- Internal corporate systems

> **Important:** Not directly visible on internet — used only inside local networks.

---

### 🔹 Public IP

**Definition**

Internet-facing IP visible to websites and external systems.

**Assigned By**
- ISP
- VPN provider
- Cloud provider

> **Important:** Websites mostly see the public IP, NOT the private IP.

---

## 🔥 NAT (Network Address Translation)

**Definition**

Router translates private IP traffic into public IP traffic.

**Example**

```text
Inside Local Network:
  Phone  → 192.168.1.5
  Laptop → 192.168.1.6

Internet Sees:
  49.205.x.x
```

> 🔑 **Key Insight:** Multiple devices can share one public IP.

---

## ⚠️ Why IP Alone Is Weak Evidence

Same public IP may represent:
- Office network
- Café WiFi
- VPN exit node
- Mobile carrier NAT
- Shared proxy infrastructure

> **Important:** Same IP ≠ same person

---

## 🔍 How Websites Differentiate Users

Websites use:
- Cookies
- Sessions
- Browser fingerprinting
- Login accounts
- Device IDs
- User-agent strings

> NOT just IP addresses.

### 🔹 Cookies
Small browser-stored data used to recognize users and maintain login state.

### 🔹 Sessions
Temporary authenticated connection between a user and a website.

### 🔹 Browser Fingerprinting
Websites collect browser type, OS, timezone, fonts, screen size, and plugins to identify devices uniquely.

### 🔹 User-Agent String
Browser information sent to the website.

```text
Mozilla/5.0 Chrome/136 Windows 10
```

---

## 🔥 Distributed Attacks

**Definition**

Attack activity spread across multiple IP addresses.

**Example**

```text
FAILED LOGIN - admin - IP: 11.1.1.1
FAILED LOGIN - admin - IP: 22.2.2.2
FAILED LOGIN - admin - IP: 33.3.3.3
FAILED LOGIN - admin - IP: 44.4.4.4
SUCCESS LOGIN - admin - IP: 55.5.5.5
```

**Why Dangerous**
- Bypasses IP-based detections
- Avoids thresholds
- Hides attacker origin

---

## 🔐 VPN

**Definition**

Routes internet traffic through VPN provider infrastructure.

**What Website Sees:** VPN Public IP — NOT attacker's real ISP IP.

**Why Attackers Use VPNs**
- Hide real IP
- Evade attribution
- Bypass geo restrictions

---

## ☁️ Cloud VPS

**Definition**

Virtual server rented from a cloud provider.

**Common Providers**
- AWS
- Azure
- Google Cloud
- DigitalOcean

**Why Attackers Use VPS**
- Cheap
- Disposable
- Internet accessible
- Useful for automation and attacks

> **Important:** Cloud providers usually keep account logs, billing information, login activity, and IP records.

---

## 🏠 Residential Proxy

**Definition**

Traffic routed through real home-user internet connections.

**Why Attackers Use Residential Proxies**
- Appear like normal users
- Harder to detect
- Harder to block

| Type | Looks Like |
|---|---|
| **VPS** | Cloud / datacenter traffic |
| **Residential Proxy** | Normal home-user traffic |

---

## 🔥 TOR

**Definition**

Anonymous network routing traffic through multiple nodes.

**What Website Sees:** TOR exit node IP.

**Why Used:** Anonymity and hiding origin.

---

## 🤖 Botnet

**Definition**

Network of compromised devices controlled by an attacker.

**Why Dangerous**
- Distributed attacks
- Many IPs across many countries
- Difficult attribution

---

## 🔐 OPSEC (Operational Security)

**Definition**

How attackers hide identity and activity.

**Good Attacker OPSEC May Include**
- VPNs
- TOR
- Residential proxies
- Fake accounts
- Burner emails
- Cryptocurrency
- Disposable infrastructure

> **Goal:** Reduce attribution and traceability.

---

## ⚠️ Important SOC Lessons

| Thinking Level | Approach |
|---|---|
| ❌ **Weak Analyst** | Same IP = attacker |
| ✅ **Strong Analyst** | What behavior occurred from this infrastructure? |

---

## 🔥 Infrastructure vs Behavior

Sometimes infrastructure looks normal but behavior looks malicious.

**Example:** Residential IP → successful login → password change → file download

> Behavior matters more than IP type alone.

---

## 🔥 Shared Infrastructure Problem

Same IP may belong to:
- Office proxy
- Shared WiFi
- VPN service
- Mobile carrier NAT

This creates false positives and attribution challenges.

---

## 🔥 Distributed Brute Force

```text
FAILED LOGIN - admin - IP: 11.1.1.1
FAILED LOGIN - admin - IP: 22.2.2.2
FAILED LOGIN - admin - IP: 33.3.3.3
FAILED LOGIN - admin - IP: 44.4.4.4
SUCCESS LOGIN - admin - IP: 55.5.5.5
```

> 🔑 **Key Insight:** Attackers distribute activity across infrastructure to bypass thresholds, IP-based detections, and same-IP monitoring.

# 🔥 Scenario 1 — Shared Infrastructure

## 📜 Logs

```text
FAILED LOGIN - user1 - IP: 103.44.1.9
FAILED LOGIN - user2 - IP: 103.44.1.9
FAILED LOGIN - user3 - IP: 103.44.1.9
```

## ❓ Questions

1. Automatically malicious or not?
2. Why can same IP be misleading?
3. What context should analyst check?

## ✅ Answers

**1️⃣ Automatically malicious or not?**

Suspicious but not enough evidence for confirmed malicious activity.

**2️⃣ Why can same IP be misleading?**

Same IP may belong to:
- Office network
- Café WiFi
- VPN
- Mobile carrier NAT
- Shared infrastructure

**3️⃣ What context should analyst check?**

- Timing of login attempts
- IP reputation
- Internal or external IP
- VPN/proxy usage
- Authentication outcomes
- Additional login activity

---

# 🔥 Scenario 2 — Distributed Brute Force

## 📜 Logs

```text
FAILED LOGIN - admin - IP: 11.1.1.1
FAILED LOGIN - admin - IP: 22.2.2.2
FAILED LOGIN - admin - IP: 33.3.3.3
FAILED LOGIN - admin - IP: 44.4.4.4
SUCCESS LOGIN - admin - IP: 55.5.5.5
```

## ❓ Questions

1. What attack type?
2. Why difficult to detect?
3. Why is IP-based detection weak here?
4. What weak analysts may miss?
5. What should good analysts correlate?

## ✅ Answers

**1️⃣ What attack type?**

Distributed brute force attack against a single account.

**2️⃣ Why difficult to detect?**

- Attack activity distributed across multiple IPs
- Prevents same-IP threshold detections
- Reduces obvious brute-force patterns

**3️⃣ Why is IP-based detection weak here?**

- Each IP generates very few failed attempts
- Same-IP detections fail because activity is distributed

**4️⃣ What weak analysts may miss?**

Weak analysts may:
- Focus only on same-IP activity
- Treat events as unrelated
- Miss targeted single-account behavior

**5️⃣ What should good analysts correlate?**

Good analysts correlate:
- Same targeted account
- Authentication timing
- Distributed IP behavior
- Eventual successful login
- Post-login activity

---

# 🔥 Scenario 3 — Residential Proxy + Post-Compromise

## 📜 Logs

```text
SUCCESS LOGIN - finance_user - Residential IP
PASSWORD CHANGE - finance_user
FILE DOWNLOAD - finance_user
```

## ❓ Questions

1. Why does residential IP complicate detection?
2. Why is this still suspicious?
3. What does password change indicate?
4. What weak analysts may miss?
5. What should good analysts investigate?

## ✅ Answers

**1️⃣ Why does residential IP complicate detection?**

- Residential IPs appear like normal home-user traffic
- Malicious activity becomes harder to distinguish from legitimate behavior

**2️⃣ Why is this still suspicious?**

Successful login followed by:
- Password change
- File download

Indicates possible account takeover and data exfiltration.

**3️⃣ What does password change indicate?**

- Possible account takeover
- Possible persistence establishment

**4️⃣ What weak analysts may miss?**

Weak analysts may:
- Trust residential IPs too much
- Assume activity is legitimate
- Ignore suspicious behavior sequence

**5️⃣ What should good analysts investigate?**

- IP reputation
- Login history
- Geo/device anomalies
- Accessed/downloaded files
- Persistence actions

---

# 🔥 Scenario 4 — Internal IP Compromise

## 📜 Logs

```text
SUCCESS LOGIN - admin - IP: 10.0.0.8
FILE ACCESS - admin
PASSWORD CHANGE - admin
```

## ❓ Questions

1. Why does internal IP NOT automatically mean safe?
2. Possible explanations?
3. Why dangerous?
4. What should SOC investigate?

## ✅ Answers

**1️⃣ Why does internal IP NOT automatically mean safe?**

- Internal systems can still be compromised
- Attackers may already be inside the network

**2️⃣ Possible explanations?**

- Insider threat
- Compromised employee machine
- Lateral movement
- Stolen internal credentials

**3️⃣ Why dangerous?**

- Admin account involved
- File access occurred
- Password change indicates possible persistence

**4️⃣ What should SOC investigate?**

- Device activity
- Login history
- Affected systems
- Lateral movement indicators
- Additional persistence actions

---

## 🔥 Internal IP Does NOT Mean Safe

Internal/private IP may indicate:
- Insider threat
- Compromised internal device
- Attacker already inside network
- Lateral movement

> **Important:** Internal IP ≠ trusted activity

---

## 🎯 Key Takeaways

| Concept | Insight |
|---|---|
| **Public IP** | Not an identity |
| **Same IP** | Does not mean same person |
| **IP alone** | Weak evidence |
| **NAT** | Many users can share one public IP |
| **Attacker tools** | VPNs, VPS, residential proxies, TOR, botnets |
| **Good OPSEC** | Complicates attribution significantly |
| **Best approach** | Behavior + timing + correlation matter more than IP alone |
