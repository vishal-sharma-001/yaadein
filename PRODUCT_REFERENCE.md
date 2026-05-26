# Yaadein — Product Reference Document

**Live at:** theyaadein.com  
**Contact:** contact.yaadein@gmail.com  
**Status:** Demo site for validation. No real backend. Email collection via Web3Forms.

---

## What Yaadein Is

One QR code. Every guest photo. Every photographer edit. One private album.

The entire product sits on a single positioning: **guest photos + photographer's edited shots in one album.** No competitor does this. The market splits into two tools — photographer delivery tools (Samaro, FotoOwl, Photomall) and guest collection tools (Wedibox, GuestCam, Kululu). Yaadein connects them.

Every design decision, every copy line, every demo screen must reinforce this.

---

## The Three Personas

There are exactly three users. Each gets their own demo view.

| Persona | Demo Screen | How they enter the product |
|---|---|---|
| Photographer | S2 — The Photographer View | Creates the event, gets a private upload link |
| Guest | S3 — The Guest View | Scans QR code at the venue |
| Couple | S4 — The Couple's View | Receives album link automatically when event is created |

---

## S2 — The Photographer View

### Who this is for
The photographer (or wedding planner) who creates the event. This is the person who pays for Yaadein and is responsible for setting up the album before the wedding.

### Section 1: Event Setup

**What they fill in:**
- Couple Names (two separate fields — first name of each person, used to generate the URL slug and QR label)
- Wedding Date (formats automatically to "18 May 2026" in the QR card)
- Expected Guests (number — informational only, no backend logic tied to it yet)
- Couple's Email (critical — this is how couples get their album link automatically)
- Couple's WhatsApp (optional — for WhatsApp delivery of the same link)
- Sub Events (chip selector — Haldi, Sangeet, Mehendi, Baraat, Reception, After Party)

**What happens on "Generate QR Code":**
- QR code renders instantly using QRCode.js (client-side only in demo)
- QR URL format: `theyaadein.com/[n1initial][n2initial]-wed-[2-digit year]` (e.g. rp-wed-26)
- The couple-notif panel appears showing "Album link sent to [email]" — in production this triggers an email + WhatsApp to the couple
- The "What to do next" panel is replaced by the couple notification

**What they get on the right side (QR card):**
- The QR code image (printable)
- Couple names + formatted date + guest URL
- Share buttons: WhatsApp, Copy Link, Download QR, Print Card
- Couple notification panel (after genQR with email)
- Photographer private link (separate from the guest QR — photographer uploads via this link, guests scan the QR)

**Key distinction to understand:**
- Guest link = the QR code URL = what guests scan = public-ish (anyone with it can upload)
- Photographer link = private = only shared with the photographer = they upload edited shots here
- Couple album link = private = sent to couple automatically = view-only access

**Product development notes:**
- On real backend: genQR creates an event record in DB, sends email to couple via SendGrid/Resend, sends WhatsApp via Twilio/WATI
- The slug needs collision-detection (two Rahul+Priya weddings in same year)
- Sub-events selection should create sub-album buckets in storage
- Guest link should expire after 7 days post-wedding date (configurable)
- Photographer link should never expire until photographer manually closes it

---

### Section 2: Live Dashboard

