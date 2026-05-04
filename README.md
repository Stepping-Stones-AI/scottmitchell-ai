# Business Booster Landing Page

Static, single-page landing site for the 12 May 2026 Business Booster talk.

**Target URL:** `https://scottmitchell.ai` (root domain)

## What's in here

- `index.html` -- the whole page in one file. Tailwind via CDN, Poppins via Google Fonts, SSAI brand colours.
- `vercel.json` -- minimal Vercel config (clean URLs, basic security headers).

## Sections

1. **Hero** -- "Welcome, Business Booster attendee" pill + headline + two CTAs.
2. **Video walkthrough** -- placeholder video tile + email capture for "notify me when it's live".
3. **Booking** -- embedded Google Calendar (same one as the SSAI website).
4. **Footer** -- name, email, link back to steppingstonesai.co.uk.

## Before deploying -- two swaps to make

### 1. Email form endpoint

Open `index.html`, find:

```html
action="REPLACE_WITH_FORMSPREE_ENDPOINT"
```

Replace with a real form endpoint. Fastest options:

- **Formspree** (free for 50 submissions/month): sign up at formspree.io, create a form, copy the endpoint (looks like `https://formspree.io/f/abcdwxyz`).
- **Tally** or **Web3Forms** also work the same way.

While the placeholder is in place, the form will pop an alert instead of submitting -- so it's safe to deploy without breaking anything.

### 2. Video embed (when the YouTube video is recorded)

Find this block in `index.html`:

```html
<div class="absolute inset-0 flex flex-col items-center justify-center text-center text-white p-8">
  ... placeholder content ...
</div>
```

Replace with:

```html
<iframe
  class="h-full w-full"
  src="https://www.youtube.com/embed/YOUR_VIDEO_ID"
  title="Your First Conversation with Claude"
  frameborder="0"
  allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  allowfullscreen
></iframe>
```

The QR code and URL stay the same forever -- the page content can swap underneath.

## Deploy to Vercel (5 min)

### Option A: Vercel CLI (fastest)

```bash
cd "outputs/marketing/speech-prep/landing-page"
npx vercel
```

Follow the prompts:
- Set up and deploy? **Y**
- Scope: your account
- Link to existing project? **N**
- Project name: `scottmitchell-ai` (or whatever you want)
- In which directory is your code? **.**
- Override settings? **N**

This gives you a `*.vercel.app` URL immediately. Then:

```bash
npx vercel --prod
```

To attach the custom domain `scottmitchell.ai`:
1. In the Vercel dashboard for the project, go to **Settings -> Domains**.
2. Add `scottmitchell.ai` (and `www.scottmitchell.ai` if you bought it).
3. Vercel shows the DNS records you need at your registrar (A record + CNAME).
4. Add them at the registrar (Namecheap, GoDaddy, etc.). DNS usually propagates in 5-30 mins.

### Option B: Vercel dashboard (no CLI)

1. Push this folder to a GitHub repo (e.g. `scottmitchell-ai-landing`).
2. On vercel.com -> Add New -> Project -> Import the repo.
3. Framework: **Other**. Build command: leave blank. Output dir: `./`.
4. Deploy. Then add the custom domain as in Option A step 3-4.

## QR code

The QR code in `../qr-code/` points at `https://scottmitchell.ai` (root). The page can change content forever -- the URL stays locked.

## Iteration plan after the talk

- Replace the placeholder video tile with the real YouTube embed.
- Wire up the email form to a real endpoint and pipe submissions into a CRM (Supabase or whatever).
- Add a thank-you page after form submission.
- Add a tiny analytics snippet (Plausible, Vercel Analytics, or Google Analytics) so you can see how many of the 72 attendees actually scanned + booked.
