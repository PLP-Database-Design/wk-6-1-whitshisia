&lt;!-- tests/test-plan.md --&gt;
# 📋 Test Plan – Book Store App QA Project  
**Team:** `PLP Testers`  
**Version:** 1.0  
**Date:** 2025-11-03  

---

## 1. Objective & Scope
Validate that the React Book Store meets stated functional requirements (FR), non-functional requirements (NFR) and intentional defect seeds, **before** the Nov 18 production release.  
Focus on risk-based coverage: cart → checkout → payment flow, accessibility, performance budgets, cross-browser compatibility, and security hygiene.

---

## 2. In-Scope Features (mapped to FR codes)
| Feature Area | ID | Description |
|--------------|----|-------------|
| Catalog | FR-C01 | Browse, search, lazy images |
| Cart | FR-O01–O03 | Add, update qty, subtotal, stock guard |
| Checkout | FR-O04–O05 | Wizard, validation, Paystack payment |
| Orders | FR-O06 | Order history, status lifecycle |
| Admin | FR-M01–M05 | CRUD, inventory, moderation |
| Reviews | FR-U01–U03 | Post-purchase rating, sanitization |
| Returns | FR-R01–R03 | 7-day window, refund simulation |
| Notifications | FR-N01–N02 | Bell, unread count, mark read |
| A11y | FR-X01 | WCAG 2.1 AA compliance |
| Performance | FR-X02 | LCP ≤ 2.5 s, TTI ≤ 1 s |
| Security | FR-S01–S03 | XSS prevention, URL whitelist |
| Intentional Defects | — | 10 seeded bugs (currency, rounding, XSS, etc.) |

---

## 3. Out-of-Scope
- Native mobile app (iOS/Android)  
- Email/SMS notification delivery (only in-app)  
- PCI-DSS audit of Paystack SDK  
- Load testing &gt; 500 concurrent users (covered in NFR but deferred to Phase-2)

---

## 4. Environments
| Tier | URL | Browsers | Devices | Throttling |
|------|-----|----------|---------|------------|
| Local | `http://localhost:3000` | Chrome Version 142.0.7444.60 (Official Build) (64-bit),| Windows 11 14inch Laptop, samsung A34 | None |




---

## 5. Tools & Extensions
- Test case mgmt: GitHub Projects (board link)  
- Exploratory notes: RapidReporter 2.3  
- A11y: axe-core 4.9, WAVE, Lighthouse a11y audit  
- Perf: Lighthouse CI, WebPageTest, React Profiler  
- Network: MSW 2.1 for mock latency/errors  
- Automation: Cypress 13 (e2e), React Testing Library (unit)  
- CSV validation: Python `pandas` + `pytest`  
- Security: OWASP ZAP baseline scan

---

## 6. Risks & Mitigations
| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| Paystack test key expires | High | Low | Alert 3 days before; key rotation script |
| Stock race condition | Major | Medium | Add pessimistic UI lock, test with 2 tabs |
| CSV export decimal comma | Minor | High | Post-process with `locale=EN-us` |
| Seeded XSS flagged by client | Major | Low | Document in test report; use safe markdown lib |

---

## 7. Test Types & Coverage Targets
| Type | Technique | Target Metric |
|------|-----------|---------------|
| Functional | Black-box + API mocking | 100 % of P1 flows, 80 % P2 |
| Accessibility | WCAG 2.1 AA checklist | 0 critical violations |
| Performance | Lighthouse CI | LCP ≤ 2.5 s, TTI ≤ 1 s, 90+ score |
| Compatibility | Cross-browser matrix | No P1 bugs on latest 2 versions |
| Security | OWASP Top-10 lite | Zero high-severity alerts |
| Exploratory | Session-based (90 min) | ≥ 5 sessions/week, 30 bugs logged |
| Regression | Automated smoke | ≤ 5 min runtime, 100 % pass before merge |

---

## 8. Entry & Exit Criteria
### Entry
- Code freeze 24 h before test cycle  
- Staging deployment green on CI  
- Test data seeded (`/data/seed.json` v1.3)

### Exit
- All P1 defects closed or accepted by PO  
- No critical a11y or security issues  
- Performance budget met on 3 consecutive runs  
- Traceability matrix ≥ 90 % FR covered with evidence

---

## 9. Deliverables & Schedule
| Artifact | Due | Owner |
|----------|-----|-------|
| This test plan | Nov 5 | QA Lead |
| Test cases | Nov 11 | Whole team |
| Defect log | Nov 11 (draft) → Nov 18 (final) | Everyone |
| Final report | Nov 18 | QA Lead |
| Video presentation | Nov 18 | PM + Designer |

---

## 10. Sign-Off
| Role | Name | Signature | Date |
|------|------|-----------|------|
| QA Lead | Asmamaw Yismaw | AY | Nov 02, 2025 |
| Dev Lead | Jostina Mwamburi | JM | Nov 02, 2025 |
| Product Owner | Whitney Shisia | WS | Nov 02, 2025 |

---

