# Quality checklist

Load this file in step 5, before showing any result, and when the user asks about printing. Go through every item. Report the result to the user as a short list: passed, fixed, not tested (and why).

## Contents

1. Facts and content
2. Design and readability
3. Mobile layout and behaviour
4. Music
5. Links, maps and calendar
6. Motion and performance
7. Sharing and privacy
8. Print specification checklist (guidance only)

---

## 1. Facts and content

- [ ] Every name, date, time, venue and address matches the locked facts list **character for character** (spelling, initials, capitalisation, punctuation).
- [ ] Muhurtham and lagna times are exactly as supplied; nothing rounded or reformatted.
- [ ] Weekdays match the dates (compute them; do not assume).
- [ ] No invented content: no taglines, stories, family names, invocations, hashtags or blessings the user did not approve.
- [ ] No text copied from a reference card or site unless the user said to use it.
- [ ] Opening the page with `?check` shows no "Unfilled" banner, or every remaining placeholder is one the user chose to leave, and you have told them.
- [ ] Regional-language lines are user-supplied or user-approved and flagged "verify with a native speaker".
- [ ] Religious motifs present only if requested.
- [ ] The config JSON is valid (no "Config error" banner; no console errors).

## 2. Design and readability

- [ ] Body and event-detail text reach 4.5:1 contrast on their ground; names and large numbers at least 3:1 (aim for 4.5:1).
- [ ] No bright gold, marigold or turmeric used as text on a light ground.
- [ ] Text over the backdrop photo measured against the busiest area; scrim or card present.
- [ ] Names, dates and numbers at weight 600 to 700; no hairline weights.
- [ ] Body text at least 16 px (Latin) and 17 px (Indic); fine print at least 13 px; Indic text never below 14 px.
- [ ] Bright ground; no black or dark navy page unless the user asked (with the cultural note given).
- [ ] Motifs fit the tradition; no Mughal arches or domes on a South Indian Hindu invite.
- [ ] Photo is a clean cut-out with soft edges or a true blurred backdrop; no hard frames; faces centred; no text over faces.

## 3. Mobile layout and behaviour

- [ ] Screenshots read back at 360, 390 and 430 px widths; nothing overflows horizontally; no clipped names.
- [ ] Section labels and buttons are the template's English strings, or the user supplied or approved replacements.
- [ ] Desktop width (about 1200 px) looks intentional, not stretched.
- [ ] Cover shows "Tap the seal to open"; tapping the seal opens the invite.
- [ ] After opening, the page is at the top (test by scrolling to the bottom, reloading, then opening).
- [ ] Buttons are at least 44 px tall and easy to tap.
- [ ] Safe-area insets respected (notch, home bar).

## 4. Music

- [ ] No sound before the cover tap.
- [ ] Music fades in after the tap; mute toggle visible and working.
- [ ] Level measured at about -30 dB mean (or say it was not measured).
- [ ] Licence is CC0, CC BY or owned by the user; not CC BY-SA, NC, or a film song.
- [ ] Footer shows "Music: <credit>" whenever a track is set (not "[MUSIC_CREDIT]").
- [ ] File is MP3, sits next to the HTML, and loads on the host.

## 5. Links, maps and calendar

- [ ] Map button opens the right place (search the venue name if the link returns a list; ask for a share link).
- [ ] Copy-address button copies the full address.
- [ ] Each "Add to Google Calendar" link shows the right title, date, time and location.
- [ ] Each downloaded .ics opens on a phone with the correct time in the venue's timezone (default Asia/Kolkata). The file stores UTC times, so the phone converts them.
- [ ] Events with no `end` show a 3-hour calendar entry; the user accepted that or gave an end time.
- [ ] Countdown targets the right event (`countdownTo`) and reaches zero at its start time.
- [ ] Calendar grid shows the right month(s) and marks every event day.

## 6. Motion and performance

- [ ] With reduced motion enabled in the operating system, the invite opens instantly with no petals or parallax.
- [ ] Animations use transform and opacity only, and finish (no infinite shimmer).
- [ ] Smooth at 4x CPU throttling when scrolling and opening (or say it was not tested).
- [ ] HTML under 150 KB without photos and music; total first load under about 1.5 MB; each photo under 200 KB.

## 7. Sharing and privacy

- [ ] `og:title`, `og:description`, `og:url` and absolute `og:image` filled; `[OG_IMAGE_URL]` replaced.
- [ ] og:image is 1200 by 630 JPEG under about 250 KB, names and date legible at thumbnail size.
- [ ] Preview card checked by sending the link to yourself on WhatsApp (ask the user if you cannot).
- [ ] noindex line uncommented while previewing (it ships commented out); launch decision on removing it made by the user.
- [ ] Photo metadata stripped; neutral filenames; no phone numbers, home addresses or guest lists unless the user insisted.
- [ ] No existing site of the user's was modified; hosting done by the user.
- [ ] Share message drafted with only supplied facts.
- [ ] Untested items listed honestly (real phone with sound, real preview card, iOS).

---

## 8. Print specification checklist (guidance only)

This skill does not produce print-ready files. Hand this list to the user or their printer and recommend a printed proof before the full run.

- [ ] **Trim size** agreed: common Indian sizes are 5 by 7 in, A5 (148 by 210 mm), 6 by 8 in, square 6 by 6 or 7 by 7 in.
- [ ] **Bleed:** 0.125 in (3 mm) on every side; a 5 by 7 in card becomes a 5.25 by 7.25 in file.
- [ ] **Safe zone:** keep text and important elements 0.25 in inside the trim.
- [ ] **Resolution:** 300 dpi at final size; warn if a photo falls below about 200 effective dpi.
- [ ] **Colour:** CMYK. Warn that jewel tones (peacock, rani pink) dull in CMYK; ask for a proof. Avoid rich black.
- [ ] **Fonts:** embedded or outlined in the PDF; licences allow it (OFL fonts do).
- [ ] **Minimum text size:** 6 pt Latin, 8 pt for Kannada, Tamil, Telugu or Malayalam.
- [ ] **Gold foil:** specify foil or metallic ink as a separate spot layer (for example named FOIL), not as a CMYK brown-yellow, which prints muddy.
- [ ] **Laser cut or emboss:** needs a separate die-line file from the printer.
- [ ] **Deity imagery:** only if requested; on the card face only, never on envelopes, folds or tear-offs.
- [ ] **Text check:** same facts as the digital invite, proof-read by the family.
