# Tanim Brand Engine — Cloud / iPhone Operating Playbook

*Compiled 2026-11-07 from a read-only deep audit of Dropbox `/T` (23,463 entries). Nothing in Dropbox was moved, copied, deleted or created.*

This adapts the Surface-Pro-era **`tanim-photo-curator`** skill (`/T/MASTER CURATED/_CURATION_SKILL/SKILL.md`) to a phone-driven cloud session: no PowerShell, no local Dropbox folder, no local disk. All curation is done with Dropbox server-side `copy`, `file_preview` thumbnails, and API calls.

---

## 1. The brand (non-negotiables, from SKILL.md and the reel docs)

- **Look:** Tanim as a *small, well-dressed figure against a grand backdrop*: skyline, luxury hotel, rooftop, waterfront, the black BMW. He is usually not facing the camera, in strong or golden-hour light. The images are curated and colour-graded.
- **Wardrobe:** all-black, with a fine gold chain.
- **Faces:** **real face only.** The rule is "FACES / SUBJECT — NO AI, EVER" in stills. A faithful master uses Tanim's original pixels only.
- **Rubric:** small figure vs. grand background · not facing camera · strong light · clean composition · high-status setting · technical quality.
- **Grade:** filmic S-curve with protected highlights and rich blacks. The palette is muted but rich, with natural skin. Lift the light, never the complexion. Finish with subtle clarity, a vignette and fine grain, cropped to **4:5**.
- **Reel grade:** teal shadows, cream mids, caramel highlights.
- **Reel voice:** quiet-luxury, witty, classy. **No romantic or lonely framing.** Text cards are typewriter mono, ALL-CAPS, at most 5 words.

## 2. Folder map: the "waterfall" (retrieval cascade)

```
RAW shoots (plain-named, DNG/HEIC, ~284 GB)        e.g. /T/Philly Trip/4 Seasons
   │  editing team returns graded JPGs
   ▼
EDIT folders (date-named, ~12 GB)                  e.g. /T/2026_4_26 - PICS ( 4 Seasons )/EDITS
   │  propose → _PENDING → Grid Room review → commit (copy)
   ▼
/T/MASTER CURATED/NN_<Section>/  (working library, 29 sections, 415 photos)
   │  derived, never hand-edited
   ▼
/T/SUPERMASTER/  Primary (128 heroes) · Secondary (263 alternates) · Raw (19 + GRADE.txt)
   │
   ▼
IG: state/ig_order.json (115 planned) · 00_IG Live (25 posted) · 00_IG Archive (7)
```

**Retrieval order for any task** (post, reel start-frame, reference):

1. SUPERMASTER/Primary
2. SUPERMASTER/Secondary
3. the EDIT folders
4. the raw shoot

Graded-first is a hard rule.

### Side tracks

