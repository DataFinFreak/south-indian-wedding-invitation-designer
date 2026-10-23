# Style presets

Load this file in step 2 (choosing a direction) or whenever the user asks about style, palette, fonts or a new look.

## Contents

1. How presets work
2. The six implemented presets
3. Choosing and pitching a direction
4. Making a new preset safely
5. Research presets (describe-only, not yet templated)

---

## 1. How presets work

- A preset is selected with the `preset` field in the `invite-config` block of `assets/invite-template.html`. To preview another preset without editing, open the page with `?preset=<id>` (for example `invite.html?autoopen&preset=temple-kolam`). The URL parameter changes only that preview; guests see the preset in the config.
- Each preset is one block of CSS custom properties under `[data-preset="<id>"]` near the top of the template's `<style>`. It sets the palette, fonts, cover gradient, envelope and wax-seal colours, gold gradient stops, the repeating band pattern, petal colours and shapes, the joiner ("&") style and the browser theme colour. Each preset also has its own hero and closing motif (an SVG symbol). All SVG colours come from the preset variables, so motifs follow the palette.
- The values below are copied from the template. If you change a preset block, update this file too.
- All six presets use light page grounds. None uses a black, dark navy or near-black page. Two covers (`mysuru-aramane`, `kanjeevaram-border`) use a deep jewel gradient behind the envelope only.
- Fonts are Google Fonts (SIL Open Font License), linked, not bundled. The page loads only the active preset's families in one stylesheet link. An Indic Noto Serif face (Kannada, Tamil, Telugu or Malayalam) is added only when `secondaryScript.lang` is set; `secondaryScript.font` replaces it with another Google Fonts family name.
- Contrast ratios below were computed with the WCAG 2.x formula against the worst case of each gradient (108 text and ground pairs across the six presets, all passing). Body text needs 4.5:1; large text (24 px regular or 18.5 px bold and up) needs 3:1.

Palette variables used in every preset:

| Variable | Use |
|---|---|
| `--bg`, `--bg-2`, `--paper` | Page ground, alternate section ground, card ground (all light). |
| `--ink` | Body text. |
| `--soft` | Secondary text (eyebrow, family line, addresses on cards). |
| `--accent`, `--accent-dk` | Section titles, buttons, countdown digits, calendar marks; dates and months on cards. |
| `--names` | Couple names (the accent in most presets; magenta in `kanjeevaram-border`, dark brown in `contemporary-ivory`). |
| `--gold`, `--gold-lt`, `--g1` to `--g5` | Decoration only: rules, motifs, gradient fills, the map button ground (`--gold-lt`). Never body text. |
| `--gold-text` | Antique gold for small capitals and the joiner. 5.8:1 or better on page and card. |
| `--field-*`, `--on-field*` | The coloured venue panel and the text on it. |
| `--cover-*` | Cover gradient and the names, capitals and hint on it. |

The joiner between the names ("&" by default) uses `--gold-text` and its own `--amp-font`, `--amp-style`, `--amp-weight` and `--amp-size`, tuned per preset so its ink height is about 0.45 of the name's. Long names shrink to fit the width and the joiner shrinks with them. A word joiner (longer than two characters, for example "weds") is set at half the name size in the name font.

---

## 2. The six implemented presets

### 2.1 `mysuru-aramane` (default)

- **Mood:** royal, quiet luxury, palace stationery. "Royal and luxurious, not loud."
- **Palette:** page #FBF5EA (alternate #F4EAD6, cards #FFFCF6); ink #3A1A14 (13.15:1 worst case); secondary #6B4536 (6.95:1); names and accent kumkum #9B1C31 (7.43:1); dates maroon #6E0F20 (11.70:1 on cards); gold text #7A5B10 (5.81:1); gold decor #B8893A and light gold #E9CF8E. Venue panel #A9283F to #6E0F20 with #FFF4E2 text (6.30:1). Cover: maroon gradient #A3243A to #520A17 with #F2D891 names (5.23:1).
- **Fonts:** Pinyon Script (names and section titles), Cinzel (small capitals), Cormorant Garamond (body, weight 500, 19 px); the "&" is Cormorant Garamond italic.
- **Motifs:** wax-sealed envelope on a maroon cover; hero of two caparisoned elephants facing a kalasha over double gold rules; a band of gold rules with kumkum drops; light paper grain. Petals: kumkum reds, gold petals and sparks.
- **Suggest when:** the user wants elegant, royal, premium, Mysuru or Karnataka flavour, or has no strong preference. Safe for any region.

### 2.2 `mysore-silk-minimal`

