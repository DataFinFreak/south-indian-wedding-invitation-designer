# Digital delivery

Load this file in step 4 (music) and step 6 (sharing), or whenever the user asks about WhatsApp previews, music, maps, calendar links, performance or hosting.

## Contents

1. WhatsApp and Open Graph link previews
2. Music: rules, sourcing, credit
3. Audio levelling recipe (ffmpeg)
4. Maps and calendar links
5. Performance budget and techniques
6. Testing (screenshots, throttled CPU, real phone)
7. Hosting options (the user deploys)
8. Privacy of shared links

---

## 1. WhatsApp and Open Graph link previews

The template's `<head>` has tags marked `EDIT:`. Fill all four:

| Tag | Content |
|---|---|
| `og:title` | Names and occasion, for example "Meera & Arjun · Wedding". |
| `og:description` | One short line with the date, for example "14 February 2027 · Mysuru". Use only supplied facts. |
| `og:image` | **Absolute HTTPS address** of the preview image on the same host as the page. A relative path does not work. Replace `[OG_IMAGE_URL]`. |
| `og:url` | The final public address of the page. |

Image specification:
- WhatsApp's documented limits: under 600 KB, at least 300 px wide, aspect ratio 4:1 or less. In practice previews get unreliable above about 300 KB.
- **Default:** 1200 by 630 JPEG, under 250 KB. Optionally also a 1080 by 1080 square for platforms that crop.
- Names and date must be legible at thumbnail size (about 300 px wide). Large type, high contrast, no fine ornament behind text.
- Make it from a screenshot of the hero section at `?autoopen`, cropped to 1200 by 630.
- `<head>` must sit within the first 300 KB of the HTML. The template keeps `<head>` small; put large data URIs (photos) in the body only.
- WhatsApp caches previews. If the user changes the image after sharing once, the old card may persist for a while; a new page path refreshes it.

Resize and compress with ffmpeg (no Python needed):

```text
ffmpeg -i hero-shot.png -vf "scale=1200:630:force_original_aspect_ratio=increase,crop=1200:630" -q:v 4 og-image.jpg
```

Raise `-q:v` (lower quality) until the file is under 250 KB.

Without tools: ask the user to take a phone screenshot of the hero section and crop it to a wide rectangle in their gallery app.

---

## 2. Music: rules, sourcing, credit

**Rules**
- Never autoplay. Tapping the wax seal on the cover ("Tap the seal to open") is the user gesture that starts music. Browsers block sound before a gesture anyway.
- The template starts music only on the seal tap, fades it in to volume 0.6 over about 4 seconds, shows a visible play/pause toggle, falls back to the paused state if the browser refuses playback, and remembers a guest's mute choice on that device. `?autoopen` never starts music.
- Remind users that a phone's silent switch or ringer mode blocks the sound.
- Never generate or synthesise music (WebAudio drones, AI music). Real acoustic recordings only.
- Quiet but audible: about -30 dB mean volume after levelling.
- Each design or invite should have its own track; never reuse a track across designs without saying so.

**What fits**
Acoustic South Indian instrumentals: veena, flute (venu or bansuri), nadaswaram with thavil, Carnatic violin, mild mangala vadyam. A regional piece if the user asks (for example a Kannada composition), but only claim regional provenance if the source states it.

**Licences**
- Prefer **CC0** (no attribution required, but still credit as a courtesy) or **CC BY** (attribution required).
- Avoid **CC BY-SA** (share-alike can extend to derived works), **NC** (non-commercial) and any film or commercial song. A song "from YouTube" is not licensed for reuse.
- A recording the user owns or commissioned is fine; ask them for the credit line.

**Where to look (describe; the user or code execution fetches)**
Public-domain and Creative Commons audio collections such as Wikimedia Commons, the Internet Archive's audio collections, and Creative Commons music libraries. Search terms: veena, nadaswaram, venu flute, Carnatic instrumental, mangala vadyam. Check the licence on each file's own page, not the collection's.

**Credit**
Put the credit in `music.credit`. When `music.src` is set, the template renders "Music: <credit>" small in the footer; if the credit is empty it shows a visible "Music: [MUSIC_CREDIT]" and `?check` warns. Format: title, performer, source, licence. Example: "Music: Raga Mohanam (veena), Example Performer, Wikimedia Commons, CC BY 4.0". The credit is required whenever `music.src` is set.

**File**
MP3 (iPhones do not play Ogg reliably), 96 to 128 kbps, stereo, 2 to 4 minutes (the template loops it), ideally under 3 MB. Keep it as a separate file next to the HTML; do not embed audio as a data URI.

---

## 3. Audio levelling recipe (ffmpeg)

Goal: a gentle, even background level around -30 dB mean volume, with a soft fade in.

1. Measure the source:

```text
ffmpeg -i source.ogg -af volumedetect -f null -
```

Read `mean_volume` and `max_volume` from the output.

2. Trim, fade, compress and lower:

```text
ffmpeg -i source.ogg -af "atrim=0:180,afade=t=in:st=0:d=5,afade=t=out:st=174:d=6,acompressor=threshold=-24dB:ratio=3:attack=20:release=250,volume=-8dB" -ac 2 -ar 44100 -b:a 128k music.mp3
```

3. Measure `music.mp3` again. Adjust the `volume=` value until `mean_volume` is about -30 dB (for example, if it reads -26 dB, lower by another 4 dB). Keep `max_volume` below about -6 dB.

Notes:
- **Use acompressor plus volume. Do not use `alimiter`**: with default settings it normalises the level back up, undoing the reduction.
- Adjust `atrim` and the fade-out start to the length you keep.
- The template's in-page volume (about 0.6 after the fade) sits on top of this; level the file, not the page.
- Without ffmpeg: ask the user for a quieter track or tell them to lower it in any audio editor; say that the level was not measured.

