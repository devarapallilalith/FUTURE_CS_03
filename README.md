# FUTURE_CS_03
# 🔐 API Security Risk Analysis Report

![Category](https://img.shields.io/badge/Category-API%20Security-blue)
![Methodology](https://img.shields.io/badge/Methodology-OWASP%20API%20Top%2010-red)
![Scope](https://img.shields.io/badge/Scope-Read--Only%20%7C%20Demo%20APIs-green)
![Year](https://img.shields.io/badge/Year-2026-lightgrey)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

> A professional API Security Risk Analysis performed against public demo APIs — following real AppSec consultant methodology, documented for SaaS security awareness and education.

---

## 📋 Project Overview

Modern SaaS applications rely on APIs for everything — mobile apps, dashboards, third-party integrations, and microservice communication. Insecure APIs are the leading vector for data breaches in 2023–2025.

This project conducts a **read-only security audit** of two widely-used public test APIs:
- **JSONPlaceholder** — `https://jsonplaceholder.typicode.com`
- **ReqRes** — `https://reqres.in`

The analysis identifies real API security risks based on observable behaviors: endpoint access patterns, response headers, data exposure, and authentication enforcement — without any exploitation or system modification.

---

## ⚠️ Scope & Ethics

| ✅ Allowed | ❌ Not Allowed |
|-----------|---------------|
| Testing public / demo APIs | Testing private or production APIs |
| Read-only GET requests | Exploitation or authentication bypass |
| Safe POST where documented | Flooding / Denial of Service testing |
| Documentation-based analysis | Attacking systems without permission |
| Header & response inspection | Sharing or misusing extracted data |

**All APIs tested are explicitly provided for developer testing and education.**

---

## 📁 Repository Structure

```
api-security-risk-analysis/
│
├── README.md                                    ← You are here
├── API_Security_Risk_Analysis_Report_2025.docx  ← Full audit report (Word)
│
├── evidence/
│   ├── E01_unauthenticated_users_endpoint.txt   ← RISK-01 request/response
│   ├── E02_excessive_data_exposure.txt          ← RISK-02 user object dump
│   ├── E03_no_rate_limiting_headers.txt         ← RISK-03 header analysis
│   ├── E04_unauthenticated_delete.txt           ← RISK-04 write operation
│   ├── E05_cors_wildcard_header.txt             ← RISK-05 CORS header
│   ├── E06_missing_security_headers.txt         ← RISK-06 header audit
│   ├── E07_user_id_enumeration.txt              ← RISK-07 enumeration log
│   └── E08_no_input_validation.txt             ← RISK-08 bad POST accepted
│
└── screenshots/
    ├── postman_users_unauthenticated.png
    ├── postman_delete_no_auth.png
    ├── postman_cors_wildcard.png
    └── postman_missing_headers.png
```

---

## 🎯 Findings Summary

| ID | Finding | Severity | OWASP Category | Priority |
|----|---------|----------|----------------|----------|
| RISK-01 | Unauthenticated Access to All Endpoints | 🔴 HIGH | API1 — BOLA | Immediate |
| RISK-02 | Excessive Data Exposure in User Object | 🔴 HIGH | API3 — Broken Property Auth | Immediate |
| RISK-03 | No Rate Limiting on Any Endpoint | 🔴 HIGH | API4 — Resource Consumption | Sprint 1 |
| RISK-04 | Unauthenticated Write Operations | 🔴 HIGH | API5 — Function Level Auth | Immediate |
| RISK-05 | CORS Wildcard Allows Any Origin | 🟡 MEDIUM | API8 — Misconfiguration | Sprint 2 |
| RISK-06 | Missing Security Headers in All Responses | 🟡 MEDIUM | API8 — Misconfiguration | Sprint 2 |
| RISK-07 | User Enumeration via Sequential IDs | 🟡 MEDIUM | API1 — BOLA | Sprint 1 |
| RISK-08 | No Input Validation on POST Endpoints | 🟢 LOW | API6 — Business Flow | Sprint 3 |

**Total: 8 findings — 4 High · 3 Medium · 1 Low**

---

## 🔍 Analysis Methodology

### Step 1 — Documentation Review
Read the official API documentation to understand intended behavior, authentication model, supported methods, and data schema before making any requests.

### Step 2 — Endpoint Enumeration
Mapped all available resources across both APIs:
- JSONPlaceholder: `/posts`, `/comments`, `/albums`, `/photos`, `/todos`, `/users`
- ReqRes: `/users`, `/login`, `/register`, `/unknown`

### Step 3 — Authentication Analysis
Tested each endpoint with and without authentication credentials:
- Sent requests with zero auth headers → observed responses
- Compared response codes (200 vs. 401/403) to determine enforcement
- Tested write methods (POST, PUT, DELETE) without any token

### Step 4 — Header Inspection
Captured full request and response headers for every endpoint:

```
Checked for:
  Authorization enforcement    → required / not required
  Rate-limit headers           → X-RateLimit-Limit, X-RateLimit-Remaining
  Security headers             → HSTS, CSP, X-Content-Type-Options, X-Frame-Options
  CORS headers                 → Access-Control-Allow-Origin value
  Information leakage          → X-Powered-By, Server header
```

### Step 5 — Response Analysis
Examined response bodies for:
- Fields returned vs. fields the client UI actually needs
- Presence of PII (email, phone, address, geo-coordinates)
- Internal metadata or debug fields
- Error message verbosity that could aid attackers

### Step 6 — Risk Scoring
Each finding rated on two axes:
- **Likelihood** — How easy is it to exploit? (unauthenticated = Very High)
- **Impact** — What can an attacker achieve? (PII theft / data loss = High)

Final severity: `HIGH` / `MEDIUM` / `LOW` aligned with OWASP API Security Top 10 (2023).

### Step 7 — Remediation Mapping
Every finding includes specific, actionable developer guidance linked to industry best practices.

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **Postman** | Primary API testing client — sending requests, inspecting responses and headers |
| **Browser DevTools (Network Tab)** | Live request capture, header inspection |
| **MXToolbox** | Domain reputation and DNS analysis |
| **OWASP API Security Top 10 (2023)** | Risk classification and category mapping |
| **API Security Checklist (shieldfy)** | Developer remediation reference |
| **JSONPlaceholder** | Primary test API — representative REST patterns |
| **ReqRes** | Secondary test API — auth simulation endpoints |

---

## 📚 OWASP API Security Top 10 Reference

This audit maps every finding to the OWASP API Security Top 10 (2023):

| ID | Category |
|----|----------|
| API1 | Broken Object Level Authorization |
| API2 | Broken Authentication |
| API3 | Broken Object Property Level Authorization |
| API4 | Unrestricted Resource Consumption |
| API5 | Broken Function Level Authorization |
| API6 | Unrestricted Access to Sensitive Business Flows |
| API7 | Server Side Request Forgery |
| API8 | Security Misconfiguration |
| API9 | Improper Inventory Management |
| API10 | Unsafe Consumption of APIs |

**GitHub:** https://github.com/OWASP/API-Security

---

## 🔑 Key Risk Explanations (Plain Language)

### RISK-01 — Unauthenticated Access
Anyone on the internet can call `GET /users` and receive all user records — names, emails, addresses, phone numbers — with zero credentials. In production, this is a data breach waiting to happen.

### RISK-02 — Excessive Data Exposure
The `/users/1` endpoint returns home address, geo-coordinates, phone number, and internal company strings. A frontend user profile page only needs a name and email. Returning everything violates data minimization and GDPR principles.

### RISK-03 — No Rate Limiting
An attacker can send 100,000 requests per minute with no throttling, enabling automated scraping of all data, credential stuffing against auth endpoints, or server overload.

### RISK-04 — Unauthenticated Writes
`DELETE /posts/1` returns `200 OK` with no token. In production, any anonymous user could delete any other user's content — complete data integrity failure.

### RISK-05 — CORS Wildcard
`Access-Control-Allow-Origin: *` means any malicious website can make API calls in a victim's browser. Combined with cookie auth, this enables silent cross-site data theft.

### RISK-06 — Missing Security Headers
No `Strict-Transport-Security` (HSTS) enables SSL stripping. No `X-Content-Type-Options` enables MIME sniffing attacks. `X-Powered-By: Express` tells attackers exactly what stack to target.

### RISK-07 — Sequential ID Enumeration
Sequential IDs (`/users/1`, `/users/2`...) allow attackers to enumerate every user in the system with a simple loop. UUIDs eliminate this attack vector.

### RISK-08 — No Input Validation
Sending `{"userId": "not-a-number", "injected": "<script>alert(1)</script>"}` to a POST endpoint returns `201 Created` with the payload echoed back. No schema, no sanitization, no rejection.

---

## 🛡️ Top Remediation Recommendations

1. **Authenticate everything** — OAuth 2.0 Bearer tokens on every non-public endpoint
2. **Authorize every resource** — verify the caller owns or has permission to access each record
3. **Rate limit at the gateway** — 100 req/min per IP, return `429 Too Many Requests`
4. **Return minimum data** — filter response fields to only what the client needs
5. **Use UUIDs** — replace sequential integers with random UUIDs for all resource IDs
6. **Fix CORS** — replace `*` with an explicit domain allowlist
7. **Add security headers** — use Helmet.js or gateway middleware for all standard headers
8. **Validate input** — enforce JSON Schema on every POST/PUT request body

---

## 📄 Report Contents

The full Word document (`API_Security_Risk_Analysis_Report_2025.docx`) includes:

1. **Executive Summary** — KPI dashboard with finding counts and severity breakdown
2. **APIs Under Analysis** — Technical profile of both test APIs
3. **Methodology** — 7-step audit process aligned with OWASP
4. **Detailed Risk Findings** — Full analysis of all 8 risks with evidence and remediation
5. **Risk Matrix & Priority Table** — Likelihood × Impact scoring with sprint priorities
6. **Strategic Recommendations** — Immediate, short-term, and architectural guidance
7. **Tools & References** — Complete tool inventory and reference links
8. **Conclusion** — Summary and developer call-to-action

---

## 🎓 Who This Is For

- **AppSec Engineers** — Methodology and OWASP mapping reference
- **Security Analysts** — API risk identification and evidence documentation
- **SaaS Developers** — Understanding what secure APIs look like vs. insecure ones
- **Security Consultants** — Report structure for client-deliverable API audits
- **Students** — Real-world API security audit practice with public APIs

---

## ⚖️ Disclaimer

This project is strictly educational. All API testing was performed against public demo services (JSONPlaceholder, ReqRes) that are explicitly designed and maintained for developer testing. No authentication was bypassed, no data was exfiltrated, no production systems were accessed, and no denial-of-service was attempted.

**Never perform security testing on APIs without explicit written permission from the system owner.**

---

## 🔗 References

- OWASP API Security Top 10: https://github.com/OWASP/API-Security
- API Security Checklist: https://github.com/shieldfy/API-Security-Checklist
- Public APIs Collection: https://github.com/public-apis/public-apis
- JSONPlaceholder: https://jsonplaceholder.typicode.com
- ReqRes: https://reqres.in

---

*API Security Risk Analysis Report — Modern SaaS Security Audit — 2025*
