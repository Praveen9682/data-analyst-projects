# Titanic Survival Analysis

A self-driven data analysis project exploring survival patterns in the Titanic dataset using SQL, Python (Pandas), and data visualization.

## Tools Used
- **SQL** — practiced JOINs, GROUP BY, and aggregate functions on related datasets (HackerRank)
- **Python (Pandas)** — data loading, cleaning, and groupby analysis
- **Matplotlib** — visualizing survival patterns across categories

## What I Did
- Loaded and explored the Titanic dataset in Google Colab
- Cleaned missing data using different strategies depending on the situation:
  - Filled missing `Age` values using the median
  - Filled missing `Cabin` values with "Unknown" (too much data missing to estimate)
  - Filled missing `Embarked` values using the mode (most common value)
- Created age groups using `pd.cut()` to bin continuous age data into categories (Child, Teen, Young Adult, Adult, Senior)
- Analyzed survival rate by gender, passenger class, and age group — individually and combined
- Built bar charts to visualize survival rate by class

## Key Insight
Gender had a stronger effect on survival than passenger class. A Class 3 female passenger (50% survival rate) was more likely to survive than a Class 1 male passenger (37% survival rate) — showing that being female mattered more for survival odds than being in a higher class.

Breaking this down further by age group showed that the "women and children first" policy applied fairly evenly to children regardless of gender (Child: female 59% vs male 57%), but the gender gap widened sharply from the teenage years onward (Teen: female 75% vs male 9%), suggesting the protection extended mainly to adult women, not adult men, once past childhood.

## Note
This is a learning project built while practicing SQL and Python fundamentals for a data analyst role. Feedback welcome.
