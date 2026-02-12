# SOP Adviser — Beta Test

AI-guided Statement of Purpose workshop for UK university applications.

## Architecture

```
┌──────────────────┐      ┌─────────────────────┐      ┌──────────┐
│  GitHub Pages     │ ───► │  Cloudflare Worker   │ ───► │  OpenAI  │
│  (index.html)     │      │  (adds API key)      │      │  GPT-4o  │
│  User's browser   │ ◄─── │  sop-adviser-proxy   │ ◄─── │          │
└──────────────────┘      └─────────────────────┘      └──────────┘
```

- **Your API key never leaves Cloudflare** — users can't see it
- **No backend server needed** — Worker runs on Cloudflare's free tier
- **Nothing stored** — conversation lives in browser memory only

## Setup (15 minutes total)

### Step 1: Deploy the Cloudflare Worker (5 min)

1. Sign up / log in at [dash.cloudflare.com](https://dash.cloudflare.com)
2. Go to **Workers & Pages** → **Create Application** → **Create Worker**
3. Name it `sop-adviser-proxy` → click **Deploy**
4. Click **Edit Code**
5. Delete the default code, paste the entire contents of `worker.js`
6. Click **Save and Deploy**
7. Go to **Settings** → **Variables and Secrets**
8. Click **Add** → Name: `OPENAI_API_KEY` → Value: your `sk-...` key → click **Encrypt**
9. Note your Worker URL (e.g. `https://sop-adviser-proxy.your-name.workers.dev`)

### Step 2: Update index.html (1 min)

Open `index.html` and find this line near the top of the `<script>` section:

```javascript
const WORKER_URL = 'https://sop-adviser-proxy.YOUR-SUBDOMAIN.workers.dev';
```

Replace it with your actual Worker URL from Step 1.

### Step 3: Deploy to GitHub Pages (5 min)

1. Create a new GitHub repo (e.g. `sop-adviser`)
2. Upload `index.html` and `README.md`
3. Go to **Settings** → **Pages** → Source: **Deploy from a branch** → **main** / **root**
4. Wait ~60 seconds, your site will be live at `https://yourusername.github.io/sop-adviser/`

### Step 4: Share with testers

Send them the GitHub Pages URL. That's it — no API key needed, no install, works on phone and desktop.

## Cost control

Set a spending limit on your OpenAI account:
1. Go to [platform.openai.com/settings/organization/limits](https://platform.openai.com/settings/organization/limits)
2. Set a monthly budget (£20 is plenty for 20 testers)

**Cost per session:** ~$0.30–0.60 (20–30 exchanges at GPT-4o pricing)
**20 testers:** ~$6–12 total

## Lock down later (optional)

For the beta test, the Worker allows requests from any origin. To restrict it to your site only, edit the Worker and change:

```javascript
'Access-Control-Allow-Origin': '*'
```

to:

```javascript
'Access-Control-Allow-Origin': 'https://yourusername.github.io'
```

## Files

```
index.html    — Complete application (single file)
worker.js     — Cloudflare Worker proxy (deploy separately)
README.md     — This file
```

## What to watch during testing

- Does the AI follow the 9-phase structure or skip phases?
- Do the motivation framing interpretations (Phase 2.5) feel accurate?
- Is the final SOP authentically in the student's voice?
- Session length? (Target: 30–45 minutes)
- Where do students get stuck or confused?
- Does the export capture everything needed for an audit trail?

## License

Proprietary — One Education Consulting Co. Ltd.

