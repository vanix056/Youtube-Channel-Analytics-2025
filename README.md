# YouTube Channel Analytics Dashboard

> A multi-audience, accessibility-first analytics platform for YouTube channel performance, niche discovery, and sponsorship intelligence.

---

## Overview

YouTube Channel Analytics Dashboard is an interactive Streamlit application that transforms raw YouTube channel data into actionable insights across three distinct user personas: existing content creators seeking performance benchmarks, aspiring YouTubers identifying low-competition niches, and marketers evaluating cost-effective sponsorship opportunities.

The dashboard ingests cleaned YouTube channel datasets (included) or accepts custom CSV uploads, computes derived engagement metrics, and renders fully interactive Plotly visualizations. It is built to comply with **Section 508** (29 U.S.C. § 794d) and **WCAG 2.1 Level AA/AAA** standards, featuring a colorblind-safe mode powered by the Okabe-Ito palette and a high-contrast mode for maximum readability.

---

## Key Features

- **Three persona-specific dashboards** — Existing Creators, New Creators, and Marketers/Advertisers, each with tailored charts and metrics
- **Derived engagement metrics** — Growth Boost Score, Audience Interest Rate, Views-per-Upload, and Cost-Effectiveness Score computed in-memory
- **Peer benchmarking** — Radar chart comparing a selected channel against age- and category-matched peers
- **Category entry difficulty analysis** — Lollipop chart ranking content categories by average subscriber barrier to entry
- **Geographic success patterns** — Treemap visualizing country × category performance scores
- **Sponsorship intelligence** — Ranked horizontal bar chart identifying channels with the best ROI for advertisers
- **Accessibility modes** — Normal, Colorblind (Okabe-Ito palette with hatching patterns), toggled via the sidebar
- **Custom CSV upload** — Replace the default dataset at runtime without restarting the app
- **Filtered data export** — Download the current filtered view as a CSV for offline analysis
- **Multi-filter controls** — Filter by content category, country, subscriber tier, and channel age across all views

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Language** | Python 3.10 |
| **UI Framework** | Streamlit |
| **Visualization** | Plotly Express, Plotly Graph Objects |
| **Data Processing** | pandas, NumPy |
| **Exploratory Analysis** | Jupyter Notebook |
| **Styling** | Custom CSS injected via `st.markdown` |
| **Accessibility** | Section 508, WCAG 2.1 AA/AAA, Okabe-Ito palette |

---

## Installation

**Prerequisites:** Python 3.10+

```bash
# 1. Clone the repository
git clone https://github.com/MAbdullahWaqar/Youtube-Channel-Analytics-2025.git
cd Youtube-Channel-Analytics-2025

# 2. (Recommended) Create a virtual environment
python -m venv .venv
source .venv/bin/activate        # macOS/Linux
.venv\Scripts\activate           # Windows

# 3. Install dependencies
pip install -r requirements.txt
```

---

## Usage

### Run the primary dashboard

```bash
streamlit run final_dashboard.py
```

The app opens at `http://localhost:8501` by default.

### Sidebar controls

| Control | Description |
|---|---|
| **Accessibility** | Toggle between Normal and Color-Blind modes |
| **Navigation** | Switch between the three audience dashboards |

### Upload a custom dataset

