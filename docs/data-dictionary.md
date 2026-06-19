# Data Dictionary — Drawing from the Crowd (Smapshot import datasets)

This dataset contains curated **metadata** for Japanese landscape prints and illustrated books (meisho-zue), 
prepared for georeferencing on the Smapshot platform. 
**No image files are included.** 
Image columns are URLs that point back to the holding institutions (NDL, ADEAC via Japan Search, the British Museum, the Met, the National Archives of Japan), which retain all rights to the images themselves. 
The standardization, enrichment, and geographic tagging are the project's own contribution.

## Files

| File | Rows | Role |
| --- | --- | --- |
| `bm_ndl_meisho_landmarks.csv` | 1,345 | Print/book records (British Museum + NDL meisho-zue + National Archives of Japan) |
| `jmet_ndl_taito.csv` | 1,757 | Print/book records (NDL via Japan Search + ADEAC) , Taito Lifelong Learning center|
| `artists.csv` | 196 | Artist lookup table |

**Key relationship:** `photographer_id` in the two record files is a foreign key into `artists.id`. (The Smapshot template reuses the field name `photographer_id` for the artist of the print.)

## File format note

These follow the Smapshot import template, which has a banded header:
- Row 1 — a category band (`Metadata displayed`, `Used for georeferencing`, etc.)
- Row 2 — the real field names listed below
- Row 3 — a human description of each field
- Row 4 onward — data
When loading programmatically, use row 2 as the header and skip row 3.

---

## Record files: `bm_ndl_meisho_landmarks.csv` and `jmet_ndl_taito.csv`

Both share the same columns.
| Field | Meaning | Notes / known issues |
| --- | --- | --- |
| (first column) | Template label column | Empty in data rows; a template artifact, safe to ignore or drop |
| `download_original_image_link` | URL to the original image at the source institution | Pointer only — not an embedded image |
| `link_id` | Identifier linking metadata to the image file | |
| `title` | Title of the image | BM/NDL: 100% filled. JMET/Taito: 2 rows blank |
| `caption` | Description of the image | BM/NDL: 100%. JMET/Taito: ~93% filled |
| `original_id` | Identifier linking the record back to the source catalogue | BM/NDL: 52 rows blank; 796 rows share duplicated IDs, reflecting multi-page meisho-zue book objects (incl. the ~181 *Miyako meisho zue* 都名所図会 rows) where many views derive from one catalogued object. JMET/Taito: all present and unique. |
| `date_shot` | Exact date (dd/mm/yyyy) if known | Empty by design — exact dates are not knowable for these works; date ranges are used instead |
| `date_min` | Start of date range | 100% filled |
| `date_max` | End of date range | 100% filled |
| `license` | Image rights + source attribution + project's curation statement | 100% filled; verbatim per source institution |
| `photographer_id` | Artist of the print → `artists.id` | BM/NDL: 31 blank; non-blank values resolve. JMET/Taito: 11 blank; a few Utagawa-lineage values are pending addition to the lookup |
| `download_link` | Optional user-facing download URL | 
| `link` | Optional deeplink to the image info page | 
| `shop_link` | Optional purchase link | Empty in both — not applicable to these sources |
| `date_orig` | Original free-text date (e.g. "19th century") | BM/NDL: ~95%. JMET/Taito: ~47% filled |
| `longitude` | A priori viewpoint longitude | 100% filled |
| `latitude` | A priori viewpoint latitude | 100% filled; 25 JMET/Taito records fall outside Japan and are flagged `in_japan_bbox: false` in `dftc_points.geojson` |
| `view_type` | One of: terrestrial / highOblique / lowOblique / nadir | BM/NDL: lowOblique (1,126) + terrestrial (219). JMET/Taito: all lowOblique |

---

## Lookup file: `artists.csv`

| Field | Meaning | Notes / known issues |
| --- | --- | --- |
| `how it appears in ARC file` | Source-form name string (kanji + romanized variants) | 192/196 filled |
| `id` | Stable artist identifier — **FK target for `photographer_id`** | 196/196; primary key |
| `first_name` | Family name (kanji + romaji) | 193/196 |
| `last_name` | Given/art name (kanji + romaji) | 191/196 |
| `link` | Authority link (mostly Wikidata) | 175/196 |
| `company` | Workshop/school (sparsely used) | 3/196 filled |
| `start` | Birth / active-from date (ISO 8601) | 169/196 |
| `end` | Death / active-to date (ISO 8601) | 168/196 |

**Stub entries:** 32 artist rows are incomplete (missing dates and/or name
fields) and are marked provisional.

---

## Schema crosswalk (Dublin Core)

A mapping of dataset fields to Dublin Core Metadata Initiative (DCMI) terms,
to support interoperability and harvesting. Terms use the `dcterms:` namespace.

### Record files

| Dataset field | DCMI term | Notes |
| --- | --- | --- |
| `title` | `dcterms:title` | |
| `caption` | `dcterms:description` | |
| `photographer_id` | `dcterms:creator` | resolved via `artists.csv`; artist `link` (Wikidata) serves as a creator authority identifier |
| `date_min` / `date_max` | `dcterms:created` | date range of the work |
| `date_orig` | `dcterms:created` | original free-text date |
| `license` | `dcterms:license`, `dcterms:rights` | per-record image rights and attribution |
| `download_original_image_link` | `dcterms:source` | link to the source image at the holding institution |
| `link` / `download_link` | `dcterms:source` | source record / download page |
| `original_id` | `dcterms:isPartOf` / `dcterms:identifier` | source-catalogue identifier; shared values group multi-page book objects (the *meisho-zue* case) |
| `link_id` | `dcterms:identifier` | record identifier within the dataset |
| `longitude` / `latitude` | `dcterms:spatial` | a-priori viewpoint coordinates (DCMI Point) |
| `view_type` | — | project-specific (Smapshot); no Dublin Core equivalent |
| holding institution (in `license`) | `dcterms:publisher` | digitizing/holding institution |

Dataset-level (Japanese-language works; resource types are still images and
illustrated books): `dcterms:language` = `ja`; `dcterms:type` = `Image` / `Text`.

### Dataset (collection) level

| DCMI term | Value |
| --- | --- |
| `dcterms:title` | see `CITATION.cff` |
| `dcterms:creator` | Santschi; Panday; Tsuji; Richardson |
| `dcterms:publisher` | ThePrintLab / University of Zurich |
| `dcterms:rights` | CC BY 4.0 (metadata); images remain with the holding institutions |
| `dcterms:subject` | ukiyo-e; meisho-zue; digital art history; citizen science; georeferencing |
| `dcterms:type` | Dataset |
| `dcterms:date` | release date |

Coordinates (`dcterms:spatial`) are free-text lat/long and are not linked to a gazetteer (e.g. GeoNames, Getty TGN).
