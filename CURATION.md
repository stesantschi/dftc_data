# Curation Log

How these datasets were assembled and what was changed from the source data.
AI assistance is disclosed in `AI_USE.md`.

The dataset is **two separate batches**, uploaded to Smapshot at different times
(not one export split in two). Each had its own preparation pass, which is why
their field profiles differ — see the data dictionary.

---

## Batch 1 — `bm_ndl_meisho_landmarks.csv` (1,345 records) - June 2026

### Sources

Three sources, prepared individually and then combined into a single import file:
- **British Museum**, Collection Online (CC BY-NC-SA 4.0)
- **National Diet Library**, meisho-zue (illustrated gazetteers), Public Domain Mark
- **National Archives of Japan**, Digital Archive / Naikaku Bunko (CC0 1.0)

### Process

Each source was worked up separately, then concatenated into one upload file.
Preparation was substantially **manual** and included:

- **Geolocation.** Inscribed place names were read from the works and looked up to assign latitude/longitude viewpoint coordinates by hand. 
  These populate `longitude` / `latitude` and are *a priori* viewpoint positions, not surveyed points.
- **Illustrator attribution.** Artist names were completed where the source record was partial, and linked to the artist lookup via `photographer_id`.
- **Credit lines.** Per-record rights and attribution statements were composed for the `license` field, preserving each institution's image terms and adding the project's curation statement.
- **Title and caption curation.** `title` and `caption` were edited to meet Smapshot's data requirements.
- **Artist-name standardization.** Names were normalized to a consistent scheme,with particular attention to the Utagawa-school naming lineage (see "Artist naming" below).

### Notes

- `original_id`: 52 records are blank; 796 records share duplicated IDs. The duplication reflects multi-page meisho-zue **book objects**, 
   where many views derive from a single catalogued object (including the ~181 *Miyako meisho zue* 都名所図会 records).
- `date_shot` is empty by design: exact dates are not knowable for these works, so `date_min` / `date_max` ranges are used instead.
- `shop_link` is not applicable to these sources and is left empty.

---

## Batch 2 — `jmet_ndl_taito.csv` (1,757 records) - November 2025

### Sources

- **National Diet Library** Digital Collections, retrieved via **Japan Search**
- **ADEAC** (Digital Archive System), retrieved via Japan Search

Images are referenced by link only and remain under the holding institutions' terms; the project redistributes **metadata, not images**. 
The educational, non-commercial nature of the project is recorded in `LICENSE-data.md`.

### Process

**Selection.** Images were included only where georeferencing is meaningful:
purely mythical depictions and extreme close-ups (where no locatable view is recoverable) were excluded, as were text-only pages. 
All output was verified to be UTF-8 encoded.

**Upstream harvest and curation.** The meisho-zue corpus was harvested from the **NDL Digital Collections** 
(with Japan Search used as a discovery tool, not a harvest source), under a hard open-access constraint. 
Corpus scope covered Edo, Kyoto, and Osaka/Kansai meisho-zue; 
genre and edition mismatches were excluded by hand (e.g. *Naniwa meisho zue* as a later Taishō compilation, and *Sumiyoshi meisho zue* for insufficient evidence as a standalone work).
Deduplication was strict — one record per unique work, earliest/best copy, with ambiguous cases sent to manual review; 
pre-Meiji nishiki-e were kept and Meiji+ standalone prints dropped. 
Romanizationwas added (Hepburn romaji derived from kana) since NDL exposes no English metadata, and *Miyako* toponyms were extracted programmatically from Nichibunken with per-record georeferencing-confidence flags. 
Fuller step-by-step methodology lives in the project's cleaning reports.

For the import file specifically: 
`original_id` values are complete and unique;
`date_orig` is ~47% populated; and `download_link` / `link` / `shop_link` were not populated for this batch, which carries the georeferencing-relevant fields rather than the optional link columns.

---

## Artist lookup — `artists.csv` (196 entries)

The artist table is a controlled vocabulary that both record files reference through `photographer_id` → `artists.id`.

### Artist naming

Names were standardized to a single scheme. Generational and successive art names within a lineage — most importantly the Utagawa school, 
where an artist could work under several names across a career — are disambiguated with numbered identifiers 
(e.g. `kunisada_utagawa_1`, `kunisada_utagawa_2`). 
Where authority data exists, entries link out (mostly to Wikidata) via the `link` field.

### Provisional entries

32 entries are incomplete (missing birth/death dates and/or name fields) and are marked provisional. The `company` field is sparsely populated (3 of 196 rows).
