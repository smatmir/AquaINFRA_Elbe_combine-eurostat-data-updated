# combine_eurostat_data — Updated Version

This repository contains the updated **`combine_eurostat_data.R`** function used in the AquaINFRA Elbe Use Case workflow.  
The updated version introduces **user‑selectable country, NUTS version year, and population reference year**, while enforcing valid Eurostat/GISCO combinations.

---

## 📌 Overview

`combine_eurostat_data()` downloads and merges:
- **GISCO NUTS3 geometries** (different schema versions limited to: 2013, 2016, 2021, 2024)
- **Eurostat population dataset** (`demo_r_pjangrp3`) for a user‑selected reference year

It outputs a GeoPackage (`.gpkg`) containing harmonized NUTS3 polygons with the selected year’s population stored in a column like:

```
POP_<year>
```

This module is used as the **first data‑retrieval step** in the Elbe D2KP workflow.

---

## 🎯 Function Purpose

This script provides a reusable and self‑contained R function that:

1. Validates user‑selected parameters  
2. Downloads official NUTS3 boundaries from GISCO  
3. Downloads population statistics from Eurostat  
4. Filters data to the requested country  
5. Ensures legal year mapping between GISCO and Eurostat  
6. Outputs a ready‑to‑use NUTS3 GeoPackage for downstream analyses

---

## 🧩 Valid Year Combinations

Not all combinations of NUTS year and population year are compatible.  
This function enforces official matching between:

### ✔ GISCO NUTS Versions  
Available GISCO NUTS geometries are limited to major releases:

| NUTS Version | Available From GISCO |
|-------------|----------------------|
| **2013** | yes |
| **2016** | yes |
| **2021** | yes |
| **2024** | yes |

### ✔ Eurostat Population (demo_r_pjangrp3)

Eurostat provides **annual population** at NUTS3 level only from **2014–2024**.

### ✔ Allowed Combinations (implemented in this function)

| NUTS Year | Allowed Pop Years |
|----------|-------------------|
| **2013** | **2014–2017** |
| **2016** | **2018–2020** |
| **2021** | **2021–2023** |
| **2024** | **2024–2030** *(future-proof)* |

These mappings reflect:
- Structural breaks in regional definitions
- Changes in NUTS3 boundaries  
- Eurostat’s population table being aligned with specific NUTS versions

---

## 🚀 Usage (CLI)

Inside a Docker container or terminal:

```bash
Rscript combine_eurostat_data.R <country_code> <nuts_year> <pop_year> <output_gpkg_path>
```

Example:

```bash
Rscript combine_eurostat_data.R DE 2016 2018 out/nuts3_pop.gpkg
```

---

## 📦 Function Arguments

| Argument | Type | Description |
|----------|------|-------------|
| `country_code` | Character (e.g., `"DE"`) | ISO‑2 country code |
| `nuts_year` | Integer | Allowed: 2013, 2016, 2021, 2024 |
| `pop_year` | Integer | Must match allowed ranges |
| `output_gpkg_path` | Character | Output path to GeoPackage |

---

## 🗂 Output

The script writes a GeoPackage containing:

- NUTS3 polygons for the selected version  
- Population column renamed to:

```
POP_<pop_year>
```

Example:

```
POP_2018
```

Layer name:

```
nuts3_pop
```

---

## 📘 Notes on Eurostat & GISCO

### GISCO limitations
GISCO only provides NUTS geometries for major revision years and it is limited to (2013, 2016, 2021, 2024).  
This ensures stable boundary definitions aligned with policy and reporting cycles.

### Eurostat dataset (`demo_r_pjangrp3`)
- Population values exist only for **2014–2024**
- Codes at NUTS3 level are always **5 characters**
- Still aligned to different NUTS versions, hence the need for year‑compatibility rules

---

## 📄 Included Script

This repository includes:

- **combine_eurostat_data.R**  
  The fully updated function ready for integration into the OGC API and AquaINFRA D2KP workflow.

---

If you need a rendered PDF, DOCX, or Markdown formatting adjustments, I can generate these as well.  
