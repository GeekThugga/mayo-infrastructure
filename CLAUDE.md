# CLAUDE.md — SIMG Project Memory
# Tony Edwards (GeekThugga) — Sage It Media Group
# Last updated: April 12, 2026
# Paste this file at the start of every new chat to restore full context.

---

## SHORTHAND

| Code   | Meaning                                         |
|--------|-------------------------------------------------|
| CC     | sage-unified-hub.html — SIMG Command Center     |
| CCSD   | spenward_command_center.html — SDG CRM          |
| MAYO   | n8n automation infrastructure (never say "n8n") |
| HGH    | GoHighLevel (never say "GHL")                   |
| SDG    = Spenward Development Group LLC          |
| SIMG   | Sage It Media Group (parent)                    |

---

## COMPANY STRUCTURE

Sage It Media Group (SIMG) — parent
└── Hyper Growth Hub (HGH) — GoHighLevel white-label agency
└── Spenward Development Group LLC (SDG) — Tony + Bryant Spencer
    ├── DBAs: ROOFNation Solutions, Growco Landscaping (Q2), Thermal Exchange (Summer)
    ├── Active Clients: KW Legacy, A-One Facility Services, CFM Security
    ├── Pipeline: 20/20 Optical, Corporate Facility Management
    ├── Delinquent: SPEC-9 (owes since 11/2025)
    ├── Watch: Legacy Mutual Mortgage
    └── Curated Partners: Tony + Bryant + John Totin + Marcus Romero → KW Legacy

Strategic goal: Delegate agency to Bryant Spencer. Tony focuses on consumer apps.
Income target: $100K/mo → $1M/mo via apps + 2M+ follower Instagram distribution.

---

## INSTAGRAM ACCOUNTS

@changeurperception   1.1M → Best Self App
@alchemichealing      588K → Alchemic AI Wellness
@herbsovermeds        198K → TBD
@geekthugga           103K → TBD

---

## APP PORTFOLIO

All apps: React 19 + Supabase + iOS Swift + Claude API
Build order: Best Self → Alchemic → Bond → Affirmations Alarm

| App                   | IG Account          | Price         | Status   |
|-----------------------|---------------------|---------------|----------|
| Best Self App         | @changeurperception | $9.99/mo      | SCAFFOLDED — needs Supabase keys |
| Alchemic AI Wellness  | @alchemichealing    | TBD           | PLANNED  |
| Bond AI Relationship  | TBD                 | TBD           | PLANNED  |
| Affirmations Alarm    | TBD                 | TBD           | PLANNED  |

Affirmations Alarm: standalone vs Best Self feature — PENDING DECISION
Affirmations Alarm IG account assignment — PENDING DECISION

Supabase strategy:
- Consumer apps: individual Supabase project per app
- Agency clients: single shared Supabase project (MAYO Infrastructure) with client_id + RLS
- Free tier (2 projects max):
    Project 1: Best Self App     ID: ywvxzwntcebzpzjpqtjy  (rename + run schema)
    Project 2: MAYO Infrastructure ID: zewzbfgefqsajnrdrmyr  (clients table live)
  MAYO Supabase URL: https://zewzbfgefqsajnrdrmyr.supabase.co

---

## PRIORITY DRILL — EVERY SESSION

1. Enter Anthropic API key into Claude Custom Auth credential in MAYO
2. Firecrawl integration into MAYO for lead enrichment (firecrawl.dev)
3. Obsidian vault build + link to MAYO for automated note capture

---

## INFRASTRUCTURE

VPS:          srv803590.hstgr.cloud (IP: 46.202.93.220)
MAYO URL:     https://n8n.srv803590.hstgr.cloud
MAYO Login:   tony@sageitmedia.com
Docker:       /root/docker-compose.yml | .env: /root/.env
UFW ports:    80, 443, 5678

GitHub Pages:
  CC:         https://geekthugga.github.io/sage-command-center
  CCSD:       https://geekthugga.github.io/spenward-command-center
  MAYO App:   https://geekthugga.github.io/mayo-infrastructure
  KW Legacy:  https://geekthugga.github.io/curated-kw-legacy

---

## MAYO CREDENTIALS

