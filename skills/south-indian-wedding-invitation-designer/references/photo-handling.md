# Photo handling

Load this file in step 4 whenever the user wants a photo in the invite.

## Contents

1. Consent and choice
2. How photos appear in the template
3. Couple cut-out recipe (Python with rembg)
4. Measuring rotation
5. Face-centred crop and fixed size
6. Backdrop photo
7. Metadata and file size
8. Honest retouching
9. Without tools

---

## 1. Consent and choice

- Ask whether both people pictured are comfortable with the photo being in a link that guests can forward.
- Let the user choose which photos appear. Do not add intimate shots (for example a kiss) unless they pick them. If they ask to remove a photo, remove it without argument.
- A professionally edited source photo works far better than heavy processing of a casual phone photo. Ask if they have one.
- Never upload the user's photos to an outside service without asking. Never use image generation to alter or recreate a real person's face or body.

---

## 2. How photos appear in the template

| Config field | What it shows | What to supply |
|---|---|---|
| `photos.couple` | Couple cut-out on a soft themed halo, faded at the edges, inside a fixed-size stage. Never a hard frame. | Transparent PNG or WebP (path next to the HTML, or a data URI). |
| `photos.backdrop` | Full-bleed, blurred, colour-washed background behind the Save the Date calendar card. | Any landscape or detail photo (hands, rings, flowers, venue). |

Empty fields hide the section; the layout stays balanced. Do not put raw rectangular photos, ovals, arches or lockets anywhere.

---

## 3. Couple cut-out recipe (Python with rembg)

Ask before installing anything. Install rembg in a virtual environment (it downloads a model of a few hundred megabytes on first use). Note that isolated Python mode hides user-installed packages, so run this script inside the environment where rembg is installed.

Model choice: **`birefnet-portrait`**. In testing, `isnet-general-use` and `u2net_human_seg` kept chairs, flowers, curtains and halos around the subject; `birefnet-portrait` gave clean edges.

Steps:
1. Load the image and apply the EXIF orientation (so it is upright as the camera intended).
2. Remove the background with `birefnet-portrait`.
3. Clean the alpha channel: values at or below 40 become 0, at or above 235 become 255, linear in between. This removes faint fringes and solidifies the subject.
4. Crop to the alpha bounding box.
5. Rotate by the measured angle (section 4), with expansion so nothing is clipped; crop to the alpha bounding box again.
6. Crop the bottom cleanly (chest-up portrait) so no slanted edge from the original frame shows; the template fades the bottom edge anyway.
7. Re-centre on the faces (section 5).
8. Resize so the longest side is at most 1000 px. Save as WebP, quality about 85 to 88.
9. Preview the result on a light ground and on the preset's accent colour; look for halos, leftover background and jagged hair edges.

```python
from PIL import Image, ImageOps
from rembg import remove, new_session
import numpy as np

SRC, OUT = "couple.jpg", "couple-cut.webp"
ROTATE_DEG = 0.0          # measured in section 4; positive = counter-clockwise
FACES_MID_X = None        # x of the midpoint between the faces after rotation, or None

im = ImageOps.exif_transpose(Image.open(SRC)).convert("RGB")
cut = remove(im, session=new_session("birefnet-portrait")).convert("RGBA")

a = np.asarray(cut.getchannel("A")).astype(np.float32)
a = np.clip((a - 40) / (235 - 40), 0, 1) * 255
cut.putalpha(Image.fromarray(a.astype(np.uint8)))
cut = cut.crop(cut.getbbox())

if ROTATE_DEG:
    cut = cut.rotate(ROTATE_DEG, resample=Image.BICUBIC, expand=True)
    cut = cut.crop(cut.getbbox())

if FACES_MID_X is not None:            # trim one side so the faces sit at the centre
    w, h = cut.size
    half = min(FACES_MID_X, w - FACES_MID_X)
    cut = cut.crop((int(FACES_MID_X - half), 0, int(FACES_MID_X + half), h))

cut.thumbnail((1000, 1000), Image.LANCZOS)
cut.save(OUT, "WEBP", quality=88, method=6)   # no exif passed, so metadata is dropped
```

