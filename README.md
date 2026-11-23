# India Flood Prediction --- Dataset Description, Preprocessing & Exploratory Data Analysis

## 1. Project Overview

This project explores an India-wide flood-related dataset using **data
description, data preprocessing, exploratory data analysis (EDA), and
visualization**. The objective of the analysis is to understand the
structure and quality of the supplied data, identify patterns in the
flood and non-flood labels, study relationships among geographical,
terrain, land-cover, population and rainfall variables, and prepare the
dataset for further analysis or predictive modelling.

The analysis is documented in the Jupyter notebook:

`India_Flood_Dataset_Description_Preprocessing_EDA.ipynb`

## 2. Dataset Description

### 2.1 Dataset details

  Property                          Description
  --------------------------------- --------------------------------------------
  Dataset file                      `final_india_flood_prediction_dataset.csv`
  Number of records                 7,086
  Number of attributes              29
  Geographic coverage               30 state/UT labels
  Target variable                   `flood_occurred`
  Target type                       Binary classification label
  Rainfall years in feature names   2014 and 2015

The supplied CSV contains **3,543 records labelled as flood (`1`)** and
**3,543 records labelled as non-flood (`0`)**. The `sample_type` field
identifies the two groups as `observed_flood` and
`pseudo_negative_matched_pool`, respectively.

> **Data provenance note:** The supplied CSV does not contain an
> embedded source URL, citation, licence, download date, formal data
> dictionary, or record-level lineage. Therefore, the exact origin of
> this consolidated file cannot be confirmed from the CSV alone. The
> official portals listed in the References section are relevant sources
> for the types of input variables represented in the file; they should
> not be described as the confirmed source of this exact CSV unless the
> original download/preprocessing records establish that connection.

### 2.2 Types of attributes

The dataset contains the following broad groups of variables:

-   **Administrative attributes:** `state`, `district`, `sub_district`
-   **Geographical attributes:** `latitude`, `longitude`,
    `geographical_area_ha`
-   **Terrain attributes:** `elevation_m`, `slope_deg`
-   **Hydrological attributes:** `river_distance_km`,
    `drainage_density_km_per_km2`
-   **Land and settlement attributes:** `soil_type`, `land_cover`,
    `built_up_pct`
-   **Demographic attribute:** `population_density_per_km2`
-   **Rainfall attributes:** annual rainfall, maximum 24-hour rainfall,
    maximum 3-day rainfall, maximum 7-day rainfall, and extreme-rain-day
    counts for 2014 and 2015
-   **Target and sample metadata:** `flood_occurred`, `sample_type`
-   **Record identifier:** `sample_id`

The units are indicated by column names where available, such as
millimetres (`mm`), metres (`m`), kilometres (`km`), hectares (`ha`),
degrees, percentages and population per square kilometre. The exact
definitions and derivation of engineered fields should be confirmed with
the dataset creator or original data dictionary.

## 3. Data Acquisition and Potential Official References

