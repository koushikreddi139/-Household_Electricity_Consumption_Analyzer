# Household Electricity Consumption Analyzer

**B.Tech Computer Science and Engineering — Sustainable Energy Engineering**

**Team No. 1**

| Member | Registration number |
|---|---|
| R Koushik | RA2311003050036 |
| K Rakesh | RA2311003050012 |
| M Pushkal | RA2311003050013 |
| P Nitin | RA2311003050064 |

**Under the guidance of Asst. Prof. Logudurai**

A software-only analytics dashboard that turns 21 days of appliance-level operating-hour records
into transparent estimates of electricity consumption, monthly cost, next-month demand, standby
waste and CO₂e emissions — and quantifies practical saving actions.

> ### Project Dataset
>
> The bundled 21-day dataset is the project's **Project Dataset**
> (`data/project_dataset_raw.csv`). It contains 273 appliance-day records across 13 appliances and
> is used throughout the dashboard, report and presentation. Its measurement provenance is not
> independently verified here, so the reported metrics demonstrate the analysis pipeline rather
> than field accuracy.
>
> Additional readings can be recorded with the blank 21-day template
> (`data/primary_observation_template.csv`). Records entered there are labelled **Observed**.
> Imputed or repaired records are labelled **Estimated**.

---

## Overview

A monthly electricity bill collapses every end use into one number. This project makes the hidden
structure visible:

- **Appliance attribution** — which appliances drive consumption and what share they hold.
- **Energy and cost** — kWh, daily/weekly/monthly totals, and a bill at an editable tariff.
- **Standby analysis** — energy drawn while equipment is idle but plugged in.
- **Forecasting** — an interpretable 30-day forecast with a chronological holdout and a naive
  baseline for comparison.
- **Sustainability** — CO₂e at an editable grid emission factor, with clearly illustrative
  equivalences.
- **Recommendations** — six quantified actions in kWh, rupees and kilograms of CO₂e per month.
- **Provenance** — every record carries a source label, and cleaning can only ever mark a repaired
  record as Estimated.

![Home dashboard](screenshots/01_home_dashboard.png)

---

## Results at a glance

Computed from the Project Dataset (21 days, 13 appliances, 273 records, 5 Estimated):

| Metric | Value |
|---|---|
| Total energy, 21 days | **240.4 kWh** |
| Average daily use | **11.45 kWh/day** |
| Projected 30-day energy (mean method) | **343.4 kWh** |
| Projected monthly bill at Rs. 8.50/kWh | **Rs. 2,919** |
| Peak day | 15.05 kWh on 25 Jan 2026 |
| Lowest day | 9.94 kWh on 09 Jan 2026 |
| Weekend uplift | +19.1% versus weekdays |
| Standby energy | 10.26 kWh (4.3% of total) |
| Top three appliances | Geyser 34.1%, refrigerator 18.7%, AC 10.5% (63.3%) |
| Monthly CO₂e at 0.71 kg/kWh | **243.8 kg** (2.97 t/year) |
| Model 30-day forecast | **362.7 kWh** (≈ Rs. 3,083) |
| Holdout MAE (Ridge vs naive) | **1.12 vs 1.48 kWh/day** |
| Combined saving, six actions | **50.6 kWh, Rs. 430, 35.9 kg CO₂e per month** |

**Why two monthly numbers?** The headline `343.4 kWh` is `mean daily consumption × 30`. The
`362.7 kWh` figure is the regression's 30-day forecast. They answer different questions and are
reported separately, never blended.

---

## Dashboard modules

| Module | What it shows |
|---|---|
| **Home** | Headline KPIs, provenance notice, daily chart, appliance donut, quick insights |
| **Consumption Analysis** | Daily and weekly trends, cost profile, distributions, weekday/weekend comparison |
| **Appliance Analysis** | Ranked consumption, operating vs standby, editable appliance scenario |
| **Prediction** | Model metrics, holdout table, fitted values and 30-day forecast with interval |
| **Sustainability Insights** | Emissions by appliance, equivalences, interactive action planner |
| **Data Management** | CSV upload and validation, manual observation entry, provenance log, resets |