| Track | Path | What it is |
|---|---|---|
| BMW composites | `/T/MASTERS_STAGING` → `/T/MASTERS_STAGING_V2` → `MASTER CURATED/03_Car Masters` | S01–S17 setups from the Jul 26 car shoot. V2 builds come in FAITHFUL and LATITUDE variants. |
| Reels / AI video | `/T/IG Grid/Reel Concept/` | 10 Micro Reels (9 produced), Concierge v1–v20, New York Itemized, Vice City. Twelve Floors and Before The City Wakes are not yet produced. |
| Identity anchors | `/T/T Character Reference.png` (7-angle sheet), `Reel Concept/seeds/face_sheet/01–07`, `Concierge/seeds/face_element.jpg`, `bmw_element.png` | Used for QC on every AI generation. |
| Brain / state | `MASTER CURATED/_CURATION_SKILL/` (SKILL.md, SHOOT_INDEX.md, state/*.json, viewer/library.html) | The Grid Room is published from this repo (`index.html` + `assets/`). |

## 3. How photos were manipulated before (MASTER PIPELINE V2, Aug 9 2026)

| Step | Tool | Rule |
|---|---|---|
| RAW develop (norm + push) | RawTherapee / darktable (local) | 16-bit TIFF + q97 JPEG |
| Subject cutout, masks, face/neck light, crops | **Adobe** (`image_remove_background`, `image_select_by_prompt`, `image_apply_adjustments`, `image_crop_and_resize`) | *Omit exposure/gamma on the face; use brightness + shadows + darks + small contrast + vibrance.* No generative work in Adobe. |
| Removals, clean plates, facade rebuilds | **Higgsfield Nano Banana Pro** | One generation per master. Re-roll rather than stack generations. Exposure-lock prompt: "never relight". |
| Composite real subject back onto AI plate | Local "ImageOps" compositor | Align on the car body or the ground, never on the sky. Add contact shadows per foot. |
| QC gate after every round | Visual: full frame + 100% crops of face and seams | Compare against `00_REF_SKIN` (`Photo Jul 26 2026, 7 18 52 AM.dng`). |
| Reels: "photos come to life" | **Higgsfield Kling 3.0** (~7.5 credits, motion-only prompt) for evidence shots + **Seedance 2.0** (~45 credits) for bridges | Seedance gets all 7 face-sheet angles as references, plus the identity-lock and hair clauses. At most 2 takes. MiniMax Hailuo via MONID is the fallback. |

File suffixes:
- `-Edit` = graded export.
- `_face-matched` = re-edit that supersedes the original (same identity, so it replaces in place).
- `_AI-clean-foreground` = AI clean-up of the foreground.
- `_FAITHFUL` / `_LATITUDE` = composite variants.
- `.GRADE.txt` = grading instructions for a raw pick.

## 4. Connector matrix (as of this session)

| Need | Connector | Status |
|---|---|---|
| Browse, preview, curate (server-side copy), share | **Dropbox** | ✅ live |
| Masks, adjustments, Lightroom presets, crops, generative expand, quick-cut video, mood boards, Express designs | **Adobe for creativity** | ✅ live. It cannot upscale, remove objects, or swap backgrounds. |
| Kling 3.0 / Kling O1 (image→video), HeyGen Video 1, MiniMax Hailuo 2.3, Topaz upscale (photo + video), GPT-Image edit (up to 16 refs), IG profile/post scraping | **MONID** | ✅ live, wallet **$22.99** |
| Nano Banana Pro, Seedance 2.0, Kling with saved face/BMW elements | **Higgsfield** | ⚠️ **needs re-auth** in claude.ai → Settings → Connectors |
| Voice-note capture | **Plaud** ("Tanim" connector) | ✅ live (no photo notes found yet) |
| Video editing | **Descript** | ⚠️ needs auth |
| Paid promotion | **Facebook Ads** | ⚠️ needs auth |
| Tasks / scheduling | Todoist, Google Calendar | ✅ live |

**Phone-native rule:** Higgsfield's `--image-references` needed *local file paths*. In the cloud, pass Dropbox temporary download links or Adobe presigned URLs instead, and keep the 7-angle face sheet as the reference set.

## 5. What needs fixing (audit findings)

1. **~322 GB of pure duplication (half of `/T`).** 75 shoot folders, plus EDITS, SUPERMASTER, both MASTERS_STAGING folders, `_claude_tmp_proxies`, Voice Assistant and Best Picks, each have a *"(T-Surface-Pro's conflicted copy 2026-10-04)"* twin. Every twin matches its original by name and size.
   - **Exception:** `IG Grid`. The *original* has one extra file (`IMG_6808.DNG`), so keep the original.
   - Two more exact dupes: `2026_5_20 … PA Falls Horse Car ) (1)` and `2026_3_23 … St Regis Part 2 ) (1)`.
2. **Inside `MASTER CURATED`, the conflicted copies are the NEWER versions.** `SKILL (…conflicted…).md` carries the 2026-10-04 path note: "everything now lives under `/T/`". The same probably applies to `00_MANIFEST`, `SHOOT_INDEX`, `config.json` and the scripts. Merge these *into* the originals before deleting anything.
3. **SHOOT_INDEX.md is missing sections 28 (NYC Luxe) and 29 (NYC Jul26),** and `00_MANIFEST.md` is missing 29.
4. **Grading backlog:** 19 raw picks in `SUPERMASTER/Raw`.
   - 11 Evidence DNGs also need +90° rotation.
   - The full list is in `25_Evidence Curated/_GRADING_NEEDED.txt`.
5. **Raw shoots with no EDIT folder:** St Regis (part 1), T Cat Cafe, Spring Pictures, One Hotel Entrancd, Amrit (×2), Sung Lee. Most are deliberately excluded (other people or off-brand), but check this against `state/processed.json`.
6. **Junk at the root:** `test123.txt`, `test549.txt`, `_sync_probe.txt`, `New folder`, `Best Picks for IG` (one stale contact sheet), `_claude_tmp_proxies` (75 proxies).
7. **Not brand-related but sitting in `/T`:** Docs (signed letters), Trent agreement, Resume, Carpentry agreement, `sportsaccess.apk`, `Qilates Ad.mp4`.

## 6. Phone commands (say these in a session)

- **"Curate new shoots"** — I scan `/T` for shoot folders not in `processed.json`. I build thumbnail contact sheets via `file_preview`, propose 2–7 heroes per shoot into `_PENDING`, and republish the Grid Room so you can approve.
- **"Commit approved"** — paste the Grid Room export. I server-side `copy` the approved picks into `NN_<Section>` and update the manifest, index and ledger. Then I re-derive SUPERMASTER.
- **"Grade the raw backlog"** — Adobe: rotate, auto-tone, select subject + face lift, then crop to 4:5. Results go back as `-Edit` files for your review.
- **"Make a reel from [photo]"** — Kling (MONID or Higgsfield) at 9:16, 5 s, motion-only prompt. Then QC against the face sheet and save to `Reel Concept/…/masters`.
- **"Plan the next 9 posts"** — I pull from `ig_order.json`, check the live grid via MONID's Instagram endpoint, and propose order + captions in the archive's caption voice.
- **"Clean up duplicates"** — I give you a reviewed delete list first, and nothing is deleted without your explicit OK. Dropbox `delete` goes to *Deleted files* and is recoverable.
