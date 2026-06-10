#  Host Risk Clustering

A machine learning pipeline that classifies hosts by risk profile using K-Means clustering over vulnerability data. Each host gets a label — **Critical**, **High**, **Moderate**, or **Low** — based on its aggregated vulnerability indicators.

![Python](https://img.shields.io/badge/Python-3.10+-blue?style=flat-square&logo=python)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?style=flat-square&logo=jupyter)
![scikit-learn](https://img.shields.io/badge/scikit--learn-KMeans-f7931e?style=flat-square&logo=scikit-learn)
![License](https://img.shields.io/badge/license-MIT-lightgrey?style=flat-square)

---

## What it does

Instead of manually triaging hosts, this pipeline:

1. Loads vulnerability data per host (IP, scores, exposure, CISA flags, etc.)
2. Aggregates the data by host — counting vulns, averaging scores, tracking exposure time
3. Normalizes all features with MinMaxScaler
4. Uses K-Means to group hosts into 4 risk clusters
5. Maps each cluster to a risk label (Critical / High / Moderate / Low)
6. Exports the result to `hosts_clusterizados.csv`
7. Visualizes clusters with a PCA scatter plot and an Elbow Method chart

---

## Visualizations

<p>
  <img src="images/Elbow Method.png" width="50%"/>
  <img src="images/Host Clustering by Risk Profile.png" width="42.1%"/>
</p>

---

## Input data

The notebook expects a CSV at `data/sample.csv` with the following columns:

| Column | Description |
|---|---|
| `asset_ip` | Host IP address |
| `score` | Vulnerability score |
| `cvss` | CVSS base score |
| `epss` | Exploit Prediction Scoring System (0–1) |
| `asset_exposure` | Exposure level (numeric) |
| `cisa` | Whether the vuln is on CISA KEV (bool) |
| `is_cve_trend` | Whether the CVE is trending (bool) |
| `first_seen` / `last_seen` | Vulnerability window dates |

---

## Features used for clustering

Each host is summarized by 9 features before clustering:

- `vuln_count` — total number of vulnerabilities
- `score_mean` / `score_max` — average and maximum vulnerability score
- `cvss_mean` — average CVSS score
- `epss_mean` — average exploit probability
- `exposure` — maximum exposure level
- `cisa_count` — number of CISA KEV vulnerabilities
- `cve_trend_count` — number of trending CVEs
- `months_open_max` — longest time a vulnerability has been open

---

## Setup

```bash
pip install pandas numpy scikit-learn matplotlib plotly
```

Then open and run `host_risk_clustering.ipynb` in Jupyter.

---

## Output

- `hosts_clusterizados.csv` — hosts with their cluster ID and risk profile
- Interactive Elbow Method chart (Plotly)
- PCA scatter plot colored by risk level

---

## Risk profiles

| Label | Description |
|---|---|
| 🔴 Critical | High vuln count, high scores, active exploits, long exposure |
| 🟠 High | Significant risk indicators, some trending CVEs |
| 🟡 Moderate | Average exposure, lower severity |
| 🟢 Low | Few vulnerabilities, low scores, short exposure |
