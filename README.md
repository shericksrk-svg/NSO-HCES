# NSO-HCES
Analysis of household consumption and expenditure patterns using NSSO data. Reproducible research with R and Quarto.

## Project Overview

Comprehensive analysis of India's household consumption and expenditure patterns using NSSO's 2023-24 Household Expenditure and Consumption Survey (HCES) unit-level data. This project employs descriptive and inferential statistics in R to examine how **location (rural/urban), education level, and social category** affect household economic well-being, nutrition, and asset ownership.

**This is reproducible research** - all code is provided for full transparency and replication.

---

## Key Findings

### 📊 Main Discoveries

1. **Significant Rural-Urban Divide** 
   - Massive gap between rural and urban expenditure (p-value < 2.22e-16)
   - Urban households spend significantly more on non-essential items and services
   - Rural households allocate more resources to food consumption

2. **Nutrition Crisis Among Poorest**
   - 80% calorie deficiency in the poorest decile of population
   - Clear link between household income and nutritional intake
   - MPCE (Monthly Per Capita Expenditure) strongly predicts food adequacy

3. **Education as Economic Multiplier**
   - Education qualification significantly increases household consumption
   - Non-linear relationship: returns accelerate at higher education levels
   - Educational attainment is strongest predictor of asset ownership

4. **Asset Inequality by Social Category**
   - **Essential assets** (clothing, shelter basics): Equally distributed across all social groups
   - **Non-essential assets** (luxury, discretionary): Highly unequal - "Others" category owns 4+ items vs SC/ST at 3.5 items
   - Statistical ANOVA: F-value = 721, p-value < 2e-16 (highly significant)
   - **Interpretation**: While survival is democratized, prosperity remains stratified by caste

---

## What This Analysis Covers

### Variables Analyzed

- **MPCE** - Monthly Per Capita Expenditure (primary economic metric)
- **Calorific Intake** - Nutritional adequacy of households
- **Asset Ownership** - Essential vs non-essential items
- **Location** - Rural vs urban households
- **Education Level** - Impact of qualifications on spending
- **Social Category** - SC, ST, OBC, Others (historical inequalities)

### Statistical Methods Used

- **Descriptive Statistics**: Mean, median, deciles, distributions
- **Independent Samples T-Tests**: Rural vs urban comparisons
- **One-Way ANOVA**: Testing expenditure differences across education and social groups
- **Tukey's HSD Test**: Post-hoc analysis for specific categorical differences
- **Data Visualization**: Bar charts, histograms, stacked bar charts, box plots

### Output Visualizations

The analysis generates:
- **MPCE by Decile**: Distribution of spending across income levels
- **Rural vs Urban Comparison**: Multiple dimensions (food, services, assets)
- **Education Impact Charts**: How qualification affects consumption patterns
- **Asset Ownership by Social Group**: Essential vs non-essential stratification
- **Calorific Intake Analysis**: Nutritional adequacy across income groups

---

## Data Source

### Official NSSO HCES 2023-24

The National Sample Survey Office (NSSO) conducts the Household Expenditure and Consumption Survey every 5 years. This analysis uses the **2023-24 wave** - the most recent comprehensive survey of Indian households.

**Survey Details:**
- **Sampling Method**: Random stratified sampling
- **Data Collection**: Questionnaire toolkit divided into 4 parts (HCQ, FDQ, CSQ, DGQ)
- **14 Sections**: Each captures specific aspects of consumption and expenditure
- **Data Levels**: Multiple hierarchical levels (L2 through L13) requiring consolidation

### How to Access the Data