Tariff and grid emission factor are editable in the sidebar and propagate to every page.

---

## Tech stack

| Layer | Tools |
|---|---|
| Language | Python 3.12 |
| Data | Pandas, NumPy, CSV |
| Modelling | Scikit-learn (Ridge regression) |
| Dashboard | Streamlit |
| Interactive charts | Plotly |
| Static report figures | Matplotlib |
| Documents | ReportLab (PDF), python-pptx (PPTX), pypdf |
| Tests | Python `unittest` (22 tests) |

No hardware, Arduino, ESP32, sensors, database server or paid API is required.

---

## Project structure

```
Household-Electricity-Analyzer/
├── app.py                          # Streamlit dashboard (six modules)
├── requirements.txt
├── README.md
├── .streamlit/
│   └── config.toml                 # Theme and server settings
├── src/
│   ├── config.py                   # Assumptions, appliance master, paths, palette
│   ├── team.py                     # Team No. 1 metadata used by the documents
│   ├── energy_calculator.py        # E = P × t / 1000 and schema helpers
│   ├── data_processing.py          # Validation, cleaning, aggregation, scenarios
│   ├── cost_analysis.py            # Tariff and bill calculations
│   ├── sustainability.py           # CO₂e and equivalences
│   ├── prediction.py               # Ridge model, holdout metrics, forecast
│   ├── recommendations.py          # Six quantified actions
│   └── charts.py                   # Plotly figures
├── data/
│   ├── project_dataset_raw.csv             # 273-record Project Dataset
│   ├── project_dataset_cleaned.csv         # Cleaned output with provenance flags
│   ├── appliance_database.csv              # Nameplate and design assumptions
│   └── primary_observation_template.csv    # Blank 21-day template
├── scripts/
│   ├── generate_dataset.py         # Deterministic 21-day dataset generator
│   ├── generate_visualizations.py  # Static Matplotlib figures
│   ├── generate_architecture.py    # Architecture diagram
│   ├── generate_report.py          # 10-page PDF report
│   ├── generate_problem_statement.py
│   └── generate_presentation.py    # Five-slide PPTX
├── tests/
│   └── test_energy_calculations.py # 22 automated tests
├── visualizations/                 # Report figures (PNG)
├── screenshots/                    # Dashboard screenshots (PNG)
├── reports/
│   ├── Project_Report.pdf          # 10 pages
│   └── Project_Presentation.pptx   # 5 slides, 16:9
└── docs/
    ├── Problem_Statement.pdf
    └── Problem_Statement.md
```

---

## Quick start

### 1. Install

```bash
python -m pip install -r requirements.txt
```

Windows, if `python` is not on `PATH`, use the full interpreter path or the `py` launcher:

```powershell
py -m pip install -r requirements.txt
```

### 2. Run the dashboard

```bash
streamlit run app.py
```

Open <http://localhost:8501>. On Windows you can also use:

```powershell
py -m streamlit run app.py
```

### 3. Run the tests

```bash
python -m unittest discover -s tests -v
```

Expected: **22 tests pass**.

### 4. Rebuild every artefact (optional)

```bash
python scripts/generate_dataset.py
python scripts/generate_visualizations.py
python scripts/generate_architecture.py
python scripts/generate_report.py
python scripts/generate_problem_statement.py
python scripts/generate_presentation.py
```

The report and presentation compute their numbers from the same modules as the dashboard, so they
cannot silently drift from the data.

---

## Configuration and assumptions

All defaults live in `src/config.py`. They are **illustrative and editable**, not official values.

