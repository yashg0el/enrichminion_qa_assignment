# Bug Report - EnrichMinion QA Assignment

This document lists discovered or potential bugs with reproduction steps and root cause analysis (RCA). Attach network logs/screenshots where applicable.

---

## BUG-001
- **Title:** Signup allows weak passwords (e.g., '123')
- **Severity:** High
- **Steps to Reproduce:**
  1. Navigate to Signup page.
  2. Enter name and email.
  3. Enter password '123' and confirm it.
  4. Click Sign up.
- **Expected Result:** Signup should be rejected with password strength error (400).
- **Actual Result:** Account created successfully.
- **Evidence:** Network request returned 201 (attach screenshot of Network tab).
- **Root Cause:** Missing server-side password strength validation. (Backend)

---

## BUG-002
- **Title:** Login returns token but UI remains unauthenticated
- **Severity:** High
- **Steps to Reproduce:**
  1. Login with valid credentials.
  2. Observe POST /api/auth/login response returns token.
  3. Dashboard remains in loading state or redirects back to login.
- **Expected Result:** Successful login should persist session and load dashboard.
- **Actual Result:** Token returned but UI does not store/use token.
- **Evidence:** Network logs show 200 for login; subsequent /auth/me returns 401 or not called.
- **Root Cause:** Frontend not storing token or not attaching Authorization header. (Frontend)

---

## BUG-003
- **Title:** CSV upload accepted without required columns, job fails later
- **Severity:** Medium
- **Steps to Reproduce:**
  1. Upload a CSV missing the 'first_name' column.
  2. Server returns jobId and queues job.
  3. Job status eventually becomes 'failed' with 500.
- **Expected Result:** Upload should be validated and rejected with descriptive 400 error.
- **Actual Result:** Job queued then fails.
- **Root Cause:** Backend lacks upfront schema validation and normalization. (Backend) Also frontend should validate before sending. (Both)

---

## BUG-004
- **Title:** Email verification API returns inconsistent field names across calls
- **Severity:** Low
- **Steps to Reproduce:**
  1. Call POST /api/verify/email multiple times for same email.
  2. Observe response keys.
- **Expected Result:** Consistent schema (e.g., always 'status' or always 'valid').
- **Actual Result:** Sometimes 'status' key appears, sometimes 'valid' boolean.
- **Root Cause:** Downstream provider returns inconsistent schema and backend does not normalize. (Backend)

---

(Attach additional bug entries as you find them during manual testing)