The current analysis uses the project-provided consolidated CSV. The
following official portals are relevant for locating or verifying the
underlying categories of data:

  -----------------------------------------------------------------------------------------------------------------------------
  Data category           Official source                                                               Relevance
  ----------------------- ----------------------------------------------------------------------------- -----------------------
  Rainfall                [India Meteorological Department (IMD) --- 0.25° Gridded                      Historical gridded
                          Rainfall](https://www.imdpune.gov.in/cmpg/Griddata/Rainfall_25_NetCDF.html)   rainfall data

  Flood observations and  [Central Water Commission (CWC) Flood Forecasting                             Flood forecasts and
  forecasts               Services](https://ffs.india-water.gov.in/)                                    related river
                                                                                                        information

  Land use / land cover   [ISRO-NRSC Bhuvan](https://bhuvan.nrsc.gov.in/)                               Geospatial and
                                                                                                        land-cover information

  Population              [Census of India](https://censusindia.gov.in/)                                Official demographic
                                                                                                        and population tables

  Elevation / DEM         [USGS EarthExplorer](https://earthexplorer.usgs.gov/)                         Elevation products such
                                                                                                        as SRTM DEM
  -----------------------------------------------------------------------------------------------------------------------------

These links are **reference portals, not verified provenance for the
supplied CSV**. To establish exact provenance, the original source
files, download records, data-generation scripts, spatial resolution,
temporal coverage and joining methodology must be checked.

## 4. Data Preprocessing and Preparation

Data preprocessing is performed before analysis to improve data quality,
consistency and suitability for statistical exploration. The notebook
documents the following checks and operations.

### 4.1 Checking missing values

Missing values are identified using counts and percentages for every
column. Their distribution is also examined across `sample_type` and
`flood_occurred`.

**Dataset-specific finding:** `rain_grid_latitude` and
`rain_grid_longitude` each contain 3,543 missing values. In the supplied
file, these missing values occur in the `pseudo_negative_matched_pool`
group. This indicates structured missingness associated with sample
construction, rather than ordinary random missingness.

These coordinates should not be blindly replaced with zero or a column
mean. Their meaning and availability should be checked with the dataset
creator before deciding how to handle them.

### 4.2 Removing duplicate records

Exact duplicate rows are checked and removed, if present. This avoids
counting identical records more than once during analysis.

### 4.3 Handling inconsistent entries

Text columns are standardized by removing leading and trailing
whitespace. Blank strings and text representations such as `nan` or
`None` are converted to missing values where appropriate. This helps
avoid treating visually identical categories as separate values.

### 4.4 Correcting data types

Columns are converted to appropriate types based on their apparent
meaning:

-   Identifiers and count/label fields are represented as integer-like
    values.
-   Measurements and coordinates are converted to numeric values.
-   Administrative, soil, land-cover and sample-type fields remain
    categorical/text attributes.

Conversion failures are represented as missing values and should be
reviewed.

### 4.5 Handling outliers

Potential outliers are identified using the **Interquartile Range (IQR)
method**:

-   IQR = Q3 − Q1
-   Lower bound = Q1 − 1.5 × IQR
-   Upper bound = Q3 + 1.5 × IQR

Values outside these limits are flagged for investigation. They are
**not automatically deleted**, because unusually high rainfall,
population density or other extreme measurements may be meaningful in
flood-related analysis.

### 4.6 Detecting invalid values

Basic range checks are applied to selected fields. Examples include:

-   Latitude and longitude are screened against broad geographic bounds
    for India.
-   `slope_deg` is checked against the physical range of 0--90 degrees.
-   `built_up_pct` is checked against 0--100 percent.
-   Area, distance, drainage density, population density and rainfall
    measurements are checked for negative values.
-   `flood_occurred` is checked to ensure that its values are limited to
    0 and 1.

These are screening rules. Any flagged record should be verified against
the original data source rather than removed automatically.

### 4.7 Before-and-after comparison

The notebook compares the dataset before and after preprocessing in
terms of:

-   Number of rows
-   Number of columns
-   Missing cells
-   Duplicate full rows

This makes the effect of the cleaning operations transparent.

## 5. Exploratory Data Analysis (EDA) and Data Visualization

EDA is performed to summarize the dataset, understand individual
variables, explore relationships and identify patterns relevant to the
project objectives.

### 5.1 Univariate Analysis

Univariate analysis studies one variable at a time.

**Numerical variables**

-   Distribution of elevation, slope, river distance and drainage
    density
-   Distribution of built-up percentage and population density
-   Distribution of annual and extreme rainfall measurements
-   Mean, median, standard deviation, minimum and maximum

**Categorical variables**

-   Frequency of states/UTs
-   Frequency of soil types
-   Frequency of land-cover categories

**Visualizations used**

-   Histograms to understand numerical distributions, spread and
    skewness
-   Boxplots to summarize median, quartiles and potential outliers
-   Bar charts to compare category frequencies

### 5.2 Bivariate Analysis

Bivariate analysis explores the relationship between two variables.

The notebook compares selected numerical variables against
`flood_occurred`, including:

-   Elevation
-   Slope
-   River distance
-   Drainage density
-   Built-up percentage
-   Population density
-   Annual and maximum rainfall measurements

Boxplots are used to compare the distributions of these variables
between flood-labelled and non-flood-labelled records.

The notebook also compares flood-label proportions across soil types and
land-cover categories using grouped summaries and bar charts.

These results describe associations within the supplied sample. They do
not establish that a variable causes flooding.

### 5.3 Multivariate Analysis

Multivariate analysis considers multiple numerical variables together.

A **Pearson correlation heatmap** is used to visualize the direction and
strength of linear relationships among numerical features. Strongly
correlated feature pairs are also listed for further inspection.

Correlation values range from −1 to +1:

-   Values near +1 indicate a strong positive linear association.
-   Values near −1 indicate a strong negative linear association.
-   Values near 0 indicate a weak linear association.

Correlation does not imply causation.

### 5.4 Statistical Summary

Descriptive statistics are generated for numerical and categorical
attributes. The notebook also summarizes numerical medians by flood
label and calculates the number of records and flood-label proportion by
state.

These summaries help identify differences in the supplied sample and
guide further investigation.

## 6. Important Dataset Observations and Limitations

The following issues must be considered while interpreting results:

1.  **Balanced target labels:** the dataset contains 3,543 records in
    each target class. This balance may be due to sampling and should
    not be treated as the real-world prevalence of floods in India.
2.  **Pseudo-negative records:** the non-flood group is labelled
    `pseudo_negative_matched_pool`. These records should not
    automatically be described as independently verified flood-free
    events.
3.  **Target leakage through `sample_type`:** in the supplied CSV,
    `sample_type` maps one-to-one to `flood_occurred`. It must not be
    used as a predictive feature.
4.  **Structured missingness:** rainfall-grid coordinates are missing
    for the pseudo-negative group. This pattern may reveal how the
    dataset was constructed and requires investigation.
5.  **Unverified feature definitions:** the exact derivation of fields
    such as `extreme_rain_days_p90_2014` and
    `extreme_rain_days_p90_2015` needs confirmation.
6.  **Temporal and spatial alignment:** rainfall, terrain, population,
    land-cover and flood labels should be checked for consistent
    geographic units, resolution and time periods.
7.  **Unverified provenance:** the supplied CSV does not provide enough
    metadata to independently verify the exact source and processing
    history.

Because of these limitations, the dataset should be treated as a
project-level analytical dataset until its lineage and label-generation
procedure are confirmed.

## 7. Tools and Libraries

The notebook uses:

-   **Python** --- programming language
-   **Pandas** --- data loading, manipulation and cleaning
-   **NumPy** --- numerical operations
-   **Matplotlib** --- charts and visualizations
-   **Jupyter Notebook** --- interactive analysis environment

## 8. How to Run the Notebook

### Step 1: Keep the files together

Place the following files in the same project directory:

``` text
project-folder/
├── final_india_flood_prediction_dataset.csv
├── India_Flood_Dataset_Description_Preprocessing_EDA.ipynb
└── README.md
```

### Step 2: Install dependencies

``` bash
python -m pip install pandas numpy matplotlib jupyter
```

### Step 3: Start Jupyter Notebook

``` bash
jupyter notebook
```

### Step 4: Open and execute

Open `India_Flood_Dataset_Description_Preprocessing_EDA.ipynb` and
execute the cells from top to bottom.

If the CSV is stored in another directory, update the `DATA_PATH`
variable in the first data-loading cell.

## 9. Expected Outputs

After executing the notebook, the analysis produces:

-   Dataset shape, columns and data-type summaries
-   Feature dictionary
-   Missing-value and duplicate summaries
-   Invalid-value screening results
-   IQR-based outlier summary
-   Before-and-after preprocessing comparison
-   Target class distribution
-   Numerical histograms and boxplots
-   Categorical frequency bar charts
-   Bivariate comparisons by flood label
-   Multivariate correlation heatmap
-   Descriptive statistical summaries
-   Tables that support interpretation and report writing

## 10. Conclusion

This project documents the structure and quality of the supplied India
flood-related dataset and performs preprocessing and exploratory
analysis before further modelling. The analysis examines geographical,
terrain, hydrological, demographic, land-cover and rainfall-related
variables and explores how they are distributed and associated with the
flood label.

The findings should be interpreted in the context of the dataset's
balanced sampling, pseudo-negative records, structured missingness and
unverified provenance. Confirming the original data sources and
label-generation methodology is an essential next step before drawing
broad conclusions or using the dataset for operational flood prediction.

## 11. References

1.  India Meteorological Department (IMD), Pune --- Gridded Rainfall
    Data:
    https://www.imdpune.gov.in/cmpg/Griddata/Rainfall_25_NetCDF.html
2.  Central Water Commission (CWC) --- Flood Forecasting Services:
    https://ffs.india-water.gov.in/
3.  ISRO National Remote Sensing Centre (NRSC) --- Bhuvan:
    https://bhuvan.nrsc.gov.in/
4.  Census of India: https://censusindia.gov.in/
5.  USGS --- EarthExplorer: https://earthexplorer.usgs.gov/

**IMD rainfall data citation listed by the official portal:**

Pai, D. S., Sridhar, L., Rajeevan, M., Sreejith, O. P., Satbhai, N. S.,
& Mukhopadhyay, B. (2014). Development of a new high spatial resolution
(0.25° × 0.25°) long period (1901--2010) daily gridded rainfall data set
over India and its comparison with existing data sets over the region.
*MAUSAM*, 65(1), 1--18.
