# 🦅 Professional Code Review Standards: Marketplace Edition

> "Quality is not an act, it is a habit." - Focusing on Scalability, Security, and Isolation in a Multi-vendor Environment.

This document outlines the standard operating procedure for code reviews within the **Mall Marketplace** project. It includes actionable AI prompts for reviewing Backend (Spring Boot), Frontend (Vue.js), and Mobile (Flutter) code.

---

## 🏗️ Core Review Dimensions

When reviewing code, assess against these **critical pillars**. Issues should be categorized by severity: **[CRITICAL]**, **[HIGH]**, **[MEDIUM]**, **[LOW]**.

1.  **DATA ISOLATION (Crucial)**: 🛡️ Does the code verify `shop_id`? (Prevent cross-shop data leaks).
2.  **CORRECTNESS**: 🧠 Does logic handle Multi-vendor scenarios (Split orders, Commission calculation)?
3.  **SECURITY**: 🔐 OWASP Top 10, IDOR (Insecure Direct Object References), SQL Injection.
4.  **PERFORMANCE**: ⚡ N+1 Queries, Index usage, Frontend bundle size, API latency.
5.  **CODE QUALITY**: 📖 Clean Code, SOLID, Layered Architecture violation checks.

---

## 🤖 AI Review Prompts (Copy & Paste)

### 🎯 Prompt 1: The "Marketplace Logic" Audit
**Use when:** Reviewing Backend Logic (`mall-seller`, `mall-portal`).

```text
Act as a Senior Backend Engineer. Review this Spring Boot code for a Multi-vendor Marketplace.

CORE CHECKS:
1. DATA ISOLATION (CRITICAL):
   • Are queries checking `shop_id`? 
   • Can a merchant access another merchant's data? (IDOR check)
   • Is `SecureUtil.getCurrentUser()` used correctly?
2. ARCHITECTURE:
   • Logic in Service Layer? (Controllers should be clean)
   • DTOs used for Input/Output? (No @Entity exposed)
3. TRANSACTIONS:
   • Are logical units (e.g. Order Creation + Stock Reduction) transactional?
   • Is exception handling correct (Asserts.fail)?

OUTPUT FORMAT:
[SEVERITY] File:Line Issue
Why: Explanation
Fix: Code snippet
```

---

### 🎯 Prompt 2: Vue.js Seller Center Review
**Use when:** Reviewing `mall-seller-web`.

```text
Act as a Senior Frontend Engineer. Review this Vue.js (Element UI) code.

FOCUS AREAS:
1. SECURITY:
   • Are API tokens stored securely (Cookies/Headers)?
   • properly handling 401/403 errors?
2. COMPONENT UTILITY:
   • Reusability? (Not copying code from mall-admin-web blindly)
   • Form Validation? (All inputs must be validated)
3. PERFORMANCE:
   • Lazy loading routes?
   • Proper cleanup in `beforeDestroy`?

OUTPUT FORMAT:
[SEVERITY] Component Issue
Suggestion: How to fix
```

---

### 🎯 Prompt 3: Flutter Mobile App Review
**Use when:** Reviewing the Buyer App.

```text
Act as a Senior Flutter Engineer. Review this code for performance and state management.

CHECKLIST:
1. STATE MANAGEMENT (Provider):
   • Logic separated from UI?
   • `notifyListeners` minimized?
2. UI PERFORMANCE:
   • Const constructors used?
   • Complex builds broken into small widgets?
3. NETWORK:
   • Proper error handling (Offline/Timeout)?
   • Models using `fromJson` safely?

OUTPUT FORMAT:
[SEVERITY] Widget/Function Issue
Fix: Improved code
```

---

### 🎯 Prompt 4: Security & Database Scan
**Use when:** Reviewing SQL changes or Sensitive logic.

```text
Act as a Security Engineer. Review these changes.

RED FLAGS TO CATCH:
🚨 Missing `shop_id` in WHERE clause for Merchant queries.
🚨 Hardcoded Secrets in application.yml.
🚨 SQL Injection risks (using ${} instead of #{} in MyBatis).
🚨 Passwords logged or stored in plain text.
🚨 Money calculations using Float/Double (Must use BigDecimal).

OUTPUT:
List any RED FLAGS found immediately.
```

---

## 📊 Quick Review Checklist (Manual)

Before approving a PR, ask yourself:

- [ ] **Data Safety**: Did I see `shop_id` in the SQL/Criteria?
- [ ] **Architecture**: Is the Controller dumb? (No logic inside)
- [ ] **Money**: Is `BigDecimal` used for prices?
- [ ] **Naming**: do naming conventions follow `Rules.md`?
