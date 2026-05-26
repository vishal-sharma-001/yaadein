# Yaadein — Master Design & Content Plan
> One document. Every screen. Every section. Every word. This is the source of truth.
> Last updated: May 2026

---

# THE PRODUCT IN ONE SENTENCE

Every photo from every guest and the photographer, in one album, organized by ceremony, built for Indian weddings.

---

# THE ONLY STORY THAT MATTERS

The wedding photo market splits into two separate tools: apps that deliver the photographer's shots to the couple, and apps that collect guest photos. Nobody has put them together. Yaadein is the first. That is the only differentiator we have and it is enough.

Competitors:
- **Samaro, FotoOwl, Photomall** — photographer-to-couple delivery only. Photographers already know these. They do not collect guest photos.
- **Wedibox, GuestCam, Kululu** — guest collection only. Do not connect to photographer delivery.

Nobody bridges both. Every design and copy decision should reinforce this one idea.

---

# PRIMARY AUDIENCE

**Photographers first. Planners second. Couples last.**

This is not about who the product is for. It is about who to reach first during validation.

**Why photographers first:**
One photographer does 30-50 weddings a year. If they adopt Yaadein, 30-50 couples get it automatically. Three photographers equal the reach of 150 direct couple DMs. This is the highest-leverage audience.

**Why planners second:**
Planners are trusted advisors. They recommend tools to couples and coordinate photographers. When a planner recommends Yaadein, couples use it. Planners also ask the photographer to set it up, which creates adoption from both sides.

**Why couples last:**
Couples are the end users but the hardest to reach cold. They have no professional accounts, no searchable category. Best reached through photographers and planners who already have their trust.

---

# TONE

## The one rule

Write as a fellow wedding professional who built the tool they always wished existed. Not a startup pitching to customers.

## What this means in practice

**Peer voice, not sales voice.**
"You know how clients always ask about the guest photos after the wedding. Now you have the answer." Not "Our platform enables seamless multi-source media aggregation."

**Outcome first, feature second.**
"Your clients receive the complete wedding." Not "Upload photos via QR code to our shared album."

**Competitive, not aggressive.**
State the fact once. "No other photographer in India offers this right now." Then move on. Do not repeat it in every section.

**India-specific language.**
Reference Haldi, Baraat, Sangeet, WhatsApp, the chacha at the baraat. Show you understand the market. Do not write generic wedding copy.

**Drop the poetic/emotional register for professionals.**
"Every tear, every dance" is for couples. Photographers and planners want edge and outcome. Reserve emotional language for the couple-facing sections only.

## The one test for every line of copy

Can a planner replace "photographer" with "planner" and have the sentence still make sense? If yes, the copy is in the right zone. If it only works for one audience, rewrite it or move it to an audience-specific section.

## Words that fit this tone
offer, deliver, edge, first, only, clients, package, add-on, complete, organized, zero extra work

## Words that break this tone
precious, memories, beautiful, forever, cherish, stunning (these are for couples, not professionals)

---

# BUSINESS MODEL

## Pricing (internal only — not on the website)

**Target model:** Monthly subscription per photographer or planner.

**Why subscription, not per-event:**
Photographers do 30-50 weddings a year. Per-event pricing creates friction every time they book a client. Subscription means they pay once and use it freely.

**Early access pricing:**
- ₹1,999/month per photographer (early adopter rate)
- ₹999/month per planner (they set it up but do not shoot)

**Cost structure:**
- Basic product (QR upload, gallery, download): near zero. Free tiers on Cloudinary or Firebase handle the first 50 weddings.
- AI reel per event: ₹150-400 depending on service
- Face recognition per event: ₹40-80 via AWS Rekognition
- **Do not build AI features until someone has paid.**

**At scale target:**
50 photographers × ₹1,999 = ₹99,950/month. Covers infrastructure with margin to invest in AI features.

## Validation goal

10 people who genuinely want this before any real backend is built. An email is not validation. A photographer using it at a real wedding and a couple saying "this is exactly what I wanted" is validation.

## Build order