- **Mood:** restrained, modern-traditional; a plain silk body with one zari border.
- **Palette:** page #FFFBF2 (alternate #F5EEDD, cards #FFFFFF); ink #1E2A24 (12.86:1); secondary #45524B (7.09:1); names and accent emerald #0E6650 (6.70:1); dates #0A4A3A (10.23:1); gold text #775A0E (6.25:1); gold decor #C9A24A and #EED9A0. Venue panel #0F6C55 to #0A4A3A with #FFF8E8 text (6.02:1). Cover: light ivory gradient with emerald names (5.67:1).
- **Fonts:** Cormorant Garamond italic 600 (names and joiner) and upright 600 (section titles), Marcellus SC (small capitals), Cormorant Garamond (body).
- **Motifs:** a zari band (emerald lines around a gold gradient stripe) at top and bottom; hero of a gold lotus between thin gold rules with a small emerald bar; no paper grain, generous whitespace. Petals: sparse emerald and gold petals and sparks.
- **Suggest when:** the user says simple, minimal, classy, "not too much", or wants something that photographs well as a screenshot. The jewel colour of real Mysore silk appears as bands and accents, not a dark page.

### 2.3 `mallige-botanical`

- **Mood:** fresh, floral, gentle; jasmine strands and leaves.
- **Palette:** page #FAFBF5 (alternate #EEF3E6, cards #FFFFFF); ink #23342A (11.65:1); secondary #46584B (6.74:1); names and accent leaf green #2E6B3A (6.15:1); dates #1F4D2A (9.75:1); gold text #735510 (6.65:1); gold decor #C99A3B and #F3E3A0; marigold #E8892B in motifs only. Venue panel #34723F to #1F4D2A with #FFFFF6 text (5.76:1). Cover: light sage gradient with green names (5.21:1) and a marigold wax seal.
- **Fonts:** Great Vibes (names, joiner and titles), Marcellus SC (small capitals), Cormorant Garamond (body).
- **Motifs:** hero of two swagged jasmine strands hanging from a gold rod, leaves at each end, and a marigold with a jasmine bud at the centre; a band of jasmine buds and marigolds on a green line; no paper grain. Petals: mostly jasmine, some marigold and leaf-green petals.
- **Suggest when:** the user mentions flowers, jasmine, garden or daytime weddings, a soft or romantic look, or a Kerala or Tamil feel without heavy gold.

### 2.4 `temple-kolam`

- **Mood:** auspicious, traditional, morning-temple; turmeric, kumkum and sandal.
- **Palette:** page #FBF1DC (alternate #F5E3BE, cards #FFF9EC); ink #3B1E0E (12.06:1); secondary #61401F (7.35:1); names #A8101A (6.80:1); accent kumkum #B5121B (6.10:1); dates #7E0A12 (10.32:1); gold text #7A5000 (6.29:1); turmeric gold #E0A21B and #FFD970 decor only. Venue panel #BF1A1E to #8A0D14 with #FFF6DC text (5.76:1). Cover: turmeric-yellow gradient #FFDD6B to #EDB02A with #7E0A12 names (5.59:1).
- **Fonts:** Yatra One (names, joiner and titles), Marcellus SC (small capitals), EB Garamond (body, 18 px).
- **Motifs:** hero of a dotted kolam (eight petals inside rings of dots, original and generated) between two lit deepa lamps; a band of kumkum lens shapes between turmeric rules; paper grain. Petals: marigold, with some kumkum petals.
- **Suggest when:** the family is traditional, wants a lagna patrike feel, or asks for "temple", "auspicious", "haldi". Pairs well with a regional-language line the user supplies.

### 2.5 `kanjeevaram-border`

- **Mood:** rich silk-saree celebration; peacock teal and rani magenta with a zari temple border.
- **Palette:** page #FFF7EC (alternate #F7E9D2, cards #FFFDF8); ink #12302F (11.78:1); secondary #3A5250 (7.01:1); names rani magenta #A3124F (7.17:1); accent peacock teal #0B5E5C (7.14:1); dates #06403F (11.39:1); gold text #745410 (6.55:1); zari gold #D4A94C and #F8E0A2 decor only. Venue panel magenta #B81B5C to #7A0C3C with #FFF5E0 text (5.79:1). Cover: magenta gradient #B31A5F to #74093A with #F8E0A2 names (5.01:1) and a teal wax seal.
- **Fonts:** Playfair Display italic 600 (names and joiner) and upright 600 (section titles), Marcellus SC (small capitals), EB Garamond (body, 18 px).
- **Motifs:** hero of a stepped gold gopuram silhouette (abstract, not a real temple) flanked by teal mango (paisley) shapes over a row of temple-border triangles; a band of gold and magenta stepped triangles under a teal line; paper grain. Teal stays in bands and accents, never a full dark page. Petals: magenta, teal and gold petals and sparks.
- **Suggest when:** the couple's outfits are silk, the family is Tamil or wants Kanchipuram flavour, or the user asks for "colourful but premium".

### 2.6 `contemporary-ivory`

