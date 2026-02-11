# UK Gender Pay Gap Analysis
## SQL Data Analysis | Public Policy Insights | Exploratory Data Analysis | Workforce Inequality Study
### Executive Summary
A UK government department needs to monitor and understand the gender pay gap across industries to identify inequality patterns and inform policy decisions.
Using SQL, I analyzed over 10,000 UK companies to measure average pay gaps, identify industries and regions with significant disparities, and evaluate whether company size influences pay inequality.
The analysis found that the average median gender pay gap across the UK is 13.45%, meaning men earn approximately 13.45% more than women on average. Certain industries, such as banking and education, show significantly higher gaps, highlighting areas where policy attention may be needed.

### Business Problem

Gender pay inequality is a major economic and social issue. The UK requires companies with more than 250 employees to report pay gap data, but raw reporting does not automatically translate into insight.

Key questions addressed in this project:

What is the true average gender pay gap across the UK?

Which companies have the largest disparities?

Do certain industries show higher gaps?

Are there regional differences (London vs Birmingham)?

Does company size affect the pay gap?

The goal was to transform raw self-reported government data into meaningful, policy-relevant insights.

### Methodology

The analysis followed a structured SQL-based workflow:

1. Data Inspection & Cleaning

Counted total companies (10,174)

Identified late submissions (361 companies)

Detected missing URLs (3,700 companies)

Evaluated missing pay gap metrics

Excluded unreliable or placeholder values (e.g., 0 entries)

Bonus pay gap metrics were excluded from primary analysis due to high missing rates (up to 40%). Hourly pay gap metrics were prioritized for reliability.

2. Metric Selection

Two key metrics were available:

DiffMeanHourlyPercent

DiffMedianHourlyPercent

I selected DiffMedianHourlyPercent because:

The median is less influenced by extreme salaries.

It better reflects the “typical” worker’s pay difference.

To calculate the UK-wide average, I used the mean of the median gaps across companies.

3. SQL Analytical Techniques Used

COUNT() for dataset inspection

AVG() for calculating national and sector averages

CASE WHEN statements for geographic grouping

ILIKE for industry filtering

GROUP BY for segmentation analysis

ORDER BY and LIMIT for ranking

Conditional filtering (WHERE)

Data validation using NULL and zero checks

### Specific Skills Used| Tools & Technologies

PostgreSQL

pgAdmin

SQL

SQL Concepts Applied

Aggregation functions

Conditional logic (CASE)

String pattern matching (ILIKE)

Data cleaning techniques

Grouped analysis

### Results & Business Recommendations
UK-Wide Pay Gap

Average Median Gender Pay Gap: 13.45%

Men earn approximately 13.45% more than women across reporting companies.

Caveat: Data is self-reported and may include inconsistencies.

Companies with the Largest Pay Gaps

Identified the top 10 companies with the highest gaps favoring men.
After filtering for large employers (>250 employees and >30% gap), major disparities remain.

### Important:
A large pay gap does not automatically imply unlawful pay discrimination. It may reflect workforce composition rather than unequal pay for identical roles.

Industry Differences

Schools/Education: 22.18% average pay gap

Banks: 25.66% average pay gap

These industries show significantly higher disparities than the national average.

Regional Differences

London: ~13%

Outside London: ~12%

Birmingham: ~11.5%

Gender pay inequality varies slightly by region.

Company Size Relationship

Larger companies tend to show smaller pay gaps compared to mid-sized firms.

This suggests organizational scale, governance structure, or reporting transparency may influence pay equality outcomes.

### Business Recommendations

Focus regulatory review on industries with gaps above 20%.

Improve transparency around workforce role distribution.

Encourage internal pay audits for mid-sized organizations.

Improve reporting guidance to reduce placeholder and zero-value entries.

Government departments should prioritize sector-specific interventions rather than applying a uniform policy approach.

### Next Steps & Limitations
### Limitations

Data is self-reported.

Pay gap measures do not control for role type or seniority.

0-values may represent missing data rather than true equality.

Bonus gap metrics had significant missing values.

### Next Steps

If given more time, I would:

Perform regression analysis to model predictors of pay gap size.

Integrate SIC code data for more precise industry classification.

Conduct time-series analysis across multiple reporting years.

Build a Power BI dashboard for interactive policy reporting.

Explore correlation between bonus gaps and workforce composition.