1. Working QR upload (guests actually upload, not demo)
2. Real gallery (photos appear, couple can view and download)
3. Photographer private link (same album)
4. WhatsApp upload
5. Face recognition
6. AI highlight reel
7. Printed photobook
8. PWA for couple dashboard

**Stop at step 3 until there are paying users.**

---

# DESIGN SYSTEM

## Colors — never change these

```
--bg:    #080403   Primary dark. Sections that need weight.
--bg2:   #130c07   Mid dark. Alternating sections.
--bg3:   #1e1209   Card dark. All card and input backgrounds.
--gold:  #c8954a   Primary accent. CTAs, highlights, active states, icons.
--glt:   #e5bc80   Light gold. Gradient endpoints, italic text accents.
--gdk:   #7a5825   Muted gold. Disabled states, secondary labels.
--cream: #f0e6d3   Primary text. Headlines, important copy.
--c2:    #b5a898   Secondary text. Descriptions, labels, metadata.
--bdr:   rgba(200,149,74,0.18)   All borders.
--red:   #8b1a1a   Pain color. Problem section only.
--red-bg:  rgba(139,26,26,0.12)
--red-bdr: rgba(139,26,26,0.25)
```

## Typography — two fonts, no exceptions

**Cormorant Garamond** (serif)
Use for: the brand name, every headline, every quote, big numbers, italic accent words, step numbers

**DM Sans** (sans-serif)
Use for: body copy, labels, buttons, captions, metadata, UI text, pill text

Never add a third font.

## Spacing rhythm

- Section padding desktop: `96px 80px`
- Section padding mobile: `56px 24px`
- Card gap: `2px` (intentionally tight — reads as premium, not cheap)
- Border radius on cards: `2px` (almost none — sharp edges feel luxurious in dark UIs)
- Border radius on pills: `100px` (full rounded)
- Border radius on buttons: `0` (sharp)

## Shared components

**Section label (.sl):**
Small, uppercase, gold, with a horizontal line extending to the right edge.
`font-size: 0.68rem, letter-spacing: 0.22em, color: --gold`
Used as the section title above every headline.

**Eyebrow (.ey):**
Same as .sl but with a short gold line on the LEFT. Used for form sections and nested contexts.

**Buttons (.btn):**
- `.bg` — gold fill, dark text. Primary action.
- `.bo` — transparent, cream text, gold border. Secondary action.
Font: DM Sans, 0.78rem, 0.08em letter-spacing, uppercase, 16px 36px padding.

**Gold line hover animation:**
All cards and steps get a 2px bottom border on hover:
`linear-gradient(90deg, transparent, var(--gold), transparent)`
This is the subtle animation that makes the UI feel alive.

---

# NAVIGATION

## Desktop (641px and above)

**Top bar, fixed, full width:**
- Height: 68px
- Background: rgba(8,4,3,0.94) + 14px blur
- Bottom border: 1px solid --bdr
- Left: Logo "Yaadein" — Cormorant Garamond, gold, uppercase, 0.2em letter-spacing, 1.3rem
- Right: Tab pill group
  - Tabs: Home | Create Event | Guest View | Live Gallery
  - Active tab: --gold fill, #080403 text, font-weight 500
  - Inactive tab: --c2 text

## Mobile (640px and below)

**Top bar:** Logo only. No tabs.
**Bottom bar (fixed):** Home | Create | Guest | Gallery
- Height: 58px + env(safe-area-inset-bottom, 0px) for iPhone notch
- 4 equal-width icon + label buttons

---

# S1: LANDING PAGE

## Arc

Attention → Recognition → Clarity → Proof → Action

5 sections. This order is not negotiable.

```
1. Hero        — "this was built for people like me"
2. Problem     — "yes, this is my exact frustration"
3. How it works — "that's actually simple"
4. Features    — "it does everything I need"
5. For whom    — "I am exactly the person this is for"
6. CTA         — "I want in"
```

---

## S1.1 — HERO

**One job:** Make a photographer or planner feel "this was built for me" in under 5 seconds.

### Content

**Badge:**
```
● For Indian Wedding Professionals · Early Access
```
- Green pulsing dot, 0.58rem, uppercase, pill-shaped

