# 1️⃣ Introduction

**Tester(s):**

- Name: Fahim Orko

**Purpose:**

- The purpose of this test is to identify vulnerabilities in the Booking System Phase 1 application, focusing specifically on the **registration page**. The goal is to discover security and functionality issues that could lead to unauthorized access, data exposure, or other system misuse.

**Scope:**

- **Tested components:**
  - Registration page (`/register`)
  - Input validation
  - Server error handling
  - Session behavior
  - Security headers
  - Automated scanning with OWASP ZAP
  - Manual browser-based testing
- **Exclusions:**
  - Login page
  - Booking features
  - Admin functionality
  - DoS testing or destructive testing
- **Test approach:**
  - **Black-box testing**

**Test environment & dates:**

- **Start:** _2025-11-28_
- **End:** _2025-11-28_
- **Test environment details:**
  - OS: Windows 11
  - Application running locally at `http://localhost:8000/`
  - Browser: Chrome
  - Tools used:
    - OWASP ZAP 2.16.1
    - Manual testing via browser

**Assumptions & constraints:**

- The application is in an early development phase and expected to have weaknesses.
- Testing time was limited, so focus was on high-impact issues.
- No user accounts were required to test registration.
- All testing was conducted locally and safely.

---

# 2️⃣ Executive Summary

**Short summary:**  
Security testing of the registration page identified several security weaknesses. Automated scanning using OWASP ZAP revealed missing CSRF protection, weak or missing security headers, and application error disclosure. In addition, manual testing performed directly in the browser identified SQL Injection behavior and path traversal input patterns that were not detected by the automated scan. These issues expose the application to serious exploitation risks if left unaddressed.

**Overall risk level:**  
➡️ **High**

**Top 5 immediate actions:**

1. Implement server-side validation and parameterized database queries.
2. Add CSRF protection tokens to all forms.
3. Add missing security headers (CSP, X-Frame-Options, X-Content-Type-Options).
4. Fix error handling to avoid exposing server messages.
5. Harden backend logic to prevent path traversal patterns.

---

# 3️⃣ Severity scale & definitions

| **Severity Level** | **Description**                                                                                                       | **Recommended Action**           |
| ------------------ | --------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| 🔴 **High**        | A serious vulnerability that can lead to full system compromise or data breach (e.g., SQL Injection, Path Traversal). | _Immediate fix required_         |
| 🟠 **Medium**      | A significant issue that may require specific conditions or user interaction (e.g., XSS, CSRF).                       | _Fix ASAP_                       |
| 🟡 **Low**         | A minor issue or configuration weakness (e.g., error disclosure).                                                     | _Fix soon_                       |
| 🔵 **Info**        | No direct risk, but useful for system hardening (e.g., missing security headers).                                     | _Monitor and fix in maintenance_ |

---

# 4️⃣ Findings

> **Note:** OWASP ZAP did not automatically detect SQL Injection or Path Traversal vulnerabilities during scanning. These issues were identified through **manual testing using the browser**, highlighting the importance of combining automated and manual testing techniques.

| ID   | Severity  | Finding                          | Description                                                                                                                                 | Evidence / Proof                                                              |
| ---- | --------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- |
| F-01 | 🔴 High   | SQL Injection behavior (Manual)  | The registration input triggered internal server errors when SQL meta-characters were submitted, indicating unsafe database query handling. | Manual browser testing resulted in HTTP 500 Internal Server Error responses.  |
| F-02 | 🔴 High   | Path Traversal patterns (Manual) | Path traversal payloads were accepted by input fields, suggesting weak backend validation and improper input sanitization.                  | Manual browser testing using traversal strings (e.g., `../`) in input fields. |
| F-03 | 🟠 Medium | Missing CSRF protection          | No anti-CSRF tokens were present on the registration form, making it vulnerable to CSRF attacks.                                            | ZAP alert: Absence of Anti-CSRF Tokens.                                       |
| F-04 | 🟠 Medium | Missing security headers         | Important security headers such as CSP and anti-clickjacking protections were not set.                                                      | ZAP alerts: CSP Header Not Set, Missing Anti-Clickjacking Header.             |
| F-05 | 🟡 Low    | Application error disclosure     | The application exposes internal server errors that may reveal implementation details.                                                      | ZAP alert: Application Error Disclosure (HTTP 500).                           |
| F-06 | 🟡 Low    | Missing X-Content-Type-Options   | Responses lacked the `X-Content-Type-Options: nosniff` header, weakening MIME-type protection.                                              | ZAP alert: X-Content-Type-Options Header Missing.                             |

---

# 5️⃣ OWASP ZAP Test Report (Attachment)

**Purpose:**

- This section includes the complete OWASP ZAP automated scan results in Markdown format.
- The report reflects configuration and header-related issues detected automatically.
- High-risk issues such as SQL Injection and Path Traversal were identified through **manual browser-based testing** and are therefore documented separately in the Findings section.

📄 **OWASP ZAP Full Report:**  
[OWASP ZAP Full Report](https://github.com/FahimOrko/Log-Book/blob/main/Lab/Part%201/zap_report_round_1.md)
