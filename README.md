# Courtly — Deployment Guide

## Files in this folder
- `index.html` — The entire Courtly application (single file)
- `netlify.toml` — Netlify routing config (required)

## How to go live on Netlify

### Option A: Drag & Drop (Easiest — 60 seconds)
1. Go to https://netlify.com and create a free account
2. From your dashboard, click **"Add new site" → "Deploy manually"**
3. Drag and drop this entire `courtly-netlify` folder onto the Netlify deploy zone
4. Your site goes live instantly at a URL like `https://random-name.netlify.app`
5. To use a custom domain (e.g. courtly.com): go to **Site settings → Domain management → Add custom domain**

### Option B: GitHub (Best for ongoing updates)
1. Create a GitHub account at https://github.com
2. Create a new repository called `courtly`
3. Upload both files (`index.html` and `netlify.toml`)
4. Go to Netlify → **"Add new site" → "Import an existing project"**
5. Connect your GitHub repo
6. Every time you push an update to GitHub, Netlify auto-deploys it

---

## Before going live — replace these placeholders

### 1. Stripe (Payments)
In `index.html`, find this line:
```
stripeInst = Stripe('pk_test_placeholder_replace_with_your_key');
```
Replace with your real Stripe **publishable key** from https://dashboard.stripe.com/apikeys

> ⚠️ You also need a backend to create PaymentIntents. Options:
> - Netlify Functions (serverless, free tier)
> - Supabase Edge Functions (free tier)
> - A simple Node.js/Express server

### 2. Email & SMS notifications
Currently simulated via `console.log`. To send real emails and SMS:
- **Email**: Sign up at https://resend.com (free tier: 3,000 emails/month)
- **SMS**: Sign up at https://twilio.com (pay-as-you-go, ~$0.01/SMS)
- Both have simple REST APIs you can call from a Netlify Function

### 3. Admin credentials
Default admin login:
- Email: `admin@courtly.com`
- Password: `admin123`

**Change this before going live.** In `index.html`, find:
```javascript
if(email==='admin@courtly.com'&&pass==='admin123')
```
Replace with your own credentials or connect to a real auth system.

---

## Important note on data storage
Courtly v5 uses **browser localStorage** — data is stored per device/browser.
This is fine for testing but means:
- Different users on different devices cannot see each other's data
- If a user clears their browser, their data is gone

**To make it a real shared database**, connect Supabase (free tier):
1. Create a project at https://supabase.com
2. Replace all `localStorage` calls with Supabase queries
3. Enable Supabase Auth for proper user authentication

---

## Test accounts to verify everything works

| Role | Email | Password |
|------|-------|----------|
| Admin | admin@courtly.com | admin123 |
| Register as Player | Any email | Any password (6+ chars) |
| Register as Owner | Any email | Any password (6+ chars) |

**Test payment card (Stripe test mode):**
- Success: `4242 4242 4242 4242`
- Decline: `4000 0000 0000 0002`
- Any future expiry date, any 3-digit CVC

---

## What's included in this version (v5)

✅ Home page with hero search (location, sport, date)
✅ Courts listing with sport filter, sort, search
✅ Court detail pages with 7-day availability calendar
✅ Court photo galleries with lightbox
✅ 4-step booking flow with Stripe payment UI
✅ My Bookings (upcoming, past, cancelled)
✅ Booking receipts (printable)
✅ Booking reminders (12hr & 1hr — email + SMS)
✅ Reviews system (star rating + written review + photo upload)
✅ Owner replies to reviews
✅ Waitlist for fully booked courts
✅ Favourites
✅ Player profiles with avatar, favourite sports, booking history
✅ Password reset flow with 6-digit code
✅ Court owner portal with earnings chart, booking calendar
✅ Recurring weekly availability + specific date blocking
✅ Payout requests with bank details
✅ Admin dashboard — bookings, courts, users, approvals, disputes, payouts, featured
✅ Platform fee (configurable in admin)
✅ Featured court listings
✅ Dispute management
✅ CSV export of all bookings
✅ Fully mobile responsive

---

Good luck with the launch! 🎾