**Headline:**
```
What no other
photographer delivers.
```
- Cormorant Garamond, clamp(2.8rem, 4.5vw, 4.5rem), weight 300, line-height 1.05
- "delivers." in italic, gold gradient (glt → gold)
- `<br>` after "other" forces clean 2-line break at all viewport sizes

**Description:**
```
Your edited shots plus every guest candid in one album. Be the only
photographer in your city who can offer the complete wedding.
No other tool connects both.
```
- DM Sans, 1rem, weight 300, --c2, line-height 1.75, max-width 470px

**CTAs:**
- Primary: "Get photographer access" — .btn.bg — scrolls to #early
- Secondary: "See how it works" — .btn.bo — scrolls to #how

### Image strip (desktop only)
- Right side, position absolute, 380px wide, full height
- 2 columns × 3 rows, gap 4px, opacity 0.65
- All images: sepia(0.3) saturate(1.3) brightness(0.75)

---

## S1.2 — THE PROBLEM

**One job:** Create recognition. A photographer or planner reads this and nods.

**Background:** --bg2

**Section label:** "Sound familiar?"

**Headline:**
```
You deliver perfect shots.
The candids disappear.
```
- "candids disappear." in italic gold gradient
- Cormorant Garamond, clamp(2.8rem, 5vw, 4.5rem), weight 300, line-height 1.1

**Body:**
```
You spend days editing. Clients love the results. Two weeks later the
questions start. Where are the guests' photos? The dance videos?
The chacha who stole the baraat? You have no answer. Neither does
Samaro. Neither does FotoOwl.
```
- DM Sans, 1.05rem, weight 300, --c2, line-height 1.8, max-width 620px
- No em dashes. No "they ask —". Rewrite as separate sentences.

**Pain pills (6 pills, red-tinted):**
```
"Can you get the guests' photos too?"
"Half the candids are still on random phones"
"Uncle's dance video is in someone's WhatsApp"
"My clients keep asking about the guest shots"
"Drive link expired before they downloaded"
"Guests never share what they actually shot"
```

**Closing statement:**
```
Yaadein makes you the only photographer who can say yes.
```
- Cormorant Garamond, 1.6rem, weight 300, --cream
- Border-left: 3px solid --gold, padding-left 24px, max-width 600px

---

## S1.3 — HOW IT WORKS

**One job:** Explain the product in 60 seconds. Neutral — works for all three audiences.

**Background:** --bg2
**Section id:** `how`

### 4 Steps (2x2 grid desktop, 1 column mobile)

**Step 01 — Create your event**
Title: "Create your event"
Copy: "Enter the couple names and wedding date. Pick sub-events: Haldi, Sangeet, Mehendi, Baraat, Reception. A QR code and a private guest link are generated instantly. Print it, WhatsApp it, put it on the welcome table."
Link: "Try it" → S2

**Step 02 — Guests scan and upload**
Title: "Guests scan and upload"
Copy: "Any guest scans the QR code. Camera opens immediately. No app. No Google account. No login. They tap upload and their photo is in the album. Every candid, every dance, every tear."
Link: "Guest view" → S3

**Step 03 — Photographer uploads edited shots**
Title: "Photographer uploads edited shots"
Copy: "The photographer gets a private upload link. Their edited shots go directly into the same album as the guest candids. Couple receives both, organized by ceremony, in one place."
Link: "Photographer access" → S2, photographer tab

**Step 04 — Download everything. AI reel ready.**
Title: "Download everything. AI reel ready."
Copy: "The complete album, organized by Haldi, Sangeet, Baraat and Reception, is yours to download in full quality. AI generates a 60-second highlight reel from the best moments the morning after."
Link: "See the gallery" → S4

**Navigation behavior:**
Clicking any step link stores `s1ReturnId = 'how'`. When navigating back to S1 from any screen, the page scrolls to #how so users can continue reading the remaining steps.

---

## S1.4 — FEATURES

**One job:** Show the complete feature set. No status badges. No "live" vs "coming soon." Just what it does.

**Background:** --bg
**Grid:** 4 columns desktop, 2 columns tablet, 1 column mobile
**8 cards, no badges, compact padding (28px 24px)**

