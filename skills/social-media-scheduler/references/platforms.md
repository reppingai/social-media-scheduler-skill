# Platform support (Repping, checked 2026-11)

This table is a guide. The live server is the truth: read `publishing.target_settings`
for exact `meta` keys, allowed values and media limits before you send settings.
Only `channels.list` (postable view) tells you which accounts this user connected.

| Platform | What you can schedule | Notes |
| --- | --- | --- |
| Instagram | Feed image, feed video, carousel (≤10), Reel (incl. trial Reels), Story | First comment is best-effort, not on Stories. Trending Reel audio via `ig_trending_audio` (Facebook-Login IG accounts only). |
| TikTok | One video, Direct Post | Privacy, Duet, Stitch, comments, branded-content and AI-label settings. Upload-to-drafts is not supported. |
| YouTube | Video and Shorts | Title, description, tags, category, privacy, made-for-kids, thumbnail, playlist, SRT captions. Uploaded at the scheduled time. First comment not used. |
| X (Twitter) | Text, ≤4 images or 1 video | Reply settings, Communities. Thread parts post as separate staggered posts, not a reply chain. |
| LinkedIn | Member text, ≤9 images, video | Personal profile; company page selection is not available. |
| Facebook | Page text, link, photo, ≤10 photos, video | Optional first comment. |
| Threads | Text, image, video, carousel (≤20) | Optional first reply. |
| Pinterest | Image, two-image, video Pin with cover | Board chosen per Pin. |
| Bluesky | Text with ≤4 images and alt text | Video is not posted. |
| Mastodon | Status with ≤4 media | Visibility, content warning, first reply. |
| Telegram | Text, photo, video, media group (≤10) | Silent send option. |
| Discord | Webhook text with ≤10 attachments | |
| Reddit | Self and link posts in a subreddit | Images are submitted as links. Optional first comment. |
| Slack, VK, Lemmy | Text/message posts | |
| Dev.to, Hashnode, Medium, WordPress | Articles | Long-form publishing. |

## Media rules of thumb
- Vertical 9:16 video for Reels, TikTok, Shorts, Stories.
- Upload once with `repping_upload_media`, then reuse the same media ID across targets.
- Attach only media whose processing is ready.

## Analytics coverage
Post and account analytics depend on the platform and connection. Call
`analytics.accounts` to see each account's collection status. Unsupported is not zero.
