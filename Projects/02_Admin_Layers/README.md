# 02_Admin_Layers — Administrative Boundaries of Egypt

## 🎯 Objective
Load, style, and label Egypt's administrative boundary data (GADM) in QGIS with a professional multi-level visualization including OSM basemap, smart labeling, and scale-dependent visibility.

---

## 🧠 What I did

### Layer Management
- Imported GADM Egypt shapefiles (EGY_0 / EGY_1 / EGY_2)
- Organized layers in correct order: EGY_2 → EGY_1 → EGY_0 → OSM
- Verified CRS consistency across all layers (EPSG:4326)
- Added OpenStreetMap basemap for geographic context

### Symbology
- Applied Categorized symbology on EGY_1 (Governorates)
- Used Qualitative color ramp (Set3) — not Sequential
- Set layer opacity to 70% to allow basemap visibility underneath
- Kept EGY_2 as transparent fill to show district boundaries only

### Labels & Expressions
- Configured smart labels using CASE WHEN expression:
  ```sql
  CASE
    WHEN "NL_NAME_1" IS NOT NULL AND "NL_NAME_1" != 'NA'
    THEN "NL_NAME_1"
    ELSE "NAME_1"
  END
  ```
- Applied Label Buffer (Halo) — 1mm white — for readability over colors
- Set Scale-Dependent Visibility on EGY_2 labels (min: 1:500,000)

### Data Quality
- Identified and handled NULL/NA values in Arabic name fields
- Applied fallback logic: Arabic name → English name → skip

---

## 📂 Input Data

| File | Description |
|------|-------------|
| gadm41_EGY_0.shp | Egypt national boundary |
| gadm41_EGY_1.shp | 27 Governorates |
| gadm41_EGY_2.shp | Districts (sub-governorate level) |

---

## 📤 Output
- Professional administrative map of Egypt
- Multi-level labeling (Arabic primary / English fallback)
- Scale-responsive label visibility

---

## ⚠️ Issues Faced & Solutions

| Issue | Root Cause | Solution |
|-------|-----------|----------|
| Labels showing "NA" | NULL values in NL_NAME field | CASE WHEN expression with fallback |
| Multiple OSM layers | Added basemap without removing duplicates | Keep ONE OSM at bottom |
| Labels unreadable | No contrast against colored polygons | Label Buffer (white halo) |
| Color ramp wrong | Sequential ramp used for categorical data | Replaced with Qualitative (Set3) |

---

## 📸 Result Preview


![Admin Layers Map](output/02_Admin_Layers.png)
---

## 📌 Key Learnings

- Administrative data has levels (0 = Country, 1 = Governorate, 2 = District)
- CRS must be consistent across all layers in a project
- Qualitative ≠ Sequential color ramps — wrong choice misleads the reader
- CASE WHEN expressions handle missing data gracefully
- Scale-dependent visibility prevents label clutter at small scales
- Layer transparency creates visual hierarchy without hiding data