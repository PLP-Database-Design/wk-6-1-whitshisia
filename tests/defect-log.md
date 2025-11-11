# 🐞 Defect Log – Book Store App QA Project  
**Team:** `&lt;PLP Testers &gt;`  
**Current as of:** 2025-11-11  
**Environment:** Local dev, Chrome 130, macOS 14, commit `bfb73f8`

| ID | Summary | Severity | Priority | Affected FR(s) | Status |
|----|---------|----------|----------|----------------|--------|
| BUG-SRCH-01 | Search trims trailing spaces but **not leading spaces** → still returns 0 hits | Minor | Medium | FR-C01 | NEW |
| BUG-RED-01 | Root → /catalog redirect issues **302** instead of **React-Router replace**; causes flash of white | Minor | Low | FR-C01 | NEW |
| BUG-CART-01 | Cart badge increments **before** async stock check → allows oversell | Major | High | FR-O03 | NEW |
| BUG-CART-02 | Subtotal calculation uses **floating-point** → $19.994 shown instead of $19.99 | Major | High | FR-O02 | NEW |
| BUG-CART-03 | “Remove” button **missing aria-label** → a11y violation WCAG 2.1.1 | Minor | Medium | FR-X01 | NEW |

---

## Detailed Reports

### BUG-SRCH-01
- **Steps**  
  1. Navigate to `/catalog`.  
  2. Enter query with **leading** space: `" the Great gatsby"`.  
  3. Press Enter.  
- **Expected**  
  Same result set as `"the Great gatsby"` (case-insensitive, trimmed).  
- **Actual**  
  0 results; leading space sent to API.  
- **Evidence**  
  Screenshot: `evidence/bug-srch-01-leading-space.png`  
- **Notes**  
  Quick fix: `trimStart()` in `SearchBar.jsx` line 42.

---

### BUG-RED-01
- **Steps**  
  1. Open fresh tab at `/`.  
- **Expected**  
  Seamless client-side redirect; no network round-trip.  
- **Actual**  
  302 from server, then JS redirect → white flash ~200 ms.  
- **Evidence**  
  Network timing HAR: `evidence/bug-red-01-flash.har`  
- **Notes**  
  Use `&lt;Navigate replace /&gt;` instead of `res.redirect(302)`.

---

### BUG-CART-01
- **Steps**  
  1. Set book stock = 1 in seed.  
  2. Two tabs open; both click “Buy Now” simultaneously.  
- **Expected**  
  Second tab receives stock-out toast.  
- **Actual**  
  Both tabs show “1 item in cart”; badge increments; oversell possible.  
- **Evidence**  
  Screen recording: `evidence/bug-cart-01-oversell.mp4`  
- **Notes**  
  Race between `localStorage` event and optimistic UI. Needs pessimistic lock.

---

### BUG-CART-02
- **Steps**  
  1. Add book priced $9.997 (test seed).  
  2. Change qty to 2.  
- **Expected**  
  Subtotal = $19.99 (rounded to currency minor units).  
- **Actual**  
  Subtotal shows $19.994.  
- **Evidence**  
  Screenshot: `evidence/bug-cart-02-float-rounding.png`  
- **Notes**  
  Replace `parseFloat` with `toMinorUnits` utility already in repo.

---

### BUG-CART-03
- **Steps**  
  1. Run axe-core on `/cart`.  
- **Expected**  
  0 violations.  
- **Actual**  
  “Buttons must have accessible name” on Remove `&lt;button&gt;`.  
- **Evidence**  
  axe report: `evidence/bug-cart-03-axe.json`  
- **Notes**  
  Add `aria-label="Remove {{title}} from cart"` dynamically.