| Title | Copy |
|---|---|
| Guest upload via QR | Any guest scans. Camera opens in 2 seconds. No app, no login, no Google account. |
| WhatsApp upload | Guests share directly from WhatsApp. The single biggest upload channel in India. |
| Photographer upload link | Your photographer gets a private link. Their edited shots merge into the album alongside guest candids. |
| Organized by ceremony | Haldi, Sangeet, Baraat, Mehendi, Reception. Every photo sorted automatically. |
| Face recognition | Guests find every photo they appear in across all ceremonies. Instantly. |
| AI highlight reel | A 60-second video from the best moments. Music included. Ready to share the morning after. |
| Full quality download | The complete album in original resolution. No compression, no watermark. |
| Printed photobook | A curated printed album from your wedding. Delivered to your door. |

**Why no badges:**
"Live" and "Coming soon" labels create doubt. A planner or photographer reading "coming soon" thinks "so it doesn't work yet." Show the complete vision. Build credibility with the full list. Deliver on it over time.

---

## S1.5 — FOR WHOM

**One job:** Each audience sees themselves and clicks their CTA.

**Background:** --bg3
**Section label:** "You bring it. Everyone benefits."
**Grid:** 3 columns desktop, 1 column mobile

**Card order: Photographer → Couple → Planner**

**Card 1 — Photographers**
Eyebrow: "For photographers"
Title: "Offer what no other photographer can."
Body: "Your edited shots plus every guest candid in one album. One private upload link. No extra work on your end. Your clients get the complete wedding. No competitor delivers this right now."
CTA: "Get photographer access" → scrolls to #early

**Card 2 — Couples**
Eyebrow: "For couples"
Title: "You planned for months. Get every photo."
Body: "Guest candids, photographer edits, organized by ceremony. You receive the complete album the morning after. Not scattered across phones and WhatsApp groups two years later."
CTA: "I want this" → scrolls to #early

**Card 3 — Planners**
Eyebrow: "For planners"
Title: "Add the one thing clients always ask for."
Body: "After every wedding, couples ask about the guest photos. Now you have the answer. One QR code, one album, zero extra work on your end."
CTA: "Add it to your package" → scrolls to #early

---

## S1.6 — CTA / EARLY ACCESS

**One job:** Get the email. Segment the lead by role.

**Background:** --bg2
**Section id:** `early`

### Left column

**Headline:**
```
Be the first
in your city.
```
- "city." in italic, --gold
- Cormorant Garamond, 2.8rem, weight 300

**Subtext:**
```
We are onboarding photographers and planners one city at a time.
Early access is limited. Leave your details and we will reach out personally.
```

### Right column — Form

**Fields (flex row, wraps on mobile):**
1. Role select: "I am a..." / Photographer / Couple / Planner — `min-width: 148px`
2. Email input: `width: 260px`, "your@email.com"
3. Submit: "I want this" — .btn.bg

**Why the role dropdown:**
Every lead arrives tagged. You write a different follow-up email to a photographer vs a planner vs a couple. Without this you are writing the same cold reply to everyone.

**States:**
- Sending: "Sending...", button disabled
- Success: "Done. We will reach out personally within 48 hours." in --gold
- Error: "Something went wrong. Please try again." in red

---

## S1.7 — FOOTER

**Background:** --bg

**Left:**
- Logo: "Yaadein"
- Tagline: "The complete wedding album platform for Indian photographers."

**Right:**
- Nav links: How it works | Create event | Guest view | Live gallery | Get early access
- "How it works" link scrolls to #how on S1
- Contact: contact.yaadein@gmail.com
- Copyright: © 2026 Yaadein. All rights reserved.

---

# S2: CREATE EVENT

Demo screen. Should feel like a real product.

**Preview bar:** "This is a preview. The actual product is coming soon." + "Get Early Access" button → #early on S1

**Layout:** 2 columns, full height.
- Left: form panel
- Right: QR panel (--bg2 background)

## LEFT PANEL

Eyebrow: "New Event"
Headline: "Create your wedding event."
Subtext: "Fill in the details and your QR code is ready instantly. Print it, WhatsApp it, or display it at the venue entrance."