| Assumption | Default | Notes |
|---|---|---|
| Tariff | Rs. 8.50 / kWh | Editable in the sidebar; no official tariff claimed |
| Fixed charge | Rs. 0.00 | Can be set in code; excluded from headline |
| Grid emission factor | 0.71 kg CO₂e / kWh | Rounded CEA-style approximation; editable |
| Projection period | 30 days | Billing-month convention |
| Forecast horizon | 30 days | Model-based cross-check |
| Car equivalence | 0.171 kg CO₂e / km | Illustrative |
| Tree absorption | 21 kg CO₂ / tree / year | Illustrative |
| Standby avoidable fraction | 90% | Recommendation assumption |
| Fan reduction | 1.0 h/day | Recommendation assumption |
| Geyser reduction | 20 min/day | Recommendation assumption |
| Washing / AC reduction | 25% | Recommendation assumption |
| LED replacement | 9 W | Recommendation assumption |

Changing the tariff or emission factor in the sidebar recalculates costs, emissions and savings
across all pages. Changing appliance parameters in **Appliance Analysis** applies a scenario to the
whole dataset and can be reset.

---

## Dataset schema and data dictionary

Required columns for upload (`data/primary_observation_template.csv` has the same schema):

| Column | Type | Meaning |
|---|---|---|
| `date` | date | Calendar day (YYYY-MM-DD) |
| `appliance_id` | string | Stable identifier, e.g. `A11` |
| `appliance_name` | string | Human-readable appliance name |
| `rated_power_w` | number | Nameplate power while operating, watts |
| `standby_power_w` | number | Power while plugged in but idle, watts |
| `units` | integer | Number of identical units |
| `operating_hours` | number | Hours of active operation in the day (0–24) |
| `standby_hours` | number | Hours in standby in the day (0–24) |
| `data_source` | string | Source label: supplied Project Dataset, `Estimated`, or `Observed` |

Optional columns produced by cleaning: `operating_kwh`, `standby_kwh`, `total_kwh`,
`day_of_week`, `week_number`, `is_weekend`, `quality_flag`, `notes`.

**Rules enforced by the pipeline**

1. Required columns must be present; dates must parse.
2. Negative or over-24-hour durations are treated as missing.
3. Missing values are imputed with the appliance median, then the global median.
4. Duplicate `(date, appliance_id)` rows are removed, keeping the last.
5. Energy is recomputed from `P × N × t / 1000`; pre-computed columns are not trusted.
6. Every imputed or repaired record is marked `Estimated`; cleaning never creates `Observed`.

---

## Collecting additional 21-day observations

1. Open `data/primary_observation_template.csv` in Excel, LibreOffice or a text editor.
2. Keep one row per appliance per day for 21 consecutive days.
3. Enter **operating hours** and **standby hours** from your own records. Use a plug-in energy
   meter, manufacturer run-time estimates, or a diary — state which in `notes`.
4. Delete rows for appliances you do not have; add rows for appliances you do have.
5. Set `data_source` to `Observed` for entered rows.
6. In the dashboard, open **Data Management → Upload**, validate and apply the file.
7. Check the provenance log and the estimated-record count before quoting results.

The dashboard rejects a blank template with a clear message: it needs at least one completed
operating-hour value. It warns when fewer than 21 distinct days are present.

---

## Methodology summary

For each appliance `i` on day `d`:

```
E_operating = P_rated   × N_units × t_operating / 1000
E_standby   = P_standby × N_units × t_standby   / 1000
E_total     = E_operating + E_standby
```

- **Daily** totals sum all appliances; **weekly** totals group days 1–7, 8–14, 15–21.
- **Monthly projection** = mean daily kWh × 30.
- **Cost** = kWh × tariff + fixed charge.
- **CO₂e** = kWh × grid emission factor.
- **Standby share** = standby kWh ÷ total kWh.

### Forecast model

Ridge regression (`alpha = 2.0`) with four features: linear trend, weekend indicator, and
`sin`/`cos` weekly terms. The last seven days are held out chronologically. A naive seven-day
moving average is evaluated on the same holdout. The final model is refitted on all 21 days for the
30-day forecast; the interval is approximately `±1.96 × holdout RMSE`.

