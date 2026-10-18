# Air Quality Clustering of Cities

Unsupervised clustering of cities and settlements based on long-term PM2.5, PM10 and NO2 concentrations from the WHO Ambient Air Quality Database (Update 2024, V6.1).

This project was prepared as a course project for an Unsupervised Learning course. The full analysis is in [`air_quality_clustering.ipynb`](air_quality_clustering.ipynb).

## Summary

- **Data:** WHO Ambient Air Quality Database V6.1 (annual mean concentrations, 7,182 settlements, 2010-2022).
- **Selection:** cities with PM2.5, PM10 and NO2 each available for at least 7 of 9 years (2013-2021), giving **945 cities** after excluding Adelaide (substantial discontinuity in NO2 measurements across years).
- **Features:** city-level median PM2.5, PM10 and NO2 (PM2.5 and PM10 log-transformed, then standardized). Population is used only for post-clustering profiling.
- **Methods:** K-Means (final model), Agglomerative Clustering, and Gaussian Mixture Models. Clustering quality is compared using the Silhouette Score, Calinski-Harabasz Index, and Davies-Bouldin Index, while Adjusted Rand Index (ARI) is used to assess agreement between the cluster assignments produced by different methods.

## Results

K-Means with k=2 gave the best internal scores (Silhouette 0.39, Calinski-Harabasz 789, Davies-Bouldin 0.96).

| Cluster | Cities | PM2.5 (median) | PM10 (median) | NO2 (median) |
|---|---|---|---|---|
| 0 - Lower pollution | 381 | 8.5 | 15.3 | 10.2 |
| 1 - Higher pollution | 564 | 13.7 | 22.2 | 24.6 |

Concentrations are in µg/m³. Cluster names are relative: they describe lower versus higher pollution within this sample, not compliance with any guideline.

The data form more of a continuum than two sharply separated groups (modest silhouette score). The three methods agree on the general lower-versus-higher pattern, but agreement on individual city assignments is moderate (ARI 0.35-0.58).

## Limitations

- Only cities with long, complete PM2.5/PM10/NO2 records are included. About 90% of the sample (850 of 945 cities) is in Europe, so the results mainly reflect European monitoring networks and should not be read as global.
- Annual values are aggregated into city-level medians, so year-to-year trends are not captured.
- Population is available for only part of the sample and is not used for clustering.

## Data

The dataset is **not included** in this repository. Download the Excel version of the *WHO Ambient Air Quality Database (Update 2024, V6.1)* from the [WHO website](https://www.who.int/data/gho/data/themes/air-pollution/who-air-quality-database) and save it as:

```
data/who_ambient_air_quality_database_version_2024_(v6.1).xlsx
```

The notebook reads the sheet `Update 2024 (V6.1)`.

Suggested citation: WHO. *WHO Ambient Air Quality Database (update 2024)*. Geneva, World Health Organization, 2024.

## How to run

```bash
git clone https://github.com/banugurbuz/air-quality-clustering.git
cd air-quality-clustering

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# create the data/ folder and add the WHO Excel file (see "Data")
jupyter notebook air_quality_clustering.ipynb
```

## Repository structure

```
.
├── air_quality_clustering.ipynb   # full analysis
├── requirements.txt
├── README.md
└── data/                          # not tracked; add the WHO Excel file here