**Fields:**
- Couple Names: two inputs side by side, updates QR live
- Wedding Date: date picker, updates QR live
- Expected Guests: number input
- Sub Events: chip grid (Haldi on, Sangeet on, Mehendi off, Baraat on, Reception on, After Party off)
- "Generate QR Code" button

## RIGHT PANEL

**Tab switcher:** Guest QR Code | Photographer Upload

**Guest QR tab:**
- 190×190px QR code (auto-generates on load via QRCode.js)
- Couple name, date, URL (all dynamic from form)
- Action buttons: WhatsApp, Copy Link, Download QR, Print Card
- Info box: what to do next

**Photographer upload tab:**
- Upload zone (dashed border, gold)
- File picker (multi-select, images)
- On file select: shows count in gold

**Navigation from step 03:** clicking "Photographer access" in S1 automatically switches to photographer tab.

---

# S3: GUEST VIEW

Shows what a guest sees after scanning. Must look simple and beautiful.

**Intro:** "Guest Experience" eyebrow, "What guests see when they scan." headline, "No app. No login. Scan and upload in under 30 seconds."

**Phone mockup (375px wide desktop):**
- Hero image with couple name pill and "Share your moment" headline
- Counter: "312 memories shared so far"
- Gold circle upload button (130px, camera icon)
- Voice message + add video buttons
- Recent uploads strip (4 thumbnails)
- Optional name field

---

# S4: LIVE GALLERY

The payoff screen. Must feel full, alive, real.

**Gallery header (sticky):**
- "Rahul & Priya's Wedding" headline
- Live pill + photo count meta (updates on toasts)
- 4 action buttons: Find My Photos, AI Reel, Download All, Share Gallery

**Filter row:** All | Haldi | Sangeet | Baraat | Reception

**Masonry grid:** 4 cols desktop, 3 cols tablet, 2 cols mobile
- House filter: sepia(0.15) saturate(1.25) brightness(0.92) contrast(1.05)
- Hover: overlay with name + event, reaction buttons (❤️ 🔥 😭), scale 1.04

**Live toasts:** every 4.5s, "[Name] uploaded [N] new photos", 3s visible

**Face recognition modal:**
- 3-phase animation (scanning → matching → found)
- Auto-closes 6 seconds after opening

**AI reel modal:**
- 4-phase status (analysing → selecting → music → ready)
- Reel player appears at 4.5s
- Auto-closes 8 seconds after opening

---

# HARD RULES — NEVER VIOLATE

1. **No pricing on the website.** Price is a conversation. This is decided and final.

2. **No em dashes anywhere in copy.** Use a comma, a full stop, or rewrite the sentence. This has been violated and corrected multiple times. Check every new line before adding it.

3. **No fake social proof.** No star ratings. No "1,000 couples." No testimonials. We have no real users.

4. **No third font.** Cormorant Garamond + DM Sans. Nothing else.

5. **No emojis in copy or UI.** Only exception: gallery reaction buttons (❤️ 🔥 😭).

6. **No light mode.** The dark/warm theme is the identity.

7. **No "live" or "coming soon" badges on features.** Removed. They create doubt. Show the full feature list without status labels.

8. **CTA button text is "I want this."** Everywhere on the site except the for-whom photographer card ("Get photographer access") and planner card ("Add it to your package"). The main form submit always says "I want this."

9. **No stats that are unearned.** No user counts, no wedding counts, nothing we do not have.

10. **No emotional/poetic copy in sections targeting professionals.** "Every tear, every dance" is for couples. Photographers and planners get outcome-focused, competitive language.

11. **The tone test.** Before publishing any copy: can a planner replace "photographer" with "planner" and have it still make sense? If it only works for one audience, it belongs in an audience-specific card, not in a shared section.

---

# WHAT IS BUILT (STATUS)

