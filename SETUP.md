# The Late Block on X: stream setup

**What you get:** guests on Google Meet → OBS adds the Kem overlay (live price ticker plus topic lower third) → OBS streams to X over RTMPS and records a full-quality copy for the clippers.

Files in this folder (copy the **whole folder** to the streaming computer, including `assets/`):
- `overlay.html`: the overlay (1920×1080, transparent)
- `control.html`: buttons for switching topics during the show
- `preview.png`: what the overlay looks like

---

## 1. Before the first show (one-time)
1. Install **OBS Studio** (free, obsproject.com).
2. In OBS **Settings → Video**, set Base and Output resolution to **1920×1080** and FPS to **30**.
3. Use headphones on the streaming computer so Meet audio doesn't echo back into your mic.

## 2. Capture the Meet call
1. Join the Meet in its **own Chrome window** and make it large (full screen on a second monitor works best). Choose a layout that shows the guests nicely, such as Tiled or Spotlight.
2. In OBS, add a source: **Window Capture** (Windows) or **macOS Screen Capture → Window** (Mac), and pick the Meet window.
   Crop off the browser bars (hold Alt/Option while dragging an edge) and scale the video to fill the **video window**: x 12→1908, y 69→917 (1896×848), i.e. between the ticker, the two slim side gutters and the bottom block (see `preview_light.png`). With `?frame=0` the window is the full width (0→1920).
3. **Meet audio (the guests):**
   - Windows: add an **Application Audio Capture** source and pick Chrome. If it's missing, use **Desktop Audio**.
   - Mac (macOS 13+): in the macOS Screen Capture source, turn on audio capture for the app.
4. **Your own voice:** add your microphone as a **Mic/Aux** source. Meet doesn't play your own voice back to you, so OBS needs your mic separately.
5. Check the audio mixer while someone talks: guests and you should both move the meters, and neither should hit red.

## 3. Add the overlay
1. Add a source: **Browser**. Put it at the **top** of the source list so it sits above the Meet video.
2. **Untick "Local file"** and type the file's address as a URL, for example:
   - Windows: `file:///C:/Users/Seth/kem-live/overlay.html`
   - Mac: `file:///Users/seth/kem-live/overlay.html`
3. Width **1920**, height **1080**. The background is already transparent.
4. Optional starting topic: add it to the end of the URL, e.g. `overlay.html?topic=Your%20money%2C%20on%20autopilot&sub=Where%20Kem%20fits`.
   Other options: `&ticker=0` hides the ticker, `&lt=0` hides the lower third, `&scroll=1` makes the ticker scroll.

The ticker refreshes every 60 seconds. If a price feed goes down or rate-limits (HTTP 429), it keeps showing the last good number (dimmed after 15 minutes without an update), never shows a broken value, and waits longer between retries (2, 4, 8 … up to 15 minutes) until the feed recovers. XAUT falls back to spot gold if CoinGecko is unavailable. Prices load fine when the overlay is opened as a local file (tested in Chrome without special flags) and from a URL.

