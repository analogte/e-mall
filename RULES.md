# � Marketplace Project Rules & Guidelines

> **Philosophy**: Quality, Scalability, and Security. This project is a **Multi-vendor Marketplace**. Every line of code must support the "Many Sellers" architecture.

---

## 🏗️ Architecture & Core Concepts

### 1. Multi-tenant Isolation (CRITICAL)
- **Rule**: Every data entity belonging to a merchant (Product, Order, coupon, etc.) **MUST** have a `shop_id` column.
- **Enforcement**:
  - `WHERE shop_id = ?` must be present in ALL Merchant-facing queries.
  - Never allow a merchant to query data without filtering by their `shop_id`.

### 2. Module Separation
- `mall-admin`: **Platform Owner Only**. Sees everything.
- `mall-seller`: **Merchant Only**. Sees ONLY their own shop's data.
- `mall-portal`: **Buyer Only**. Public facing APIs.

---

## ☕ Backend Rules (Spring Boot / Java)

### 1. Naming Conventions
- **Controllers**: `UmsSellerController` (Entity + Role + Controller)
- **Services**: `UmsSellerService` (Interface), `UmsSellerServiceImpl` (Implementation)
- **DTOs**: `SellerLoginParam` (Inputs), `SellerLoginResult` (Outputs)

### 2. Layered Architecture
Code must strictly follow this flow:
`Controller` -> `Service` -> `Mapper/Repository` -> `Database`
- ❌ No business logic in Controllers.
- ❌ No Database calls in Controllers.

### 3. MyBatis & Database
- Use `mall-mbg` (MyBatis Generator) for base CRUD.
- **Do not modify** generated Mapper XML files (`*.xml`).
- Write custom queries in `*Dao` (Interface) and `dao/*.xml` (XML) to avoid overwriting.

---

## 🖥️ Frontend Rules (Vue.js - Seller Center)

### 1. Technology Stack
- **Framework**: Vue.js 2.x (Options API) - to match `mall-admin-web`.
- **UI Library**: Element UI.
- **HTTP Client**: Axios (with specific interceptors for JWT).

### 2. Project Structure
- `src/api`: One file per Controller (e.g., `login.js`, `product.js`).
- `src/views`: Group by feature (e.g., `product/add.vue`, `order/list.vue`).

---

## � Mobile Rules (Flutter - Buyer App)

### 1. Widget Structure
- **Small Widgets**: Break down complex UIs into small, reusable widgets.
- **State Management**: Use `Provider` (as per existing setup).

### 2. Constraints
- Files > 300 lines should be refactored.
- No logical calculations in UI build methods.

---

## 🛡️ Security Rules

1. **Passwords**: Never store plain text. Use `BCryptPasswordEncoder`.
2. **API Keys**: Never commit `application.yml` with real production keys (use env vars).
3. **Validation**: All Inputs (`@RequestBody`) must use JSR-303 annotations (`@NotEmpty`, `@NotNull`).

---

## 🔄 Git Strategy

1. **Commit Messages** (Conventional Commits):
   - `feat:` New feature
   - `fix:` Bug fix
   - `docs:` Documentation
   - `style:` Formatting
   - `refactor:` Code restructuring
   - `test:` Adding tests
   - `chore:` Build/Tool changes

   *Example*: `feat: add register endpoint for seller`

2. **Branching**:
   - `master`: Production ready.
   - `dev`: Development branch.
   - `feature/xxx`: New features.

---

## ✅ Pre-Work Checklist (For AI)

Before writing any code, verify:
1. [ ] Am I working in the correct module? (`mall-seller` vs `mall-admin`)
2. [ ] Does this change respect `shop_id` isolation?
3. [ ] Have I checked `pom.xml` dependencies?
4. [ ] Is the naming consistent with the existing project?
