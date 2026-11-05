# The Late Block on X: Streamlabs quick start

You need: the overlay folder (`late_block_site_v17.zip`), Streamlabs Desktop, and a second screen for the control page (a laptop today; a phone once the hosted link is set up).

## 1. Unzip
Unzip `late_block_site_v17.zip` somewhere permanent, e.g. `Documents/LateBlock/`. Don't move or rename the folder after you add it to Streamlabs.

## 2. Add the overlay
1. In your scene, click **+** under Sources, then choose **Browser Source** and name it "Late Block overlay".
2. Pick **one** of these:
   - **Local file (works today):** tick **Local File** and choose `overlay.html` from the unzipped folder.
   - **URL:** paste the hosted link instead. A permanent hosted link needs GitHub connected, which is still pending, so use Local file for now.
3. Set **Width 1920**, **Height 1080**, and **FPS 60**.
4. Leave **Custom CSS** empty (delete anything in the box).
5. Turn **OFF** "Shutdown source when not visible".
6. Turn **OFF** "Refresh browser when scene becomes active".
7. Click **Done**.

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

## 5. Four topics at a time
- The control page has **4 slots**, each with a title and a subtitle. Type your 4 topics; they're saved on that device. The 4 that come prefilled are samples.
- Tap **Go** on a slot to put it on stream. The old topic slides up and out, and the new one slides in (about half a second).
- **Next ▶** and **◀ Prev** step through the 4.
- **Auto-rotate**: tick it and set the seconds (e.g. 90). The overlay rotates by itself, even if the control page is closed. Untick it to stay on the current topic.
- **One-off topic**: type a title and tap **Show on stream** for something outside the 4.
- On a computer, keys work on the control page: **1–4** = Go, **← / →** = Prev / Next, **A** = auto-rotate on/off.
- The overlay remembers the 4 topics and the current one after a restart.

## 6. Options you can add to the overlay link
Add these after `overlay.html` with `?` for the first and `&` between them, e.g. `overlay.html?theme=light&stores=0`.
- `theme=light` white bars · `theme=dark` force dark
- `tether=0` hide Powered by Tether · `xbadge=0` hide the "Streaming exclusively on X" block
- `logo=0` hide the Late Block logo · `stores=0` hide the store badges · `ig=@handle` change the Instagram handle
- `pulse=0` stop the LIVE dot pulsing · `frame=0` remove the side gutters
- Backup without the control page: `t1=Title&s1=Subtitle` … `t4=…&s4=…`, `slot=2`, `auto=90`

## 7. Pre-show checklist
1. On the control page, check the **Theme** button says dark. If the overlay is white, tap **Theme** once. (Tests sometimes leave it on light.)
2. Tap **Go** on your opening topic (slot 1).
3. Check the price ticker shows numbers.
4. Do a 2-minute test recording and check the guests fill the video window.

## 8. Troubleshooting
- **Prices show "—" or look dim:** the price service is busy or offline. The overlay keeps the last good prices and retries by itself (waiting longer each time, up to 15 minutes). Check the computer's internet.
- **Overlay not updating from the control page:** right-click the Browser Source → **Refresh**. It also reconnects by itself after a network drop.
- **Overlay is white:** tap **Theme** on the control page, or add `?theme=dark`.
- **Guest video peeks out at the edges:** re-check the Meet source's position and size (12, 69 / 1896 × 848).
