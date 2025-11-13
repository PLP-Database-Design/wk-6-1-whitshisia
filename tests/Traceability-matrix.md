# 🔗 Requirements Traceability Matrix  
**Team:** PLP Testers  
**Version:** 1.0  
**Date:** 2025-11-14  

&gt; Maps every **Functional Requirement (FR)** to at least one **test case (TC)** and **evidence** (executed or seeded defect).  
&gt; Green ✔ = covered & passed 🟡 = covered with known bug ❌ = not yet executed

| FR Code | Requirement (short) | Test Case / Bug ID | Evidence Path | Status |
|---------|---------------------|--------------------|---------------|--------|
| **Catalog & Discovery** |
| FR-C01  | Browse, search, lazy images | TC-001, TC-005, TC-006 | `tests/evidence/tc-001-search.png` | ✔ |
| **Cart & Checkout** |
| FR-O01  | Add / remove cart items | TC-003, T2 | `tests/evidence/tc-003-add-cart.png` | ✔ |
| FR-O02  | Update qty, sub-total | TC-004, T3 | `tests/evidence/tc-004-qty-update.png` | ✔ |
| FR-O03  | Stock guard | **BUG-CART-01** | `tests/evidence/bug-cart-01-oversell.mp4` | 🟡 |
| FR-O04  | Checkout wizard | TC-007, T15 | `tests/evidence/tc-007-checkout-wizard.png` | ✔ |
| FR-O05  | Paystack payment | TC-008, T12 | `tests/evidence/tc-008-paystack.png` | ✔ |
| **Orders** |
| FR-O06  | Order history & CSV | TC-025 | `tests/evidence/tc-025-order-lifecycle.png` | ✔ |
| **Returns / Refunds** |
| FR-R01  | 7-day return window | **Seeded defect** | `tests/evidence/seeded-return-window.png` | 🟡 |
| FR-R02  | Refund audit trail | TC-024 | `tests/evidence/tc-024-refund-audit.png` | ✔ |
| **Reviews & Q&A** |
| FR-U01  | Post-purchase review | TC-016 | `tests/evidence/tc-016-book-details.png` | ✔ |
| FR-U02  | Admin moderation | TC-022 | `tests/evidence/tc-022-review-mod.png` | ✔ |
| FR-U03  | Safe markdown Q&A | **Seeded XSS** | `tests/evidence/seeded-xss-markdown.png` | 🟡 |
| **Admin Console** |
| FR-M01  | Catalog CRUD | TC-009, T21 | `tests/evidence/tc-09-admin-crud.png` | ✔ |
| FR-M02  | Inventory & low-stock | TC-021 | `tests/evidence/tc-021-low-stock.png` | ✔ |
| FR-M03  | Order dashboard | TC-023 | `tests/evidence/tc-023-admin-auth.png` | ✔ |
| **Notifications** |
| FR-N01  | Unread badge | TC-018 | `tests/evidence/tc-018-badge.png` | ✔ |
| FR-N02  | Mark-all-read | **BUG-CART-03** | `tests/evidence/bug-cart-03-axe.json` | 🟡 |
| **Accessibility** |
| FR-X01  | WCAG 2.1 AA | TC-017, axe scans | `tests/evidence/axe-report.html` | ✔ |
| **Performance** |
| FR-X02  | LCP ≤ 2.5 s, TTI ≤ 1 s | Lighthouse CI | `tests/evidence/lighthouse.pdf` | ✔ |
| **Compatibility** |
| FR-X03  | Latest 2 browsers | T19, T20 | `tests/evidence/cross-browser.png` | ✔ |
| **Security Hygiene** |
| FR-S01  | Sanitization | **Seeded XSS** | `tests/evidence/seeded-xss-markdown.png` | 🟡 |
| FR-S02  | URL whitelist | TC-026 | `tests/evidence/tc-026-url-whitelist.png` | ✔ |
| FR-S03  | Storage quota | TC-010 | `tests/evidence/tc-010-persist.png` | ✔ |

### Summary Statistics
- **Total FRs in scope:** 22  
- **Covered (✔ + 🟡):** 22 (100 %)  
- **Passed (✔):** 17 (77 %)  
- **Known bugs / seeded (🟡):** 5 (23 %)  
- **Not executed (❌):** 0

---

## 🔍 Gap Analysis & Recommendations
1. **Stock race (FR-O03)** – implement pessimistic lock before payment.  
2. **Return window off-by-one (FR-R01)** – fix date comparison (`&lt;= 7` → `&lt; 7`).  
3. **Notification badge (FR-N02)** – dispatch reset action after “mark all read”.  
4. **XSS markdown (FR-U03, FR-S01)** – upgrade markdown renderer to latest `DOMPurify`.

---

## 📎 Appendix
- Full defect log: `tests/defect-log.md`  
- Full test case sheet: `tests/test-cases.md`  
- Evidence folder: `tests/evidence/`
