# Third-party notices

The code, templates and documentation in this repository are licensed under Apache-2.0 (see `LICENSE`), except where noted below.

## Fonts (referenced, not bundled)

The template `skills/south-indian-wedding-invitation-designer/assets/invite-template.html` and the example pages load fonts from the Google Fonts service by link. **No font files are included in this repository or in the skill zip.** The fonts remain under their own licence, the SIL Open Font License 1.1 (OFL), and are served by Google Fonts with their licence information.

Families the template and references may load or suggest (each listed on Google Fonts under the OFL):

| Family | Use |
|---|---|
| Pinyon Script, Great Vibes | Script names and titles (template) |
| Cormorant Garamond, EB Garamond | Body text and some names (template) |
| Cinzel, Marcellus SC | Small capitals and labels (template) |
| Playfair Display, Bodoni Moda, Yatra One | Names and titles (template) |
| Jost | Sans labels and body (template) |
| Noto Serif Kannada, Noto Serif Tamil, Noto Serif Telugu, Noto Serif Malayalam | Regional-language line (template; loaded only when `secondaryScript.lang` is set) |
| Tiro Kannada, Tiro Tamil, Tiro Telugu, Baloo Tamma 2, Anek Kannada, Anek Latin, Hind Madurai, Ponnala, Manjari, Gayathri | Suggested alternatives in the references (not loaded by default) |

The template loads only the active preset's families; the per-preset list is in the boot script in its `<head>`. If you vendor any font file for offline or print use, include its `OFL.txt` and copyright notice beside it, keep it unmodified, and do not use a Reserved Font Name for a modified version.

## Music

**No music or audio files are bundled with the skill.** Users supply their own track or choose a CC0 or CC BY recording; the skill requires the credit to be shown in the invite's footer. CC BY-SA, NC and commercial film music are not recommended.

The demo gallery in `docs/` (not part of the skill) includes two public-domain recordings, re-encoded to mono MP3 at 64 kbps; each is credited on its demo page:

| File | Recording | Licence |
|---|---|---|
| `docs/demos/music-rangoli.mp3` | "Viriboni" on veena, Pushpa Srivatsan | CC0 1.0 (public domain dedication) |
| `docs/demos/music-kagada.mp3` | "Shri Nilotpala Nayike" on veena, L Ramakrishnan | CC0 1.0 (public domain dedication) |

## Demo photos

The demo pages in `docs/demos/` embed one blurred backdrop photo of the author's own (metadata stripped), published with permission. It is not licensed for reuse under Apache-2.0 and is not part of the skill.

## Photos and images

**No photographs are bundled with the skill.** The example invite uses no real person's photo. Decorative motifs in the template are original SVG created for this project and are covered by the repository licence (Apache-2.0).

## Services referenced

The template builds links to Google Maps search and Google Calendar event templates from the user's own data. No API keys are used and no data is sent anywhere until a guest taps a link.
