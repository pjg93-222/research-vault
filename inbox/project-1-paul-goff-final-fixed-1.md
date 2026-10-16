---
type: inbox-item
title: "project_1_paul_goff_FINAL_fixed (1)"
status: "curated"
source_path: "C:\\Users\\pgoff\\Downloads\\project_1_paul_goff_FINAL_fixed (1).md"
sha256: a7535c56cca868d24213f01589b298bf9c3960e78c0e03d6e1663d5a44bcbe23
downloaded_at: "2026-09-16T11:16:08-05:00"
size: 17822
copy: "_sources/inbox/2026-10-15/project_1_paul_goff_FINAL_fixed (1).md"
tags: [inbox, type/inbox]
canonical_note: "obsidian://open?vault=linked&file=knowledge%2Fcoursework%2Fgeneral%2Fcoding-utf-8-f5deb72ca0"
collection: "coursework"
extraction_status: "text-extracted"
evidence_status: "UNVERIFIED"
curated_at: "2026-10-16"
---

# project_1_paul_goff_FINAL_fixed (1)

Raw inbox item picked up from Downloads by `vaultlink/research_inbox.py`. No bibliographic metadata has been extracted or inferred; the title above is the file name.

- [ ] Paul: classify this item as **paper**, **coursework** or **other**.

> [!quote] Text snippet (first characters, unverified extraction)
> ```python # -*- coding: utf-8 -*- ``` # Project One **Paul Goff** *9/15/2026* ## Pre-Processing ```python import pandas as pd import numpy as np import statsmodels.api as sm from statsmodels.stats.outliers_influence import variance_inflation_factor from sklearn.model_selection import train_test_split from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score import matplotlib.pyplot as plt def vif_table(model): """Variance inflation factor for every predictor in a fitted model
