# Worked workflows

IDs below (`665f...`) are placeholders. Replace them with IDs from tool results.

## 1. One post, three platforms, one time

1. `repping_read {domain:"channels", operation:"list", input:{view:"postable"}}`
   (check the schema with `repping_discover` if the input shape differs).
2. `repping_upload_media` for the local video. Note the media ID.
3. `repping_read {domain:"publishing", operation:"target_settings", input:{...platform...}}`
   for each platform.
4. Prepare:
```json
{
  "mode": "publisher",
  "requestId": "<new uuid v4>",
  "input": {
    "type": "schedule",
    "caption": "Base caption used when a target has none",
    "mediaIds": ["665f1c2e9b1e8a0012345678"],
    "scheduledAt": "2026-12-01T18:30:00-05:00",
    "timezone": "America/New_York",
    "targets": [
      {"channelId": "665f...01", "caption": "IG hook line\n\nBody...", "meta": {}},
      {"channelId": "665f...02", "caption": "Short X version", "meta": {}},
      {"channelId": "665f...03", "meta": {}}
    ]
  }
}
```
5. Give the user the review link. Say nothing is scheduled until they approve.

## 2. A week of content (bulk)

Use `publishing.create_batch` (≤50 items). One item per post time.
```json
{
  "mode": "product",
  "domain": "publishing",
  "operation": "create_batch",
  "requestId": "<new uuid v4>",
  "input": {
    "items": [
      {"caption": "Mon post", "mediaIds": [], "scheduledAt": "2026-12-01T09:00:00+05:30", "timezone": "Asia/Kolkata",
       "targets": [{"channelId": "665f...01"}, {"channelId": "665f...04"}]},
      {"caption": "Wed post", "scheduledAt": "2026-12-03T09:00:00+05:30", "timezone": "Asia/Kolkata",
       "targets": [{"channelId": "665f...01"}]}
    ]
  }
}
```
Before you prepare, show the user a calendar table (date, time, accounts, caption start)
and get a yes.

## 3. Best-time scheduling
1. `analytics.best_times` for the account. Note sample size and confidence.
2. If confidence is low, say these are starter suggestions, not observed performance.
3. Or use `publishing.find_slot` to get the next free queue slot.

## 4. Fix a failed post
1. `publishing.list_posts` with a failed status filter.
2. `publishing.get_post` → read target errors.
3. If the cause is fixed (for example account reconnected), prepare `publishing.retry_target`.
4. If the account needs reconnect, prepare `channels.reconnect` and share the link.

## 5. Lost response
Call `repping_action_status` with the saved requestId. If no record exists, resend the
exact same input with the same requestId.
