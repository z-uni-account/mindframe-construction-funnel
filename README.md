# Mindframe Construction VSL Funnel

> **What this is:** Mindframe Media's VSL landing page for the **construction / trades vertical** (the agency offer that sells done-for-you lead-gen to construction & roofing companies). Modeled on vslqueen.com: Step 1 watch the training video → Step 2 book a call, with the booking form **embedded on the same page** (Jeremy Haynes: inline embed converts better than sending traffic off-site). This is the start of the construction funnel; **we keep iterating on it through Claude.**

Started 2026-07-01. Sibling to the SoCal roofing quiz funnel (`context/roofing-funnel/`), but different: that's a client (Julio) quiz form; this is Mindframe's own top-of-funnel VSL offer page.

**Build doctrine → `reference/jeremy-haynes-sops/call-funnel-scaling.md`** (Jeremy Haynes' $1M/mo call-funnel playbook, watched 2026-07-02). Refer to it for every structure/tech decision on this funnel. Key rulings already applied to our thinking: **VSL → embedded application → integrated scheduler, NO opt-in**; **Typeform + Calendly** is the proven low-CPC stack and they must be **integrated** (16% drop-off) not split across pages (50% drop-off); booking window 24–72h; qualify in the form + capture phone from everyone so disqualified leads route to the setter for education-first text outreach.

---

## Live + source

| | |
|---|---|
| **Live URL (share this)** | **https://mindframeconstruction.com/** — custom domain live 2026-08-02. The old `https://z-uni-account.github.io/mindframe-construction-funnel/` still resolves; use the real domain everywhere. |
| **Pre-call page** | **https://mindframeconstruction.com/confirmation** (clean path; `confirmation.html` kept as a fallback). |
| **Domain / DNS** | `mindframeconstruction.com` at **Porkbun** (acct Goatarreola), registered 2026-08-02, expires 2027-08-02. DNS: **ALIAS** apex → `z-uni-account.github.io` + **CNAME** `www` → `z-uni-account.github.io`. Porkbun's ALIAS flattens to GitHub's four IPs, so no manual A records. MX/SPF left intact for Porkbun email forwarding; the two `_acme-challenge` TXT records are Porkbun's own SSL validation and are harmless. **Never use a `*.mindframemedia.us.com` subdomain** — see [[domain_standard]]. |
| **Host** | GitHub Pages (free, static, auto-builds on push). Chosen after Z declined Vercel. |
| **Deploy repo** | `github.com/z-uni-account/mindframe-construction-funnel` (public, gh account `z-uni-account`) |
| **Source of truth** | `construction-landing/vsl-funnel/index.html` (single self-contained file, tracked in the ops repo) |
| **Assets** | `construction-landing/vsl-funnel/assets/logos/` |

The GitHub repo is the deploy target only. **Edit the source here in the ops repo, then push a copy to the deploy repo** (recipe below). Do not treat the scratchpad clone as canonical — it's throwaway.

---

## Brand decisions (locked)

- **Brand = Mindframe Media.** Wordmark in nav + footer. Theme = the Mindframe aesthetic: near-black `#080808`, red accent `#dc2626`, Inter font, warm red glow + subtle grain. (Same tokens as `construction-landing/app/globals.css`.)
- **E3 and Quest MS Construction are CLIENTS**, shown in the "worked with" logo strip. They are NOT the brand. (Z corrected this explicitly: "e3 is supposed to be one of the clients... the branding logo and business is mindframe.")
- Logos in the strip render in **full color** (not grayscale) so they read as real social proof.
- **No em dashes in visible UI copy** (standing Mindframe UI rule).

---

## To go live / edit: the CONFIG block

Everything swappable lives in one JS `CONFIG` object at the bottom of `index.html`. Edit only that:

1. ~~**`vslEmbedUrl`**~~ — **NOT IN USE, LEAVE IT EMPTY.** The live VSL is a **Vidalytics** embed hard-wired into the markup (see next section). Vidalytics ships a `<div>` + a loader `<script>`, and this CONFIG path builds an `<iframe>` from a URL — it cannot run their script. **Setting `vslEmbedUrl` would overwrite the Vidalytics player.** The field still works if we ever move back to YouTube/Vimeo/Wistia.
2. **`bookingUrl`** — paste a **Calendly OR Tally** link. Auto-detects which, embeds inline, and themes Calendly dark to match. Empty = placeholder.
3. **`logos`** — array of `{src, alt}`. Drop logo files in `assets/logos/` first. Remaining strip spots auto-fill with dashed placeholder slots (up to 5 total).

Placeholder copy still to replace with real content: hero sub-headline specifics, footer email (`hello@mindframemedia.com` placeholder), Privacy/Terms links (currently `#`).

**Removed 2026-07-05 (Z):** the fabricated **stats band** (8.4× / $42 / 7 days / 100%) and the 3 placeholder **results/testimonial cards** are both deleted from `index.html` (no fake numbers on a live page). Page now flows: hero+VSL → booking → logo strip → comparison → final CTA. Re-add real versions once we have documented construction wins.

## VSL video — VIDALYTICS, WIRED 2026-08-02 ✅

The page finally has its video. **Live: "Jonathan VSL 2 (Construction)"** — 2:35, 1920x1080 (16:9).

| Piece | Value |
|---|---|
| Host | **Vidalytics** (Z's account, already signed in in his browser) |
| Vid ID | **`BqowP6uVrPPVM_fd`** |
| Account / embed path | `JwcAQOTG` → `https://fast.vidalytics.com/embeds/JwcAQOTG/BqowP6uVrPPVM_fd/` |
| Source file | `~/Desktop/Mindframe Main/Jonathan Construction VSL/Jonathan VSL 2.mp4` |
| Why this cut | Outcome-first opener, and the only cut on film that says "GC in Orange County." Reasoning: `plans/2026-08-02-construction-vsl-launch.md` |

**How it's wired (and why not through CONFIG):** the embed sits directly in the markup — the `<div id="vidalytics_embed_…">` inside `.video-frame`, with Vidalytics' loader `<script>` immediately after the `.video-wrap`. It has to be inline because **a `<script>` injected via `innerHTML` never executes**, which is what the CONFIG/iframe path would have done.

**One CSS gotcha:** `.video-frame` sets `aspect-ratio:16/9` and Vidalytics' own div sets `padding-top:56.25%`. Both are 16:9, but stacking them makes the player clip. Fixed with the **`.video-frame--live`** class (`aspect-ratio:auto`) on the frame, letting Vidalytics' padding govern the height. Keep that class if you swap the video.

**Verified 2026-08-02:** served locally, player loads, autoplays muted with the "Tap to Unmute" overlay, fills the frame edge-to-edge, page flows normally into the application below.

### Swapping the VSL (e.g. after the ad hook test picks a different opener)
All four cuts are uploaded, so a swap is a copy-paste, not a re-shoot:
1. In Vidalytics, open the new Vid → **Embed/Share → Inline Embed → Copy Code**.
2. In `index.html`, replace **both** the `<div id="vidalytics_embed_…">` and the **two** ids in the loader script's final line (they must match).
3. Keep `.video-frame--live` on the frame and keep `CONFIG.vslEmbedUrl` empty.
4. Redeploy (recipe below).

> **Do NOT turn on Vidalytics' split-testing / Experiments feature.** A page split test plus the ad hook test = four combinations and no attributable winner, at a traffic level that can't read two. Sequence them: ad test first, page test only after it resolves. Full reasoning: `plans/2026-08-02-construction-vsl-launch.md`.

## "The Operator" / about section — ADDED 2026-08-02 (`#about`)

Sits **between the comparison table and the final CTA**, on purpose. Jeremy AI: the qualified prospect converts at the application right under the video and never scrolls this far, so *"the about section isn't for him. It's for the guy who watched the video, saw the application, and didn't fill it out."* On-page retargeting for the hesitater. Its one job is to answer **"can this guy actually do what he just described in the video?"** — not biography.

- **Header:** eyebrow "Who Runs Your Demand" + h2 *"The guy behind the camera is the guy running your ads."* Deliberately **not** "About Us" — the frame is capability, not personality.
- **AUTO-SLIDING CAROUSEL, 4 photos** (Z, 2026-08-02). Jeremy AI's objection to a carousel was that it is interaction-dependent and scrollers will not swipe — **auto-advance answers that**, since every slide is seen without a tap. **Keep it auto.** If it is ever switched to manual-only, most visitors will see slide 1 and the other three are wasted.
  - 4.5s per slide, `translateX` on `.about__track`. Pauses on hover, focus, touch, and when the tab is hidden. Swipe works for anyone who wants to drive it. Honours `prefers-reduced-motion`. Dots are real buttons with `role="tab"` + `aria-selected`.
  - **Order tells the job start to finish:** identity → script prep → directing the shoot → the client on camera.
- **Photos** (`assets/about/`, from a real client shoot 2026-05-30, photographer @NateThaPhilosopher, source: Lightroom share `83912753fcfe4c5384996ac313d88692`, 55 frames):
  - `operator-on-site.jpg` — identity shot, Jonathan with the camera rig on a residential site. Reads "operator" in half a second.
  - `reviewing-script.jpg` — going through the script with the client, camera on the tripod beside them.
  - `directing-the-shoot.jpg` — Jonathan behind the camera running the take.
  - `client-on-camera.jpg` — the client mid-take in front of his branded van, with "…arger Installation / …ical Panel Upgrades" legible, which reinforces the trade list in the hero pill.
  - Unused alternates prepped in the same folder: `filming-client.jpg` (over-the-shoulder, camera + client) and `script-review.jpg`.
- **⚠️ ONE client face on this page, and only one (Z, 2026-08-02).** Jeremy AI: *"The branded van is fine because a van isn't a name. A face is."* A second client-facing photo starts naming clients by proxy. Any future photo added here must not show a client's face.
- **Captions must describe only what the frame shows.** `client-on-camera.jpg` has no camera in it (just the script at the right edge), so its caption says "mid-take", not "filming". Image 1 carries the camera.
- **"Five years" is Z's confirmed number** (2026-08-02). Do not soften back to "years of experience" — the hedge reads like a resume template, a number reads verifiable.
- **Known cosmetic:** the client's phone number is legible on the van in `client-on-camera.jpg`. Left as-is for authenticity; blur or re-crop if it ever reads as a distraction.

## Post-booking confirmation page — BUILT 2026-07-05 (`confirmation.html`)
Sibling page shown AFTER a lead books, where show-rate is won (Jeremy doctrine → `construction/vsl-doctrine.md`). Self-contained, same brand as `index.html` (same tokens, grain, glow, nav/footer, no em dashes). Sections top-to-bottom: booking-confirmed hero (green check) → **"Accept your invite"** with a CSS-built Gmail mock highlighting **"I know this sender"** (the unknown-sender fix, responsive, no screenshot asset needed) → **main video** (3-5 min) → **6 breakout videos** (CONFIG-driven grid) → testimonials with the honest transferable-proof framing line → **add-decision-maker** CTA (mailto-forward fallback). Verified desktop + mobile.

- **Edit only the `CONFIG` block** (bottom of the file): `mainVideoUrl`, `breakouts[].url` (6 slots), `addDecisionMakerUrl`, `supportEmail`. Videos accept YouTube/Vimeo/Wistia; empty = branded placeholder. Titles/descriptions are locked to the scripts.
- **DEPLOYED + LIVE 2026-07-05:** https://z-uni-account.github.io/mindframe-construction-funnel/confirmation.html (200). Deployed site files only (index.html + confirmation.html + assets); the ops README is intentionally NOT pushed to the public repo.
- **Redirect WIRED 2026-07-05:** GHL calendar `A1zzJxrPKW1feORjJg4r` set to `formSubmitType: RedirectURL`, `formSubmitRedirectUrl` = the confirmation URL (via REST PUT, browser UA — MCP `update_calendar` doesn't expose it). Booked leads now land on the confirmation page automatically.
- **Still to finish:** film + paste the 7 videos (main + 6 breakouts) into the CONFIG block; optionally swap the CSS Gmail mock for a real screenshot.

## Meta pixel — INSTALLED + LIVE 2026-08-02

**Pixel `1611734647006601` — "Mindframe Construction Pixel"** (account `act_870605539364297`). Base code sits in the `<head>` of **both** `index.html` and `confirmation.html`.

| Page | Fires |
|---|---|
| `index.html` | `PageView` |
| `confirmation.html` | `PageView` + **`Schedule`** |

`confirmation.html` is only reachable after a booking (the GHL calendar redirects there), so a hit on it *is* a booked call. That makes `Schedule` a legitimate conversion signal for the construction funnel.

**Two rules that matter:**

1. **Never put the tattoo pixel (`1352232626711265`, "Tattoo Funnel Pixel 2") on these pages.** It lives in the same ad account. A pixel that has spent years firing on tattoo-artist conversions has learned who converts for *that* offer and will bias early delivery on a construction campaign. This was the whole reason a fresh pixel was created (Jeremy AI, 2026-08-01 — `reference/ad-strategies/jeremy-ai-advisor-notes.md`).
2. **Sending `Schedule` is not the same as optimising on it.** Do NOT switch a construction ad set to optimise for Schedule until booked volume holds around 15/week — a rare event starves learning. Same rule we run on the tattoo side (memory `meta_booked_schedule_capi`, `reference/ads-bible.md` §6).

**Verified live 2026-08-02:** both URLs return 200 with 3 pixel references each, `PageView` on both, `Schedule` on the confirmation page only, and zero references to the tattoo pixel.

**Not yet wired:** no `Lead` event on the Tally application submit. The form is an embed, so that needs either a Tally webhook to CAPI or a redirect step. Worth doing before the funnel spends meaningfully, since `Lead` is the event a cold VSL campaign would actually optimise on.

## Qualifying application (Tally prototype — BUILT 2026-07-02)
- **Form:** `Mindframe: Free Construction Growth Call` — **formId `zxO8l8`**, live at **https://tally.so/r/zxO8l8** (workspace `mYZkRd`). Dark theme (bg #0a0a0a, accent #dc2626, Inter) to match the page.
- **Flow (9 pages, one question per screen):** intro → trade type → owner/decision-maker → monthly revenue → how they get jobs now → #1 bottleneck → ready-to-invest → name/email/**phone (all required)** → **Calendly embedded inline as the final step** (Jeremy's integrated app+scheduler; avoids the 50% split-page drop-off).
- **Attribution:** hidden fields wired (utm_source/medium/campaign/content/term + ad_id), same as the roofing form.
- **AUTO-JUMP ON (enabled 2026-08-02, Z):** picking an option now advances to the next page on its own, no Next click. Form-level setting, Tally **Settings → Behavior → Auto-jump to next page** ([docs](https://tally.so/help/form-settings)). Works because every one of pages 1-7 holds a single multiple-choice question; it only applies to multiple choice / dropdown / rating / linear scale, and **never auto-submits** — page 8 (name, company, email, phone) still needs a manual Submit, which is what we want. **Verified live 2026-08-02:** clicking through pages 1 → 2 → 3 advanced on selection alone.
  - ⚠️ **MCP gotcha:** the field is **`pageAutoJump`**, not `autoJumpToNextPage`. `update_settings` accepts an unknown key **silently, returning `{}`**, and `save_form` reports success, so a wrong field name looks like it worked and changes nothing. Always re-test the live form after a settings change rather than trusting the response.
  - ⚠️ **Watch the "Other" option** on the trade question: auto-jump may advance the moment it is picked, before the respondent types their free-text answer. Q1 is being rewritten anyway (trade list + disqualify logic) — re-test that specific option afterwards.
- **No hard disqualify branch** — everyone reaches the calendar and phone is captured from all, so disqualified/unbooked leads route to the setter for education-first text (Z's requirement + Jeremy's setter system). (Construction setter TBD; note the MF tattoo funnel setter changed 7/5 — Quadra out, Angelica + Beyoncé in — see [[setter_hire_q2]]. Don't assume Quadra anywhere.)
- **Placeholder in prototype:** the Calendly embed points at `https://calendly.com/mindframemedia/growth-call` (not a real link yet) → swap for the real Mindframe Calendly.

## Booking calendar — DECISION 2026-07-02: GHL calendar, NOT Calendly
Chose a **GoHighLevel calendar** embedded as the Tally form's final step, over Calendly.
- **Why GHL:** everything already runs in GHL (native pipeline/setter, no webhook bridge), I can build + own it via API, no extra tool/cost, bookings land natively for setter follow-up, clean CAPI attribution.
- **Cost of the choice (watch these):** GHL widget loads a notch heavier than Calendly on cold traffic (MindFrame already learned GHL funnels raise CPL on cold paid traffic — but here only the *calendar* embeds, the form stays fast Tally); it steps off Jeremy's validated Typeform+Calendly combo; name/email prefill into the embed needs URL-param wiring (not automatic). Full gain/loss analysis was given to Z 2026-07-02.
- **Separation: DEDICATED GHL SUB-ACCOUNT** (Z's call — must be fully walled off from the tattoo funnel). **Calls go to Jeremiah.**
### Sub-account (PROVISIONED 2026-07-02)
- **Location:** `MF - Construction Offer` · **locationId `FVr2fNAYZQIWomzTaO6E`** · companyId `DezhZBsBOcaeWuulKMrL` · tz America/Los_Angeles.
- **Token:** Private Integration token stored in `.mcp.json` under the **`ghl-construction`** MCP server (gitignored, private repo — NOT in any tracked file). Activates as `mcp__ghl-construction__*` tools next session; this session used curl. Token is location-scoped (agency-level calls 403; can't create users via API → see below). GHL API writes need a browser User-Agent header (Cloudflare 1010 blocks Python's default client; curl works).
- **RESET DONE 2026-07-02:** was leftover IG-DM/Adrian scaffolding (never took off). Deleted 6 DM tags + 9 DM custom fields + the "DM leads Pipeline". Account had 0 contacts / 0 opps. Built a clean **`Construction Sales` pipeline** (id `80Vx9nY7YtiCKRIrP1sq`): New Application → Call Booked → Showed → Won → Lost. Now 0 calendars / 0 workflows / 0 forms / 0 users.

### Calendar — BUILT + EMBEDDED 2026-07-02 ✅
- **Jeremiah Acosta** (closer) added as user `OYsliWXb3I97BvikCYiy` (`acostagabriel79@gmail.com`) to the sub-account (agency-UI step; API add fails "user already exists"). His Google calendar connection carried over — availability is live.
- **Calendar:** `Construction Client Growth Call` · id **`A1zzJxrPKW1feORjJg4r`** · slug `construction-growth-call` · round_robin/Jeremiah · 30-min · allowBookingFor 7 days · allowBookingAfter 0h · autoConfirm. (Mirrors his tattoo "Mindframe Strategy Call" `NypMzSwn2MR7kzkH4GQq`. Note: Jeremy's doctrine says cap booking window at 72h for show rate — currently 7 days to match his tattoo setup; tighten later if show rate dips.)
- **Booking widget:** `https://api.leadconnectorhq.com/widget/booking/A1zzJxrPKW1feORjJg4r` (200, iframe-embeddable, no X-Frame-Options).
- **Embedded** as the Tally form's final step (form `zxO8l8` page 9, block `c98b1559-a9da-416a-9a15-98f2def2a4fb`, replaced the Calendly placeholder). Free slots verified populating (8am–4:30pm weekdays).
- **Polish TODO:** Tally embed height defaults to 400px — may show an internal scrollbar on the calendar; bump height for cleaner display.

### Next wiring (not blocking; makes leads flow)
1. [ ] **Form → GHL:** webhook form `zxO8l8` → create GHL contact + opportunity in the `Construction Sales` pipeline (`80Vx9nY7YtiCKRIrP1sq`), capture phone from ALL (disqualified → Jeremiah/setter). Recreate attribution custom fields (utm_*, ad_id) to match the form's hidden fields.
2. [ ] Booked call → CAPI Schedule (mirror tattoo bridge) once volume warrants.
3. [x] Tally form embedded into the VSL page (`bookingUrl` CONFIG = `https://tally.so/r/zxO8l8`) via Tally's official embed.js → auto-resizes per step. On submit, `redirectOnCompletionUrl` → native GHL calendar page (self-sizing, reliable). `book.html` custom booking page was tried + removed (form_embed.js collapsed the iframe). Booking-page branding = future GHL white-label. Verified 2026-07-02.

## Status — other pending
- [ ] Decide: embed the Tally form into the VSL page (`bookingUrl` in CONFIG) vs run it standalone
- [ ] **VSL video link** (nothing recorded yet → placeholder showing)
- [ ] More **client logos** (have E3 + Quest; 3 slots open)
- [ ] Real testimonial/results numbers

---

## Redeploy recipe (any change → live)

Source lives in the ops repo; the deploy repo is a separate GitHub repo. After editing `index.html` (or assets) here:

```bash
SRC="construction-landing/vsl-funnel"
D="$(mktemp -d)/deploy"
git clone -q https://github.com/z-uni-account/mindframe-construction-funnel.git "$D"
cp -R "$SRC/." "$D/"          # sync source -> deploy clone (README/.git preserved by clone)
touch "$D/.nojekyll"           # required so /assets and dotfiles serve
cd "$D" && git add -A \
  && git -c user.email="ops@mindframe.local" -c user.name="MindframeOps" commit -m "update funnel" \
  && git push origin main
# Pages rebuilds in ~1-5 min. Verify:
#   gh api repos/z-uni-account/mindframe-construction-funnel/pages/builds/latest --jq .status  (want: built)
#   curl -so /dev/null -w "%{http_code}" https://z-uni-account.github.io/mindframe-construction-funnel/
```

> **OPEN 2026-07-06 — GH Pages build wedged.** The `index.html` results/stats removal is committed and correct in the repo (verified 0 placeholder occurrences), and `confirmation.html` is LIVE (200). But GitHub Pages has been stuck on `status: building` for the results-removal commit for 13+ hours (Pages component reports "operational", so it's a per-repo wedge, not an outage). Pushed an empty commit + `POST /pages/builds` to force a rebuild — still queued. **Action next session:** re-check `gh api repos/z-uni-account/mindframe-construction-funnel/pages/builds/latest --jq .status`; if still `building`, push another commit or re-request a build until `built`, then confirm live `index.html` has 0 "Real Results". The live site keeps serving the last good build meanwhile.

Notes:
- `.nojekyll` MUST be present or GitHub Pages ignores the `assets/` folder.
- First build after enabling Pages can take several minutes; later pushes rebuild in ~1-2 min. CDN caches assets, so append `?v=N` when checking a freshly changed file.
- To enable Pages on a fresh repo: `gh api -X POST repos/<owner>/<repo>/pages -f "source[branch]=main" -f "source[path]=/"`.

## Pulling a client's logo from their Instagram (reusable)

Client profile pic = their logo. Instagram's public endpoint works without login:

```bash
curl -s 'https://www.instagram.com/api/v1/users/web_profile_info/?username=<handle>' \
  -H 'x-ig-app-id: 936619743392459' \
  -H 'User-Agent: Mozilla/5.0 ... Chrome/120.0 Safari/537.36'
# JSON path: data.user.profile_pic_url_hd  (320px) — download it, drop in assets/logos/, add to CONFIG.logos
```

Used this to pull E3 and Quest MS Construction (@questmsconstruction). Beats spinning up an Apify actor for a single profile pic.

## Verifying mobile (headless quirk to know)

Chrome `--headless --screenshot --window-size=375,...` renders at a **500px minimum viewport** and captures a 375px slice → content looks falsely "cut off." That is a capture artifact, not a real overflow. To test mobile for real, drive CDP with `Emulation.setDeviceMetricsOverride {width:390, mobile:true}` (Node 24 has a global `WebSocket`), then check `document.documentElement.scrollWidth === innerWidth` and screenshot with `Page.captureScreenshot {captureBeyondViewport:true}`. Verified 2026-07-01: zero horizontal overflow at 390px.
