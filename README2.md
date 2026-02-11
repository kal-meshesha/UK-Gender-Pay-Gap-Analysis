# Gender Pay Gap Lab - SQL Answers
## Question 1: How many companies are in the data set?
SELECT COUNT(*) AS total_companies

FROM gender_pay_gap_21_22;

#### Answer: There are 10,174 companies in the data set.

## Question 2: How many of them submitted their data after the reporting deadline?
SELECT COUNT(*) AS late_submission

FROM gender_pay_gap_21_22

WHERE submittedafterthedeadline = TRUE;

#### Answer: 361 companies submitted their data after the reporting deadline.

## Question 3:  How many companies have not provided a URL?
SELECT COUNT(*) AS missing_urls

FROM gender_pay_gap_21_22

WHERE companylinktogpginfo= '0';

#### Answer: 3700 companies did not provide URLs.

## Question 4: Which measures of pay gap contain too much missing data, and should not be used in our analysis?
#### Explanation: to answer this we need to focus on the diffmeanhourlypercent, diffmedianhourlypercent, diffmeanbonuspercent and diffmedianbonuspercent.
#### since this columns contain the core pay gap metrics and they directly measure the difference in pay between men and women.

SELECT COUNT(*)

FROM gender_pay_gap_21_22

WHERE diffmeanhourlypercent = 0

AND diffmedianhourlypercent = 0

AND diffmeanbonuspercent = 0

AND diffmedianbonuspercent = 0;

To calculate the percentage of the data missing
 
SELECT

ROUND(100.0 * SUM(CASE WHEN diffmeanhourlypercent = 0 THEN 1 ELSE 0 END) / COUNT(*), 2) AS mean_hourly_missing_pct,

ROUND(100.0 * SUM(CASE WHEN diffmedianhourlypercent = 0 THEN 1 ELSE 0 END) / COUNT(*), 2) AS median_hourly_missing_pct,

ROUND(100.0 * SUM(CASE WHEN diffmeanbonuspercent = 0 THEN 1 ELSE 0 END) / COUNT(*), 2) AS mean_bonus_missing_pct,

ROUND(100.0 * SUM(CASE WHEN diffmedianbonuspercent = 0 THEN 1 ELSE 0 END) / COUNT(*), 2) AS median_bonus_missing_pct

FROM gender_pay_gap_21_22;

#### Answer:Based on the data, the hourly pay gap metrics (DiffMeanHourlyPercent and DiffMedianHourlyPercent) are mostly complete and suitable for analysis,
#### with less than 9% missing.

#### However, the bonus pay gap metrics have more missing values: around 28% for mean bonus and 40% for median bonus.
#### Because of this, we will primarily focus on hourly pay gap measures for more consistent analysis, and treat bonus gap metrics with caution.

## Question 5: Choose which column you will use to calculate the pay gap. Will you use DiffMeanHourlyPercent or DiffMedianHourlyPercent? Can you justify your choice?

#### Answer: I chose the median hourly pay gap because it tells me what the typical person earns.

#### The median is the middle value, so it's not affected by a few people who earn a lot more or a lot less than everyone else.

#### This makes it a fairer and more accurate way to see the real difference in pay between men and women in most companies.

## Question 6:  Use an appropriate metric to find the average gender pay gap across all the companies in the data set.
Did you use the mean or the median as your averaging metric? Can you justify your choice?

SELECT AVG(diffmedianhourlypercent) AS avg_median_gap

FROM gender_pay_gap_21_22

WHERE diffmedianhourlypercent <> 0;

### Answer: I calculated the mean of all companies' DiffMedianHourlyPercent values to get the average UK gender pay gap.
### I used the median hourly gap column because it shows the typical difference in pay.
### The final result was an average median gap of 13.45%, meaning men earn about 13.45% more than women on average across the UK.

## Question 7:What are some caveats we need to be aware of when reporting the figure we’ve just calculated?
#### Answer: The data is self-reported by companies, which means there may be errors or inconsistencies.
#### Some companies may have submitted missing or placeholder values (like 0s) that affect the average.

## Question 8:What are the 10 companies with the largest pay gaps skewed towards men?

SELECT employername, diffmedianhourlypercent

FROM gender_pay_gap_21_22

WHERE diffmedianhourlypercent > 0

ORDER BY diffmedianhourlypercent DESC

LIMIT 10;

#### Answer: top 10 companies with the largest median gender pay gaps favoring men are
1 "HPI UK HOLDING LTD."

2 "ATFC LIMITED"

3 "M. ANDERSON CONSTRUCTION LIMITED"

4 "PSJ FABRICATIONS LTD"

5 "HULL COLLABORATIVE ACADEMY TRUST"

6 "SERVICE INNOVATION GROUP-UK LIMITED"

7 "BRAND ENERGY & INFRASTRUCTURE SERVICES UK, LTD."

8 "ROBINSON WEBSTER (HOLDINGS) LIMITED"

9 "THE LEARNING FOR LIFE PARTNERSHIP"

