# **Booking System Phase 1 – Penetration Test Report**

---

## **1️⃣ Introduction**

### **Tester(s)**

- **Name:** Fahim Orko

### **Purpose**

Identify vulnerabilities and weaknesses in the registration system, focusing on:

- Form validation
- Server-side input handling
- Security headers
- CSRF protection

### **Scope**

#### **Tested Components**

- `/register` page
- Form fields: `username`, `password`, `birthdate`
- Server response behavior and headers

#### **Exclusions**

- Login page
- Booking functionality
- Admin endpoints

### **Test Approach**

- **Gray‑box testing**

### **Test Environment & Dates**

- **Start:** 29.11.2025
- **End:** 29.11.2025

### **Environment Details**

- **OS:** Windows 11
- **Browser:** Chrome
- **Database:** PostgreSQL (Docker)
- **Backend:** Deno runtime (Docker)
- **Testing Tool:** OWASP ZAP

### **Assumptions & Constraints**

- Testing limited to local environment
- No privileged credentials used
- Backend not modified during testing

---

## **2️⃣ Executive Summary**

### **Summary**

Penetration testing exposed **3 medium‑risk** vulnerabilities and **5 low‑risk** issues.  
The primary issues involve missing security headers, lack of CSRF protection, and susceptibility to clickjacking.

### **Overall Risk Level**

**Medium**

### **Top 5 Immediate Actions**

1. Implement CSRF tokens in registration POST requests.
2. Add security headers:
   - `Content-Security-Policy` (CSP)
   - `X-Frame-Options`
   - `X-Content-Type-Options`
3. Prevent exposure of internal server error messages.
4. Use HTTPS and enforce `Strict-Transport-Security` (HSTS).
5. Enforce backend-side validation for all input fields.

---

## **3️⃣ Severity Scale & Definitions**

| Severity Level | Description                                                                    | Recommended Action     |
| -------------- | ------------------------------------------------------------------------------ | ---------------------- |
| 🔴 **High**    | Serious vulnerability with potential for full system compromise or data breach | Immediate fix required |
| 🟠 **Medium**  | Significant issue such as missing CSRF or XSS risk                             | Fix ASAP               |
| 🟡 **Low**     | Minor weakness or misconfiguration                                             | Fix soon               |
| 🔵 **Info**    | Non‑critical, for hardening only                                               | Monitor or patch later |

---

## **4️⃣ Findings**

| ID       | Severity  | Finding                           | Description                                                    | Evidence / Proof                                    |
| -------- | --------- | --------------------------------- | -------------------------------------------------------------- | --------------------------------------------------- |
| **F‑01** | 🟠 Medium | Absence of CSRF token             | POST request sent without CSRF protection                      | ZAP: `<form action="/register" method="POST">`      |
| **F‑02** | 🟠 Medium | Missing CSP header                | Increases risk of XSS or content injection                     | ZAP: _Content Security Policy (CSP) Header Not Set_ |
| **F‑03** | 🟠 Medium | Missing Anti‑clickjacking header  | Page can be iframed → clickjacking risk                        | ZAP: _Missing Anti-clickjacking Header_             |
| **F‑04** | 🟡 Low    | Missing X‑Content-Type-Options    | Enables MIME‑sniffing                                          | ZAP: _X-Content-Type-Options Header Missing_        |
| **F‑05** | 🟡 Low    | Multiple missing security headers | Weakens transport security                                     | ZAP: _HTTP header analysis suggestions_             |
| **F‑06** | 🟡 Low    | Error disclosure risk             | Internal error details revealed when invalid data is submitted | ZAP: Error response evidence                        |
| **F‑07** | 🟡 Low    | Incomplete HTTP header hardening  | Headers like X‑Frame‑Options or CSP not fully enforced         | ZAP: Header misconfiguration notes                  |

---

## **5️⃣ OWASP ZAP Test Report (Attachment)**

### **Purpose**

Contains the automated scan results produced by OWASP ZAP.

### **Location**

📁 `Lab/Part 1/Zap Report.md`

---
