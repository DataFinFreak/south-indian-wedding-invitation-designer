---
name: south-indian-wedding-invitation-designer
description: Designs South Indian and Indian wedding invitations as animated mobile e-invites to share on WhatsApp, for Kannada, Tamil, Telugu and Malayalam families, and as digital wedding cards. Interviews for the exact event facts, offers style presets (Mysuru palace, Mysore silk, mallige jasmine, temple kolam, Kanjeevaram border, contemporary ivory), fills one self-contained HTML template with events, calendar grid, countdown, map, add-to-calendar and optional couple photo and music, then checks contrast, phone layout, link previews and privacy before sharing. Use when someone asks for a wedding invitation, e-invite, lagna patrike, subhalekha, muhurtham or reception invite, WhatsApp wedding card or link, wedding website page, or a print specification checklist for a South Indian wedding. Uses only details the user supplies and never invents names, dates, timings or regional-language wording.
license: Apache-2.0
metadata:
  author: "DataFinFreak"
  version: "1.0.0"
---

# South Indian Wedding Invitation Designer

Produce a single-file, mobile-first, animated wedding invitation (HTML) that a couple or family shares as a link, plus the share message and link-preview image guidance. Print is guidance only: give the print-spec checklist, never a print-ready file.

The template is `assets/invite-template.html`. Edit only its `invite-config` JSON block and the `<head>` tags marked `EDIT:`. Everything else (layout, presets, runtime) stays untouched.

## Hard rules

1. Use only facts the user supplies. Never invent names, family names, dates, muhurtham or lagna times, venues, taglines, love stories, hashtags or blessings. A printed card, PDF, screenshot or another website the user shares is reference only, not content to copy, unless the user says "use this text".
2. Print auspicious times exactly as given. Never round, reformat or "tidy" a muhurtham window.
3. Write English wording by default. Add Kannada, Tamil, Telugu or Malayalam text only when the user supplies it or explicitly approves a draft, and label every regional-language line "verify with a native speaker or the family priest". Never machine-translate silently.
4. Keep deity and religious motifs off unless the user asks. Honour a secular choice. Never assume a Hindu structure for Christian, Muslim or interfaith weddings.
5. Keep palettes bright and celebratory. Do not use black, dark navy or near-black grounds unless the user explicitly asks; if they do, comply and add a one-line cultural note (dark grounds can read as mourning).
6. Match motifs to the couple's tradition. For a South Indian Hindu wedding use kolam, thoranam, mallige, kalasha, zari borders, gopuram silhouettes, elephants, peacocks; avoid Mughal pointed arches, domes, onion shapes and jaali.
7. Never autoplay music. The cover tap is the gesture; show a visible mute toggle.
8. Never generate or synthesise music. Use a real recording the user owns or a CC0 / CC BY track, and always render the credit.
9. Never show a raw photo in an oval, arch or locket frame. Use a clean cut-out with a soft halo and edge fade, or a blurred full-bleed backdrop.
10. Never deploy for the user and never touch their existing websites. Describe hosting options; the user deploys.
11. Do not build per-guest personalised links or RSVP backends unless asked.
12. Be honest about limits: say what was not tested (a real phone with sound, a real WhatsApp preview).

## Capability matrix

Check what the environment can do before promising an output. Ask before installing anything.

| Environment | What to deliver |
|---|---|
| No tools (chat only) | Interview, wording, direction sketches, the filled config block, the full HTML as an artifact or file the user saves, share message, print checklist. Say that visual QA was not run. |
| Code execution | All of the above, plus: fill and save the template, run the `?check` placeholder scan, take phone-width screenshots with a headless browser, resize the og:image, strip photo metadata, generate base64 data URIs. |
| Python with rembg (optional) | Photo background removal with the `birefnet-portrait` model, alpha clean-up, measured rotation, face-centred crop. See references/photo-handling.md. |
| ffmpeg (optional) | Music trimming, fade-in, loudness levelling to about -30 dB mean, conversion to MP3. See references/digital-delivery.md. |
| Image generation | Never assumed. Offer written prompts only (for example a non-identifying og:image background or motif artwork); never generate or alter a real person's likeness. |

If a tool is missing, say so in one line and offer the no-tool path. Never claim a step ran when it did not.

## Workflow

### Step 1. Collect and lock the facts

Ask for missing essentials one question at a time. With each question, give a recommended answer or a sensible default the user can accept with "yes". Essentials, in order:

1. Who is sending it (couple, bride's family, groom's family) and to whom (friends, everyone, elders).
2. Couple names exactly as they should appear, and the order (first / second).
3. Events (1 to 4): name as the family uses it (for example Muhurtham, Dhare Muhurtha, Aarathakshate, Reception, Varaverpu, Sumuhurtham), date, start and end time, the time text to print.
4. Venue name and full address per event; a map link if they have one.
5. Region, language and tradition; religious motifs wanted or not (default: none).
6. Optional: couple photo, backdrop photo, music, family details line, closing line.

