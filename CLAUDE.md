# CLAUDE.md — SIMG / MAYO Infrastructure
# Orchestration file for Claude Code (Cursor) and all Claude chat sessions
# Last updated: April 12, 2026

---

## WHO I AM

Tony Edwards (GeekThugga)
Founder — Sage It Media Group (SIMG)
Goal: $100K/month → $1M/month via consumer apps + 2M+ follower distribution

---

## COMPANY STRUCTURE

```
Sage It Media Group (SIMG) — parent
├── Hyper Growth Hub (HGH) — GoHighLevel agency
└── Spenward Development Group (SDG) — co-founded with Bryant Spencer
    ├── Clients: KW Legacy, A-One Facility Services, CFM Security
    ├── DBAs: ROOFNation Solutions, Growco Landscaping, Thermal Exchange
    └── Pipeline: 20/20 Optical, Corporate Facility Management
```

---

## CRITICAL LANGUAGE RULES — NEVER BREAK THESE

| Never say | Always say |
|---|---|
| n8n | MAYO |
| GoHighLevel / GHL | HGH |
| database | data layer |
| Supabase (in UI copy) | data infrastructure |

These rules apply to ALL UI text, button labels, tooltips, page titles, and any copy that appears inside CC, CCSD, or the MAYO app. Technical comments in code are fine.

---

## FILE SHORTHAND

| Shorthand | Full filename | What it is |
|---|---|---|
| CC | `sage-unified-hub.html` | SIMG unified command center |
| CCSD | `spenward_command_center.html` | SDG/Spenward CRM |
| MAYO app | `index.html` (this repo) | MAYO Infrastructure portal |
| ROOFNation funnel | `roofnation-funnel.html` | ROOFNation landing + thank you |

---

## DESIGN SYSTEMS

### SIMG / CC Design System
```css
--bg:          #0e0c0a   /* warm dark background */
--red:         #b40101   /* primary accent */
--gold:        #c9a96e   /* secondary accent */
--teal:        #2a9d8f   /* tertiary accent */
--blue:        #264653   /* supporting */
--white:       #ffffff
--light:       #FAF8F4   /* ivory / light sections */

Fonts:
  Display:  Playfair Display (serif)
  Body:     Lato 300 / 400
  UI:       Barlow Condensed

Rules:
  - Warm editorial dark aesthetic
  - Never use pure black (#000000) — use #0e0c0a
  - Red accent for active states, CTAs, highlights
  - Gold for secondary emphasis
```

### Spenward / CCSD Design System
```css
--primary:     #157efb   /* blue — Spenward brand */
--gradient:    linear-gradient(135deg, #157efb, #4da3ff)
--dark:        #0a0a0a
--dark2:       #111111

Fonts:
  Display:  Orbitron (400–900)
  Body:     Montserrat (300–700)

Rules:
  - Blue scheme is SEPARATE from SIMG red palette
  - Full-width breakout: calc(-50vw + 50%) technique
  - Mobile-first, all blocks responsive
```

### KW Legacy Design System
```css
--dark-bg:     #4A4A4A
--red:         #b40101
--gold:        #B8963E
--bright-gold: #f5d78e   /* on red backgrounds */
--ivory:       #FAF8F4

Fonts: Playfair Display, Lato, Bodoni Moda

Full-width breakout: width:100vw; position:relative; left:50%; margin-left:-50vw;
```

### ROOFNation Design System
```css
--red:         #b82429
--dark:        #1a1a1a
--dark2:       #222222

Fonts: Bebas Neue (display), Barlow Condensed, Barlow

Phone format: Always E.164 (+1XXXXXXXXXX) for GHL API
```

### MAYO App Design System
```css
Light mode:
  --bg:        #F8F9FC
  --purple:    #7C5CFC
  --cyan:      #00B8D9
  --green:     #00C48C

Dark mode:
  --bg:        #0A0A0F
  --purple:    #9B6DFF
  --cyan:      #00D4FF

Fonts: DM Sans (body), Montserrat 700–900 (display)
```

---

## INFRASTRUCTURE

```
VPS:           srv803590.hstgr.cloud (IP: 46.202.93.220)
MAYO (n8n):    https://n8n.srv803590.hstgr.cloud
MAYO login:    tony@sageitmedia.com
Docker:        /root/docker-compose.yml
.env:          /root/.env
```

### Supabase Projects
```
Org: Sage It Media
  └── Best Self App (ywvxzwntcebzpzjpqtjy) — consumer app

Org: Spenward Development Group  
  └── MAYO Infrastructure App (zewzbfgefqsajnrdrmyr)
      URL: https://zewzbfgefqsajnrdrmyr.supabase.co
      Tables: clients
```

### MAYO Supabase clients table schema
```sql
create table clients (
  id uuid default gen_random_uuid() primary key,
  client_name text not null,
  location_id text,        -- GHL sub-account location ID
  private_token text,      -- GHL private integration token
  telegram_chat_id text,   -- for Telegram notifications
  webhook_path text,       -- n8n webhook path/UUID
  account_tag text,        -- tag applied to HGH contacts
  claude_prompt text,      -- full system prompt for Claude
  active boolean default true,
  created_at timestamp default now()
);
```

---

## ACTIVE CLIENTS (HGH Sub-accounts)

| Client | Location ID | Status | Tier |
|---|---|---|---|
| KW Legacy | haYBppQ7i0NmK3BQNKyG | ACTIVE | Pro (SEO+AEO+GEO) |
| ROOFNation Solutions | fmDrRACH0jn8iOtZxpXG | ACTIVE | Pro |
| A-One Facility Services | TBD | ACTIVE | Basic |
| CFM Security | TBD | ACTIVE | Basic |
| Shot Doctor (P. Scales) | angLr7uC9e5yltccWprT | ACTIVE | Client |