**Split layout — left (photographer's upload area) / right (stats panel)**

**Left — Your edited shots:**
- Photo grid showing all uploaded shots (6 columns, 1:1 aspect ratio thumbnails)
- "48 shots uploaded" count
- Upload more button + drag-drop zone
- File drop area at bottom (JPEG, PNG, RAW)
- Demo uses real images from `images/` folder across all ceremonies

**Right — Stats panel:**
- Event card (couple name, date, venue)
- Guest photo stats: total count (312), breakdown by ceremony (Haldi 64, Sangeet 108, Baraat 78, Reception 62)
- Guest count (47 guests)
- Total album count: 360 (312 guest + 48 photographer)
- "Copy couple's album link" button (so photographer can also manually share it)

**What the photographer can see:**
- Their own uploaded shots
- Total guest photo count per ceremony
- Combined album size
- The couple's private link (to share manually if needed)

**What the photographer cannot see:**
- Individual guest names or identities (privacy — guests haven't consented to photographer seeing their identities)
- Guest gallery browsing (that is the couple's privilege)
- Couple's private dashboard or couple controls

**Product development notes:**
- Real upload: multipart file upload to S3/Cloudflare R2, with ceremony tag from sub-event chips
- RAW files: convert to JPEG on server before serving to guests/couple (ImageMagick or Sharp)
- The stat counts are live-updating in real product — WebSocket or polling every 30s
- "Copy couple's album link" — generates a signed URL with expiry if using S3, or a token-protected route

---

### Section 3: Album Preview

**What it is:** The photographer sees exactly what the couple will see — a mixed grid of guest photos and their own edited shots, each labelled with a "Guest" or "Your edit" badge.

**Layout:**
- Header showing total count ("360 photos ready · 312 guest · 48 yours")
- 4-column photo grid (16 photos shown, mix of guest and pro)
- Each photo has a badge: amber "Your edit" for photographer shots, white "Guest" for guest photos
- "Ready to deliver" panel at the bottom with "Copy couple's album link" CTA

**What this communicates to the photographer:**
- This is the finished product you're delivering
- Guest photos and your shots coexist seamlessly in one album
- The couple gets full quality, organized by ceremony

**Product development notes:**
- This preview should pull from the actual mixed album, not a separate data source
- Badge logic: any photo uploaded via photographer link = "pro" badge, any via guest QR = "guest" badge
- Photo count in preview header should be real-time, not hardcoded

---

## S3 — The Guest View

### Who this is for
Any wedding guest who scans the QR code at the venue. They have no account, no login, no app. They land directly on this page.

### Phone mockup section

**What guests see immediately (no friction):**
- Couple names + "Wedding 2026" pill at the top of the phone hero image
- Running count of memories shared so far (starts at 312, increments with each tap in demo)
- "You're at" ceremony selector — chips for Haldi, Sangeet, Baraat, Reception (they select which sub-event they're currently at, so their photo is tagged correctly)
- Large circular upload button (gold, pulsing glow animation) — the primary action

**The upload button (tap):**
- On real product: opens native file picker (camera or gallery) — `<input type="file" accept="image/*" capture="environment">` for mobile
- Demo: simulates upload, button shows "Photo Live!", counter increments, hint changes to "Your photo is now in the album", then resets after ~2s
- This must be the fastest possible action — scan QR, tap button, done in under 10 seconds

**Other actions in the phone:**
- Voice Message button — records a voice wish for the couple (demo: simulates 4-second recording, saves, shows "Message Saved" in gold)
- Add Video button — same flow as photo upload for videos
- Recent from guests — 4 small thumbnail images showing latest uploads (social proof, "you're contributing to something live")
- Name input (optional) — "Your name (optional) · So the couple knows it's from you" — improves attribution on the couple's dashboard

**What guests can NOT do:**
- Download all photos (that's the couple's privilege)
- Delete their own photo after upload (no undo in v1)
- See the photographer's edited shots (those are for the couple only)
- Access the couple's private dashboard

**Guest Gallery section (below phone):**

This is the key experience differentiator for guests. After uploading, they can scroll down and browse the shared album — their photo is already in there.

- Section label: "Photos from the wedding"
- Subtitle: "312 moments shared by 47 guests · React, find yourself, save the ones you love"
- Ceremony filter chips (All / Haldi / Sangeet / Baraat / Reception) — guests can filter by ceremony
- 4-column photo grid, guest photos only (no photographer edits here)
- On photo hover: reactions (❤️ 🔥 😭) + individual save/download button
- "Find my photos" button — triggers face recognition modal (scanning animation, then "23 photos found")

**Why guests see the gallery:**
The product must give guests a reason to care beyond uploading. Seeing everyone's photos, reacting to them, finding themselves — this is the shareable, sticky moment. Guests who see this will tell others. This is the viral loop.

**What the guest gallery does NOT show:**
- Photographer's edited shots (they come later, they're not ready the night of the wedding)
- Download All (saves individual photos only)
- AI Reel (couple's feature)
- Moderation controls

**Product development notes:**
- Real guest upload: `FormData` POST to `/api/upload` with `ceremony`, `guestName`, `weddingSlug` fields
- Face recognition: needs a face-detection ML model (TensorFlow.js client-side for demo quality, or server-side with AWS Rekognition for production)
- Guest gallery: served from the same S3 bucket as couple's gallery, but filtered to show only guest-uploaded photos (not photographer's)
- No auth for guests — the wedding slug is effectively the auth token. Rate-limit by IP to prevent abuse.
- Voice messages: stored as audio/webm blobs, transcribed by Whisper (optional), surfaced in couple's dashboard as "Wishes"

---

## S4 — The Couple's View

### Who this is for
The couple (Rahul and Priya in demo). They receive a private link via email and WhatsApp when the photographer creates their event. This is their personal album, combining everything.

### Header

**Album title:** "Rahul & Priya's Wedding"

**Status line:** "312 guest photos · photographer edits added after" — honest about what's there and what's coming.

**Contribution summary (below status):**
- "47 guests · Sangeet had the most activity · 3 voice wishes"
- This tells the couple the social story of their wedding — who showed up, what happened, who left wishes

**Action buttons:**
- Find My Photos — face recognition (same modal as guest view)
- AI Reel — generates a 60-second highlight video (demo: 4.5-second generation animation, then shows collage preview)
- Download All — downloads full album (demo: "Preparing..." → "Ready to download" → resets)
- Share Gallery — copies link (demo: "Link copied" → resets)

### Source Toggle

Two tabs that fundamentally change what is displayed:

**Guest Shots (default):**
- Shows all 312 guest-uploaded photos and videos
- Grid is live — new photos appear as guests upload during the event
- Photos attributed to the guest who uploaded them (hover shows name + ceremony)
- Live notification banner appears when new photos arrive ("Anjali added 3 new photos")
- Reactions on each photo (❤️ 🔥 😭) — couple can react to guest moments

**Photographer's Edits · Added after:**
- Shows an empty state — NOT the photos (yet)
- Empty state copy: "Photographer's edits arriving soon — 2-3 days"
- Explanation: "Your photographer is still editing. Their curated shots will appear here in 2-3 days, alongside all guest photos. You'll get a notification when they're added."
- "312 guest photos are ready now in Guest Shots"
- This is intentionally empty. The photographer hasn't finished editing. The "Added after" label in the tab itself signals this. Showing photos here before the photographer delivers would be a product error.

**Why this separation matters:**
The couple can enjoy guest photos the night of the wedding. Photographer edits are a premium delivery that comes later. These are two distinct emotional moments — both important, both in one place.

### Category Filters

All / Haldi / Sangeet / Baraat / Reception — filters both guest shots and (when ready) photographer's edits within the selected source.

### Voice Wishes Bar

Between the category filters and the photo grid. Shows 3 voice messages from guests.
- Each card: guest name + duration + playback progress bar
- Click to play (demo simulates playback with a timer-driven progress bar)
- Click again to pause
- Only one plays at a time

**What this is for:** During the wedding night, guests who couldn't find the right words left voice messages. The couple listens to them the next morning. This is emotionally the most powerful feature and the easiest to explain in one sentence.

### Photo Grid (Guest Shots)

- 4-column grid, `aspect-ratio: 4/3` on each photo
- Live photos inject at the top as guests upload
- Guest name + ceremony shown on hover overlay
- Reactions on hover (❤️ 🔥 😭)
- Red X moderation button on hover — couple can hide any photo (it fades out and is removed from their view)
- Photo lightbox on click — full-screen view with prev/next arrows, keyboard navigation, counter

**Moderation (couple only):**
Couples can hide inappropriate or unflattering photos. One-click, irreversible in demo (in production: soft-delete with restore option, never actually deletes from storage unless couple explicitly purges).

### Live Notification Banner
- Appears at the top of the grid when new photos arrive
- "Anjali added 3 new photos" — slides in, shows for 3 seconds, slides out
- Guest count in status line increments in real time
- New photos appear at the top of the grid with a fade-in animation

**Product development notes:**
- Real-time updates: WebSocket (preferred) or Server-Sent Events — push new photo events to couple's browser
- Face recognition: store face embeddings per wedding, match against couple's selfie or scan in-browser
- AI Reel: queue-based — trigger on couple request, process server-side (ffmpeg), notify when ready
- Download All: zip the entire S3 folder server-side (use a background job + pre-signed S3 URL for download)
- Moderation: soft-delete — `hidden_by_couple: true` flag in DB, photo stays in storage, just excluded from served gallery
- Photo attribution: guest name stored with each upload record, shown to couple only (not to other guests)
- Voice wishes: audio files in S3, listed separately from photos, served via pre-signed URLs

---

## How the Three Views Connect (The Full Flow)

```
PHOTOGRAPHER
  1. Creates event → enters couple names, date, ceremonies, couple's email
  2. Clicks "Generate QR Code"
  3. Receives: guest QR code + photographer private link
  4. COUPLE RECEIVES: album link automatically via email + WhatsApp
  5. Prints QR, places at venue
  6. Shares photographer link to self (or planner shares it)

WEDDING DAY
  ↓
GUESTS
  7. Scan QR → phone camera opens → tap upload → photo goes in album
  8. Select ceremony (Haldi/Sangeet/etc) before uploading
  9. Leave voice wishes, add videos
  10. Browse gallery, react to photos, find themselves

PHOTOGRAPHER
  11. Uploads edited shots via private link (during or after wedding)
  12. Sees combined album preview in S2 → confirms it's ready → copies couple's link

COUPLE
  13. Open album link (received step 4 — they can open it during the wedding)
  14. Watch guest photos arrive live
  15. Listen to voice wishes
  16. 2-3 days later: photographer's edits appear in the Photographer's Edits tab
  17. Download everything in full quality
```

---

## Permissions Matrix

| Feature | Guest | Photographer | Couple |
|---|---|---|---|
| Upload photos | Yes | Yes (via private link) | No |
| Upload videos | Yes | No (in v1) | No |
| Leave voice wishes | Yes | No | No |
| See guest photos | Yes (guest gallery) | Yes (in album preview) | Yes |
| See photographer edits | No | Yes (their own uploads) | Yes (after delivery) |
| React to photos (❤️ 🔥 😭) | Yes (in gg-section) | No | Yes |
| Find my photos (face recognition) | Yes | No | Yes |
| Download individual photo | Yes | No | Yes |
| Download all photos | No | No | Yes |
| Share gallery link | No | No | Yes |
| AI Highlight Reel | No | No | Yes |
| Hide/moderate photos | No | No | Yes |
| See couple's dashboard | No | No | Yes |
| See guest names | No | No | Yes |
| See voice wishes | No | No | Yes |
| See live upload stats | No | Yes | Yes |
| Copy couple's album link | No | Yes | Yes |

---

## Design Rules (Hard — Never Violate)

1. **No em dashes** anywhere in copy. Use commas, full stops, or restructure.
2. **No pricing on the site.** Pricing is a conversation.
3. **CTA is "I want this"** — not "Get Early Access", not "Sign Up", not "Join."
4. **No emojis** outside gallery reactions (❤️ 🔥 😭).
5. **Two fonts only:** Cormorant Garamond (headings/display) + DM Sans (UI/numbers/labels).
6. **No fake testimonials, no fake stats.** The stats on the homepage (200+, 0, 1) are conceptual, not fabricated user reviews.
7. **No light mode.** Dark/warm/gold is the product identity.
8. **Nothing gets a "live" badge unless it actually is live.** In demo: only the QR upload and demo screens are live.
9. **DM Sans for ALL numbers.** Cormorant Garamond renders "1" as a capital "I" — never use it for numeric display.

---

## What is Demo vs What Needs to Be Built

| Feature | Current State | Build Priority |
|---|---|---|
| QR code generation | Live (client-side QRCode.js) | Keep — works fine |
| Email collection (early access form) | Live (Web3Forms) | Keep for validation |
| Guest upload (tap button) | Demo simulation | Priority 1 |
| QR → camera open on mobile | Not implemented | Priority 1 |
| WhatsApp upload link | Demo | Priority 2 (India's biggest advantage) |
| Couple email notification on event creation | Demo | Priority 1 |
| Photographer upload via private link | Demo | Priority 1 |
| Live gallery with real photos | Demo (static images) | Priority 1 |
| Face recognition | Demo (animation only) | Priority 4 |
| AI Highlight Reel | Demo (animation only) | Priority 5 |
| Voice wishes recording | Demo (timer simulation) | Priority 3 |
| Photo moderation | Demo (removes from DOM) | Priority 2 |
| Download All | Demo (button state only) | Priority 1 |
| Ceremony filtering | Demo (static) | Priority 1 |
| Live notifications (new photo toasts) | Demo (setInterval simulation) | Priority 1 (WebSocket) |
| Photographer edits delivery | Demo (empty state) | Priority 1 |
| Printed photobook | Not built | Priority 6 |
| PWA for couple | Not built | Priority 7 |

---

## Build Priority Order

1. **QR guest upload** — core product, nothing works without this
   - Guest scans QR → browser opens → picks photo → uploads to S3 → appears in album
   - Tech: `<input type="file">`, multipart POST, S3 presigned URL
2. **WhatsApp upload link** — India-specific killer feature
   - Guest receives a WhatsApp link instead of scanning → taps → same upload flow
   - Tech: WhatsApp Business API or WATI, deep link to upload page
3. **Photographer portal** — the actual differentiator
   - Private link, bulk upload, edited shots go into same album as guest photos
   - Tech: authenticated route, multi-file upload, S3 with ceremony tagging
4. **Couple album view** — what you're selling
   - Real-time gallery with WebSocket updates, filter by source and ceremony
   - Tech: signed URL for access, WebSocket or SSE for live updates
5. **Face recognition** — premium feature, not MVP
6. **AI highlight reel** — premium feature, not MVP
7. **Printed photobook** — partnership or API integration later

---

## Tech Stack Decisions (Current Thinking)

- **Hosting:** GitHub Pages (current). Move to Vercel or Cloudflare Pages for serverless functions.
- **Storage:** Cloudflare R2 (cheaper than S3, S3-compatible API) for photos/videos/voice
- **Database:** PlanetScale (MySQL) or Supabase (Postgres) — events, uploads, guest names, face embeddings
- **Email:** Resend or SendGrid — couple notification on event creation
- **WhatsApp:** WATI or Twilio — WhatsApp link delivery
- **Real-time:** Supabase Realtime or Pusher — live photo toasts in couple view
- **Face recognition:** AWS Rekognition or DeepFace server-side
- **AI Reel:** ffmpeg on a VPS or AWS Lambda — triggered by couple, async job
- **Auth:** None for guests (slug = token). Simple JWT for photographer and couple private links.
- **Domain:** theyaadein.com (Wix DNS → GitHub Pages currently)

---

## Validation Goal

10 people who genuinely want this before building real backend.

Current outreach: one friend handling Instagram, LinkedIn, Twitter, Threads, WhatsApp. 

If someone signs up via the early access form, Vishal reaches out personally. No automated sales sequence. The form data goes to contact.yaadein@gmail.com via Web3Forms (key: `23dd6ec7-5a8e-4690-a10d-98d7ad9d2477`).