Rules for this step:
- Use only what the user gives. If something is missing, ask; if the user declines, leave the visible placeholder (for example `[VENUE_ADDRESS]`) and say so.
- If a supplied reference card contains extra lines (family names, invocation, regional text), ask before using any of them.
- Confirm spellings that differ between sources (folder name vs card vs message).
- Before building, read back a short "locked facts" list and get a yes. Treat it as the single source of truth from then on.

### Step 2. Choose a direction

Offer exactly three directions as a short multiple choice, each with a 2 to 3 line text sketch (palette, motif, opening animation, mood). A useful spread: one that follows the user's stated taste, one recommended by you, one fresher or trending. Mark one as recommended.

- Map each direction to one of the six implemented presets: `mysuru-aramane` (default), `mysore-silk-minimal`, `mallige-botanical`, `temple-kolam`, `kanjeevaram-border`, `contemporary-ivory`. Load references/style-presets.md for moods and when to suggest each.
- If the user wants something the presets cannot express, describe it as a new preset and follow the safe-preset rules in references/style-presets.md.
- Honour the religious or secular choice from step 1. No deity motifs unless asked.
- When the user asks for a "₹1 lakh" or "premium" look, raise craft (spacing, restraint, typography, one strong motif), not ornament count.

### Step 3. Fill the template

Copy `assets/invite-template.html` to a new file named for the couple (for example `meera-arjun-invite.html`) and edit only:

- The `invite-config` JSON block. Fields:

| Field | What to put |
|---|---|
| `preset` | One of the six preset ids. |
| `couple.first`, `couple.second`, `couple.joiner` | Names exactly as locked; joiner usually "&" (a word such as "weds" also works). |
| `eyebrow`, `invitationLine`, `closingLine` | Short English lines the user approved. Templates in references/regional-conventions.md. |
| `events[]` | 1 to 4 items: `id`, `title`, `subtitle`, `date` (ISO YYYY-MM-DD), `start` and `end` (HH:MM, 24-hour), `timeText` (exact printed text), `note`. An event with no `end` gets a 3-hour calendar entry; ask for a real end time when it matters. |
| `timezone` | IANA zone, default Asia/Kolkata. Ask if guests or venue are abroad. |
| `venue` | `name`, `address`, `mapUrl` (leave empty to build a map search from name and address). |
| `countdownTo` | The `id` of the event to count down to. |
| `photos.couple`, `photos.backdrop` | Path or data URI; empty hides the section. |
| `music.src`, `music.credit` | Empty hides the music button. When `src` is set the footer shows "Music: <credit>", or a visible `[MUSIC_CREDIT]` if the credit is missing, so a credit is always required. |
| `secondaryScript` | `lang` (kn, ta, te, ml), `invitationLine`, `font`; only with user-supplied or approved text. The line shows only when both `lang` and `invitationLine` are set. The Indic font (Noto Serif for that script, or the Google Fonts family named in `font`) loads only when `lang` is set. |
| `familyDetails` | Only if the user asks for it and supplies it. |

- The `<title>` and Open Graph tags in `<head>` marked `EDIT:` (title, description, og:image absolute URL, og:url).
- Keep the JSON valid: plain strings, double quotes, no trailing commas. If it does not parse, the page shows a "Config error" banner at the bottom with the parser message.
- Any field still holding a `[PLACEHOLDER]` renders visibly as that placeholder (dashed outline). An unparseable date shows "Date to be announced" in the calendar, and an event without a valid date and start time gets disabled calendar buttons. Never replace a placeholder with a guess.

What the template does on its own (do not promise more):
- Section labels and buttons are fixed English strings in the template, not config: "Tap the seal to open", "Mark your calendar", "Save the Date", "Join us for", "The Celebrations", "The venue", "Open in Maps", "Copy address", "Google Calendar", "Download .ics", "Until the <event>", the countdown units and the English month and weekday names. For another language, edit those strings in the template's HTML and script (search for the English text) only with wording the user supplies or approves, and flag it for a native-speaker check.
- Calendar: each event gets a Google Calendar link (UTC times plus `ctz=<timezone>`) and a downloadable `.ics` file with UTC times (no TZID block). Times are converted from `timezone` in the browser.
- URL parameters for checking: `?autoopen` skips the cover instantly with motion and petals off; `?check` lists every unfilled item; `?preset=<id>` previews another preset without editing. They combine, for example `?autoopen&preset=temple-kolam`.
- The `robots` noindex tag ships commented out in `<head>`; see step 6.