CRITICAL: ALL custom auth JSON requires "headers" wrapper:
  { "headers": { "key": "value" } }
  Without wrapper → headers sent as body → 401 errors

Claude Custom Auth:
  { "headers": { "x-api-key": "ANTHROPIC_KEY", "anthropic-version": "2023-06-01", "content-type": "application/json" } }
  STATUS: ⚠️ Working structure — needs API key entered

HGH-Authorization KW Legacy:
  { "headers": { "Authorization": "Bearer pit-e1910b00-a679-46a4-b279-455cbdd07a83", "Version": "2021-07-28" } }
  STATUS: ✅ Working

RULE: NEVER use HighLevel OAuth2 with n8n — Private Integration tokens only.
  Agency-level = 22 scopes (useless for CRM). Sub-account = 139 scopes (always use this).

Sage It Media Automation GHL App:
  App ID / Client ID: 69d4119dd85ee3e2cebe79b2
  OAuth Redirect: https://n8n.srv803590.hstgr.cloud/rest/oauth2-credential/callback

Chase Smith (admin proxy): chase@mail.sageitmedia.com

---

## MAYO WORKFLOWS

### Claude AI → HGH Lead Intelligence
Workflow ID:   CdA6rbUKjQWdKi7z
Status:        ✅ LIVE — contact creation confirmed in KW Legacy (April 7, 2026)
Test webhook:  https://n8n.srv803590.hstgr.cloud/webhook-test/hgh-lead-intake
Live webhook:  https://n8n.srv803590.hstgr.cloud/webhook/hgh-lead-intake

Node architecture:
  1. HGH Lead Webhook (trigger)
  2. Claude API (POST api.anthropic.com/v1/messages)
  3. Parse Claude Response (Code — extracts TAG/PRIORITY/FOLLOW_UP/NOTE)
  4. Merge (Combine by Position — Input 1: Parse, Input 2: Webhook)
  5. Create HGH Contact (POST services.leadconnectorhq.com/contacts/)
  6. Notify Agent (needs HGH-Authorization KW credential assigned ← TODO)

Expression syntax:
  Webhook data:  $input.item.json.body.firstName
  Claude data:   $('Parse Claude Response').item.json.tag

Claude output tags: buyer-lead | seller-lead | agent-recruit | luxury-lead | commercial-lead | general-inquiry

Test CURL:
  curl -X POST https://n8n.srv803590.hstgr.cloud/webhook-test/hgh-lead-intake \
  -H "Content-Type: application/json" \
  -d '{"firstName":"Tony","lastName":"Test","email":"tony@sageitmedia.com","phone":"2104823200","source":"KW Legacy Buyers Form","message":"Buyer interested Stone Oak 500k"}'

