# 04_Field_Calculator — Governorate Area Calculation

## 🎯 Objective
Add a calculated area field to Egypt's governorate dataset and visualize the spatial distribution of governorate sizes using graduated symbology.

---

## 🧠 What I did

### Field Calculator
- Opened Attribute Table → Field Calculator (Ctrl+I)
- Added new field `area_km2`:
  ```
  Type       → Decimal number (real)
  Expression → round($area / 1000000, 2)
  ```
- QGIS 4.x uses Ellipsoidal calculation automatically → accurate results without reprojection

### Logarithmic Scale Field
- Added second field `area_log` to handle extreme outliers:
  ```
  Expression → log10("area_km2")
  ```
- Reason: Al Wadi al Jadid (426,526 km²) is 115x larger than Al Uqsur (618 km²)
- Log scale compresses the range: 2.79 → 5.63 (vs 618 → 426,526)

### Graduated Symbology
- Applied Graduated symbology on `area_log`
- Color ramp: YlOrRd (Yellow → Orange → Red)
- Mode: Quantile (Equal Count) — 5 classes
- Added OSM basemap with 75% opacity
- Labels: NAME_1 + area_km2 with white halo buffer

---

## 📂 Input Data

| File | Description |
|------|-------------|
| gadm41_EGY_1.shp | 27 Egypt Governorates |

## 📤 Output Fields Added

| Field | Type | Expression | Description |
|-------|------|-----------|-------------|
| area_km2 | Decimal | `round($area / 1000000, 2)` | Area in km² |
| area_log | Decimal | `log10("area_km2")` | Log scale for visualization |

---

## ⚠️ Issues Faced & Solutions

| Issue | Root Cause | Solution |
|-------|-----------|----------|
| All colors same (red) | Outlier (Al Wadi al Jadid) dominates scale | Used log10 transformation |
| Color ramp inverted | Default direction wrong | Used Invert Color Ramp |
| Area differs from official (~2-3%) | GADM includes territorial waters | Known limitation — acceptable for admin analysis |

---

## 📌 Data Accuracy Note

GADM area values include territorial waters and may differ slightly from official land-only figures (~2-3%). This is acceptable for administrative analysis purposes.

| Governorate | GADM (km²) | Official (km²) | Difference |
|-------------|-----------|----------------|------------|
| Al Gharbiyah | 1,999 | 1,943 | +2.8% ✅ |

---

## 📸 Result Preview

![Field Calculator Map](../../output/04_Field_Calculator.png)

---

## 📌 Key Learnings

- `$area` in QGIS 4.x = Ellipsoidal calculation = accurate without CRS change
- Outliers destroy graduated color scales → use log transformation
- Quantile mode = equal number of features per class (better visual distribution)
- Real datasets always have minor accuracy differences — document them
- `log10()` compresses extreme value ranges for better visualization