Expand the **"Upload Custom Dataset (Optional)"** section on the main page and upload a CSV file matching the expected schema (see [Configuration](#configuration)).

### Export filtered data

On the **Marketers/Advertisers** view, use the **Download CSV** button to export the current filtered dataset.

---

## Analysis Workflow

The exploratory notebooks (`YouTube_EDA_Three_Approaches.ipynb`, `YouTube_EDA_Three_Approaches_Explained.ipynb`) document the full data preparation pipeline:

1. **Data ingestion** — Loading raw channel data from `Orignal_dataset/` (v1 and v2 CSVs)
2. **Cleaning & normalization** — Handling missing values, standardizing country codes, parsing timestamps, and computing `channel_age_years`
3. **Feature engineering** — Log-transforming skewed count columns (`view_count_log1p`, `subscriber_count_log1p`, etc.) for downstream analysis
4. **Subscriber tier segmentation** — Binning channels into `0-1K`, `1K-10K`, `10K-100K`, `100K-1M`, `1M+` tiers
5. **Output** — Cleaned CSVs written to `Cleaned_dataset/` and the project root for dashboard consumption

To run the notebooks:

```bash
jupyter notebook YouTube_EDA_Three_Approaches_Explained.ipynb
```

---

## Visualizations

| Chart | Dashboard | Description |
|---|---|---|
| Radar / Spider chart | Existing Creators | Percentile-ranked comparison of Growth Boost, Audience Interest, and Views-per-Upload vs. peer average |
| Overlay bar chart | Existing Creators | Posting frequency buckets (0–4 to 30+ videos/month) vs. median views, with selected channel overlay |
| Lollipop chart | New Creators | Category entry difficulty ranked by average subscriber count; marker shapes encode difficulty in colorblind mode |
| Treemap | New Creators | Country × category performance scores; box size = performance, color depth = average views |
| Horizontal bar chart | Marketers | Top-15 channels by cost-effectiveness score with direct value labels |
| Comparison bar charts | Marketers | 2×2 grid comparing a selected channel against the dataset average across four KPIs |

---

## Project Structure

```
Youtube-Channel-Analytics-2025/
├── final_dashboard.py              # Primary Streamlit dashboard (Section 508 compliant)
├── requirements.txt                # Python dependencies
├── Architecture.md                 # Component and data-flow overview
├── cleaned_youtube_data_3.csv      # Default dataset (primary)
├── cleaned_youtube_data_new_1.csv  # Alternate cleaned dataset
├── Cleaned_dataset/                # Additional cleaned CSV outputs
│   ├── cleaned_youtube_data_3.csv
│   └── cleaned_youtube_data_new_1.csv
├── Orignal_dataset/                # Raw source data
│   ├── youtube_channel_info_v1.csv
│   └── youtube_channel_info_v2.csv
├── YouTube_EDA_Three_Approaches.ipynb           # EDA notebook (concise)
└── YouTube_EDA_Three_Approaches_Explained.ipynb # EDA notebook (annotated)
```

---

## Configuration

### Default dataset

The dashboard loads `cleaned_youtube_data_3.csv` from the project root by default. To change the default file, update `DEFAULT_DATA_FILE` at the top of `final_dashboard.py`:

```python
DEFAULT_DATA_FILE = "cleaned_youtube_data_3.csv"
```

### Expected CSV schema

| Column | Type | Description |
|---|---|---|
| `channel_name` | string | YouTube channel display name |
| `view_count` | int | All-time total view count |
| `category` | string | Content category/genre |
| `country` | string | ISO 3166-1 alpha-2 country code |
| `subscriber_count` | int | Current subscriber count |
| `created_date` | datetime | Channel creation timestamp (ISO 8601) |
| `video_count` | int | Total published videos |
| `videos_last_30_days` | int | Videos uploaded in the past 30 days |
| `views_last_30_days` | int | Views accumulated in the past 30 days |
| `channel_age_years` | float | Computed age of the channel in years |

Log-transformed variants (`*_log1p`) and `channel_created_year` are generated during EDA and included in the cleaned CSVs.

### Accessibility modes

Two modes are configurable from the sidebar at runtime:

| Mode | Palette | Additional cues |
|---|---|---|
| **Normal** | Standard blue/orange | — |
| **Color-Blind** | Okabe-Ito (8-color safe) | Hatching patterns, varied marker shapes, thicker lines, direct value labels |

---

## Contributing

Contributions are welcome. Please follow these guidelines:

1. Fork the repository and create a feature branch (`git checkout -b feature/your-feature`)
2. Keep changes focused — one logical change per pull request
3. Ensure new visualizations respect the accessibility mode system (`get_chart_colors`, `get_bar_pattern`, etc.)
4. Test with both Normal and Color-Blind modes before submitting
5. Open a pull request with a clear description of the change and its motivation

---

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).

---

## Author

**Muhammad Abdullah Waqar**
- GitHub: [@MAbdullahWaqar](https://github.com/MAbdullahWaqar)
- Repository: [Youtube-Channel-Analytics-2025](https://github.com/MAbdullahWaqar/Youtube-Channel-Analytics-2025)
