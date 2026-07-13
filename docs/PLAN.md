# build plan

## phase 1 - ingestion
- [ ] webhook receiver (FastAPI) with signature verification
- [ ] event store: append-only table, raw payload + normalized event row
- [ ] event types: push, pull_request, pull_request_review, issues, issue_comment
- [ ] identity resolution: map commit emails + GitHub logins to one engineer record
- [ ] replay tooling: rebuild derived state from raw events

## phase 2 - summarization
- [ ] nightly job: group last 24h of events per engineer
- [ ] prompt: terse standup voice, every claim must carry an event link
- [ ] claim checker: parse summary claims, verify each maps to a real event, block on failure
- [ ] "nothing shipped" is a valid summary - no padding

## phase 3 - slack delivery
- [ ] bot posts team digest at configured time, threads per engineer
- [ ] per-person DM opt-in
- [ ] blocker section: PRs stale > 24h, review requests unanswered

## phase 4 - analytics
- [ ] PR review latency (request -> first review), p50/p90 weekly
- [ ] commit velocity trends per repo
- [ ] reopened-issue and force-push counts as smell indicators
- [ ] small dashboard (one page, no framework circus)

## phase 5 - write actions
- [ ] agent tools: comment on issue, add label, request reviewer, move project card
- [ ] every action goes through a Slack approval button first
- [ ] audit log of every action taken and who approved it

## phase 6 - evals
- [ ] golden set: 30 hand-written event bundles with expected summaries
- [ ] hallucination rate = claims without matching events / total claims. target: zero
- [ ] regression run before any prompt change merges

## decisions
- append-only event store so everything is replayable and debuggable
- summaries verified before posting, not after someone complains
- write actions human-approved. an unsupervised bot editing tickets erodes trust in one bad day