Bundled-data metrics: MAE 1.12 kWh/day, RMSE 1.33, MAPE 9.47%, R² 0.399. Baseline: MAE 1.48,
RMSE 1.82. These demonstrate the pipeline on the Project Dataset, not field accuracy.

---

## Tests

`tests/test_energy_calculations.py` covers:

- energy and standby formulas, unit counts and rounding;
- tariff, cost and savings calculations;
- CO₂e conversion and invalid-input guards;
- schema validation, cleaning, imputation and provenance labels;
- daily/weekly/appliance aggregation;
- forecast shape, holdout split and non-negative predictions;
- recommendation quantification and combined savings;
- scenario application and database diffs.

Run:

```bash
python -m unittest discover -s tests -v
```

---

## Report and presentation

| File | Pages / slides | Notes |
|---|---|---|
| `reports/Project_Report.pdf` | 10 pages | Cover, abstract, methodology, results, forecast, sustainability, dashboard, discussion, references, appendix |
| `reports/Project_Presentation.pptx` | 5 slides | 16:9, green theme, charts and computed figures |
| `docs/Problem_Statement.pdf` | 2 pages | Problem, objectives, scope, provenance, deliverables |
| `docs/Problem_Statement.md` | — | Editable Markdown version |

**Team metadata.** Team No. 1 details are stored once in `src/team.py` and used by the report,
problem statement and presentation generators. Rebuild the documents after changing any name,
registration number or guide.

---

## Limitations

- The Project Dataset is used as supplied; its measurement provenance is not independently verified, so reported metrics demonstrate the pipeline rather than field accuracy.
- Nameplate power differs from real draw, which varies with load, temperature, age and duty cycle.
- Operating hours are self-reported and rounded; 21 days is a short modelling window.
- The regression omits weather, occupancy, holidays and tariff changes; R² is modest.
- The tariff is flat and illustrative; the emission factor is a static approximation.
- Savings assume independent actions and stated adherence; rebound effects are not modelled.
- The report's tree and car equivalences are communication aids, not offsets.

---

## Troubleshooting

| Symptom | Fix |
|---|---|
| `streamlit` not recognised | Use `python -m streamlit run app.py` or add Python Scripts to `PATH` |
| `ModuleNotFoundError: src` | Run commands from the project root, not from `scripts/` |
| Dashboard shows old module behaviour | Restart Streamlit after editing files in `src/` |
| Upload rejected | Check required columns, date format and at least one non-blank operating-hour value |
| Report build fails on fonts | The generator expects Segoe UI in `C:/Windows/Fonts`; edit `FONT_DIR` for another OS |
| PowerPoint export not needed | The `.pptx` is the deliverable; PNG previews are only for checking |

---

## Deliverables checklist

- [x] Working Streamlit dashboard with six modules
- [x] Project Dataset (273 records, 13 appliances)
- [x] Blank 21-day primary observation template
- [x] Cleaning, validation and provenance pipeline
- [x] Energy, cost, sustainability and prediction modules
- [x] Six quantified recommendations
- [x] Eight static report figures plus architecture diagram
- [x] Seven dashboard screenshots
- [x] 22 automated tests
- [x] 10-page project report (PDF)
- [x] Five-slide presentation (PPTX)
- [x] Problem statement (PDF + Markdown)
- [x] README and requirements

---

## Academic context

This project was developed for a B.Tech Computer Science and Engineering programme with a
Sustainable Energy Engineering focus. It is intended for academic demonstration and further
development. The code and documents may be reused with attribution. Third-party libraries retain
their own licences.

**Team No. 1:** R Koushik (RA2311003050036), K Rakesh (RA2311003050012), M Pushkal
(RA2311003050013), P Nitin (RA2311003050064). Guide: Asst. Prof. Logudurai.