**Live in demo (theyaadein.com):**
- QR code generation (client-side, QRCode.js)
- Sub-event chip selection
- Couple names + date → dynamic QR card
- Guest view phone mockup
- Voice message recording simulation (4s fake)
- Photo upload counter (tap to increment)
- Live gallery masonry (local images)
- Gallery filter by sub-event
- Live toasts (every 4.5s)
- Photo reactions (client-side, no persist)
- Face recognition modal (timed animation, fake result, auto-closes at 6s)
- AI reel modal (timed status updates, fake player, auto-closes at 8s)
- Photographer upload tab (file picker, no actual upload)
- Email form (Web3Forms, actually sends to contact.yaadein@gmail.com)
- Role dropdown in form (photographer / couple / planner)

**Not built — do not build until paying users:**
- Real backend (auth, storage, database)
- WhatsApp upload integration
- Real photographer upload
- Face recognition (AWS Rekognition)
- AI reel generation
- Printed photobook ordering
- PWA for couple dashboard

---

# TECH STACK (when the time comes)

Frontend: Vanilla HTML/CSS/JS (current) → Next.js when complexity demands it
Storage: Cloudflare R2 (no egress cost, cheapest for Indian users)
Database: Supabase (free tier to start, Postgres)
Auth: Supabase Auth
WhatsApp: Twilio WhatsApp Business API
AI Reel: FFmpeg on a queue worker (Railway or Fly.io)
Face Recognition: AWS Rekognition
Payments: Razorpay (Indian gateway, INR)
Hosting: Vercel (frontend) + Railway (backend)

**Do not touch this stack until 10 real people say they want it.**

---

# OUTREACH STRATEGY

## Rules for every message

1. Say what it does in 2 lines. Do not be mysterious.
2. Lead with their pain, not the product.
3. "We built" not "I built."
4. One link. No ask. The website converts.
5. Never follow up more than once. One DM. One follow-up after 5 days. Then move on.
6. Volume beats perfection. 50 good messages beat 5 perfect ones.

---

## AUDIENCE 1: PHOTOGRAPHERS

### Where to find them
- Instagram: "wedding photographer [city]" — Mumbai, Delhi, Bangalore, Hyderabad, Pune, Jaipur
- 5,000–100,000 followers. Real working photographers, not hobbyists.
- Hashtags: #weddingphotographerindia #shaadiphotographer #mumbaiweddingphotographer

### Instagram DM
```
Hey [Name], your work is gorgeous. The candids especially.

Your clients are getting 800 edited shots from you. They're also missing
3,000 more from that same day — every guest had a phone and none of those
photos ever make it anywhere permanent.

We built a tool for this. One QR code at the venue, guests upload directly
into a shared album organized by ceremony. The couple gets everything.
You can offer it as part of your package.

theyaadein.com
```

### WhatsApp DM
```
Hi [Name], saw your work online — genuinely impressive.

Quick question: do your clients ever ask about the guest photos after the
wedding? The ones from everyone's phones that end up scattered in WhatsApp
groups?

We built something to fix that. One QR code, one album, works at any Indian
wedding. Takes 2 minutes to set up.

theyaadein.com
```

### LinkedIn DM
```
Hi [Name],

You deliver 800 edited shots to your clients. They're missing 3,000 more
from the same day that will never make it out of WhatsApp groups.

We built Yaadein — one QR code at the venue, every guest uploads into one
shared album organized by ceremony. You can offer it as an add-on to every
wedding package.

No other photographer in India is delivering this right now.

theyaadein.com
```

### Instagram posts — photographer angle

**Post 1:**
```
You deliver the edited shots.
But what about the 3,000 candids from every guest's phone?

The chacha who caught the exact moment.
The best friend who got the baraat on video.
The kid who was in the right place at the right time.

Your clients are losing all of it. We built the fix.

theyaadein.com
```

**Post 2:**
```
What no wedding photographer in India is offering yet:

Guest photos + your edited shots. One album. Organized by ceremony.

The couple does not have to choose between your work and what their guests captured.
They get both.

theyaadein.com
```

**Post 3 (reel hook):**
```
"What happens to all the photos guests take at Indian weddings?"

Nobody has a good answer. Until now.
```

---

## AUDIENCE 2: WEDDING PLANNERS

### Where to find them
- Instagram: "wedding planner [city]", "shaadi planner", "wedding coordinator india"
- LinkedIn: "wedding planner" India — many have professional profiles
- Hashtags: #weddingplannerindia #shaadiplanner #luxuryweddingindia

