# # FIFA 2021 Player Data: Exploratory Data Analysis

A Jupyter notebook that performs a thorough exploratory data analysis (EDA) on the FIFA 2021 Complete Player Data set, following the approach in the [NIST/SEMATECH Engineering Statistics Handbook, Exploratory Data Analysis chapter](https://www.itl.nist.gov/div898/handbook/eda/eda.htm).

## Dataset

- **Source:** [Kaggle: FIFA 2021 Complete Player Data](https://www.kaggle.com/aayushmishra1512/fifa-2021-complete-player-data)
- **File:** `FIFA-21_Complete.csv` 
- **Size:** 17,981 rows, 9 columns

| Column | Type | Description |
|---|---|---|
| `player_id` | identifier | Unique player ID (excluded from analysis) |
| `name` | identifier | Player name (excluded from analysis) |
| `nationality` | categorical | Player's country |
| `position` | categorical | Positions the player can play, separated by `\|` |
| `overall` | numerical | Overall rating |
| `age` | numerical | Age in years |
| `hits` | numerical | Number of page hits on the source site |
| `potential` | numerical | Potential rating |
| `team` | categorical | Club |

### Derived variables

The raw file has only 4 usable numerical columns, so these were built from existing columns to meet the "5 or more numerical attributes" requirement:

| Variable | Definition |
|---|---|
| `potential_gap` | `potential - overall` (room to grow) |
| `n_positions` | Number of positions listed for the player |
| `primary_position` | First listed position (e.g. `ST\|CF\|RW` becomes `ST`) |
| `position_group` | Goalkeeper, Defender, Midfielder, or Forward |

### Requirement check

| Requirement | Result |
|---|---|
| At least 30 rows | 17,981 |
| At least 8 columns | 9 (plus 4 derived) |
| At least 5 numerical attributes | 6 (`overall`, `age`, `potential`, `hits`, `potential_gap`, `n_positions`) |

## What the notebook does

1. **Setup and load:** reads the CSV, cleans column names and whitespace, builds derived variables.
2. **Summary statistics:** min, max, mean, median, standard deviation, IQR fences, and outlier counts (1.5 x IQR rule) for every numerical variable, plus a missing-value check.
3. **One-dimensional views:**
   - Histogram (with KDE, mean, and median lines) for each numerical variable, with written min/max/mean/median/outlier commentary.
   - Bar chart for each categorical variable (top 15 categories when there are many).
4. **Side-by-side box plot:** all numerical variables in one plot, standardized (z-score) so different scales can be compared.
5. **Pairwise relationships:**
   - **Numerical vs numerical:** scatter plot matrix and a correlation heatmap.
   - **Categorical vs categorical:** aggregated cross-tabulations drawn as count heatmaps and stacked percentage bars.
   - **Categorical vs numerical:** box plots of a numeric variable grouped by category, and a scatter plot with points colored by category.
6. **Conclusions:** written summary of distributions, outliers, relationships, and data quality.

## How to run

### Google Colab
1. Open a new Colab notebook and paste each cell in order.
2. Run Cell 1. When the **Choose Files** button appears, upload `FIFA-21_Complete.csv`.
3. Run the remaining cells top to bottom.

Note: files uploaded to Colab are deleted when the runtime resets, so re-upload in a new session.

### Local Jupyter
1. Download the CSV from Kaggle and place it in the same folder as the notebook.
2. Install the requirements: `pip install pandas numpy matplotlib seaborn jupyter`
3. Set `CSV_PATH = "FIFA-21_Complete.csv"` in the first cell (and remove the `google.colab` upload block).
4. Run all cells.

## Requirements

- Python 3.9+
- pandas
- numpy
- matplotlib
- seaborn

## Key findings (fill in after running)

- **Distributions:** `hits` is extremely right-skewed (most players have 0, a few stars have hundreds). `overall` and `age` are roughly bell-shaped.
- **Outliers:** `hits` has many outliers under the 1.5 x IQR rule. These are real (famous players draw far more attention), not data errors.
- **Relationships:** `overall` and `potential` are strongly correlated. `potential_gap` is strongly related to `age` (younger players have more room to grow).
- **Categories:** position groups differ in rating, age, and number of listed positions.
- **Data quality:** no missing values in the raw columns. `team` had trailing whitespace, and
