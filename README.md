# Colombo Rainwater Harvesting Potential

Spatial analysis of rooftop rainwater harvesting potential across the administrative divisions of Colombo District, Sri Lanka. The project combines building footprints from OpenStreetMap, GADM administrative boundaries, and daily rainfall data to estimate the recoverable rainwater from each building.

## Project Workflow

1. Load Colombo DS Division boundaries from the GADM Level 2 shapefile.
2. Collect building footprints from OpenStreetMap using the exact administrative polygons.
3. Collect daily Colombo rainfall data for 2015-2025 from the Open-Meteo Historical API.
4. Clean and analyse building, roof-type, area, and rainfall data.
5. Estimate annual rainwater harvesting potential for each building.
6. Export summary tables, charts, and interactive Folium maps.

## Calculation

Annual harvesting potential is calculated as:

```text
Annual Harvest (L) = Roof Area (m2) x Annual Rainfall (m) x Runoff Coefficient x 1,000
```

Runoff coefficients used in the analysis:

| Roof type | Coefficient |
| --- | ---: |
| Metal sheet | 0.90 |
| Concrete | 0.85 |
| Tile | 0.80 |
| Asbestos | 0.70 |

Potential classes are assigned from annual harvesting volume:

| Class | Annual harvest |
| --- | ---: |
| High | 150,000 L or more |
| Medium | 50,000-149,999 L |
| Low | Less than 50,000 L |

## Repository Layout

```text
data/
|-- buildings.geojson       # Building footprints and attributes
|-- gadm41_LKA_2.shp        # Colombo administrative boundaries
`-- rainfall.csv            # Daily rainfall observations, 2015-2025
documents/                  # Supporting reports, source notes, and reference data
notebooks/
|-- Data_Collection.ipynb   # Boundary, OSM, and rainfall data collection
`-- Preprocess_EDA_RWH.ipynb# Cleaning, EDA, calculations, and visualisation
outputs/
|-- charts/                 # Rainfall, building, ranking, and RWH charts
|-- colombo_buildings_clean.csv
|-- rwh_final_results.csv
`-- rwh_colombo_map*.html   # Interactive result maps
```

## Running the Notebooks

Create a Python environment with the geospatial and analysis packages used by the notebooks, then run the notebooks in order from the repository root:

```powershell
pip install numpy pandas geopandas shapely matplotlib folium requests
jupyter notebook
```

Run `notebooks/Data_Collection.ipynb` first if the raw inputs need to be collected or refreshed. Then run `notebooks/Preprocess_EDA_RWH.ipynb` to regenerate the cleaned datasets, charts, calculations, and interactive maps. Data collection requires internet access to the Overpass API and Open-Meteo Historical API.

## Data Sources

- [OpenStreetMap](https://www.openstreetmap.org/) building footprints, queried through the Overpass API.
- [GADM](https://gadm.org/) administrative boundaries for Sri Lanka.
- [Open-Meteo Historical Weather API](https://open-meteo.com/en/docs/historical-weather-api) rainfall data using ERA5 reanalysis.
- Roof-type distribution references in `documents/`.

## Notes

The generated maps are self-contained HTML files and can be opened directly in a browser. `data/buildings.geojson` is tracked with Git LFS because it exceeds GitHub's standard 100 MB file limit.
