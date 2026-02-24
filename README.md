# 🚀 QA Hiring Challenge  
## Voucher Discount System – Incident & Testing Assessment

Welcome 👋  

Thank you for your interest in joining our team.  
This challenge is designed to evaluate how you think, not just what you know.

We are looking for candidates who can:
- Analyze production issues logically
- Design strong test scenarios
- Understand system risks (transactions, concurrency, monitoring)
- Communicate clearly in structured documentation

---

# 🏢 Context

e-commerce platform provides a **Voucher Discount** feature during checkout.

Recently, multiple production issues were reported related to vouchers.  
You are assigned as the QA responsible for investigating and improving this feature.

---

# 📜 Business Rules

- Voucher is valid only for registered users.
- Voucher has:
  - Minimum purchase requirement
  - Expiration date
  - Total usage quota (e.g., 1000 total uses)
- Voucher can only be used once per user.
- When user clicks **Apply Voucher** → system validates eligibility.
- When order is created → a record is inserted into `voucher_usage` with status = `PENDING`.
- If payment is successful → status updated to `SUCCESS`.
- If payment fails or is cancelled → status updated to `FAILED`.
- Voucher quota should count **only SUCCESS transactions**.

---

# 🚨 Production Incident Report (Last 7 Days)

The following issues were reported in production.

---

## Issue 1 – Cannot Use Voucher

- Users cannot use voucher `VC-001`.
- System shows: *“Voucher already used.”*
- Database shows `voucher_usage` record with status = `PENDING`.
- Users claim their previous payment failed.

---

## Issue 2 – Voucher Deducted but Payment Failed

- Payment failed due to gateway timeout.
- Voucher quota decreased.
- `voucher_usage` record remains `PENDING` for more than 2 hours.
- User cannot reuse voucher.

---

## Issue 3 – Quota Exceeded During Flash Sale

- Voucher quota = 1000
- SUCCESS transactions recorded = 1002
- Two users successfully completed payment after quota should have been exhausted.
- Requests were sent nearly at the same time (high concurrency).

---

# 🗄 Database Reference

### `voucher_usage`

```
id | user_id | voucher_code | order_id | status | created_at
```

Status values:
- `PENDING`
- `SUCCESS`
- `FAILED`

---

# 🧠 Your Tasks

## Section A – Incident Analysis

For each reported issue:

1. What are the possible root causes?
2. What data would you check to validate your hypothesis?
3. Which issue indicates:
   - A transaction handling problem?
   - A concurrency/race condition problem?
   - A frontend/backend inconsistency?
4. Rank the issues by business severity and explain your reasoning.
5. If you confirm this is a system bug, create a proper bug ticket.

---

## Section B – Test Design

1. List at least **12 test scenarios** (functional + edge cases).
2. What non-functional testing is required?
3. How would you test quota exhaustion under high concurrency?
4. How would you validate correct rollback behavior when payment fails?
5. What negative test cases would you design?

---

## Section C – Bug Investigation Scenario

A user claims:

> “I only checked out once.”

Database shows:

```
id | user_id | voucher_code | order_id | status   | created_at
---------------------------------------------------------------------------
1  | UID-101 | VC-001       | OID-555  | PENDING  | 2026-02-20 10:15:03
2  | UID-101 | VC-001       | OID-556  | SUCCESS  | 2026-02-20 10:15:05
```

Answer the following:

1. What are the possible causes?
2. What additional data/logs would you request from the developer team?
3. Could this be caused by:
   - Double-click?
   - Network retry?
   - Payment webhook duplication?
4. Is this more likely a system bug or user behavior? Explain.

---

## Section D – Monitoring & False Positive Case

Monitoring Alert:

```
ALERT: Voucher VC-003 overused
Threshold: usage > quota
Detected usage: 1002
Quota: 1000
```

After investigation:

- SUCCESS transactions: 998
- Remaining records are `PENDING` or `FAILED`
- No SUCCESS transactions exceeded quota

Questions:

1. Is this a real issue or a false positive? Explain.
2. Why was the alert triggered?
3. What metric was incorrectly designed?
4. What query would you run to validate real usage?


---

# 📦 Submission Guidelines

Please:
Submit your answers in a well-structured document (Docs, PDF or Markdown).

Include:

  - Clear reasoning and analysis
  - Any assumptions you make
  - Structured sections matching the challenge

Ensure your answers are organized and easy to read.

📅 Deadline: 3 days from receiving this assignment

Send to: trinda@yoripe.com

---


We are not looking for “perfect answers”.  
We are looking for **how you think.**

Good luck 🚀  
We look forward to reviewing your work.

Weyoco Team
