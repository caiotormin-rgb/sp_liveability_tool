# SP Liveability Tool

A data-driven tool to rank and explore municipalities in **São Paulo State, Brazil** by quality of life. It pulls data from public APIs, builds a composite liveability score, and provides an interactive widget to adjust weights and filter cities in real time.

## What It Does

- Fetches demographic, economic, environmental, and climate data for all 645 SP municipalities
- Computes a normalized **liveability score** (0–1) from multiple indicators
- Ranks municipalities and allows filtering by population range
- Displays a choropleth map and correlation heatmap
- Provides an **interactive explorer** with sliders to reprioritize metrics on the fly

## Data Sources

| Dataset | Source | API / Access |
|---|---|---|
| Municipality list | IBGE Localidades API | Automatic |
| Population (2022 Census) | IBGE SIDRA, table 9514 | Automatic |
| GDP & GDP per capita | IBGE SIDRA, table 5938 | Automatic |
| Territorial area | IBGE SIDRA, table 615 | Automatic |
| Human Development Index (IDHM 2010) | IPEADATA OData API | Automatic |
| Land cover / forest coverage | MapBiomas Collection 10 | Manual download (~81 MB, see below) |
| Climate (temperature, precipitation, humidity) | INMET automatic stations | Automatic |
| Municipal boundaries (GeoJSON) | IBGE Malhas API | Automatic |

> **MapBiomas file:** `mapbiomas_col10_municipalities.xlsx` is excluded from this repo due to size. The notebook downloads it automatically on first run if it is not present locally.

## Liveability Score

The composite score is built from the following indicators (all normalized 0–1 with `MinMaxScaler`):

| Indicator | Direction | Default weight |
|---|---|---|
| IDHM (Human Development Index) | Higher = better | High |
| GDP per capita | Higher = better | Medium |
| Forest / vegetation coverage (%) | Higher = better | Medium |
| Mean annual temperature (°C) | Lower = better | Low |
| Annual precipitation (mm) | Higher = better | Low |
| Relative humidity (%) | Higher = better | Low |

Weights can be customized interactively in Section 13 of the notebook.

## Output Files

| File | Description |
|---|---|
| `sp_municipalities_data.csv` | Raw merged dataset (all indicators per municipality) |
| `sp_municipalities_livability.csv` | Final liveability scores and ranking |

## Setup

```bash
pip install pandas numpy matplotlib seaborn requests plotly geopandas folium scikit-learn openpyxl ipywidgets
```

Then open and run `sp_municipalities_exploration.ipynb` top to bottom. All external data is fetched automatically.

## Top Ranked Municipalities (sample)

| Rank | Municipality | Liveability Score |
|---|---|---|
| 1 | Santos | 0.807 |
| 2 | Águas de São Pedro | 0.781 |
| 3 | São Caetano do Sul | 0.760 |
| 4 | Jundiaí | 0.730 |

See `sp_municipalities_livability.csv` for the full ranked list.

## Notebook Structure

| Section | Description |
|---|---|
| 1 | Municipality list from IBGE |
| 2 | Demographics — Census 2022 |
| 3 | Economics — GDP (IBGE SIDRA) |
| 4 | Human Development Index (IDHM) |
| 5–6 | Land cover / forest coverage (MapBiomas) |
| 7 | Climate data (INMET stations + Haversine matching) |
| 8 | Territorial area & population density |
| 9 | Composite scoring & ranking |
| 10 | Geographic map |
| 11 | Correlation analysis |
| 12 | Summary & next steps |
| 13 | Interactive liveability explorer (ipywidgets) |