---

## MAYO WORKFLOW STANDARD TEMPLATE

Every client workflow follows this exact pattern:
```
HGH Lead Webhook
      ↓
Fetch Client Config (Supabase GET — reads clients table)
      ↓
Prepare Lead + Config (Code node — merges webhook + Supabase data)
      ↓
Claude API (HTTP Request — Claude Custom Auth credential)
      ↓
Parse Claude Response (Code node)
      ↓
Create HGH Contact (HTTP Request — token from Supabase)
      ↓
Telegram Notify + Webhook Response (parallel)
```

### Claude API node standard config
```
Authentication: Generic Credential Type
Generic Auth Type: Custom Auth
Custom Auth: Claude Custom Auth  ← always this saved credential
Send Headers: OFF
Send Body: ON
Body Content Type: Raw
Content Type: application/json
Body: ={{ JSON.stringify({...}) }}
```

### Token management rule
- GHL private integration tokens NEVER live in MAYO credentials
- Tokens always stored in Supabase clients table
- MAYO workflow fetches token fresh from Supabase on every execution
- To rotate a token: update the Supabase row — all workflows pick it up automatically

---

## APP PORTFOLIO

| App | Channel | Followers | Status |
|---|---|---|---|
| Best Self App | @changeurperception | 1.1M | IN BUILD |
| Alchemic AI Wellness | @alchemichealing | 588K | PLANNED |
| Bond AI Relationship | TBD | — | PLANNED |
| Affirmations Alarm | TBD | — | PLANNED |

Stack: React 19 + Supabase + iOS SwiftUI
IDE: Cursor (Claude Code + Composer mode)
Pricing model: Free / $9.99/mo / $99/yr founding member

---

## GITHUB REPOS

```
Account: GeekThugga
mayo-infrastructure    → geekthugga.github.io/mayo-infrastructure
sage-command-center    → geekthugga.github.io/sage-command-center
spenward-command-center → geekthugga.github.io/spenward-command-center
curated-kw-legacy      → push pending
```

---

## SESSION PRIORITIES (update each session)

### ✅ COMPLETED
- All 5 GHL private integration tokens created and in MAYO
- MAYO Claude → HGH workflow confirmed live (ID#322, 3.7s)
- ROOFNation funnel live at roofnationsolutions.com/free-inspection
- MAYO Supabase clients table created with RLS policy
- ROOFNation row inserted into clients table
- Telegram notification node wired (Chat ID: 5609414903)

### 🔴 PRIORITY 1
- Firecrawl → MAYO (get API key, build HTTP node, test enrichment)

### 🟠 PRIORITY 2 (parallel)
- Wire MAYO app (index.html) to Supabase clients table (replace localStorage)
- Best Self App Supabase: rename project → run schema → grab keys → wire
- CFM Security Google Ads cleanup (Ad v1 URL, Ad v2 eligibility, KPI columns)
- SEO/AEO/GEO live data wiring in CC

### 🔵 PRIORITY 3 (parallel)
- ROOFNation homepage build (needed before ads)
- MAYO prompt library in Supabase
- Instagram monetization funnel build
- Obsidian vault → MAYO

### ⬜ BACKLOG
- Clawdbot (implement Claude directly, needs Firecrawl first)
- Instantly → MAYO → HGH
- Fiverr gig catalog (10 gigs scoped)
- Bond AI + Alchemic AI (after Best Self ships)
- Coding training continuation

---

## HTML FILE UPDATE RULES

When updating CC, CCSD, or MAYO app:
1. Always paste the CURRENT file into the chat before asking for changes
2. Claude makes targeted edits — never rebuilds from scratch unless asked
3. Claude outputs the COMPLETE updated file — never partial
4. Always preserve existing sections when adding new ones
5. Download the output and commit to GitHub after every session
6. Never use localStorage for persistent data — always Supabase

---

## HOW TO START A NEW CHAT

Paste this at the top of every new Claude session:

```
I am Tony Edwards (GeekThugga), founder of Sage It Media Group (SIMG).

KEY RULES:
- MAYO = n8n (never say n8n in UI)
- HGH = GoHighLevel (never say GHL in UI)  
- CC = sage-unified-hub.html
- CCSD = spenward_command_center.html

Read my CLAUDE.md for full context: [paste this file]
Master Handoff: [attach SIMG_Master_Handoff.docx]

Files for this session:
[attach relevant HTML files]

Today's task: [describe what you need]
```

---

## HOW TO EXTRACT INSTRUCTIONS FROM OLD CHATS

Paste this prompt into any old Claude chat to extract what's needed:

```
Please do the following for this conversation:
1. SUMMARY — 3-5 sentence overview of what we built
2. COMPLETED BUILDS — every file with name and description  
3. CREDENTIALS & IDs — all API keys, tokens, webhook URLs (mark any that may be rotated)
4. DECISIONS MADE — strategic and technical decisions locked in
5. DESIGN DECISIONS — colors, fonts, layout rules, component patterns used
6. HTML/CSS RULES — any specific rules about how components were built
7. UNFINISHED TASKS — everything started but not finished
8. CLAUDE.md ADDITIONS — list anything from this chat that should be added to CLAUDE.md

Format as a copy-paste block for the SIMG Master Handoff.
```

---
*This file is the source of truth for all Claude sessions on SIMG projects.*
*Update after every major session. Commit to GitHub after every update.*
