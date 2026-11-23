# Be'er Sheva Innovation District | Real Estate Market Analysis

**Independent industry capstone · NUR Lab × Be'er Sheva Innovation District · Tel Aviv University · 2026**

An end-to-end **Python data engineering and statistical analysis project** designed to establish a real-estate market baseline (**T0**) for Be'er Sheva's Innovation District. The project turns heterogeneous historical property-transaction records into a structured analytical foundation for monitoring price patterns, comparing neighborhoods and cities, and supporting evidence-informed urban planning.

> **Project focus:** Build a transparent, reusable measurement framework—not a predictive machine-learning model—so future transaction data can be compared against a consistent starting point.

## Project at a glance

| Category | Description |
|---|---|
| Partner | NUR Lab and the Be'er Sheva Innovation District |
| My contribution | Independently implemented data integration, cleaning, feature engineering, statistical analysis, visualizations, and the reusable analysis workflow |
| Geographic scope | Be'er Sheva, including the Innovation District, plus **18 comparison cities** |
| Primary outcome | Transaction price per square meter (`price_per_sqm`) |
| Additional measures | Transaction value, transaction volume, property characteristics, and time trends |
| Core methods | ETL, schema normalization, exploratory data analysis (EDA), outlier handling, ANOVA, correlations, and temporal comparisons |
| Deliverable | Reusable analytical baseline (T0) with documented code and saved statistical and visual outputs |

## 1. Business context and objective

The Be'er Sheva Innovation District brings together academic, healthcare, and industry institutions as part of a broader urban-development initiative. One of its evaluation priorities is understanding **real-estate market activity within and around the district**.

NUR Lab and the district administration needed a consistent way to assess the market at a starting point and revisit the same indicators over time. Raw records came from multiple historical and updated datasets with inconsistent schemas, property descriptions, and levels of completeness, making direct comparisons unreliable.

**The objective was to create a baseline study (T0)** that could help stakeholders:

- Compare real-estate prices **inside and outside** the Innovation District.
- Track changes in price per square meter, transaction value, and transaction activity over time.
- Place Be'er Sheva's trends in the context of **18 other Israeli cities**.
- Examine which observed property characteristics are associated with price differences.
- Support future monitoring of urban development and potential affordability or displacement pressures.

## 2. Research questions

1. Do properties within the Innovation District differ in average price per square meter from properties elsewhere in Be'er Sheva?
2. How do transaction prices and volumes evolve across years, quarters, and months?
3. How do Be'er Sheva's real-estate trends compare with those in other Israeli cities?
4. How are property type, size, rooms, and floor-related characteristics associated with prices?
5. Can the data-processing and reporting workflow be reused as new observations become available?

## 3. Key findings

### Statistically significant price differences by district location

A one-way **ANOVA** reported a significant difference in price per square meter between properties located within and outside the Innovation District:

- **F = 535.12**
- **p < 0.0001**

In the project's reported comparisons of categorical features, `in_innovation_district` produced the highest ANOVA F-statistic, ahead of floor-related characteristics. The descriptive comparisons also indicated higher prices per square meter within the district.

**Interpretation:** District location is strongly associated with transaction price per square meter in the analyzed data. This result **does not establish that the Innovation District caused the difference**, nor does a univariate ANOVA isolate location from differences in property mix, timing, or other confounders.

### A standardized baseline suitable for repeated measurement

The initial cleaning stage retained **42,077 records** after excluding **23,458 incomplete records**, as documented in the project report. The analysis pipeline was subsequently extended to support a consistent multi-city comparison across **19 cities in total**.

The baseline includes yearly and quarterly comparisons, district-versus-city benchmarks, and distributions of transaction prices and property characteristics.

### Controlled outlier treatment improved interpretability

The workflow used percentile-based screening and an expanded **interquartile range (IQR) rule with k = 3.0** to target extreme errors without automatically excluding legitimate high-value transactions. The final report states that this outlier-filtering step removed approximately **1.5%** of records and reduced the difference between mean and median price per square meter to approximately **₪100** in the evaluated data.

These are results reported for the project's analyzed datasets and cleaning configuration; they should not be generalized to every future dataset.

## 4. Technical approach

### A. Data integration and schema normalization

- Reconciled field names across heterogeneous historical and updated transaction tables.
- Created dictionaries mapping equivalent columns to a shared schema and normalized field-name casing.
- Preserved identifiers and city fields while adding missing schema fields where necessary.
- Combined records into a consistent structure for Be'er Sheva and the comparison cities.

### B. Data cleaning and feature engineering

- Examined missing values, sparse rows, duplicate or inconsistent representations, and invalid price-related records.
- Standardized roughly **140 textual variants of property types** into common categories, according to the final report.
- Parsed Hebrew free-text floor descriptions into structured features:
  - `floor_min`: lowest floor associated with the property.
  - `num_floors_in_asset`: number of floors in the property, where identifiable.
