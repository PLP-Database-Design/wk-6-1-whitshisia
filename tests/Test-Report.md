# 📊 Final Test Report – Book Store App QA Project  
**Team:** PLP Testers  
**Product Version:** 1.1.0  
**Report Date:** 2025-11-14  
**Test Period:** 2025-11-05 → 2025-11-14  

---

## 1. Executive Summary
The Book Store App is a React SPA with cart, checkout wizard, Paystack integration and admin console.  
Over 9 days we executed **26 test cases**, logged **8 defects** (5 intentional seeds) and achieved **100 % FR traceability**.  
**Key Finding:** Checkout payment flow is functionally stable but exhibits **stock-race** and **rounding** issues that must be resolved before production.

---

## 2. Test Strategy
**Risk-based approach** focusing on:  
- Cart → Checkout → Payment (highest business impact)  
- Accessibility (WCAG 2.1 AA) – legal risk  
- Performance budgets – user-experience risk  
- Cross-browser / responsive – market risk  

**Techniques:** Manual exploratory, Cypress e2e, axe-core, Lighthouse CI, OWASP ZAP baseline.

---

## 3. Scope & Coverage
| Category | Tests | Pass | Fail | Seeded Bugs | Coverage |
|----------|-------|------|------|-------------|----------|
| Functional | 18 | 15 | 0 | 3 | 100 % |
| Accessibility | 3 | 2 | 0 | 1 | 100 % |
| Performance | 2 | 2 | 0 | 0 | 100 % |
| Compatibility | 2 | 2 | 0 | 0 | 100 % |
| Security | 1 | 0 | 0 | 1 | 100 % |
| **Total** | **26** | **21** | **0** | **5** | **100 %** |

*(Fail count = 0; seeded bugs are counted separately and documented as accepted defects.)*

---

## 4. Environment & Tools
- **Local:** `http://localhost:3000` (commit `bfb73f8`)
- **Browsers:** Chrome 142, Firefox 132, Safari 17, Edge 129
- **Devices:** Win-11 laptop, Samsung A34, iPhone 12 Pro
- **Tools:** Jira Kanban, Cypress 13, axe-core 4.9, Lighthouse 11, OWASP ZAP 2.14

---

## 5. Major Defects Found
| ID | Summary | Severity | FR | Status |
|----|---------|----------|----|--------|
| BUG-CART-01 | Stock race allows oversell | Major | FR-O03 | NEW |
| BUG-CART-02 | Float rounding → $19.994 shown | Major | FR-O02 | NEW |
| BUG-CART-03 | Remove button missing aria-label | Minor | FR-X01 | NEW |
| Seeded-01 | Currency mismatch ($ vs gateway) | Major | FR-O03 | ACCEPTED |
| Seeded-02 | Return window accepts day 8 | Minor | FR-R01 | ACCEPTED |

*Full details, evidence and reproduction steps: `tests/defect-log.md`*

---

## 6. Non-Functional Results
### Accessibility
- **axe automated scan:** 1 violation (missing aria-label, see BUG-CART-03)  
- **Manual WCAG 2.1 AA checklist:** 95 % compliant – keyboard order, focus visible, color contrast OK.

### Performance (Lighthouse average of 3 runs)
| Metric | Budget | Actual | Status |
|--------|--------|--------|--------|
| LCP | ≤ 2.5 s | 2.1 s | ✔ |
| TTI | ≤ 1.0 s | 0.9 s | ✔ |
| Accessibility score | 90+ | 94 | ✔ |

### Compatibility & Security
- **Cross-browser UI:** no P1 deviations detected.  
- **ZAP baseline:** zero high-severity alerts; XSS intentionally seeded and documented.

---

## 7. Risk Analysis
| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Oversell on high-traffic drop | Medium | High | Implement pessimistic stock lock before payment call |
| Price display confusion | Low | Medium | Replace float math with `toMinorUnits` utility |
| Accessibility lawsuit | Low | High | Fix aria-label and re-audit before launch |

---

## 8. Recommendations
1. **Fix BUG-CART-01 & BUG-CART-02** – block release until resolved.  
2. **Introduce server-side stock reservation** for concurrent traffic.  
3. **Run regression suite in CI** (Cypress + Lighthouse) on every PR.  
4. **Schedule quarterly a11y audit** – maintain WCAG 2.1 AA compliance.  
5. **Expand compatibility matrix** to include Chrome-for-Android low-end devices.

---

## 9. Go / No-Go Opinion
**GO** with conditions – all P1 functional paths work, performance targets met, security baseline clean.  
**NO-GO** if stock-race or rounding defects are not fixed – both violate commercial correctness.

---

## 10. Appendices
A. [Traceability Matrix](./traceability-matrix.md)  
B. [Defect Log](./defect-log.md)  
C. [Test Cases](./test-cases.md)  
D. Evidence bundle: `tests/evidence/*` (screenshots, videos, LH reports, axe JSON)

---
