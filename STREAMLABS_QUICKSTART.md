# The Late Block on X: Streamlabs quick start

You need: the overlay folder (`late_block_site_v18.zip`), Streamlabs Desktop, and a second screen for the control page (a laptop today; a phone once the hosted link is set up).

## 1. Unzip
Unzip `late_block_site_v18.zip` somewhere permanent, e.g. `Documents/LateBlock/`. Don't move or rename the folder after you add it to Streamlabs.

## 2. Add the overlay
1. In your scene, click **+** under Sources, then choose **Browser Source** and name it "Late Block overlay".
2. Pick **one** of these:
   - **Local file (works today):** tick **Local File** and choose `overlay.html` from the unzipped folder.
   - **URL:** paste the hosted link instead. A permanent hosted link needs GitHub connected, which is still pending, so use Local file for now.
3. Set **Width 1920**, **Height 1080**, and **FPS 60**.
   Also check **Settings → Video**: Base (Canvas) Resolution **1920×1080**. If the canvas is smaller (e.g. 1280×720), the overlay gets cut off and looks soft.
4. Leave **Custom CSS** empty (delete anything in the box).
5. Turn **OFF** "Shutdown source when not visible".
6. Turn **OFF** "Refresh browser when scene becomes active".
7. Click **Done**, then right-click the source → **Transform → Reset Transform** so it sits at exactly 1920×1080 (not stretched or shrunk).

Dark bars are the default. To add options like the white look (section 6) while using the local file: untick **Local File** and paste the file's address with the option on the end, e.g. `file:///C:/Users/you/Documents/LateBlock/overlay.html?theme=light` (on a Mac: `file:///Users/you/Documents/LateBlock/overlay.html?theme=light`).

## 3. Layer order and the guest video
- The Browser Source goes at the **top** of the Sources list (the overlay sits on top).
- The Google Meet window capture goes **under** it.
- Fit the Meet capture into the overlay's clear video window: **x 12 to 1908, y 69 to 917** (1896 × 848 px). Right-click the Meet source → **Transform → Edit Transform**: Position 12, 69 and Size 1896 × 848. Crop off the browser bars first (hold Alt while dragging an edge).
- Use Meet's grid view, or pin whoever is speaking.

## 4. Open the control page
- **Today:** open `control.html` from the unzipped folder in Chrome or Safari on the computer, or on a laptop that has the same folder. It doesn't have to be the streaming computer.
- **On a phone:** this needs the hosted link, which is still waiting on GitHub being connected.
- Enter the passcode. It's in `secrets/passcode.txt` on the computer. Type it in by hand and **never paste it in chat or share it**.
- The control page talks to the overlay over a signed relay, so it works from any device with internet.

## 5. Centre of the bottom bar: Promo or Topic
- By default, the centre shows the **Kem Card promo**: fanned Kem cards, a slowly turning globe, and "Get your AI travel card with Kem / Spend your USDT anywhere Visa is accepted. / Download Kem".
- At the top of the control page, tap **Promo (Kem Card)** or **Topic** to switch. It slides smoothly between the two.
- Tapping **Go**, **Next** or **Prev** on a topic switches the centre to Topic automatically. Tap **Promo** to bring the card back.
- To change the promo wording for a show, add it to the overlay link: `promo=Headline`, `promosub=Support line`, `promocta=Call to action` (leave one empty to hide that line).

## 6. Four topics at a time
- The control page has **4 slots**, each with a title and a subtitle. Type your 4 topics; they're saved on that device. The 4 that come prefilled are samples.
- Tap **Go** on a slot to put it on stream. The old topic slides up and out, and the new one slides in (about half a second).
- **Next ▶** and **◀ Prev** step through the 4.
- **Auto-rotate**: tick it and set the seconds (e.g. 90). The overlay rotates by itself, even if the control page is closed. Untick it to stay on the current topic.
- **One-off topic**: type a title and tap **Show on stream** for something outside the 4.
- On a computer, keys work on the control page: **1–4** = Go, **← / →** = Prev / Next, **A** = auto-rotate on/off.
- The overlay remembers the 4 topics and the current one after a restart.

## 7. Options you can add to the overlay link
Add these after `overlay.html` with `?` for the first and `&` between them, e.g. `overlay.html?theme=light&stores=0`.
- `centre=topic` or `centre=promo` lock the centre (the control page can't switch it while this is in the link) · `globe=0` still globe
- `theme=light` white bars · `theme=dark` force dark
- `tether=0` hide Powered by Tether · `xbadge=0` hide the "Streaming exclusively on X" block
- `logo=0` hide the Late Block logo · `stores=0` hide the store badges · `ig=@handle` change the Instagram handle
- `pulse=0` stop the LIVE dot pulsing · `frame=0` remove the side gutters
- Backup without the control page: `t1=Title&s1=Subtitle` … `t4=…&s4=…`, `slot=2`, `auto=90`

## 8. Pre-show checklist
1. On the control page, check the **Theme** button says dark. If the overlay is white, tap **Theme** once. (Tests sometimes leave it on light.)
2. Check the centre shows what you want to open with: **Promo** (default), or tap **Go** on your opening topic.
3. Check the price ticker shows numbers.
4. Do a 2-minute test recording and check the guests fill the video window.

## 9. Troubleshooting
- **Prices show "—" or look dim:** the price service is busy or offline. The overlay keeps the last good prices and retries by itself (waiting longer each time, up to 15 minutes). Check the computer's internet.
- **Overlay not updating from the control page:** check the link doesn't contain `centre=` (that locks the centre). Then right-click the Browser Source → **Refresh**. It also reconnects by itself after a network drop.
- **Overlay cut off at the edges, or looks soft/blurry:** set Settings → Video → Base (Canvas) Resolution to 1920×1080, then right-click the source → Transform → Reset Transform. Keep the source at Width 1920 / Height 1080. Don't resize it by dragging the corners. A low streaming bitrate can also make it look soft on X. Check Settings → Output (about 6,000–9,000 Kbps for 1080p).
- **Globe not turning:** it falls back to a still picture if the video can't play. Everything else works the same.
- **Overlay is white:** tap **Theme** on the control page, or add `?theme=dark`.
- **Guest video peeks out at the edges:** re-check the Meet source's position and size (12, 69 / 1896 × 848).
