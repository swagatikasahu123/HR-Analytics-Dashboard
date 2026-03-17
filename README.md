# HR Employee Distribution Dashboard

An end-to-end HR analytics project that cleans, analyses, and visualises employee data from a 22,000+ record dataset using Python, MySQL, and Power BI.

---

## Project overview

This project explores workforce composition across departments, demographics, locations, and time. It answers 11 business questions about gender, race, age, tenure, turnover, and hiring trends, culminating in an interactive Power BI dashboard.

---

## Tools used

| Tool | Purpose |
|---|---|
| **Python / Jupyter Notebook** | Environment, SQL Magic extension, data connection |
| **MySQL** | Data cleaning, transformation, and all analytical queries |
| **Power BI Desktop** | Interactive dashboard and visualisations |

---

## Dataset

- **Source:** `hr` table in a local MySQL database (`projects`)
- **Raw records:** 22,214
- **Usable records:** 21,247 (employees aged 18 and above)
- **Fields:** `emp_id`, `first_name`, `last_name`, `birthdate`, `gender`, `race`, `department`, `jobtitle`, `location`, `hire_date`, `termdate`, `location_city`, `location_state`, `age`

---

## Data cleaning steps

The raw data required significant cleaning before analysis:

1. **Renamed column** — `id` → `emp_id` (removed BOM character prefix)
2. **Standardised `birthdate`** — converted mixed formats (`MM/DD/YYYY` and `MM-DD-YY`) to `YYYY-MM-DD`; updated column type to `DATE`
3. **Standardised `hire_date`** — same mixed-format conversion; updated column type to `DATE`
4. **Cleaned `termdate`** — stripped UTC timestamps, converted to `DATE`, handled null/blank values
5. **Added `age` column** — calculated using `TIMESTAMPDIFF(YEAR, birthdate, CURDATE())`
6. **Filtered invalid records:**
   - 967 records excluded for age < 18 (negative birthdates in source data)
   - 1,599 records excluded for term dates in the future

---

## Analysis questions

1. What is the gender breakdown of employees?
2. What is the race/ethnicity breakdown?
3. What is the age distribution?
4. How many employees work at headquarters vs. remote?
5. What is the average length of employment for terminated employees?
6. How does gender distribution vary across departments?
7. What is the distribution of job titles?
8. Which department has the highest turnover rate?
9. What is the distribution of employees across states?
10. How has employee count changed over time (2000–2020)?
11. What is the average tenure per department?

---

## Key findings

- **Gender:** More male (50.8%) than female (46.5%) employees; 2.7% non-conforming
- **Race:** White is the most represented group (6,057); Native Hawaiian / Other Pacific Islander and American Indian / Alaska Native are the least represented
- **Age:** Employees range from 20 to 57; the largest cohort is 25–34, smallest is 55–64
- **Location:** 75.3% work at headquarters (Cleveland, Ohio); 24.7% remote
- **Tenure:** Average employment length for terminated employees is 7 years
- **Turnover:** Marketing has the highest turnover rate (94.4%), followed by Training (93.8%); Research & Development and Legal have the lowest
- **Geography:** The vast majority of employees are based in Ohio (17,252), followed by Pennsylvania and Illinois
- **Hiring trend:** Net employee growth has increased steadily from ~88% in 2000 to ~98% in 2020
- **Dept tenure:** Average tenure before termination is ~8 years across most departments; Legal and Auditing are highest at 9 years

---

## Dashboard

The Power BI dashboard includes:

- Gender distribution bar chart
- Race/ethnicity bar chart
- Age group distribution (overall and by gender)
- HQ vs. remote donut chart
- US state distribution map
- Employee count change over time (2000–2020) line chart
- Gender distribution by department grouped bar chart
- Department turnover rate table

---

## Project structure

```
hr-dashboard/
├── data/
│   └── HR Data.csv            # Raw source data
├── notebooks/
│   └── hr_cleaning.ipynb      # Python/SQL data cleaning notebook
├── sql/
│   └── hr_analysis.sql        # All analytical SQL queries
├── dashboard/
│   └── HR_Dashboard.pbix      # Power BI dashboard file
│   └── HR_Dashboard.pdf       # Exported dashboard PDF
└── README.md
```

---

## How to run

### 1. Set up the database

```sql
CREATE DATABASE projects;
USE projects;
-- Import HR Data.csv into the `hr` table
```

### 2. Run data cleaning (Python / Jupyter)

Install the SQL Magic extension and connect to MySQL:

```bash
pip install ipython-sql
```

```python
%load_ext sql
%sql mysql://root:yourpassword@localhost/projects
```

Then run the cleaning notebook: `notebooks/hr_cleaning.ipynb`

### 3. Run SQL analysis

Execute the queries in `sql/hr_analysis.sql` against the cleaned `hr` table.

### 4. Open Power BI dashboard

Open `dashboard/HR_Dashboard.pbix` in Power BI Desktop and refresh the data connection to your local MySQL instance.

---

## Limitations

- Records with negative calculated ages (967) were excluded — likely due to data entry errors in the source file
- Term dates set in the future (1,599) were excluded from turnover and tenure calculations
- Data covers employees hired between 2000 and 2020 only
