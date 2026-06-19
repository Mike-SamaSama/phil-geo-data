# Geographic Data Registry: District Payloads

This repository serves as the absolute, data-driven authority for onboarding and whitelabeling congressional district offices on the platform. It hosts the standardized 9-column PSGC barangay datasets and optimized GeoJSON boundary shapefiles.

By utilizing this pure data model, geographic dropdowns, dashboards, and map components render dynamically across instances with **zero code deployments or hardcoded backend maps**.

---

## 📋 Ingestion Schema Contract (V2 Strict)

All incoming `.csv` payloads must strictly follow this 9-column specification. Legacy 8-column files will be rejected by the validation linter. Whatever literal text is written in the `municipality_name` column acts as the absolute display authority in the frontend dropdown UI.

### Column Definition Matrix

| Index | Column Header | Data Type | Example | Notes |
| --- | --- | --- | --- | --- |
| 0 | `psgc_10_digit` | String (10) | `1704001001` | Unique Barangay identifier |
| 1 | `name` | String | `Agot` | Official Barangay title |
| 2 | `geographic_level` | String | `Bgy` | Baseline aggregation layer |
| 3 | `urban_rural` | String (1) | `R` | `U` (Urban) or `R` (Rural) |
| 4 | `correspondence_code` | String | `174001001` | Legacy code mapping |
| 5 | `income_classification` | String | `""` | Optional blank override field |
| 6 | `municipality_psgc_code` | String (7) | `1704001` | Parent Municipality Identifier |
| 7 | **`municipality_name`** | String | `Boac` | **Verbatim dropdown string text** |
| 8 | `province_name` | String | `Marinduque` | Parent Province Layer |

---

## 🗺️ GeoJSON Vector Maps Configuration

Shapefiles must be converted to optimized GeoJSON format and linked directly inside the platform configurations.

> ⚠️ **CRITICAL (CDN Delivery Rule):** When referencing a `.geojson` vector file in the dashboard settings, do not use the standard repository page view link. You must grab the raw coordinate asset path by clicking the **"Raw"** button inside the GitHub file viewer.

* **❌ Wrong (HTML Wrapper):** `https://github.com/username/repo/blob/main/marinduque.geojson`
* **✅ Right (Pure Web Vector CDN):** `https://raw.githubusercontent.com/username/repo/main/marinduque.geojson`

---

## 🚀 Onboarding a New District (Runbook)

To provision a completely new district office instance without modifying software code, execute the following steps:

1. **Format Data Sheet:** Extract the new territory records from the official PSA registry and build a 9-column CSV file matching the matrix contract above.
2. **Compress Shapefiles:** Drag your raw boundaries into `mapshaper.org`, simplify coordinates down to **10%–15%** to reduce footprint weight, and export as a clean `.geojson` file.
3. **Commit Assets:** Push both the updated dataset CSV and the map vector GeoJSON into this repository.
4. **Link Properties:** Copy your raw GitHub file delivery strings and configure your platform instance dashboard profile with the target district prefix parameters and the map URL.
5. **Run Provisioning:** Hit **Re-provision** to wipe out legacy partitions and watch the environment instantly render the fresh baseline payload.