### ROOFNation Workflow
Workflow ID:   J3B1Rw66mXWJWPpg
Webhook UUID:  363cb6a1-bace-4617-af09-5ee7568d1836
Status:        ✅ LIVE (succeeded ID#322 in 3.7s)
Telegram Bot:  TG_ROOFNATION_AI / Tony Chat ID: 5609414903
Supabase row:  clients table — location_id, token, prompt all set
RLS policy:    anon read enabled

### Shot Doctor Workflow (P. Scales)
Workflow ID:   jzSGqizGTeZ5m3RU
Status:        ✅ LIVE — first post confirmed
Webhook URL:   https://n8n.srv803590.hstgr.cloud/webhook/36a4e946-67ac-46d9-8f81-fa0e2235c4c2/webhook
GHL POST body: accountIds must be clean single-quoted strings — no extra double quotes

---

## HGH CLIENT CREDENTIALS

### KW Legacy
Location ID:   haYBppQ7i0NmK3BQNKyG
Token:         pit-e1910b00-a679-46a4-b279-455cbdd07a83
Site:          legacy.hypergrowthhub.io
SEO Tier:      Pro
Logo CDN:      https://assets.cdn.filesafe.space/haYBppQ7i0NmK3BQNKyG/media/69ceffea25159e065117feb9.png

Partner Photo CDN base: https://assets.cdn.filesafe.space/haYBppQ7i0NmK3BQNKyG/media/
  Steven Gragg:       69cf1cc64cde4bbc2a668bce.png
  John Totin:         69cf1cc6fe4f1c0b67ce5274.png
  Janeene Edwards:    69cf1cc6849c507b52647ef1.png
  Judy Rodriguez:     69cf1cc6a7dcb4cff0459dc4.png
  Steven Noriega:     69cf1cc6fe4f1c0b67ce5271.png
  Kristin Bengoechea: 69cf1cc6fa2dde9742e1a473.png
  Caroline Ramsay:    69cf1cc6fcf3f9acd752a96a.png
  Kimberly Howell:    69cf1cc64cde4bbc2a668bcd.png
  Joey Gonzales:      69cf1cc6849c507b52647ef2.png
  Steve Wilhelmy:     69cf1cc6fe4f1c0b67ce5270.png

### ROOFNation Solutions
Location ID:   fmDrRACH0jn8iOtZxpXG
Webhook UUID:  363cb6a1-bace-4617-af09-5ee7568d1836
Bot Token:     8172572798:AAEpmw3jOq7hY3GKeqXoPWbjaxrP36IgxAE
Tony TG ID:    5609414903
FB Account:    68c5a6f335c709b829d68bba_fmDrRACH0jn8iOtZxpXG_694457683756902_page
IG Account:    68c5a722d08cb7000d275213_fmDrRACH0jn8iOtZxpXG_17841475544975444
SEO Tier:      Pro

### Shot Doctor (P. Scales)
Location ID:   angLr7uC9e5yltccWprT
Webhook UUID:  36a4e946-67ac-46d9-8f81-fa0e2235c4c2
Bot Token:     8747832521:AAFen_J2l2aZplknX3eA51J5OSE_9K0MY6w
FB Account:    69b35c179b61dd367cc24008_angLr7uC9e5yltccWprT_1086702864522663_page
IG Account:    69b431409874e0424e190ce9_angLr7uC9e5yltccWprT_17841442924224691

### CFM Security
SEO Tier:      Basic
Google Ads Account:    217-082-7690 | Login: cfmsecure@gmail.com
Campaign ID:           23642041673 | Ad Group ID: 193984212373
Ad Group Name:         Video views - 2026-03-02
Budget:                $600 total (~$14.77/day) through April 30, 2026
Video Ads:             Ad v1 — CFM Security | Professional (Eligible)
                       Ad v2 — "Wharehouse Heist" (under review)
                       New Video URL: https://youtu.be/3jPIC3vM7aY
Final URL (all ads):   https://cfmsecurityservices.com/booking-page
Website:               cfmsecurityservices.com
Google Video Partners: Enabled
Target geo:            Bessemer, Alabama + 5 more (verify cities)
KPIs to track:         impressions, TrueView views, CPV, view rate, CTR, booking clicks
Google Ads rules learned:
  - Budget type (total vs daily) CANNOT be changed on a live campaign with spend
  - Always set Final URL to booking/conversion page (never homepage)
  - Always apply Google Video Partners on YouTube campaigns
  - Add vertical video (9:16) to enable Shorts inventory
  - Up to 5 videos per ad group for A/B testing
Planned funnel:
  Campaign 1 (live): Awareness — YouTube views
  Campaign 2 (future): Consideration — retarget 50%+ video viewers
  Campaign 3 (future): Conversion — Search, "security company Birmingham" keywords

---

## SEO · AEO · GEO MODULE

Lives inside CC as admin-only Page 5.
Tiers:
  Basic = SEO + AEO     → $500–750/mo + $1,500–2,500 setup
  Pro   = Basic + GEO   → $1,200–2,500/mo + $3,500–5,000 setup

Client tiers: KW Legacy (Pro) | ROOFNation (Pro) | A-One (Basic) | CFM Security (Basic)

GEO = brand mention tracking across ChatGPT, Perplexity, Gemini, Bing Copilot
Branded metric name: "AI Visibility Index"

MAYO pipeline (scoped — NOT YET BUILT):
  - GSC API pull (weekly, per client)
  - GMB stats webhook
  - GEO scraper (headless queries every 48h)
  - GHL blog post scanner + FAQ schema auto-injector
  - Alert trigger → Telegram on rank drop or mention spike

---

## CC DESIGN SYSTEM (CANONICAL)

Aesthetic: Warm editorial dark (Playfair/Lato). REPLACED sci-fi cyan/Space Mono.

CSS variables:
  --void: #0e0c0a | --card: #1e1a14 | --card2: #252019
  --red: #b40101 | --red-dim: rgba(180,1,1,0.12)
  --gold: #c9a84c | --teal: #1d9e75 | --blue: #4a8fc2 | --amber: #d4920a
  --text: #e8e4dc | --text2: #8a8578 | --text3: #4a4640
  --border: rgba(255,255,255,0.06) | --border2: rgba(255,255,255,0.10)
  --serif: 'Playfair Display' | --sans: 'Lato'

Component rules:
  - Cards: border-radius 6px, 1px border, top accent line via ::after
  - Badges: border-radius 2px, 9px Lato 700 uppercase
  - Progress bars: height 2px
  - Section titles: <span class="accent">—</span> Title (red em-dash prefix)
  - Sidebar active: border-left 2px solid var(--red)
  - Kanban: Done=teal, InProgress=red, Review=gold, Backlog=text2

---

## KW LEGACY DESIGN SYSTEM

Colors:
  #4A4A4A = ALL dark backgrounds (NEVER #35322c)
  #b40101 = Red (CTAs, buttons)
  #B8963E = Gold (on light/neutral only)
  #f5d78e = Bright gold (on red/dark only)
  #FAF8F4 = Ivory (light section backgrounds)
  #1A1A1A = Near black (headline text)

Fonts: Playfair Display | Lato | Bodoni Moda

GHL full-width breakout (all sections):
  width:100vw; position:relative; left:50%; margin-left:-50vw; box-sizing:border-box;

GHL override rules:
  - display:flex for rows (not grid/table)
  - !important on ALL critical styles
  - border-radius:0 !important on inputs
  - Custom checkboxes: hide native, sibling <span class="box">

CSS prefix system:
  kwb- kws- kwa- kwc- (forms) | com- (commercial) | kwn- (nav)
  kwf- (footer) | pp- (privacy) | tu- (terms) | sl- (leadership)
  kwbc- (buyers CTA) | cmv1/2/3 (market variants)

Form submit handler (Option B — written, needs paste):
  Each form posts to: https://n8n.srv803590.hstgr.cloud/webhook/hgh-lead-intake
  Source field values:
    "KW Legacy Buyers Form" | "KW Legacy Sellers Form"
    "KW Legacy Agents Form" | "KW Legacy General Contact"

Contact info:
  Office:  1102 E. Sonterra Blvd Suite 106, San Antonio TX 78258
  Phone:   210-482-3200
  Legal:   Spenward Development Group LLC
  Address: 21750 Hardy Oak Suite 102, San Antonio TX 78258
  Email:   support@spenward.com

---

## BEST SELF APP — SCAFFOLD DETAILS

Status: Scaffolded (14 files created) — awaiting Supabase credentials
IDE: Cursor (primary — over VS Code for native Claude integration + Composer mode)
Location: ~/Projects/best-self (or wherever downloaded)
Orchestration: best-self/CLAUDE.md controls agent routing

Architecture:
  iOS:    SwiftUI + MVVM + async/await + Swift Package Manager
  Web:    React 19 + Vite + Tailwind CSS (admin dashboard)
  DB:     Supabase with RLS on all user tables
  Auth:   Supabase Auth email/password (Apple Sign In planned)

Monetization tiers: Free | Premium $9.99/mo | Founding Member $99/yr
Distribution: @changeurperception (1.1M) primary channel

Scaffolded files:
  best-self/CLAUDE.md                                — multi-agent orchestration
  best-self/README.md                                — full setup docs
  best-self/supabase/migrations/001_initial_schema.sql — full schema (10 tables + RLS)
  best-self/supabase/seed.sql                        — 12 achievements + 10 quotes
  best-self/ios/BestSelf/BestSelfApp.swift           — app entry, MainTabView (5 tabs)
  best-self/ios/BestSelf/Models/Models.swift         — all data models (Codable)
  best-self/ios/BestSelf/Services/AuthManager.swift  — auth state management
  best-self/ios/BestSelf/Services/SupabaseService.swift — Supabase REST client
  best-self/ios/BestSelf/Services/AppState.swift     — global state management
  best-self/ios/BestSelf/Views/TodayView.swift       — main dashboard
  best-self/ios/BestSelf/Views/PlaceholderViews.swift — all other views
  best-self/web/package.json                         — React 19 + Vite + Tailwind deps
  best-self/web/src/App.jsx                          — full admin dashboard
  best-self/web/src/lib/supabase.js                  — Supabase client + auth helpers

Supabase tables: profiles, habits, habit_logs, goals, goal_progress,
  journal_entries, inspiration_content, fitness_logs, achievements, user_achievements

CREDENTIAL PLACEHOLDERS (need replacing):
  SupabaseService.swift line 9:  "https://YOUR_PROJECT.supabase.co"
  SupabaseService.swift line 10: "YOUR_ANON_KEY"
  supabase.js line 5:            "https://YOUR_PROJECT.supabase.co"
  supabase.js line 6:            "YOUR_ANON_KEY"
→ Best Self Project ID: ywvxzwntcebzpzjpqtjy (needs rename + schema run first)

iOS design:
  Primary: SwiftUI .indigo | Tab icons: sun/checkmark/target/book/person
  Progress rings: 60x60pt, 8pt stroke | Cards: 16pt corner radius

Web admin design:
  Primary: Indigo-600 #4F46E5 | Background: Gray-50 #F9FAFB
  Cards: bg-white rounded-xl shadow-sm | Sidebar: 256px white

Agent routing pattern (in Cursor with CLAUDE.md):
  "work on the app"     → /ios/
  "work on the admin"   → /web/
  "work on the backend" → /supabase/

Phase 2 features: Apple Health (HealthKit), push notifications (APNs + Supabase Edge),
  offline support, Apple Sign In, RevenueCat paywall, iOS Widgets

---

## CODING TRAINING PROJECT

Status: CLI Lesson 1 + Python Lesson 1 complete
Location: ~/Projects/coding-training
Learning path: CLI → Python → JSON → JavaScript
Purpose: Tony building deep coding fundamentals

Completed files:
  coding-training/README.md                          — curriculum + progress tracker
  coding-training/cli/lessons/01-navigation.md       — pwd, ls, cd, paths
  coding-training/cli/exercises/01-navigation-exercises.md — 4 challenges + answers
  coding-training/cli/cheatsheet.md                  — full CLI reference
  coding-training/python/lessons/01-variables-and-types.md — variables, f-strings, types

Remaining tracks:
  CLI: Lessons 2-3 (file ops, permissions)
  Python: Lessons 2–10
  JSON: all lessons
  JavaScript: all lessons

---

## CLAWDBOT (PLANNED)

Architecture: Claude API → MAYO approval queue → HGH API execution
Every action requires Tony's explicit approval before execution.
Build order: GHL Private App → Firecrawl → Action Library → MAYO approval queue

---

## STANDING TECHNICAL RULES

1. NEVER say "n8n" in UI → always "MAYO"
2. NEVER say "GHL"/"GoHighLevel" in UI → always "HGH"
3. NEVER use HighLevel OAuth2 → Private Integration tokens only
4. All custom auth credentials require "headers" wrapper in JSON
5. Terminal commands in separate copyable code blocks with explanatory text outside
6. Tony's location: San Antonio, Texas

Traefik webhook fix:
  - labels: 4 spaces indented under service name
  - Separate n8n-webhooks router with PathPrefix(/webhook)
  - loadbalancer.server.port=5678
  - After edit: docker compose down && docker compose up -d
  - Re-register Telegram webhook after every restart:
    curl "https://api.telegram.org/bot{TOKEN}/setWebhook?url=https://n8n.srv803590.hstgr.cloud/webhook/{ID}/webhook"
  - DO NOT open Telegram Trigger node in n8n after activating (re-registers test URL)

n8n caption fix pattern:
  const caption = gptResponse?.output?.[0]?.content?.[0]?.text
    || gptResponse?.message?.content
    || gptResponse?.text
    || topic;
  const cleanCaption = caption.replace(/^["']|["']$/g, '').trim();

---

## OPEN TASKS — CONSOLIDATED

### 🔴 NEXT SESSION START
- [ ] Supabase: rename GeekThugga's Project → "Best Self App"
- [ ] Supabase: run 001_initial_schema.sql in SQL Editor
- [ ] Supabase: grab Project URL + anon key
- [ ] Wire Supabase keys into React 19 config

### 🔴 MAYO WIRING
- [ ] Enter Anthropic API key into Claude Custom Auth credential in MAYO
- [ ] Wire MAYO app (index.html) localStorage → Supabase JS SDK
- [ ] Add phone formatter to ROOFNation Prepare Lead + Config node
- [ ] Assign HGH-Authorization KW to Notify Agent node
- [ ] Generate KW Legacy workflow JSON (pattern: ROOFNation J3B1Rw66mXWJWPpg)

### 🔴 FIRECRAWL
- [ ] Get API key at firecrawl.dev
- [ ] Add Firecrawl HTTP node (between webhook and Claude API)

### 🟡 BEST SELF APP
- [ ] Rename GeekThugga's Project → "Best Self App" in Supabase
- [ ] Run 001_initial_schema.sql in Supabase SQL Editor
- [ ] Run seed.sql for sample data
- [ ] Grab Project URL + anon key → replace placeholders in SupabaseService.swift + supabase.js
- [ ] Wire Supabase keys into React 19 config
- [ ] Create actual Xcode project (.xcodeproj)
- [ ] Build HabitsView, GoalsView, JournalView (beyond placeholders)
- [ ] Subscription paywall (RevenueCat or Stripe)
- [ ] Apple Sign In
- [ ] @changeurperception 3-tier pricing funnel page
- [ ] Decide Affirmations Alarm: standalone vs Best Self feature
- [ ] Decide Affirmations Alarm IG account

### 🟡 CFM SECURITY
- [ ] Update Ad v1 Final URL → https://cfmsecurityservices.com/booking-page
- [ ] Confirm Ad v2 goes Eligible after Google review (1–2 hrs)
- [ ] Set up KPI columns in Google Ads dashboard (View Rate, CPV, video played %)
- [ ] Add vertical video (9:16) to enable Shorts inventory
- [ ] Verify target locations (Bessemer + 5 more — confirm all cities)
- [ ] Rename campaign from "Mar 2026" to reflect Apr 30 end date
- [ ] Build Client Metrics module in CC (open MAYO chat, attach sage-unified-hub.html)
- [ ] Build full CFM funnel: Consideration campaign + Search conversion campaign
### 🟡 KW LEGACY
- [ ] Add JS submit handlers to all 4 forms (Option B)
- [ ] Update footer social links (all href="#")
- [ ] Connect CTA buttons to GHL calendar/form URLs
- [ ] Build /associate-leadership-council page
- [ ] Build /preferred-industry-partners page

### 🟡 SEO PIPELINE
- [ ] Build MAYO SEO pipeline (GSC + GMB + GEO scraper + schema injector + alerts)
- [ ] Push sage-unified-hub.html to GitHub Pages
- [ ] Wire SEO module footer buttons to MAYO webhooks
- [ ] Replace static SEO demo data with live feeds

### 🟡 ROOFNATION
- [ ] Build ROOFNation homepage
- [ ] Build ROOFNation full marketing funnel

### 🟢 DEFERRED
- [ ] Coding Training: CLI Lessons 2–3, Python Lessons 2–10, JSON track, JavaScript track
- [ ] MAYO Infrastructure Supabase schema (clients/posts/executions tables)
- [ ] HGH Client Build Status module → CC
- [ ] Obsidian vault: SIMG-Brain (Templater, Dataview, QuickAdd, Git, Tasks)
      + obsidian-local-rest-api → MAYO webhook
- [ ] Clawdbot full build
- [ ] Alchemic AI Wellness app (after Best Self)
- [ ] Bond AI app (after Alchemic)
- [ ] Vaso Group pilot (blog + LinkedIn automation, contact: Reggie Jonaitis rjonaitis@vasogroup.com)
- [ ] Import ChatGPT conversation history into Claude
- [ ] Chief Aim / $1M/mo strategy session

---
# END CLAUDE.md — April 12, 2026
