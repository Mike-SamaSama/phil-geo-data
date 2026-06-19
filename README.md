# Geographic Data Registry & Shapefile Provisioning

This repository serves as the absolute, data-driven authority for onboarding and whitelabeling congressional district offices on the platform. It hosts the standardized 9-column PSGC barangay datasets and optimized GeoJSON boundary shapefiles.

By utilizing this automated framework, geographic dropdowns, dashboards, and map components render dynamically across instances with **zero code deployments or hardcoded backend maps**.

---

## 📋 1. Ingestion Schema Contract (V2 Strict)

All incoming `.csv` payloads must strictly follow this 9-column specification. Legacy 8-column files will be rejected immediately by the client-side validation linter. The literal text written in the `municipality_name` column acts as the absolute display authority in the application frontend UI.

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

## 🗺️ 2. Shapefile Processing Pipeline (via Mapshaper)

To render optimized map borders without throttling client-side browser performance, raw national shapefiles from the Humanitarian Data Exchange (HDX) must be isolated, downsampled, and converted using **[mapshaper.org](https://mapshaper.org/)**.

### Step 2.1: Select the Target HDX Admin Layer

Locate the official *"Philippines - Subnational Administrative Boundaries"* dataset on HDX provided by OCHA FISS. Select your layer package based on your interface mapping targets:

* **Admin 3 File (`phl_admbnda_adm3_ocha_fiss`):** Contains **Municipality and City** boundaries. Use this if your dashboard highlights municipal borders.
* **Admin 4 File (`phl_admbnda_adm4_ocha_fiss`):** Contains **Barangay** boundaries. Use this for high-resolution interactive barangay maps.

### Step 2.2: Import and Filter via Console

1. Drag and drop your downloaded file package directly into **mapshaper.org**. If using standard shapefiles, import the entire unzipped folder containing the `.shp`, `.shx`, `.dbf`, and `.prj` file variations simultaneously.
2. Click the **Console** button in the top right corner of the menu bar to open the dark command-line interface.
3. Paste the case-sensitive regional filter expression matching your district project and press **Enter** to instantly drop the other 81 provinces:

* **For the Marinduque Instance:**
```text
-filter "ADM2_EN == 'Marinduque'"

```


* **For the Basilan + Isabela City Instance:**
```text
-filter "ADM2_EN == 'Basilan' || ADM3_EN == 'Isabela City' || ADM2_EN == 'Isabela City'"

```



### Step 2.3: Simplify and Downsample

1. Click the **Simplify** button in the top menu bar.
2. Keep the default configuration set to **Visvalingam / weighted area**, ensure **prevent shape removal** is checked, and click **Apply**.
3. Use the slider widget at the top of the interface to compress the map weight down to **`10.0%`** or **`15.0%`**. This strips out microscopic, unneeded coastline coordinate noise while preserving clean visual borders.

### Step 2.4: Export to GeoJSON

1. Click **Export** in the upper right menu layout.
2. Select **GeoJSON** as your output option format and save the file locally.
3. Rename your asset file clearly (e.g., `marinduque-district.geojson`).

---

## 🔗 3. Generating the CDN-Ready Asset URL

GitHub acts as our high-availability asset CDN. To stream vector coordinates into the system maps, you must grab the raw layout link instead of the standard web repository link.

1. Push your updated `.geojson` map asset and your 9-column `.csv` data payload file to your GitHub repository main branch.
2. Open the `.geojson` file within the GitHub web browser player interface and click the **"Raw"** button located on the file layout header.
3. Inspect your browser's address bar to ensure it points to the pure data stream:
* **❌ Wrong (HTML App Wrapper):** `https://github.com/username/repo/blob/main/marinduque.geojson`
* **✅ Right (Pure Map Vectors CDN):** `https://raw.githubusercontent.com/username/repo/main/marinduque.geojson`



---

## 🚀 4. Step-by-Step Onboarding Runbook

To provision a completely new territory without making code modifications or requesting backend deployments, execute this checklist:

1. **Format the Ingestion Dataset:** Format your district target records into a clean 9-column CSV matching the **Ingestion Schema Contract**.
2. **Generate the Optimized Map:** Run the raw subnational shapefiles through the **Mapshaper Processing Pipeline** to create a lightweight `.geojson` asset.
3. **Commit Assets to Main:** Push both your pristine `.csv` data payload and your compressed `.geojson` vector map to your repository.
4. **Copy the Direct Raw Links:** Click **Raw** on both files within GitHub and capture their absolute delivery paths.
5. **Configure the Office Profile:** Go to your administrative settings interface on the platform. Insert your target district's numeric PSGC prefix parameters and paste your Raw Map link into the **`GeoJSON URL`** field.
6. **Execute Provisioning Workflow:** Open the `officeProfile` ingestion dashboard modal, upload your 9-column CSV file, and select **Re-provision**. The environment will instantly purge stale cached data, establish your strict schema properties, and render your custom white-labeled maps and selection selectors dynamically.
