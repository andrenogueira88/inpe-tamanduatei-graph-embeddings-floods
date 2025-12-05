# inpe-tamanduatei-graph-embeddings-floods
INPE — Reproducible geospatial pipeline for the Tamanduateí basin: builds a street-network graph, extracts terrain-based features and node/edge embeddings, and trains/evaluates a CatBoost model to predict flooding risk.

# Tamanduateí Flooding — Graph & ML Pipeline

**Project**: Tamanduateí Flooding risk analysis using street networks, terrain features, graph embeddings and CatBoost classification.

This repository contains a reproducible pipeline (notebooks) to: load and preprocess street network data, engineer geospatial features from a Digital Terrain Model (DTM), build a network graph, compute node/edge embeddings, train a flood risk classifier, and compute graph metrics.

**Recommended Notebooks (workflow order)**
- **`01_load_raw_graph.ipynb`**: Download streets from OSM inside the study polygon derived from the DTM, save raw graph as `Raw_graph/raw_street_graph.graphml` and `Raw_graph/raw_street_graph.gpkg`.
- **`02_raw_graph_visualization.ipynb`**: Visualize raw graph, DTM, rivers and flood occurrences (useful for inspection and figures).
- **`03_feature_engineering.ipynb`**: Create raster features (TWI, slope, flow accumulation, distance-to-river), compute per-edge mean statistics and flood labels, build `ml_ready_graph.graphml` and `visualization_ready.gpkg` in `Final_Graph/`.
- **`04_graph_embeddings.ipynb`**: Run Node2Vec on the prepared graph and create per-edge embeddings; output CSV: `Final_Graph/street_embeddings.csv`.
- **`05_binary_classification.ipynb`**: Load `street_embeddings.csv`, prepare train/test splits (geographic split), tune CatBoost with Optuna, train final model and evaluate predictions.
- **`06_graph_metrics.ipynb`**: Compute graph-level and node-level metrics (connectivity, density, clustering, centralities, betweenness sampling) for `Final_Graph/ml_ready_graph.graphml`.

**Repository Structure (key files/folders)**
- `Cut_data/` : raw geospatial data and DTM used as input (e.g., `dtm.tif`, shapefiles for rivers and flood occurrences).
- `Raw_graph/` : outputs from notebook 1 (`raw_street_graph.graphml`, `raw_street_graph.gpkg`).
- `Final_Graph/` : processed graph and outputs used for ML (`ml_ready_graph.graphml`, `visualization_ready.gpkg`, `street_embeddings.csv`).
- `Features/` : rasterized feature files produced by feature engineering (e.g., `feature_twi.tif`, `feature_slope.tif`, `feature_distance_to_combined_river.tif`).
- Notebooks: `01_load_raw_graph.ipynb` … `06_graph_metrics.ipynb` implementing each pipeline step.
- `catboost_info/` : logging and training artifacts from CatBoost runs.

**Data & Paths**
- The notebooks expect the input data in `Cut_data/` and will write outputs to `Raw_graph/`, `Features/`, and `Final_Graph/`.
- Key expected files:
  - `Cut_data/dtm.tif` (DTM used to define the study polygon and compute raster features)
  - `Cut_data/Alagamentos_2019.shp` (flood occurrences)
  - `Cut_data/tamanduatei.shp` and `Cut_data/afluentes.shp` (river/tributary geometry)