### Step 4. Optional photo and music

- Photo: ask for consent from both people pictured and let them choose which photo. Prefer a professionally edited source over heavy retouching. Load references/photo-handling.md for the cut-out, rotation, crop, backdrop and metadata steps, and for what to do without tools.
- Music: ask for a track the user has rights to, or offer to find CC0 or CC BY instrumentals (veena, flute, nadaswaram, violin). Avoid CC BY-SA and NC tracks and film songs. Level it quietly. Load references/digital-delivery.md for sourcing, licences, the levelling recipe and the credit line.
- Never autoplay. Confirm the credit appears in the footer.

### Step 5. Quality assurance

Inspect before showing anything. With code execution, open the page at a phone width (390 px wide) with `?autoopen` (cover skipped, motion off), screenshot the full page, read the screenshot back, and fix what is wrong before presenting it. Also screenshot the cover without parameters. Open it with `?check`: a banner at the bottom (and the console) lists every placeholder left in the config and the `<head>` tags, plus warnings such as a bad date or time, an invalid timezone, more than 4 events or music without a credit. Resolve each item or list the ones the user chose to leave.

Then run references/quality-checklist.md end to end: facts match the locked list character for character, no placeholders left, contrast, 360 to 430 px layouts, opens at the top after the cover tap, music credit, links, calendar times in the right timezone, reduced motion, performance, privacy.

Without code execution, say plainly that no visual check was run and ask the user to open the file on their phone and send a screenshot.

### Step 6. Share

- Fill the Open Graph tags so WhatsApp shows a card: an absolute og:image (1200 by 630 JPEG under about 250 KB, names and date legible at thumbnail size), og:title, og:description, og:url. Details in references/digital-delivery.md.
- Draft a short share message in the user's tone (two or three lines, the link, the date). Offer an English version and, only if the user supplies or approves one, a regional-language version.
- Describe hosting options generically (any static host, a file-sharing link, the user's own site) and let the user deploy. Suggest a fresh, unlisted site or path for previews. The template ships the `<meta name="robots" content="noindex">` line commented out: uncomment it while previewing, and the user decides whether to keep it at launch (keeping it is reasonable for a private wedding). Never touch an existing site.
- Recommend a self-test first: send the link to themselves on WhatsApp, check the preview card, tap open with the phone's silent switch off.
- If the user also wants print, hand over the print-spec checklist from references/quality-checklist.md and recommend a professional printer's proof.

## Pitfalls (learned the hard way)

- Lifting family names, invocations or regional text from a reference card. Ask first.
- Adding invented lines (taglines, story, "dinner and music"). Every line must trace to the user.
- Copying the look of a reference website (fonts, page dots, story format) when it was given only as reference.
- Mughal or Islamic arch motifs on a South Indian Hindu invite.
- Dark navy, indigo or black themes, which read as mourning.
- Generated or synthesised music; loud music. Level to about -30 dB mean; use acompressor plus volume, not alimiter (it auto-boosts).
- Raw photos in frames; faint photo tints that read as mistakes. Use a clean cut-out or a true full-bleed blurred backdrop.
- Heavy retouching or body slimming. Do neither unless explicitly asked; prefer a better source photo.
- Eyeballing rotation. Measure the eye line.
- Centring a cut-out on its bounding box instead of on the faces; letting its size vary with viewport width.
- Opening at the bottom because the browser restored the scroll position. The template forces top; re-check after edits.
- Hairline weights for names and numbers. Use weights 600 to 700 and dark ink on light grounds.
- Gold text on cream (fails contrast). Gold is decoration; use antique gold, maroon or green for text.
- Janky animation on mid-range phones. Test with the CPU throttled four times.
- Showing pages without looking at them first.
- Keeping working files only in a temporary folder. Save the filled invite where the user can find it.
- Reusing the same music track across different designs without saying so.

## Exclusions

Do not use this skill for: generic graphic design, non-wedding events, deploying or hosting for the user, RSVP backends or databases, composing or generating music, print-ready press files (v1 gives a checklist only), video or WhatsApp Status video invites (v1), per-guest personalised links, translating text into regional languages without a native-speaker check.

## References (load only when needed)

- references/style-presets.md: load in step 2, or when the user asks for a style, palette, font or a new preset.
- references/regional-conventions.md: load in step 1 when the user names a region or tradition, wants regional event names, asks for wording, or wants a regional-language line.
- references/digital-delivery.md: load in steps 4 and 6 for WhatsApp preview specs, music rules and levelling, maps and calendar links, performance and hosting.
- references/photo-handling.md: load in step 4 whenever a photo is involved.
- references/quality-checklist.md: load in step 5 before showing any result, and when the user asks about printing.
