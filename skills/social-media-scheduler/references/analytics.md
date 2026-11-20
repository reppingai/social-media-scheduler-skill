# Cross-channel analytics report

## Reads
1. `analytics.social_overview` — totals and per-account summary.
2. `analytics.top_posts` — top and bottom posts by a metric (engagement rate, saves, shares, watch time).
3. `analytics.account` — follower and engagement series for one account.
4. `analytics.best_times` and `analytics.recommendations`.

Read schemas first with `repping_discover {domain:"analytics", operation:"..."}`.

## Rules
- `null` = not collected. Never report it as 0. Never average it in.
- State the date range, the timezone, and data freshness.
- Compare like with like (Reels vs Reels, not Reels vs Stories).
- Small samples: say so. Do not invent trends from 3 posts.

## Report template
```
# Social performance — <range>
## Summary (3 bullets)
## By platform (table: platform, posts, reach/views, engagement rate, followers Δ)
## Top 5 posts (what worked and why)
## Bottom 5 posts (what to change)
## Best posting times (with confidence)
## Next 7 days: 3 concrete actions
```
Offer to turn the actions into a scheduled batch (workflow 2).
