# 🇳🇵 nepalboundaries  
### Administrative Boundaries of Nepal for R & Python

<p align="center">
  <img src="icon.png" width="180"/>
</p>

<p align="center">

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![R Package](https://img.shields.io/badge/R-ready-blue.svg)]()
[![Python Package](https://img.shields.io/badge/Python-ready-blue.svg)]()
[![Version](https://img.shields.io/badge/version-0.1.0-green.svg)]()

</p>

---

`nepalboundaries` provides easy access to Nepal’s administrative boundary datasets at multiple levels including:

- Country
- Province
- District
- Municipality
- Ward

Built for spatial analysis, mapping, visualization, and geospatial workflows in **R** and **Python**.

Inspired by the excellent [`rgeoboundaries`](https://github.com/wmgeolab/rgeoboundaries) project but focused specifically on Nepal with locally curated datasets.

---

# Features

✅ Multi-level administrative boundaries  
✅ Easy filtering by province, district, municipality, or ward  
✅ Clean standardized spatial datasets  
✅ WGS84 projection (EPSG:4326)  
✅ Compatible with `sf`, `leaflet`, `ggplot2`, etc.  
✅ Multiple export formats (GeoJSON, GPKG, Shapefile)  
✅ Consistent API for both R and Python  

---

# Installation

## R Package

```r
# Install remotes if needed
install.packages("remotes")

# Install from GitHub
remotes::install_github("Lalitgis/nepalboundaries")
```

---

# Quick Start

## Load Packages

```r
library(nepalboundaries)
library(ggplot2)
```

---

## Provinces of Nepal

```r
provinces <- nb_province()

ggplot(provinces) +
  geom_sf(fill = "lightblue", color = "black") +
  geom_sf_label(aes(label = province_name)) +
  theme_minimal()
```

---

## Districts of Bagmati Province

```r
bagmati <- nb_district(province = "Bagmati")

plot(bagmati["district_name"])
```

---

## Wards of Kathmandu

```r
kathmandu_wards <- nb_ward(
  municipality = "Kathmandu"
)
```

---

# Administrative Structure

```text
Nepal
├── 7 Provinces
│   ├── 77 Districts
│   │   ├── 753 Municipalities
│   │   └── Protected Areas
│   │       └── ~6,743 Wards
```

---

# Available Functions

| Function | Description |
|---|---|
| `nb_country()` | Nepal country boundary |
| `nb_province()` | Provincial boundaries |
| `nb_district()` | District boundaries |
| `nb_municipality()` | Municipality boundaries |
| `nb_ward()` | Ward boundaries |
| `nb_get_multiple()` | Multiple administrative levels |
| `nb_summary()` | Summary statistics |

---

# Filtering Examples

## Get a Province

```r
nb_province(province = "Bagmati")
```

## Get Districts Within a Province

```r
nb_district(province = "Koshi")
```

## Get Municipalities Within a District

```r
nb_municipality(district = "Kathmandu")
```

## Get Wards Within a Municipality

```r
nb_ward(municipality = "Pokhara")
```

---

# Advanced Spatial Analysis

## Calculate District Areas

```r
library(sf)
library(dplyr)

districts <- nb_district() %>%
  mutate(
    area_km2 = as.numeric(st_area(geometry)) / 1e6
  )
```

---

## Largest Districts

```r
largest <- districts %>%
  st_drop_geometry() %>%
  arrange(desc(area_km2)) %>%
  head(10)
```

---

## Spatial Join Example

```r
point <- st_point(c(85.3240, 27.7172)) %>% 
  st_sfc(crs = 4326) %>% 
  st_as_sf()

district <- st_join(point, districts)
```

---

## Adjacent Districts

```r
kathmandu <- districts[
  districts$district_name == "Kathmandu", 
]

adjacent <- districts[
  st_touches(kathmandu, districts, sparse = FALSE)[1, ],
]
```

---

# Coordinate Reference System

All datasets use:

```text
WGS84 (EPSG:4326)
```

Transform to another CRS if needed:

```r
districts_utm <- st_transform(
  districts,
  crs = 32645
)
```

---

# Export Data

## Shapefile

```r
st_write(districts, "nepal_districts.shp")
```

## GeoJSON

```r
st_write(
  districts,
  "nepal_districts.geojson",
  driver = "GeoJSON"
)
```

## GeoPackage

```r
st_write(districts, "nepal_districts.gpkg")
```

## CSV

```r
districts %>%
  st_drop_geometry() %>%
  write.csv("nepal_districts.csv")
```

---

# Interactive Mapping

## Leaflet Example

```r
library(leaflet)

leaflet(data = nb_district()) %>%
  addTiles() %>%
  addPolygons(
    popup = ~district_name,
    color = "blue",
    weight = 1,
    opacity = 0.8
  )
```

---

# Repository Structure

```text
nepalboundaries/
├── R/
│   ├── DESCRIPTION
│   ├── NAMESPACE
│   ├── R/
│   │   └── functions.R
│   ├── data/
│   │   ├── country.rds
│   │   ├── province.rds
│   │   ├── district.rds
│   │   ├── municipality.rds
│   │   └── ward.rds
│   ├── man/
│   └── README.md
│
├── README.md
└── LICENSE
```

---

# Data Preparation

To rebuild datasets:

```r
source("data_preparation_R.R")
```

This generates processed `.rds` files inside the `data/` directory.

---

# Citation

```bibtex
@software{nepalboundaries2026,
  title={nepalboundaries: Administrative Boundaries of Nepal},
  author={Lalit BC},
  year={2026},
  url={https://github.com/Lalitgis/nepalboundaries}
}
```

---

# Contributing

Contributions are welcome.

1. Fork the repository  
2. Create a feature branch  

```bash
git checkout -b feature/amazing-feature
```

3. Commit changes  

```bash
git commit -m "Add amazing feature"
```

4. Push to GitHub  

```bash
git push origin feature/amazing-feature
```

5. Open a Pull Request

---

# 🐞 Issues & Support

Please report bugs or request features here:

[GitHub Issues](https://github.com/Lalitgis/nepalboundaries/issues)

---

# Acknowledgments

- Survey Department of Nepal  
- Nepal administrative boundary contributors  
- R geospatial community  
- Developers of `sf`, `leaflet`, `ggplot2`, and related libraries  

---

# 🔗 Related Resources

- [Nepal Central Bureau of Statistics](https://cbs.gov.np/)
- [OpenStreetMap Nepal](https://wiki.openstreetmap.org/wiki/Nepal)
- [Humanitarian Data Exchange Nepal](https://data.humdata.org/dataset?tags=nepal)

---

# License

This project is licensed under the **MIT License**.

---

<p align="center">
  Made with ❤️ for Nepal GIS & Spatial Analysis Community
</p>
