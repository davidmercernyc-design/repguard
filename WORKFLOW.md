# RepGuard — Daily Operations Workflow

## How It Works

Every morning, a cron job triggers the agent to run the daily scan for all active clients. The agent produces a digest for each client, which Shant reviews and forwards.

---

## Daily Scan Process (Per Client)

### 1. Load Client Config
Read `clients/<client-name>.json` for platforms, brand voice, and last scan date.

### 2. Search for New Reviews
For each platform, search the web for recent reviews:
- `"<business name>" "<city>" site:yelp.com reviews`
- `"<business name>" "<city>" site:tripadvisor.com reviews`
- `"<business name>" "<city>" Google reviews`
- `"<business name>" "<city>" site:facebook.com reviews`

Also search for:
- `"<business name>" "<city>" reddit OR twitter OR tiktok`

### 3. Fetch & Analyze Reviews
- Pull review content from search snippets and accessible pages
- Categorize each review: ⭐ positive / ⚠️ negative / ❓ question / 😐 neutral
- Flag urgency: 🔴 urgent (1-2 stars, angry tone) / 🟡 normal / 🟢 positive

### 4. Draft Responses
For each review, draft a response using the client's brand voice:
- **Negative reviews**: Empathetic, acknowledge the issue, offer resolution, invite back
- **Positive reviews**: Thank them personally, mention something specific from their review
- **Questions**: Provide a helpful, complete answer
- **Neutral**: Brief thank you, invite them to return

### 5. Compile Digest
Generate a digest file at `digests/<client-name>/<YYYY-MM-DD>.md` with:
- Summary stats (new reviews count, sentiment breakdown)
- Each review with: platform, stars, text, draft response, urgency flag
- Recommended priority order (urgent first)

### 6. Update Client Config
Set `last_scan` to today's date.

---

## Weekly Report (Growth + Premium tiers)

Every Monday, generate a weekly summary:
- Total reviews received
- Sentiment trend (improving/declining/stable)
- Top complaints and praise themes
- Competitor mentions (if configured)
- Response rate improvement since starting service

Save to `reports/<client-name>/<YYYY-MM-DD>-weekly.md`

---

## Delivery

### Starter Plan
- Shant reviews digest, forwards to client via email

### Growth Plan
- Same as Starter + weekly report attached

### Premium Plan
- Same as Growth + Shant posts approved responses directly on platforms

---

## Onboarding a New Client

1. Copy `clients/TEMPLATE.json` → `clients/<client-name>.json`
2. Fill in all fields (get from client during onboarding call)
3. Run initial audit (same as free audit but saved to client folder)
4. Set `status` to `active`
5. Client will receive first digest the next morning

---

## File Structure

```
replypulse/
├── clients/
│   ├── TEMPLATE.json
│   └── <client-name>.json
├── digests/
│   └── <client-name>/
│       └── YYYY-MM-DD.md
├── reports/
│   └── <client-name>/
│       └── YYYY-MM-DD-weekly.md
├── audits/
│   └── <client-name>-audit.html
├── WORKFLOW.md
├── PROJECT.md
└── (website files)
```
