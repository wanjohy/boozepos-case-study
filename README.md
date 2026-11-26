# BoozePOS: multi-branch POS, stock & credit SaaS

> **Case study.** BoozePOS is a commercial product by Topend IT Solutions, so its source code is private. This repository describes the problem, the design and my role. It contains no proprietary code.

**Role:** Full-stack engineer. I designed and built the product end to end: data model, APIs, front end, payments integration, deployment and testing.
**Status:** In production, used daily by liquor stores, wines & spirits shops and bars in Kenya.

**Stack:** Python · Django REST Framework · Vue 3 · TypeScript · Tailwind CSS · PostgreSQL · AWS (ECS/ECR, RDS, S3) · Docker · GitHub Actions · M-Pesa Daraja

---

## The problem

General shop software misses the problems that cost liquor businesses money:

- **Missing stock:** bottles disappear and nobody knows where or when.
- **Unpaid credit:** sales on credit go unrecorded or are never collected.
- **Cash and M-Pesa don't match** what should be in the drawer or on the till.
- **Different prices per branch** are hard to manage.
- **Profit is unclear** because expenses live somewhere else.
- **The owner can't be in every branch** at once.

## What I built

### Sales & checkout
- Fast product search and checkout, with each sale marked **Cash, M-Pesa or Credit**
- M-Pesa Daraja integration (STK Push, callbacks, verification) handling **1,000+ transactions a month**
- End-of-shift summary per cashier

### Stock control
- Tracking by product and bottle size
- Stock takes, transfers between branches, low-stock alerts
- **Stock "as at" any past date**, rebuilt from the movement history

### Customer credit
- Every credit sale recorded against a customer
- Repayments, balances and **overdue tracking** against agreed payment dates

### Owner & branch dashboards
- **Owner view:** business-wide sales, payment mix, money owed, overdue debts, low stock and a per-branch summary
- **Cashier view:** scoped to their own branch, with one-tap actions
- **Role-based access control:** business-wide figures and settings stay with the owner

### Pricing & reporting
- Per-branch pricing for the same product, managed from one place
- Expenses recorded alongside sales, giving a real **Profit & Loss** report

### Works anywhere
- Responsive, mobile-first interface on any phone, tablet or computer, with no special hardware

## Architecture

```
 Browser (Vue 3 + TypeScript, mobile-first)
        │  HTTPS / JSON (token auth)
        ▼
 Django REST Framework API  ──►  PostgreSQL (sales, stock movements, credit ledger)
        │
        ├──► M-Pesa Daraja (STK Push → callback → verification)
        └──► Background jobs (reports, reconciliation)

 Deployment: Docker images → AWS ECR → ECS · RDS PostgreSQL · CI/CD with GitHub Actions
```

## Engineering highlights

- **Ledger-style data model.** Stock and credit are recorded as movements, not overwritten balances. That keeps history auditable and makes "as at date" reports possible.
- **Tenant and branch scoping** is enforced in the API layer, so a cashier can never read another branch's data.
- **Payment reliability:** M-Pesa callbacks are idempotent and verified before a sale is marked paid.
- **Performance:** query profiling and indexing cut slow report and dashboard queries noticeably.
- **Quality:** unit and functional tests, code review, and documented APIs.

## Screenshots

_Screenshots and a short demo video can be shared on request._

---

📫 Bernard Wanjohi Gitahi · [Portfolio](https://bernardwanjohi.web.app) · bernardgitahi2018@gmail.com