- **Mood:** modern, editorial, calm; for couples who want minimal design with one cultural signal.
- **Palette:** page #FAF7F2 (alternate #F1EBE1, cards #FFFFFF); ink charcoal #2B2B2B (11.94:1); secondary #55504A (6.73:1); names dark brown #3A2E28 (12.27:1); accent terracotta #9E4728 (5.80:1); dates #7A351C (8.92:1); gold text #735618 (6.39:1); gold decor #C9A86A and #F1E2C0. Venue panel #A04A2B to #7E3820 with #FFF8F0 text (5.68:1). Cover: light ivory gradient with dark brown names (10.59:1).
- **Fonts:** Bodoni Moda 500 (names and titles; the "&" is Bodoni Moda italic), Jost (small capitals and body, 17 px).
- **Motifs:** hero of a small gold lotus inside a thin circle between hairline rules with terracotta diamonds; a band of two thin gold lines; no paper grain. Petals: terracotta, blush and gold petals and sparks. The lotus keeps one cultural signal.
- **Suggest when:** the user says modern, minimal, English-only, destination wedding, mixed-culture guest list, or "no traditional stuff".

---

## 3. Choosing and pitching a direction

- Offer three directions: one following the user's stated taste, one you recommend, one fresher option. Mark the recommendation.
- Write each as a 2 to 3 line sketch: palette in words, one motif, the opening animation, the mood. Example: "Mysuru Aramane: ivory and maroon with antique-gold rules; a wax-sealed envelope opens to the names; elephants and lotus in the corners; quiet palace luxury."
- If the user wants all three built, build them one at a time and screenshot each before showing.
- Variations change the look, never the content.

---

## 4. Making a new preset safely

Only add a preset when the six cannot express what the user wants. Copy the closest `[data-preset]` block, give it a new lowercase hyphenated id, and change values. Also add the id to the font map (`P`, in the boot script in `<head>`) and the motif map (`MOTIF`, in the main script), or the page falls back to `mysuru-aramane`. Rules:

1. **Bright ground.** Light ground colours only. If the user explicitly asks for black or very dark navy, comply, and add a one-line note that dark grounds can read as mourning in many South Indian households. Deep bottle green or deep maroon with gold is a safer "dark" option.
2. **Contrast.** Compute every text-on-ground pair. Body and event details: at least 4.5:1. Names and big numbers (24 px and up, or 18.5 px bold and up): at least 3:1, aim for 4.5:1. Measure text over photos against the busiest region; add a scrim or solid panel if it fails.
3. **Gold is decoration.** Bright gold, marigold and turmeric fail as text on light grounds (about 1.5:1 to 2.9:1). Use them for borders, rules and motifs, capped at about 15 percent of the surface. For gold-coloured text use an antique gold such as the template's `--gold-text` values (#735510 to #7A5B10, 5.8:1 or better).
4. **Weights.** Names, dates and numbers at weight 600 to 700 on light grounds. Script fonts only at 28 px and up, on a plain area.
5. **Fonts.** At most two families plus one script face; OFL licensed (Google Fonts). Check the licence before suggesting an unlisted font.
6. **Motifs.** One or two regional motifs, generated as original SVG. Never trace a designer's card, a saree, a painting, a deity image from the web, or a photo of a real temple. Avoid Mughal pointed arches, domes, onion shapes and jaali for a South Indian Hindu wedding.
7. **Religious imagery.** Off by default. If requested, small (10 to 15 percent of width), top-centre, respectful line art; never on envelopes or near edges.
8. **Motion.** Transform and opacity only; finite (under about 6 seconds); reduced-motion fallback.
9. Re-run the full quality checklist after adding a preset.

---

## 5. Research presets (describe-only, not yet templated)

These came out of design research. They are not implemented in the template. Describe them, or approximate them with the closest implemented preset and say so.

| Preset | Description | Closest implemented |
|---|---|---|
| Lagna Patrike Classic (Kannada traditional) | Haldi-kumkum palette, kalasha header and torana, regional-language-first card order. | `temple-kolam` |
| Subhalekha (Telugu) | Turmeric corner dabs, kumkum accents, Ganesha optional on request; Tiro Telugu for Telugu lines. | `temple-kolam` |
| Kasavu (Kerala) | Cream with a broad gold border, nilavilakku (brass lamp) and jasmine, very restrained; Manjari or Gayathri for Malayalam lines. | `mysore-silk-minimal` |
| Royal Foil (luxury) | Deep bottle green or maroon with a foil gradient, laser-cut mask layer, letterpress emboss. Dark ground, so only on explicit request. | `mysuru-aramane` |
| Portrait Story | Full-bleed couple photo cover in 9:16 with scrimmed text panels. | any preset with `photos.couple` |
| Video Status Invite | 1080 by 1920 video, 30 to 60 seconds. Out of scope in v1. | none |
| Print Suite | Card, event inserts, map card, envelope sharing one palette. Out of scope in v1 (print checklist only). | none |

An "invocation" modifier (none, Ganesha, family deity named by the user, other faith emblem) can apply to any of these, off by default.
