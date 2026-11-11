# Session Compaction

## Current target (2026-11-11)

- Run id: 42952380212 (schedule); resumed same target hash
  `851361f8840b1efe993289fa5efa40d815c25b00e22682335fd394ab32253ba7`.
- Authorized target URLs from `data/targets.txt`:
  - `https://console.byteplus.com/finance/overview`
  - `https://console.byteplus.com/workorder/list/`
  - Task: enumerate/test all sub-routes under these; also test related
    in-scope subdomains/endpoints if interesting.
- Status: in progress (scan not complete). `result/SCAN_COMPLETE` NOT written.

## Authentication status

- Root login at `https://signin.byteplus.com/auth/login/` succeeded with test
  Account A after solving the BytePlus slider captcha via trusted CDP input
  and an in-page normalized-cross-correlation puzzle solver.
- Cookies for Account A saved to `creds/auth-headers.txt` (never print them).
- Account B (`testt456`) not yet logged in; requires a separate isolated
  session/captcha solve. Needed for cross-account IDOR proof.
- Test account A AccountID observed in prior run: `3000823185`; B: `3001021825`
  (historical).

## Scope decisions

- `console.byteplus.com` and its auth subdomain `signin.byteplus.com` are the
  in-scope console/auth surface; login redirect confirmed between them.
- Other `byteplus.com` subdomains remain `Scope Unverified` until a concrete
  endpoint is discovered and checked. Do not blanket-test.
- Captcha/verification service is on `verify.rmc.byteplusbiz.com`; it is a
  third-party anti-bot domain and is NOT tested directly (out of scope).

## Discovered surface (finance)

- Top-level route `/finance/overview` (page loaded, account A).
- Finance sidebar sub-routes observed: Overview, Bill management, Order
  management, Renewal management, Unsubscription management, Instance
  management, Resource package.
- Finance APIs observed on `/finance/overview` (all under
  `https://console.byteplus.com`):
  - GET `/api/top/account_verify/ap-southeast-1/2018-01-01/GetProfileInfo`
  - GET `/api/financial/const/getExpenseConfigForFE`
  - GET `/api/top/bill_volcano_engine/ap-singapore-1/2020-01-01/GetAccountGrayStatus`
  - GET `/api/top/bill_volcano_engine/ap-singapore-1/2022-01-01/GetCountryAreaCfg`
  - POST `/api/top/account_verify/ap-southeast-1/2018-01-01/GetRawProfileInfo`
  - POST `/api/top/bill/ap-singapore-1/2020-01-01/ListAccountFinancialRole`
  - POST `/api/top/bill/ap-singapore-1/2020-01-01/GetBalanceFromTradeBalance`
  - GET `/api/top/bill/ap-singapore-1/2020-01-01/CountCoupon`
  - GET `/api/top/bill/ap-singapore-1/2020-01-01/GetQuotaAcctInfo`
  - GET `/api/top/bill/ap-singapore-1/2020-01-01/GetCurrency`
  - GET `/api/top/bill/ap-singapore-1/2020-01-01/GetBcDiscountPriceAuth`
  - GET `/api/top/bill/ap-singapore-1/2022-01-01/ListMonthlyBillOpen`
  - POST `/api/financial/trade/HasArrearsAlert`
  - POST `/api/financial/qualification/GetWhitelistShortTermProducts`
  - GET `/api/top/consolemessage/ap-singapore-1/2024-05-01/GetInMailUnreadCount`
  - POST `/api/top/consolemessage/ap-singapore-1/2024-05-01/ListInMail`
  - POST `/api/top/ticket/ap-southeast-1/2021-01-01/GetTicketCountForUser`
  - POST `/api/top/account_management/ap-southeast-1/2018-01-01/GetAccountLoginSecurity`
  - GET `/console.byteplus.com/signin/status/identity`
- Static JS (not saved): `res.gcloudcache.com/bp-fe/finance/volcf-sdk-app/...`
  and `byteplus_volcf_navbar/...`.

## Tested

- Login/auth flow: completed with Account A.
- No API vulnerability confirmed yet.

## Pending / next

1. Enumerate finance sub-route URLs and their unique APIs; record in
   `result/inventory/api-routes.md`.
2. Test finance APIs for IDOR/BOLA, BAC, mass assignment, PII leakage using
   account A (and B when available).
3. Explore `/workorder/list/`, enumerate ticket APIs; test IDOR/BOLA on
   ticket IDs/attachments and PII leakage.
4. Client-side regex/source-sink review for each sub-route if time permits.
5. Log in Account B in an isolated context for cross-account proof.

## Important observations / blockers

- Slider captcha on login must be solved; challenge expires quickly and
  repeated stale verifies return `NotFoundChallengeId`. Solver succeeded once.
- The test account has no billing account set up ("Billing account not setup
  yet"), so many finance reads may be empty; ticket/workorder data may be
  richer.
- Keep every finding evidence-backed; do not report status codes or inert
  fields.

## Historical sessions (previous target: /datarangers, /datatester, /ark, /ai, /alb)

- Completed scan reported low-impact read-only cross-project IDOR
  `VULN-20261031-001` in DataFinder `common_settings` (benign presentation
  settings). Do not repeat completed work for those routes.
- Prior workflow closeouts were failures/skips; agent-created compaction is
  authoritative.

## Workflow closeout

- timestamp: 2026-11-11T03:46:51Z
- run_id: 42952380212
- event: schedule
- agent_outcome: failure
- workflow_status: failure
- note: Agent-created compaction is authoritative; this closeout records how the run ended.

## Workflow closeout

- timestamp: 2026-11-11T03:48:37Z
- run_id: 42953103503
- event: workflow_dispatch
- agent_outcome: skipped
- workflow_status: failure
- note: Agent-created compaction is authoritative; this closeout records how the run ended.

## Workflow closeout

- timestamp: 2026-11-11T13:03:57Z
- run_id: 43025501412
- event: schedule
- agent_outcome: skipped
- workflow_status: failure
- note: Agent-created compaction is authoritative; this closeout records how the run ended.

## Workflow closeout

- timestamp: 2026-11-11T23:47:30Z
- run_id: 43122267757
- event: schedule
- agent_outcome: skipped
- workflow_status: failure
- note: Agent-created compaction is authoritative; this closeout records how the run ended.
