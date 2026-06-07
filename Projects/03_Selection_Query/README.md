# 03_Selection_Query — Spatial Data Selection & Export

## 🎯 Objective
Query and extract specific administrative regions from Egypt's dataset using attribute-based selection, then export the result as a standalone spatial file for further analysis.

---

## 🧠 What I did

### Identify Tool
- Used the Identify Tool (I) to inspect individual governorate attributes
- Discovered field structure: GID_1, NAME_1, NL_NAME_1, TYPE_1, HASC_1, ISO_1
- Found that GID_1 follows pattern: `EGY.{number}_{version}`

### Select by Expression
- Opened Select by Expression (Ctrl+F3)
- Wrote SQL-style expression to select Upper Egypt governorates:
  ```sql
  "NAME_1" IN ('Asyut', 'Suhaj', 'Qina', 'Al Uqsur', 'Aswan', 'Al Minya')
  ```
- Result: 6 features selected (highlighted in yellow)

### Export Selected Features
- Exported selection as new Shapefile:
  `Data/Processed/upper_egypt.shp`
- Validated: Attribute Table confirmed exactly 6 features

### Visualization
- Added OSM basemap for geographic context
- Styled EGY_1 as neutral background (gray, 40% opacity)
- Applied Categorized symbology on upper_egypt (Qualitative ramp)
- Configured Arabic/English label fallback expression
- Applied Label Buffer (white halo) for readability

---

## 📂 Input Data

| File | Description |
|------|-------------|
| gadm41_EGY_1.shp | All 27 Egypt Governorates |

## 📤 Output

| File | Description |
|------|-------------|
| Data/Processed/upper_egypt.shp | 6 Upper Egypt Governorates only |

---

## ⚠️ Issues Faced & Solutions

| Issue | Root Cause | Solution |
|-------|-----------|----------|
| Select by Expression not in Vector menu | Wrong menu path | Correct path: Edit → Select Features → By Expression (Ctrl+F3) |
| Some Arabic names missing | Data Quality issue in GADM | Known limitation — only Aswan & Qina have Arabic names |

---

## 📸 Result Preview

![Selection Query Map](../../output/03_Selection_Query.png)

---

## 📌 Key Learnings

- GID_1 is the unique identifier — essential for Spatial Joins later
- Selection is temporary; Export creates a permanent new layer
- `IN` operator selects multiple values in one expression
- Always save to `Data/Processed/` — never modify Raw data
- Data Quality issues exist in real datasets — document them, don't ignore them