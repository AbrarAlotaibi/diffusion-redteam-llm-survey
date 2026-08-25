# Diffusion Models for Red Teaming Large Language Models: A Critical Survey and Research Agenda

Companion repository for the manuscript

> Alotaibi, A. and Ahmed, M. (2026). *Diffusion Models for Red Teaming Large Language Models: A Critical Survey and Research Agenda.* Submitted to *ACM Computing Surveys.*

This repository contains the manuscript source, the relational catalog of cataloged papers, the title-and-abstract screening log, the two-agent re-screening artifacts, and the per-paper quality assessment.

## What's in the repository

```
.
├── README.md                                  This file
├── diffusion_attacks_screening.xlsx       Primary artifact (5 sheets, see below)
├── prompts.md       Agents prompts

```

The primary artifact is `diffusion_attacks_screening.xlsx`, which contains the screening log, the relational catalog, the legend, and the quality assessment as five sheets.

## Spreadsheet contents

`diffusion_attacks_screening.xlsx` has the following sheets.

**Summary.** Numerical summary of every Section 2.3 claim. 154 candidate records after de-duplication, 94 passed title-and-abstract screening, 58 papers in the final catalog, 44 excluded at full-text, the 4+6+8+18+12+10 family breakdown, Cohen's $\kappa$ = 0.781 with 89.6% observed agreement on a 52.5% chance-corrected baseline, agreement with the authors' catalog (50/50 inclusions and 88/104 exclusions), 16 boundary cases.

**Screening.** Row-per-candidate screening log for all 158 records (the 154-record de-duplicated pool plus four records added by later supplementary runs): arXiv ID or venue marker, title, one-line summary, surfacing query (Q1 through Q14 or supplementary), title-and-abstract decision (PASS or FAIL), full-text decision (INCLUDE or EXCLUDE), exclusion reason, and a flag indicating whether the record is in the final 58-paper catalog. Rows colour-coded for outcome: light green = in catalog, light yellow = passed screen but excluded at full-text, light orange = failed at title-and-abstract.

**Catalog.** Row-per-paper metadata table for the 58 retained papers. Columns: ID, Title, Authors (short), Year, Venue, arXiv/DOI URL (hyperlinked), Scope, Diffusion role, Diffusion type and space, Training method, Datasets, Metrics, Target models, Code link, Key result and contribution, Critique and limitation.

**Catalog_Legend.** Colour key for scope tags and definitions for the diffusion-role categories.

**Quality_Assessment.** Row-per-paper Q1–Q8 scoring under the quality checklist defined in Table 1 of the manuscript. Scale: 1 = clearly yes, 0.5 = partial or unclear, 0 = no or not reported. Row total and aggregate per-question means included at the bottom.

## Citation

```bibtex
@article{alotaibi2026diffusion,
  author    = {Alotaibi, Abrar and Ahmed, Moataz},
  title     = {Diffusion Models for Red Teaming Large Language Models:
               A Critical Survey and Research Agenda},
  journal   = {ACM Computing Surveys},
  publisher = {Association for Computing Machinery},
  year      = {2026},
  note      = {Under review}
}
```

## License

- Manuscript text, figures, and tables: Creative Commons Attribution 4.0 (CC BY 4.0).
- Spreadsheet data (screening log, catalog, quality assessment): CC0 1.0 Universal (public domain dedication) to maximise reusability.
- LaTeX source and any helper scripts: MIT License.

## Funding and acknowledgements

This research is supported by a grant (No. CRPG-25-2057) under the Cybersecurity Research and Innovation Pioneers Initiative, provided by the National Cybersecurity Authority. The authors also acknowledge the support received from the Saudi Data and AI Authority (SDAIA) and King Fahd University of Petroleum and Minerals (KFUPM) under the SDAIA-KFUPM Joint Research Center.

## Contact

- **Abrar Alotaibi** (corresponding author): `amotaibi@iau.edu.sa`. College of Computer Science and Information Technology, Imam Abdulrahman Bin Faisal University; SDAIA-KFUPM Joint Research Center for Artificial Intelligence.
- **Moataz Ahmed**: `moataz@kfupm.edu.sa`. Information and Computer Science Department, King Fahd University of Petroleum & Minerals; SDAIA-KFUPM Joint Research Center for Artificial Intelligence.

Please open an issue on this repository for corrections, additions, or methodological questions.