---

## 4. Measuring rotation

Never eyeball rotation. In testing, an angle chosen by eye from a contact sheet was off by about 20 degrees; the measured angle was right first time.

1. Crop a region around the faces and overlay a fine grid (for example every 40 px) with coordinates, or use a face-landmark detector if one is available.
2. Read the pixel centres of the left eye (x1, y1) and right eye (x2, y2) for each face. Image y grows downward.
3. Angle in degrees = atan2(y2 - y1, x2 - x1) converted to degrees. Average across faces if there are two and they are roughly level with each other.
4. Rotate counter-clockwise by that angle (in Pillow, `rotate(angle)` is counter-clockwise). A positive angle means the right eye was lower.
5. Re-measure on the rotated image: the eye line should be within about 1 degree of horizontal.
6. If the couple are deliberately leaning (a candid pose), level the image so the overall composition looks upright and confirm with the user.

---

## 5. Face-centred crop and fixed size

- Centre the cut-out on the **midpoint between the two faces**, not on the image bounds. A shoulder or saree spreading to one side otherwise pushes the faces off-centre.
- Find each face's centre (eye midpoint), take the midpoint of the two, and trim the wider side so that point lands at the horizontal centre.
- The template places the cut-out and its halo together in one fixed-size stage (about min(320 px, 80 vw)), so the photo does not change size or drift with the viewport. Do not override this with percentage widths.
- Keep faces out of the top 10 percent (status bar, notch) and away from text. Never put names over faces.

---

## 6. Backdrop photo

- "Background" means a true backdrop: full-bleed, blurred, colour-washed, with the calendar card (frosted glass) on top. Not a visible standalone band, and not an almost-invisible tint.
- The template applies the blur and wash in CSS. Because the image is blurred, it can be small: about 900 to 1200 px wide, JPEG or WebP under about 150 KB. Pre-blurring slightly before compressing saves more bytes.
- Good choices: hands with rings or mehendi, flowers, a venue detail, fabric. Faces are not needed.
- Brighten it if it reads muddy behind the wash; check the calendar text contrast over the busiest part.

---

## 7. Metadata and file size

- **Strip all metadata** (EXIF, XMP, GPS location, camera serial numbers) from every image before embedding or uploading. Re-encoding with Pillow without passing exif drops it; verify by reading the saved file's metadata.
- Rename files to neutral names (`couple-cut.webp`, `backdrop.jpg`); do not keep camera filenames or names that reveal personal details.
- Targets: cut-out at most 1000 px, under about 200 KB WebP; backdrop under about 150 KB; total photos under about 400 KB so first load stays under 1.5 MB.
- Data URIs grow files by about a third. Prefer a separate image file next to the HTML when hosting; use data URIs only when the user needs a single self-contained file. Keep data URIs out of `<head>`.

---

## 8. Honest retouching

- Allowed by default: exposure, white balance, gentle contrast, cropping, straightening, background removal.
- Only when explicitly asked: blemish removal, mild skin smoothing.
- Never unless explicitly asked by the person pictured: body or face reshaping, slimming, skin lightening, changing features, swapping clothes or backgrounds with generated content.
- In testing, frequency-separation retouching plus a slight slim still looked wrong to the user and the photo was dropped; a professionally edited photo replaced it. Recommend a better source photo over heavy retouching.

---

## 9. Without tools

- Ask the user for an already cut-out PNG. Many phone gallery apps and photo editors can lift a subject from the background; a studio or photographer can supply one.
- Ask them to straighten and crop it chest-up, centred between the faces, before sending.
- The backdrop needs no processing; any reasonably sized photo works because it is blurred.
- Tell the user that metadata stripping could not be run, and suggest exporting or screenshotting the photo (which usually drops location data) before sharing it.
- If no suitable photo exists, leave the photo fields empty; the invite works without photos.