---

## 4. Maps and calendar links

**Maps**
- If the user gives a map share link, put it in `venue.mapUrl`.
- If `mapUrl` is empty, the template builds a Google Maps search link (api=1 with the URL-encoded venue name and address). A non-unique venue name can return a list instead of one place; if so, ask the user for their map share link.
- Never guess coordinates or a place ID.

**Calendar**
- The template builds, per event, an "Add to Google Calendar" link and a "Download .ics" file from the config.
- Inputs that must be right: `date` (YYYY-MM-DD), `start` and `end` (24-hour HH:MM), `timezone` (IANA, default Asia/Kolkata; an invalid zone falls back to Asia/Kolkata with a `?check` warning). `end` may be empty (for example "7 pm onwards"); the calendar entry then lasts 3 hours. Ask for an end time when it matters (muhurtham windows).
- The browser converts each local time in `timezone` to UTC. The Google link passes `dates=<UTC>/<UTC>` plus `ctz=<timezone>`. The .ics follows RFC 5545 (CRLF line endings, lines folded at 75 octets, escaping, UID, DTSTAMP, one VEVENT per file) and uses UTC times (`...Z`), not TZID with a VTIMEZONE block.
- An event without a valid date and start time shows disabled calendar buttons.
- Check: open the .ics on a phone and confirm the time shown matches the printed time in the venue's timezone. For guests abroad, the calendar converts to their local time; the printed `timeText` stays in venue time.

---

## 5. Performance budget and techniques

Budget (mid-range Android phones with 2 to 3 GB RAM are common among guests):
- Template HTML under 150 KB without photos and music.
- First load under about 1.5 MB total including photos; each photo under 200 KB (WebP).
- Fonts: only the active preset's two or three families, in one Google Fonts link with display swap; an Indic face is added only when `secondaryScript.lang` is set.
- No external JavaScript libraries, no video backgrounds.

Techniques the template already uses (keep them when editing):
- Animate only `transform` and `opacity`; never top, left, width or height.
- Petals on one canvas with pre-rendered sprites, device pixel ratio capped at 1.5, finite duration.
- Cover set to `display:none` about 2 seconds after opening.
- Scroll handlers batch all reads, then all writes; passive listeners.
- No infinite shimmer or blur animations; no `getPointAtLength` loops; small masks.
- `prefers-reduced-motion: reduce` (and the `?autoopen` test parameter) turns off petals, parallax and the opening motion.

If you add effects: pre-render, keep them finite (under about 6 seconds), and re-test.

---

## 6. Testing

**Screenshots (with code execution).** Use any headless Chromium-based browser at phone size. `?autoopen` skips the cover instantly with motion and petals off, so the page is in its final state; add `&preset=<id>` to compare presets without editing. Example with Chrome or Edge:

```text
chrome --headless=new --hide-scrollbars --window-size=390,844 --virtual-time-budget=8000 --screenshot=hero.png "file:///path/to/meera-arjun-invite.html?autoopen"
```

Take a tall capture for the full page (for example `--window-size=390,6000`) and read the image back before showing it. Repeat at 360 and 430 px widths. Headless browsers can freeze CSS animations after a click; judge final states, not mid-animation frames.

**Placeholder scan.** Open with `?check`: the page shows an "Unfilled: ..." banner at the bottom and logs the list to the console. It covers every `[PLACEHOLDER]` in the config and in the title, description and og tags, plus warnings (bad date or time, invalid timezone, more than 4 events, music without a credit, placeholder photo or music path). With nothing left it shows "Check: no unfilled placeholders." A JSON syntax error shows a "Config error" banner instead, with or without `?check`.

**Opens at top.** Scroll to the bottom, reload, tap open: the page must start at the hero.

**Throttled CPU.** In browser developer tools, set CPU throttling to 4x slowdown, record scrolling and the opening; frames should stay smooth (no long tasks during scroll). Headless measurement via the DevTools protocol is possible but noisy; compare before and after in the same run.

**Real phone (ask the user).** Send the link to yourself on WhatsApp; check the preview card; open with sound on; scroll the whole page; tap map, calendar and copy-address. Say clearly which of these were not tested.

---

## 7. Hosting options (the user deploys)

Never deploy for the user and never modify a site they already have. Describe options and let them choose:

| Option | Fits when | Notes |
|---|---|---|
| Static web host with drag-and-drop or CLI deploy | Most users | Upload the HTML (renamed `index.html`), the MP3 and the og-image JPEG together. Free tiers are enough. |
| Repository-based static pages | User already uses a code host | Public repositories expose the files; consider privacy. |
| The user's own domain or site | They already run one | Use a new folder or subdomain; never overwrite existing pages. |
| File share (cloud drive link) | No hosting at all | Many drive viewers do not run JavaScript; animations, music and calendar buttons may not work. Prefer a static host. |

Advice to give:
- Use a fresh site or path for previews.
- While previewing, uncomment the `<meta name="robots" content="noindex">` line, which the template ships commented out in `<head>`; remove it for launch only if they want search visibility. For a private wedding, keeping it is reasonable.
- Upload the og-image to the same host and put its absolute address in `og:image`.
- After deploying, open the public link on a phone before sending it to guests.

---

## 8. Privacy of shared links

- Anyone with the link can see the page and forward it. Treat it as semi-public.
- Do not include phone numbers, home addresses, guest lists or ID details unless the user insists.
- Strip photo metadata (location, camera serials) before embedding. See references/photo-handling.md.
- Use noindex while previewing; consider taking the page down some weeks after the wedding.
- Keep personal data out of filenames and page titles beyond the couple's names.
- Never upload the user's photos to third-party services without asking.
