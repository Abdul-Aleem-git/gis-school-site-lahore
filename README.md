# GIS-Based School Site Selection in Lahore, Pakistan

**Muhammad Abdul Aleem | Roll No: MSDS25022**  
*M.Sc. Data Science — Information Technology University (ITU), Lahore*  
*Course: Spatial Data Science*

---

## Overview

Lahore is one of Pakistan's fastest-growing cities, with a population exceeding 14 million. Public schools are unable to keep pace with educational demand, creating a genuine investment opportunity for private schools — but picking the wrong location means low enrollment, stiff competition, and early closure.

This project uses **geospatial analysis** to answer two practical questions:
1. **Where** in Lahore are there high child populations but insufficient school coverage?
2. **How much** investment would a new private school realistically require at those locations?

---

## Objectives

- Identify underserved zones in Lahore using population data and existing school locations
- Estimate property rental/purchase costs in candidate zones
- Assess road accessibility and travel-time to candidate sites
- Understand socioeconomic profiles to calibrate appropriate fee structures
- Produce a ranked, investable map of top school site recommendations

---

## Methodology

### Step 1 — Demand Mapping
- **WorldPop 2024** (100m resolution) population raster clipped to Lahore district boundary
- **Kernel Density Estimation (KDE)** applied to school point data with 1–2 km bandwidth to identify existing school concentration hotspots
- Underserved zones identified where child population density is high but KDE school density is low

### Step 2 — Multi-Criteria Decision Analysis (MCDA)
| Criterion | Data Source | Weight |
|-----------|-------------|--------|
| Child population density | WorldPop 2024 via KDE | 35% |
| School gap (underserved zones) | OSM + Fixed-bandwidth spatial weights | 30% |
| Property cost | Zameen.com rental/purchase rates | 20% |
| Road accessibility | OSMnx travel-time network analysis | 15% |

### Step 3 — Break-Even Financial Model
- Per-site cost estimation: rent, renovation, furniture, staff salaries
- Minimum enrollment threshold calculated for each top-ranked zone

---

## Data Sources

| Dataset | Source | Description |
|---------|--------|-------------|
| Lahore boundary | [GADM](https://gadm.org/) (`gadm41_PAK_3`) | District-level administrative boundary |
| School locations | [OpenStreetMap](https://www.openstreetmap.org/) via Overpass Turbo | 472 school points within Lahore |
| Population grid | [WorldPop 2024](https://www.worldpop.org/) | 100m resolution population raster, Pakistan |
| Road network | [OSMnx](https://osmnx.readthedocs.io/) | Drive-network for travel-time analysis |
| Property prices | [Zameen.com](https://www.zameen.com/) | Area-level rental and purchase rates |
| Spatial weights | [libpysal](https://pysal.org/libpysal/) | Fixed-bandwidth kernel weight matrix |

> **Note:** Large data files (`.tif`, `.geojson`, `.json`) are excluded from this repository via `.gitignore`. See the data pipeline in `notebooks/DataPreprosessing.ipynb` for download instructions and paths.

---

## Repository Structure

```
gis-school-site-lahore/
│
├── notebooks/
│   └── DataPreprosessing.ipynb   # Full data pipeline: boundary, schools, population, KDE, road network
│
├── presentation/
│   └── Abdul_Aleem_final.pptx    # Project presentation slides
│
├── abstract/
│   └── Abstract_MSDS25022_M_Abdul_Aleem.pdf   # Project abstract
│
├── data/                         # Place raw data here (excluded from git — see .gitignore)
│   ├── Boundary of Lahore from GADM/
│   ├── Schools data from OSM overpass turbo/
│   └── Pakistan population 2024 100m/
│
├── docs/                         # Additional documentation (outputs, maps, reports)
│
└── README.md
```

---

## Setup & Requirements

```bash
pip install geopandas rasterio numpy matplotlib osmnx scipy libpysal
```

### Running the Notebook

1. Clone this repository
2. Download raw data files into the `data/` directory (see sources above)
3. Update the `BASE` path in Cell 2 of `DataPreprosessing.ipynb` to your local `data/` folder
4. Run all cells top-to-bottom

---

## Expected Outputs

- **Optimal Zoning Map** — color-coded ranked map of top school investment zones across Lahore
- **Cost Comparison Table** — site-by-site property and setup cost estimates
- **Break-Even Analysis** — minimum student enrollment needed to cover costs per location

---

## Known Data Limitations

- **OSM Schools (472 records):** Coverage is denser in the urban core and sparser in southern rural tehsils (Raiwind, Manga Mandi). A supplementary dataset from Punjab School Education Department (PSED) would improve rural coverage.
- **WorldPop 2024:** Max density observed at 345.7 persons per 100m cell. No missing values after nodata removal.

---

## Acknowledgements

Developed as part of the Spatial Data Science coursework at Information Technology University (ITU), Lahore.