#### Option 1: MoSPI (Ministry of Statistics & Programme Implementation) Website
- **Website**: [MOSPI Official](https://mospi.mos.gov.in/)
- **Download**: HCES 2023-24 SPSS files
- **Access**: Nesstar application available for data exploration

#### Option 2: World Bank Microdata Library
- **Website**: [World Bank Microdata Library](http://microdata.worldbank.org/index.php/catalog/2930)
- **Format**: Multiple file formats available
- **Documentation**: Complete survey documentation and questionnaires

#### Download Steps

1. Visit either website above
2. Search for "HCES 2023-24" or "NSSO Household Expenditure"
3. Download SPSS files (`.sav` format) or other preferred format
4. Extract to a folder (e.g., `/NSSO_DATA/`)
5. Update file paths in `analysis.qmd` to match your local directory

---

## Project Structure

```
NSSO-Household-Survey-Analysis/
├── README.md                    # This file
├── analysis.qmd                 # Main Quarto markdown file (reproducible report)
├── data/
│   └── [Your NSSO HCES 2023-24 data files]
│   └── (Not included - download from sources above)
└── outputs/
    ├── charts/                  # Generated visualizations
    ├── tables/                  # Summary statistics
    └── analysis_report.pdf      # Final rendered report
```

---

## How This Code Works

### Data Workflow

```
Raw NSSO Data (Multiple Levels)
    ↓
Import SPSS Files into R
    ↓
Create Unique Household ID (HHID) across levels
    ↓
Consolidate Multiple Levels into Single Dataset
    ↓
Clean & Categorize Item Codes
    ├─ Food vs Non-Food Items
    ├─ Essential vs Non-Essential Assets
    └─ Create MPCE (Monthly Per Capita Expenditure)
    ↓
Subset Data by Analysis Question
    ↓
Descriptive Statistics & Visualization
    ↓
Inferential Statistics (T-tests, ANOVA)
    ↓
Generate Report & Charts
```

### Key Techniques in the Code

1. **Efficient Data Handling**
   - Uses `lapply()` and nested functions to automate tedious processes
   - Manages multiple large SPSS files systematically
   - Removes unnecessary objects to keep RAM usage low

2. **Custom HHID Creation**
   - Combines 14 variables across levels into unique household identifier
   - Enables joining data from different levels (L2, L3, L5, etc.)

3. **Flexible Item Categorization**
   - Over 650 item codes classified as Food/Non-Food
   - Durable goods classified as Essential/Non-Essential
   - Case-when logic for complex categorizations

4. **Statistical Rigor**
   - Tests assumptions before applying statistics
   - Uses appropriate post-hoc tests (Tukey's HSD)
   - Reports p-values and effect sizes

---

## Required R Packages

```R
# Install required packages:
install.packages(c(
  "dplyr",      # Data manipulation
  "tidyr",      # Data tidying
  "purrr",      # Functional programming
  "haven",      # Read SPSS files
  "ggplot2",    # Visualization
  "gridExtra",  # Multi-panel plots
  "scales"      # Formatting scales
))
```

---

## How to Run This Analysis

### Step 1: Setup
1. Download NSSO HCES 2023-24 data from MoSPI or World Bank (see Data Source section)
2. Extract to a folder (e.g., `/Users/YourName/NSSO_DATA/`)
3. Have R and RStudio installed

### Step 2: Prepare Data Files
The code expects these SPSS files:
- `LEVEL - 02 (Section 3).sav`
- `LEVEL - 03.sav`
- `LEVEL - 05 ( Sec 5 & 6).sav`
- `LEVEL - 06 (Section 7).sav`
- `LEVEL - 08 (Section 8).sav`
- `LEVEL - 09 (Section 9 & 10 & 11).sav`
- `LEVEL - 10 (Section 12).sav`
- `LEVEL - 12 (Section 13).sav`
- `Level - 13 (Section 14).sav`

Update the file paths in the code to match your directory structure.

### Step 3: Run the Analysis
1. Open `analysis.qmd` in RStudio
2. Install packages if needed (see above)
3. Click **"Render"** to run the entire analysis
4. Output: PDF report with all visualizations and findings

**Alternative (if you want to run interactively):**
- Run code chunks individually in RStudio
- View outputs in the Viewer pane
- Modify and re-run as needed

---

## What the Output Looks Like

### Final Report Includes:

1. **Abstract** - Executive summary of findings
2. **Introduction** - Context on why household expenditure matters
3. **Methodology** - Survey design and analytical approach
4. **Data Preparation** - How raw data was cleaned and organized
5. **Analysis Sections** - Deep dives into:
   - MPCE distribution and deciles
   - Rural vs urban expenditure (with statistical tests)
   - Impact of education on consumption
   - Calorific intake and nutritional adequacy
   - Asset ownership and social stratification
6. **Visualizations** - 15+ charts and graphs showing patterns
7. **Statistical Results** - ANOVA tables and test results
8. **Conclusion** - Key insights and implications

### Sample Output Visualizations

- **MPCE Decile Charts**: How spending is distributed across population
- **Rural-Urban Comparison**: Side-by-side bar charts showing disparities
- **Education Impact**: How education multiplies consumption power
- **Social Group Asset Comparison**: Essential vs non-essential ownership by caste
- **Nutrition Analysis**: Caloric adequacy across income levels

---

## Key Insights You'll Gain

From running this analysis, you'll understand:

✅ How India's poorest households struggle with nutrition
✅ Why education creates non-linear economic returns
✅ How social category (caste) predicts asset ownership
✅ The different consumption patterns of rural vs urban India
✅ How to handle complex survey data with multiple hierarchical levels
✅ How to use statistical testing to validate findings
✅ How to create publication-quality visualizations in R

---

## Skills Demonstrated

This project showcases:

- **Data Wrangling**: Managing 9 SPSS files with 600k+ rows
- **Statistical Analysis**: T-tests, ANOVA, Tukey's HSD post-hoc tests
- **R Programming**: Functions, lapply, list operations, piping (dplyr)
- **Data Visualization**: ggplot2 with multiple chart types
- **Reproducible Research**: Quarto markdown for transparent analysis
- **Domain Knowledge**: Understanding NSSO survey design and Indian economics
- **Problem-Solving**: Creating unique identifiers across data levels

---

## Reproducibility & Transparency

This analysis follows **reproducible research principles**:
- ✅ All code is provided and commented
- ✅ Data source is clearly specified
- ✅ All assumptions and methods are documented
- ✅ Results can be exactly replicated by anyone
- ✅ Uses Quarto for dynamic reporting

Anyone with access to the NSSO data can run this code and get identical results.

---

## About the Author

**Sherick** - Data Analytics Student at TISS, Mumbai
- 4th Semester, CGPA: 8.5/10
- Focus: Sustainability, Development, and Data Analytics
- Experience with: Government survey data, international development data, field research

---

## Contact & Questions

For questions about this analysis or the code:
- Feel free to open an issue on this repository
- Or reach out directly

---

## References & Resources

**NSSO Documentation:**
- HCES 2023-24 Questionnaire Toolkit
- NSS 82nd Round Technical Notes
- Item Code Classification Guide

**Statistical Methods:**
- Tukey's HSD Test for multiple comparisons
- One-Way ANOVA assumptions and interpretation
- Survey sampling methodology

**Related Resources:**
- [Engel's Law in Household Economics](https://en.wikipedia.org/wiki/Engel%27s_law)
- Understanding India's consumption patterns
- Social inequality and asset distribution

---

**This is a course project conducted as part of data analytics studies at TISS, Mumbai. It demonstrates practical application of statistical analysis to real government survey data.**

**Last Updated**: 2024
