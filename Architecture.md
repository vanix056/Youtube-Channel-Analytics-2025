# Architecture Overview

## Components
- **Dashboards (Streamlit)**: `final_dashboard.py` (primary, accessibility-first)
- **Data**: Default CSVs (`cleaned_youtube_data_3.csv`, `cleaned_youtube_data_new_1.csv`). Users can upload custom CSVs via the UI.
- **Notebooks**: Exploratory analysis (`YouTube_EDA_Three_Approaches.ipynb`, `YouTube_EDA_Three_Approaches_Explained.ipynb`).

## Data Flow
1. CSV is loaded (default or uploaded) into pandas.
2. Metrics and derived features are calculated in-memory.
3. Visuals rendered via Plotly and displayed in Streamlit.

## Entry Points
- Primary run: `streamlit run final_dashboard.py`.
- Alternate runs: choose another dashboard file with `streamlit run <file>.py`.

## Dependencies (core)
- Streamlit for UI, Plotly for charts, pandas/numpy for data prep.