## 4. Switch topics live (control page, v17)
Centre of the bottom bar (v18/v19): the **Kem Card promo** is the default (v19: it rotates between the travel-card and Grok Bot cashback messages every 60 s). Tap **Promo (Kem Card) / Topic** at the top of the control page to switch (Go/Next/Prev switch to Topic by themselves).
Using **Streamlabs Desktop**? Follow `STREAMLABS_QUICKSTART.md` instead of sections 1–3 for the overlay setup.
1. Open `control.html` (same folder) in any browser, on any device with internet, and enter the passcode (it's in `secrets/passcode.txt` on the computer; never paste it in chat). In OBS you can also add it as a Custom Browser Dock.
2. **Topic set:** 4 slots, each a title + subtitle, saved on that device. Tap **Go** on a slot to put it on stream, or use **Next ▶ / ◀ Prev**. The topic area animates: the old text slides up and out (0.2 s) and the new text rises in (0.34 s), flat, transform/opacity only. With "reduce motion" turned on it's a quick fade instead.
3. **Auto-rotate:** tick it and set the seconds (min 3, default 90). The rotation runs inside the overlay itself, so it keeps going if the control page is closed. Untick to hold the current topic.
4. Every Go/Next/Save pushes the whole set + current slot over the signed relay, so the overlay remembers both after a reload or restart (and a freshly opened overlay picks them up from the relay).
5. **One-off topic:** type a title/subtitle and tap **Show on stream** (this takes you out of the set until you tap Go again).
6. **Keys:** on the control page (on a computer), 1–4 = Go, ←/→ = Prev/Next, A = auto-rotate on/off. On the overlay itself (Streamlabs "Interact" window), 1–4 and ←/→ also switch, but only locally, and the next relay message wins.
7. **No relay fallback:** preload the set in the overlay URL: `overlay.html?t1=Title&s1=Subtitle&t2=…&s2=…&t3=…&t4=…&slot=1&auto=90`.
8. The prefilled 4 topics are **samples**: "Your money, on autopilot", "Sending money home, simplified", "Digital dollars, explained", "Spending abroad, without surprises". Replace them with each show's real topics.

## 5. Get the X stream URL and key
Streaming from OBS to X needs an account on a paid X plan (Premium or Verified Organizations), so @kem_app's subscription has to be active.
- **Option A:** Grok Bot creates an X **stream source** for @kem_app once livestream access is connected, then gives you the **RTMPS URL** and **stream key**.
- **Option B (do it yourself):** open **X Media Studio → Producer** (studio.x.com) while signed in as @kem_app. Create a **source** (choose RTMP and the region closest to you) and copy its RTMPS URL and stream key. Then create a **broadcast** that uses that source.
- In OBS **Settings → Stream**: Service **Custom**, Server = the RTMPS URL, Stream Key = the key.
- **Keep the stream key private.** Anyone who has it can stream to @kem_app.
- In Producer, OBS sending video is not the same as being live. You usually still have to start the broadcast on X's side. Check that the broadcast shows as live before you start talking.

## 6. OBS output settings for X
**Settings → Output → Output Mode: Advanced → Streaming:**
| Setting | Value |
|---|---|
| Encoder | H.264 (NVENC/Apple VT hardware if available, else x264) |
| Rate control | CBR |
| Bitrate | **6,000 Kbps** (X allows up to about 9,000. Go higher only if your upload speed is at least 2× the bitrate) |
| Keyframe interval | **3 s** (the value X's partner guides list; 2 s also works) |
| Profile | high |
| Audio | AAC, 128 Kbps, 48 kHz |
| Resolution / FPS | 1920×1080 @ 30 |
Don't use HEVC/H.265 or AV1 for X.

## 7. Record a full-quality file for the clippers (at the same time)
**Settings → Output → Recording:**
- Recording Path: a **shared Google Drive folder** (see below)
- Format: **Hybrid MP4** in OBS 30.2+ (otherwise MKV, then **File → Remux Recordings** to MP4 after the show)
- Encoder: a separate high-quality setting is best, e.g. CQP/CRF ~18–20 or 15–25 Mbps. "Same as stream" also works if your computer is slow.

**On show day,** click **Start Streaming** *and* **Start Recording**. You can also turn on Settings → Advanced → "Automatically record when streaming".

## 8. Share the file with the clippers
1. In Google Drive, create a folder like **Kem Live / Recordings** and **share it** with the clippers (Viewer if they only download, Editor if they upload clips back).
2. Install **Google Drive for Desktop** on the streaming computer.
   - Set the folder to **Mirror** (or "Available offline") so files are saved locally first, then uploaded.
   - Point OBS's Recording Path at that folder, e.g. `G:\My Drive\Kem Live\Recordings` on Windows or `~/Google Drive/My Drive/Kem Live/Recordings` on Mac.
3. After the show, Drive uploads the file automatically. A 1-hour 1080p recording is several GB, so leave the computer on until Drive shows the upload finished. Then the clippers download it from the shared folder.
4. Name each file by date, e.g. `2026-11-08_kem-live.mp4`. Under Settings → Advanced → Filename Formatting, OBS can add the date automatically.
5. **Backup:** the X replay stays on the broadcast's page on @kem_app, so clippers can also work from the post. The local recording is higher quality and has no stream compression or dropouts, so use it first.

## Show-day checklist
- [ ] Meet window open, guests in, headphones on
- [ ] OBS overlay shows prices (not "—") and the right first topic
- [ ] Control dock says "Overlay connected ✓"
- [ ] Mic and Meet audio both moving in the mixer
- [ ] Start Recording **and** Start Streaming, then confirm the broadcast is live on X
- [ ] After the show: stop both, wait for the Drive upload to finish, tell the clippers

---
## UPDATE (Nov 4, 2026): remote-controlled overlay (The Late Block)

**How it works now:** `overlay.html` and `control.html` are plain static files. Topic changes travel as
cryptographically *signed* messages over the free public relay **ntfy.sh** (no account). The overlay only
contains the *public* verification key, so nobody can push to it without the passcode; the control page
holds the signing key encrypted with the passcode (PBKDF2 + AES-GCM). Updates land in well under 1 second.
Prices still come straight from the public market feeds every 60 s.

### OBS (streaming PC)
1. Unzip `late_block_site.zip` somewhere permanent (e.g. `Documents/LateBlock/`).
2. Sources → **+ → Browser** → tick **Local file** → pick `overlay.html` → 1920×1080.
   (When a public https host is set up, untick Local file and paste the URL instead, e.g. `https://<host>/overlay.html`.)
3. Leave "Shutdown source when not visible" OFF. Layer it above the Meet window capture.
4. Theme: **dark (black bars) is the default** since v16; `?theme=light` gives the white bars, `?theme=dark` forces dark, or use the "Theme" button on the control page. Note: the control page remembers the last theme it sent (the remote test ends on light), so if the overlay comes up white, tap **Theme** once on the control page, or put `?theme=dark` in the OBS URL (a URL theme always wins). Optional URL params still work: `?logo=0` (hide the Late Block logo panel), `?tether=0` (hide the Powered by Tether tag, top left), `?frame=0` (no side gutters), `?xbadge=0` (hide the Live on X pill, top right), `?stores=0` (hide the App Store / Google Play badges), `?bar=0`, `?ig=@handle`, `?ticker=0`, `?lt=0`, `?scroll=1`, `?remote=0` (disable remote).
5. **Clock (v10): off by default.** The ticker no longer shows a clock; prices use the full width. Opt back in with `?clock=1` (then `?tz=America/Los_Angeles`, `?tzlabel=KWT`, `?secs=1`, `?h24=1` still work). The control page's clock toggle only has an effect when `?clock=1` is set.
6. **Ticker (v9):** Tether Gold and gold are now one item, `XAUT · GOLD`, showing the XAUT price and 24h change (CoinGecko; if that feed is down it falls back to spot gold so the slot never sits empty). The separate GOLD item is gone. Items (as of v11): BTC · SOL · HYPE · XAUT · GOLD · SILVER · USDT/USD. With one fewer item and no clock label, prices render at full size again.
7. **LIVE dot pulse (v10, updated v15):** the red dot in the top-right **LIVE · Streaming exclusively on X** block pulses gently (2-second fade, 100% → 35% → 100%, no glow). The separate red LIVE tag at the top left was replaced by the Powered by Tether tag in v15. `?pulse=0` keeps the dot static, and it's static automatically if the computer has "reduce motion" turned on.
8. **Ticker & topic text (v11):** the USD/KWD item is gone; the ticker now shows only BTC · SOL · HYPE · XAUT · GOLD · SILVER · USDT/USD, spread evenly across the full width. Long subtitles now shrink slightly (22px down to 17px) before they'd ever be cut off with "…", and the title/subtitle fit uses exact text width so nothing is clipped by a pixel. (Sample topic updated in v15, see Layout.)

**Layout (v16, dark by default, flat, no shadows, everything flush):** **top band** — **POWERED BY tether** tag at the far left (the only Tether placement: white "POWERED BY" over the official green/teal Tether wordmark at 32px, on a flush full-height black block with generous padding) · ticker BTC · SOL · HYPE · XAUT · GOLD · SILVER · USDT/USD (prices at ~87% size, about 20px, so every item keeps clear space; full size with `?xbadge=0` or `?tether=0`) · full-height, flush **LIVE / STREAMING EXCLUSIVELY ON | X** block in the top-right corner (pulsing dot, 34px X logo), shiny gold hairline under the band. **Frame** — 12px side gutters in the overlay colour; the video window (x 12→1908, y 69→917) is fully transparent and nothing sits on the guests. **Bottom block (160px, one piece, gold hairline on top)** — **left** the show logo panel, wider with 60px padding each side, centred horizontally and vertically: Kem logo + "PRESENTS" over THE LATE BL[cube]CK; **centre** the topic: title 50px (auto-shrinks to 30px) and subtitle 24px (auto-shrinks to 17px); **right** the Kem unit: Kem logo + "Money moves where you do." with the official App Store / Google Play badges stacked right beside them, then Instagram glyph + handle as its own item on the far right. `?tether=0` hides the Tether tag. No text is smaller than 14px. Sample topic used in the previews: **Your money, on autopilot** — *How AI agents could pay for things, and where Kem fits*. (The "Topic N of 5" label and the not-financial-advice strip were removed at Seth's request — say "Not financial advice" on air / in post captions instead.) Instagram handle: edit `IG_HANDLE` at the top of the overlay script, use `?ig=`, or the control page field. With `?clock=1` the clock sits after the Tether tag and the ticker shrinks a lot (~61%) to make room, so it's best left off.

### Control (any phone/laptop, anywhere)
Open `control.html` (hosted URL once available, or the local file on any laptop), enter the passcode once
(remembered on that device). Next/Prev, edit title/subtitle, paste "Title | Subtitle" lines, toggle
ticker / topic / show logo / bottom bar / Live on X pill / app store badges / theme, and update the Instagram handle. Every device sees the same live state. Last state is kept by the relay for 12 h,
and the overlay also remembers it locally.

Passcode: ask Seth / stored on the build box at `/workspace/kem-live/secrets/passcode.txt` (never in the overlay).
To rotate it: `node secrets/genkeys.mjs`, re-run the site build, redistribute both files.

## UPDATE v17 (Nov 4, 2026): Streamlabs-ready, 4-topic set
- **Streamlabs Desktop:** see `STREAMLABS_QUICKSTART.md` (Browser Source, local file or URL, 1920×1080, 60 FPS, "Shutdown source when not visible" OFF, "Refresh browser when scene becomes active" OFF). The look is unchanged from v16; the video window is still x 12→1908, y 69→917.
- **Topic set:** 4 slots with Go / Next / Prev / auto-rotate and an animated swap (section 4).
- **Relay reconnect:** the overlay reconnects by itself after a network drop (retries with backoff, reconnects as soon as the network is back, and a watchdog restarts a stalled stream), then re-reads the last state so nothing is missed.
- **Prices:** per-source backoff on errors/429, last good values kept (stored on the computer, so they show instantly after a reload).
- **Local file vs URL:** all asset paths are relative, so both work. In Chrome, the live price and relay requests work from a local file. Streamlabs' built-in browser loads local files through its own internal address, which also allows these requests; it couldn't be tested here, so check the prices on first setup.
- **Hosting:** a permanent hosted URL (needed to open the control page on a phone) still needs GitHub connected. The local file works today.

## UPDATE v18 (Nov 4, 2026): Kem Card promo in the centre
- The centre of the bottom bar now shows a **Kem Card promo** by default: three fanned Kem Visa Signature cards (cut out from the official card image, flat, thin edge highlight) in front of the Kem globe (a slowly turning transparent video, one turn per 50 s; a still PNG is used if video can't play, with `?globe=0`, or with reduce-motion on).
- Wording: headline **"Get your AI travel card with Kem"**, support line **"Spend your USDT anywhere Visa is accepted."**, call to action **"DOWNLOAD KEM"** (gold). To change it permanently, edit `const PROMO={…}` in overlay.html. To change it per show, use `?promo=`, `?promosub=`, `?promocta=` (an empty value hides the line).
- **Promo / Topic toggle** on the control page (signed over the relay like other commands; key C). `?centre=topic|promo` in the overlay link locks the centre and overrides the control page. A `?topic=` or `?t1=` link with no `?centre=` starts in Topic mode.
- The switch animates the same way as a topic swap (out 0.2 s, in 0.34 s; a fade with reduce-motion on). The topic set, auto-rotate and keys work exactly as in v17. Auto-rotate keeps counting while the promo is showing.
- Quality: every raster image is at least 2× its on-screen size (cards about 4.3×, globe 3×, Late Block logo 2.9×, Kem logos 6–10×); the rest are SVG. If it looks soft or cut off in Streamlabs, check that the canvas is 1920×1080 and the source isn't scaled (see the quickstart).

## UPDATE v19 (Nov 4, 2026): rotating promo, 3D cards, real Earth, Grok Bot robots
- The centre promo now **rotates between two messages every 60 seconds** with a soft fade-and-slide (0.4 s out, 0.5 s in):
  - **A:** "Get your AI travel card with Kem" / "Spend your USDT and more, anywhere Visa is accepted." / DOWNLOAD KEM
  - **B:** "Get up to 5% cashback on Grok Bot with Kem" / "Pay for Grok Bot with your Kem card." / DOWNLOAD KEM, with seven small flat robot characters (original artwork drawn in SVG) that bob and blink on staggered beats. They stay inside the bottom bar, never over the guests.
- Edit both messages in `const PROMOS=[…]` and the interval in `const PROMO_SEC=60` in overlay.html. Per-show link options: `?promosec=N` (0 = no rotation), `?promopin=1|2` (or `a|b`) to hold one message, `?promo= / ?promosub= / ?promocta=` for message A, `?promob= / ?promobsub= / ?promobcta=` for message B. A custom `?promo=` without `?promob=` shows only A.
- **Cards:** real CSS 3D now: perspective tilt, a slow sway, visible card thickness with a thin gold metal core, rounded corners, a moving light sheen and gloss, and small contact shadows between the fanned cards (none on the bar). The card face is re-cut from the original card photo at 768×484.
- **Globe:** a real Earth drawn live on a transparent canvas from NASA's Blue Marble (public domain, `assets/earth_day.jpg` + a water mask `assets/earth_water.png` for the ocean glint), with day lighting, a soft terminator and an atmosphere rim. One turn every 2 minutes, ~30 fps, it pauses while the topic is showing. No WebGL or internet library needed. `?globe=0` or reduce-motion shows a still frame; if the texture can't load, `assets/kem_globe.png` is shown. The old `kem_globe.webm` video is no longer used.
