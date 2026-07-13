# scrummy

AI scrum copilot. GitHub webhooks feed an event pipeline that tracks commits, PRs, reviews, and issue updates per engineer; an LLM turns each person's activity into a short standup summary; a Slack bot posts the team digest every morning. Nobody repeats what they did yesterday out loud ever again.

On top of that: sprint analytics (PR review latency, commit velocity, blocker detection from stale PRs and reopened issues) and an agent that can act - move tickets, nudge reviewers, update issue status - with approvals, not autonomously.

## why

Standups at both of my internships were 15 minutes of people reading their GitHub activity out loud. All of that information is already structured and sitting in webhooks. The interesting engineering is everything around the summary: multi-source ingestion, attribution (matching commits to the human, not the bot), and knowing when a summary is wrong.

## architecture

1. ingest: GitHub webhooks -> verified -> event store (append-only, replayable)
2. summarize: nightly job groups events per engineer, LLM writes the standup entry with links
3. deliver: Slack bot posts the digest; per-person DM option
4. analytics: review latency, velocity trends, blocker heuristics -> small dashboard
5. act: agent tools for issue updates and review nudges, human-approved via Slack buttons

## the eval part

Summaries get scored against ground truth: does every claim in the summary trace to a real event? A hallucinated "shipped the auth refactor" in a standup bot is worse than no bot. Claim-level attribution checking runs on every summary before it posts.

See docs/PLAN.md. Status: early.