10 "GREENBROOK HEALTHCARE (HOUNSLOW) LIMITED"

## Question 9: What do you notice about the results? Are these well-known companies?
#### Answer: I searched the top companies on Google and Companies House.
#### One of the top 10 companies, Brand Energy, is a large international business.
#### HPI UK Holding is a medium-sized hospitality company.
#### The others are mostly small or education-related.
#### This shows that big gender pay gaps can happen in any size or type of company, not just big or famous

## Question 10: Apply some additional filtering to pick out the most significant companies with large pay gaps

SELECT employername, employersize, diffmedianhourlypercent

FROM gender_pay_gap_21_22

WHERE diffmedianhourlypercent > 30

AND employersize IN ('250 to 499',

'500 to 999',

'1,000 to 4,999',

'5,000 to 19,999',

'20,000 or more')

AND diffmedianhourlypercent <> 0

ORDER BY diffmedianhourlypercent DESC

LIMIT 10;

#### Answer: I wanted to find the companies with the biggest gender pay gaps that also have a lot of employees.
#### So I looked at companies with more than 250 workers and a pay gap higher than 30%.
#### I left out any companies that had a gap of exactly 0%, since that might mean they didn’t report properly.
#### Then I sorted the list from biggest to smallest gap and picked the top 10 companies.

## Question 11: How would you report on the results? Can we say that these companies are engaging in unlawful pay discrimination?
#### Answer: These companies have big gaps between what men and women earn, but that doesn’t always mean they’re breaking the law.
#### A company can have a gap if more men have higher-paid jobs or more women are in lower-paid roles.
#### We don’t have enough information to know if men and women are being paid differently for the same job.
#### So we can’t say for sure that these companies are doing anything illegal — but the gap still shows there may be a problem.

## Question 12: What’s the average pay gap in London versus outside London?

SELECT

CASE

WHEN postcode LIKE 'E%' OR postcode LIKE 'EC%' OR postcode LIKE 'N%'

OR postcode LIKE 'NW%' OR postcode LIKE 'SE%' OR postcode LIKE 'SW%'

OR postcode LIKE 'W%' OR postcode LIKE 'WC%' THEN 'London'

ELSE 'Outside London'

END AS location_group,

AVG(diffmedianhourlypercent) AS avg_pay_gap

FROM gender_pay_gap_21_22

GROUP BY location_group;

### Answer:I grouped companies into “London” and “Outside London” based on their postcode.

### London companies had an average pay gap of approximately 13%, and companies outside London had a gap of 12%.



## Question 13: What’s the average pay gap in London versus Birmingham?

SELECT

CASE

WHEN postcode LIKE 'B%' THEN 'Birmingham'

WHEN postcode LIKE 'E%' OR postcode LIKE 'EC%' OR postcode LIKE 'N%'

OR postcode LIKE 'NW%' OR postcode LIKE 'SE%' OR postcode LIKE 'SW%'

OR postcode LIKE 'W%' OR postcode LIKE 'WC%' THEN 'London'

END AS city,

AVG(diffmedianhourlypercent) AS avg_pay_gap

FROM gender_pay_gap_21_22

WHERE postcode LIKE 'B%'

OR postcode LIKE 'E%' OR postcode LIKE 'EC%' OR postcode LIKE 'N%'

OR postcode LIKE 'NW%' OR postcode LIKE 'SE%' OR postcode LIKE 'SW%'

OR postcode LIKE 'W%' OR postcode LIKE 'WC%'

GROUP BY city;

### Answer:I compared companies in London and Birmingham based on postcode.

### London companies had an average pay gap of 13%, while Birmingham companies had a gap of 11.5%.

### This shows that gender pay gaps may also vary by city, not just industry or company size.

## Question 14: What is the average pay gap within schools?

SELECT AVG(DiffMedianHourlyPercent) AS avg_school_pay_gap

FROM gender_pay_gap_21_22

WHERE employerName ILIKE '%school%'

OR employerName ILIKE '%academy%'

OR employerName ILIKE '%college%'

OR employerName ILIKE '%trust%'

OR employerName ILIKE '%education%'

OR employerName ILIKE '%learning%';
### Answer:The average median hourly pay gap in these school-related employers is 22.18%.

## Question 15: What is the average pay gap within banks?

SELECT AVG(diffmedianhourlypercent) AS avg_bank_pay_gap

FROM gender_pay_gap_21_22

WHERE employerName ILIKE '%bank%';
### Answer: The average median hourly pay gap in banks is 25.66%

## Question 16:Is there a relationship between the number of employees at a company and the average pay gap?

SELECT employersize, AVG(diffmedianhourlypercent) AS avg_pay_gap

FROM gender_pay_gap_21_22

WHERE diffmedianhourlypercent IS NOT NULL

GROUP BY employersize

ORDER BY avg_pay_gap DESC;

### Answer: I looked at the pay gap for companies of different sizes.
--The results show that bigger companies usually have smaller gender pay gaps.
--For example, companies with over 20,000 employees had the smallest gap, and smaller companies had bigger gaps.
