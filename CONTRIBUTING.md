# Contributing

There are two different ways to contribute, and they happen in different places.

## 1. Contributing observations (georeferencing)

If you want to help locate prints, that work happens on the **Smapshot
platform**, not in this repository. You pick a print, mark where the view was
taken from, and align it with the 3D terrain — no Japanese or art-history
background needed. Start here: https://landscapes.theprintlab.org

Submissions are reviewed by the project's PI before entering the dataset;
"unsuccessful" attempts are still useful data. By contributing on the platform
you agree that your contributions and username may be used for non-commercial
and educational purposes, including research outputs.

## 2. Proposing corrections to this dataset

Corrections, additions, and source verification for the data files here are welcome via an **issue** or **pull request**. 
Particularly useful:
- completing or verifying the provisional artist entries (the stubs noted in `CURATION.md`);
- confirming or fixing artist attributions and viewpoint coordinates;
- backfilling missing `original_id` values.

When you open an issue or PR, please include:
- the file and the row (its `link_id`, and `original_id` if present);
- the field and the proposed value;
- a source for the change, such as an institutional catalogue, NDL, Japan Search, or a Wikidata entity.

## Conventions to keep
- The record files follow the **Smapshot import template** (banded header rows). Don't shift or reorder columns — downstream steps read some fields by position.
- `photographer_id` must resolve to an `id` in `artists.csv`. If you add an artist reference, add the artist row too.
- Dates use ISO 8601; files are UTF-8.
- Where the project maintains Japanese and English, keep both in sync.

Substantive data contributions are acknowledged in releases and, where
appropriate, in research outputs.

## Acknowledgments

**Team**
- **Stephanie Santschi** (PI) — project conception, management, art-historical research, publishing, networking, data selection, data curation, outreach, documentation. University of Zurich, Chair of East Asian Art History.
- **Himanshu Panday** — data-preparation advice, computer-vision experiments, and the project website pipeline; digital prototyping and citizen-science platform development. Dignity in Difference.
- **Hirohito Tsuji** (辻 博仁) — networking, background research, and Japanese translation/transcription. University of East Anglia.
- **Drew Richardson** — collaborator. University of California, Santa Cruz.

**Partners**
- Smapshot Lab (HEIG-VD / EPFL) — the georeferencing platform.
- Art Research Center, Ritsumeikan University (ARC-iJAC) — Japanese prints portal database.

**Funders**
- The Nippon Foundation Scholars Association (TNFSA) — prototype development, via the 2024 Nippon Social Innovators Collaboration (NSIC).
- Graduate Campus, University of Zurich — implementation, via a Career Grant to the PI.

**Data sources**
- Metropolitan Museum of Art (Open Access); British Museum; National Diet Library, Taito City Library (via Japan Search); National Archives of Japan / Naikaku Bunko; Art Research Center, Ritsumeikan University.
- Terrain: Geospatial Information Authority of Japan (GSI).
- Enrichment: Wikidata; GeoHack Toolforge.

## Contact

Questions or collaboration ideas: see the contact details at
https://landscapes.theprintlab.org