### Instagram DM
```
Hey [Name], love how you put events together.

One thing we've noticed: after every wedding, couples spend weeks trying to
collect photos from guests. WhatsApp groups, Drive links, camera rolls.
It becomes a whole separate project.

We built a tool for this. One QR code at the venue, guests upload directly
into a live album, organized by ceremony. You can include it in your
coordination package.

theyaadein.com
```

### WhatsApp DM
```
Hi [Name], hope the season is going well.

Do your clients ever come back after the wedding asking for help collecting
guest photos? We hear this happens a lot and nobody has a clean answer for it.

We built one. One QR code at the venue, everything collected automatically,
organized by Haldi, Sangeet, Baraat, Reception. You can offer it to every client.

theyaadein.com
```

### LinkedIn DM
```
Hi [Name],

After the wedding, couples spend weeks trying to collect photos from 400
guests across WhatsApp groups, Google Drive links, and camera rolls nobody
will ever share.

We built Yaadein — a QR code placed at the venue. Guests upload directly
into one shared album, organized by ceremony. It is something you could offer
to every client as part of your coordination package. Real value, zero extra
work on your end.

theyaadein.com
```

### Instagram posts — planner angle

**Post 1:**
```
You coordinate the venue.
The catering.
The decor.
The photographer.
The seating.
The families.

And then the wedding ends and someone asks:
"How do we get everyone's photos?"

There is a tool for that now.

theyaadein.com
```

**Post 2:**
```
The thing every wedding planner's clients ask after the wedding:

"Can you help us collect the guest photos?"

Now you can actually answer that.

theyaadein.com
```

---

## AUDIENCE 3: COUPLES

Reach couples through photographers and planners first. Direct outreach is secondary.

### Where to find them
- Instagram: engagement announcement posts, #weddingseason2026 #shaadi2026
- Facebook wedding groups (still very active in India)
- Reddit: r/IndianWeddings, r/weddingplanning
- WhatsApp: personal network

### Instagram DM
```
Hey [Name], congratulations on your upcoming wedding!

One thing most couples don't think about until after: the thousands of photos
guests take on their phones that never make it anywhere. Everyone says they
will share and nobody does.

We built Yaadein. One QR code at your venue. Every guest scans and uploads
directly. Photos organized by Haldi, Sangeet, Baraat, Reception.
You download everything after.

No app for guests. No login. Just scan.

theyaadein.com
```

### Instagram posts — couple angle

**Post 1:**
```
After your wedding:

Your photographer delivers 800 edited shots in 4-6 weeks.
Your guests take 3,000 photos that day.
You will see maybe 60 of them.
The rest live in camera rolls until the phone breaks.

We built a fix.

theyaadein.com
```

**Post 2:**
```
Every Indian wedding ends the same way.

"Send me the photos yaar"
"I'll send tomorrow"
"Drive link?"
"Which Drive link"
"I'll forward from WhatsApp"

There is a better way now.

theyaadein.com
```

---

## DAILY ACTION TABLE

| Day | Action |
|---|---|
| Mon | DM 10 photographers (new accounts, not already messaged) |
| Tue | DM 10 wedding planners |
| Wed | Post one piece of content (rotate: photographer / planner / couple angle) |
| Thu | DM 10 couples (engagement posts, recent wedding hashtags) |
| Fri | Reply to any responses. Follow up with engaged non-visitors. |
| Sat | DM 10 more photographers. They post most on weekends after weddings. |
| Sun | Stories only. Behind the scenes, "we're building this" energy. |

**Weekly target:** 40-50 DMs, 1-2 posts, daily stories.
**Monthly target:** 160-200 DMs. At 5% response rate = 8-10 real conversations.

---

## TRACKING

| Name | Type | Platform | Date sent | Reply? | Visited site? |
|---|---|---|---|---|---|
| @name | Photographer | Instagram | Date | Yes/No | Yes/No |

**The goal is not conversations. The goal is email signups.** Every DM exists to get someone to theyaadein.com. The site does the converting.
