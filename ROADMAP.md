# integration-cookbook roadmap

Runnable, tested integration recipes plus the written artifacts a Solutions Engineer hands a
customer. Recipes run on HookScope. One checkbox = one commit with its tests (or, for
documents, a finished, reviewed page). Items tagged **[owner]** wait for the repo owner.

## Schedule (about 4 commits a day, shared with HookScope)

| Dates | Cookbook work |
|---|---|
| Oct 12 – Oct 25 | Scaffold, R1, R2 |
| Oct 26 – Nov 8 | R3, R4, R5 |
| Nov 9 – Nov 22 | R6, R7, R8 |
| Nov 23 – Dec 6 | R9, R10, SE documents D1–D8 |
| Dec 7 – Dec 25 | Buffer |

## What "done" means for a recipe (7 commits each)
Every recipe lives in `recipes/<name>/` and gets these, one commit each:
1. **Problem and architecture**: README opening, mermaid architecture and sequence diagrams
2. **Config**: HookScope rules and a `docker-compose.yml` that runs the whole recipe
3. **Core code**: the handler or transform, with typed, small functions
4. **Happy-path tests**: realistic, signed provider fixtures
5. **Failure-mode tests**: retries, duplicates, out-of-order events, target 429/500, replay after an outage
6. **Troubleshooting**: symptom, likely cause, how to check, fix; with real log and metric examples
7. **Customer pack**: FAQ, discovery questions, go-live checklist for this integration

## Scaffold (8)
- [x] S.01 Repo skeleton: README (what, why, who it is for), LICENSE, `.gitignore`
- [x] S.02 `recipes/_template/` with the seven-part README skeleton
- [ ] S.03 `common/` package: signed-payload fixtures for each provider
- [ ] S.04 `common/mocks`: Jira-, CRM- and warehouse-style mock servers with failure injection (500, slow, 429)
- [ ] S.05 Makefile (`make test RECIPE=...`) and a base compose file running HookScope
- [ ] S.06 CI: matrix over recipes, lint, tests
- [ ] S.07 Docs checks in CI: link checker and diagram syntax
- [ ] S.08 `CONTRIBUTING.md` with the recipe definition of done

## Recipes (10 x 7)
- [ ] R1 **stripe-invoice-paid-to-slack**: finance alert with a minimum-amount filter and a readable message
- [ ] R2 **github-pr-to-jira**: create and transition issues from pull request events; idempotent by PR id
- [ ] R3 **shopify-order-to-warehouse**: reshape orders into the warehouse schema; retries, dead letters, replay after an outage
- [ ] R4 **stripe-subscription-lifecycle-to-crm**: out-of-order events, a small state machine, idempotency
- [ ] R5 **twilio-sms-to-support-ticket**: form-encoded bodies and the proxy/URL signature pitfall
- [ ] R6 **webhook-to-queue-on-aws**: ack fast, process later (HookScope, SQS, worker); Terraform checked with `validate`, tests against LocalStack
- [ ] R7 **slack-slash-command-bot**: the 3-second rule, delayed responses via `response_url`, signature checks
- [ ] R8 **customer-rest-api-client**: OAuth2 client credentials, pagination, 429 backoff, idempotency keys
- [ ] R9 **bulk-csv-import**: partial-failure report, resumable runs, rate-aware batching
- [ ] R10 **trace-an-integration**: correlation ids end to end, a Grafana panel, an on-call runbook

(Per-recipe checklist: R1.1 problem/diagram, R1.2 config, R1.3 code, R1.4 tests, R1.5 failure tests, R1.6 troubleshooting, R1.7 customer pack; same for R2 to R10.)

## Solutions Engineer documents (8)
- [ ] D1 Integration design checklist
- [ ] D2 Discovery question bank by integration type (push, pull, batch)
- [ ] D3 Proof-of-concept plan and success-criteria template
- [ ] D4 Go-live and launch-readiness checklist
- [ ] D5 Troubleshooting decision tree ("the webhook never arrives", "it arrives but fails", "it works, then stops")
- [ ] D6 Answers to common security-questionnaire questions about webhooks and APIs
- [ ] D7 Sizing and rate-limit worksheet
- [ ] D8 Choosing push vs pull vs batch

## Notes
- Mock servers stand in for Jira, a CRM and a warehouse; the recipes say so plainly and never claim a vendor-specific certification.
- Anything that needs real cloud credentials is validated (`terraform validate`, LocalStack), never applied.
- No recipe ships without failure-mode tests; that is what makes it useful to a customer.