- Normalized room counts to half-room increments.
- Derived year and quarter indicators for time-series grouping.
- Used existing district-location classifications to distinguish transactions inside and outside the Innovation District; the original spatial assignment was prepared by a NUR Lab collaborator before this project.
- Applied controlled percentile and IQR-based outlier filtering.

### C. Exploratory and statistical analysis

- Investigated correlations with price per square meter and total transaction value.
- Used **one-way ANOVA** to examine mean-price differences across categorical groups.
- Compared price and property-feature distributions with box plots and other exploratory charts.
- Built annual, quarterly, and monthly trend views, including district, non-district, and broader comparison benchmarks.

### D. Reusable analysis design

Organized the workflow into reusable functions and analysis stages, with safeguards for missing or constant columns. The original internal deliverable supported generating refreshed analytical outputs from new input files; this **public portfolio version omits bulk dataset export and download operations**.

## 5. Visualizations and outputs

The accompanying notebook retains saved charts and analytical results. Selected views include:

| Visualization | What it helps investigate |
|---|---|
| Price-per-square-meter distributions, inside vs. outside the district | Differences in typical prices, spread, and outliers |
| Quarterly price-per-square-meter trends | How the district compares with the rest of Be'er Sheva and broader benchmarks over time |
| Quarterly total transaction-value trends | Differences in transaction values across geographic reference groups |
| Correlation heatmaps | Relationships between numeric property features and price outcomes |
| Property-type and numeric-feature box plots | How transaction prices vary across property characteristics |
| Transaction counts by quarter and year | Changes in observed transaction activity |

**[Explore the full notebook and saved figures](beer_sheva_real_estate_analysis.ipynb)**

## 6. Value for stakeholders

The work provides a **repeatable measurement foundation**, rather than a one-time chart or model score. Its potential applications include:

- **Urban planning:** Comparing transaction patterns across neighborhoods and property segments.
- **Impact monitoring:** Establishing a reference point against which later market changes can be assessed.
- **Housing and inclusion:** Tracking signals that may warrant closer investigation of affordability and displacement risks.
- **Communication and reporting:** Making complex transaction data easier to interpret for the district administration and external stakeholders.

The analysis and its outputs were developed for NUR Lab and the Innovation District and informed stakeholder-facing discussions. **This repository does not claim that the project demonstrated a causal impact or that a particular policy decision resulted from the analysis.**

## 7. Why a statistical baseline instead of machine learning?

The project initially considered predictive modeling, but the final scope prioritized a **transparent, interpretable T0 baseline**. For ongoing monitoring, clear definitions, reproducible cleaning rules, group comparisons, and explainable statistics were more directly aligned with the stakeholder need than a prediction-only model.

This was a deliberate project-design choice, not an unfinished ML task.

## 8. Repository contents

| File | Description |
|---|---|
| [`beer_sheva_real_estate_analysis.ipynb`](beer_sheva_real_estate_analysis.ipynb) | Analytical notebook containing data-processing logic, statistical analysis, and saved outputs |
| `README.md` | Project overview, findings, methods, and limitations |
| `.gitignore` | Prevents accidental tracking of local datasets and temporary files |

## 9. Data access and reproducibility

The study used historical and updated real-estate transaction information from external systems, including information that was publicly viewable. **The underlying source datasets are not included or redistributed in this repository.** The original data-acquisition workflow is not included, and no official API-based collection is claimed.

The public notebook is intended to document the analysis and preserve its results, **not** to provide bulk access to the transaction records. It retains saved outputs but has **not been re-run end to end in a clean environment** after preparing the public version. Reproducing the results requires appropriately authorized source data and may require adapting local input paths.

Public visibility of individual records should not be assumed to authorize automated collection or bulk redistribution.

## 10. Limitations

- **Observational design:** Associations and ANOVA results do not demonstrate causal effects of district development.
- **Potential confounding:** Property composition, transaction dates, location within the city, and other factors may contribute to price differences.
- **Source consistency:** Coverage, field definitions, and completeness differ across historical datasets and cities.
- **Cleaning sensitivity:** Missing-data and outlier decisions affect the analytic sample and reported summary statistics.
- **No live refresh in this public repository:** The documented baseline and saved outputs are historical; the source datasets and internal export workflow are intentionally excluded.

## 11. Potential next steps

- Extend the baseline with rental prices, construction permits, transit accessibility, and demographic indicators where authorized.
- Build a stakeholder-facing dashboard or geographic visualization for recurring reporting.
- Introduce multivariable or hedonic-price models to adjust for observable property characteristics.
- Compare later time periods with the baseline to study how patterns evolve, using stronger causal methods if impact attribution becomes a goal.

## 12. Tools and skills

**Python · pandas · NumPy · Matplotlib · Seaborn · statsmodels · scikit-learn · Regular Expressions · Data Integration · ETL · Data Quality · Feature Engineering · Exploratory Data Analysis · ANOVA · Statistical Interpretation · Data Visualization · Stakeholder Reporting**

---

*Academic context: Digital Sciences for High-Tech, Tel Aviv University. Industry project conducted with NUR Lab and the Be'er Sheva Innovation District.*
