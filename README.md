# Education-Institutions-Fee-Analysis

![image_alt](https://github.com/narayanareddy7129/Education-Institutions-Fee-Analysis/blob/564a9e771f95e96b4945af31f641342c82d9d5da/college_fee_analysis_collegedunia_flow.svg)
 ## Overview
 This project aims to scrape college fee data from Collegedunia using Python and BeautifulSoup, followed by Exploratory Data Analysis (EDA) to uncover fee trends, identify affordable colleges, and understand the distribution of fees across courses, states, and college types.

## Objectives
1. Scrape college fee data from Collegedunia across multiple pages
2. Extract key fields: college name, course, tuition fee, hostel fee, location, and exam accepted
3. Clean and preprocess the raw data
4. Perform EDA to analyse fee patterns and distributions
5. Identify affordable colleges and fee outliers

## Dataset
 
| Field | Details |
|---|---|
| **Source** | [Collegedunia.com](https://collegedunia.com) (web scraped) |
| **Degrees Covered** | B.Tech, M.Tech, MBA, BCA, B.Com, B.Sc, BA |
 
### Columns
 
| Column | Type | Description |
|---|---|---|
| College Names | Categorical | Name of the college/university |
| Degree | Categorical | Academic degree offered (B.Tech, MBA, etc.) |
| Course | Categorical | Specialization (e.g., Computer Science) |
| City | Categorical | City where the college is located |
| State | Categorical | State in India |
| Rating for 5 | Numerical | Student/reviewer rating out of 5 |
| Rank | Numerical | College rank for a given year |
| Total Rank | Numerical | Total colleges in the ranking list |
| Year of Rank | Numerical | Year of the ranking |
| Fees in Rupees | Numerical | Tuition fee in Indian Rupees |
| Approved | Categorical | Accreditation body (AICTE, UGC, NBA, etc.) |
 
---
## Project Pipeline
 
```
Project Setup
     │
     ▼
Define Target Website (Collegedunia)
     │
     ▼
Send HTTP Request (requests + custom headers)
     │
     ├── Response NOT OK → Retry / Log
     │
     ▼
Parse HTML (BeautifulSoup4 + html.parser)
     │
     ▼
Extract Raw Fee Data (College, Course, Tuition, Location)
     │
     ├── More Pages? → Loop back
     │
     ▼
Save Raw Data (CSV via pandas DataFrame)
     │
     ▼
Clean & Transform (remove ₹ symbols, nulls, type casting)
     │
     ▼
EDA: Descriptive Stats │ Visualizations │ Correlation & Outliers
     │
     ▼
Insights & Report (fee trends, affordable colleges)
```
 
---
 
## Data Cleaning Steps
 
- **Dropped** unnecessary auto-generated index columns (`Unnamed: 0`)
- **Removed rows** where Rank, Total Rank, and Year of Rank were all null simultaneously
- **Filled** `Rating for 5` nulls with the **median** (robust to extreme values)
- **Stripped** `₹` and `,` symbols from the fee column, then cast to `float` → imputed median → cast to `int`
- **Filled** `Approved` nulls with `"No Info"` using `fillna()`
- **Winsorization (IQR capping)** used to treat fee outliers — upper fence = Q3 + 1.5×IQR
- **Dropped** `Average Package` and `Highest Package` columns — too many nulls to impute
> Cleaning was done **separately** for each degree dataset (B.Tech, M.Tech, MBA) before merging.
 
---
 
## EDA Highlights
 
### Univariate
 
| Finding | Insight |
|---|---|
| Fee Distribution | Most colleges charge around ₹1 lakh; range is ₹20K – ₹15L |
| Fee Box Plot | Median fee ~₹2.5L; bulk between ₹50K – ₹6.5L |
| Ratings | Most colleges rated between 3.5 – 4.5; peak at 4.0 |
| State-wise Count | Maharashtra, Delhi, Tamil Nadu, Karnataka each have 200+ colleges |
| Degree Split | BE/B.Tech dominates; BCA has the fewest colleges |
| Top City | New Delhi has the highest number of colleges |
| Top Courses | BCA and B.Tech CSE are the most offered courses |
 
### Bivariate
 
| Finding | Insight |
|---|---|
| Fee vs Rating | No strong relationship — higher fees don't guarantee better ratings |
| Degree by State | B.Tech/BE dominates Karnataka; MBA dominant in Maharashtra |
| Fee by State | J&K and Manipur have the highest average fees |
| Fee by Course | PGDM has the highest average fee among all courses |
 
### Multivariate
 
| Feature | Correlation with Fee | Interpretation |
|---|---|---|
| Rating | 0.15 | Very weak — not a fee driver |
| Rank | -0.11 | Very weak — rank alone doesn't set fees |
| Total Rank | **0.56** | Moderate positive — larger ranking pools → higher fees |
| Year of Rank | 0.02 | Near zero |
 
---
 
## Key Insights
 
- **Tamil Nadu** has the **most institutions** across India and offers diverse degrees at **lower fee ranges** — likely why it attracts students from other states
- **MBA degree** consistently charges above ₹7 lakh across most states
- **College rank has only a marginal effect on fees** — degree type and state matter more
- **BE/B.Tech and M.Tech** are available in virtually every state; BA degree fees stay below ₹3L
-  Most **northeastern states** have very few colleges
---


## Project Structure
```
Education-Institutions-Fee-Analysis/
│
├── Data sets/
│   ├── B.Tech data.csv
│   ├── M.Tech data.csv
│   └── MBA data.csv
│
├── scraping.py                          # Web scraping script (requests + BeautifulSoup)
├── datacleaning_and_vizualization.ipynb # Full EDA notebook
├── EDA_PROJECT.pptx                     # Project presentation
└── README.md
```

## How to Run
 
```bash
# 1. Clone the repo
git clone https://github.com/<your-username>/Education-Institutions-Fee-Analysis.git
cd Education-Institutions-Fee-Analysis
 
# 2. Install dependencies
pip install pandas numpy matplotlib seaborn requests beautifulsoup4
 
# 3. Run the scraper (optional — raw CSVs already included)
python scraping.py
 
# 4. Open the EDA notebook
jupyter notebook datacleaning_and_vizualization.ipynb
```
 
---

 
## License
 
This project is for educational purposes only. Data sourced from [Collegedunia.com](https://collegedunia.com).
