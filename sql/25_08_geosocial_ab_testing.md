---
title: 'A/B Testing on Mobile Verification Methods'
author: 'Chiawei Wang, PhD'
role: 'Data & Product Analyst'
email: 'chiawei.w@outlook.com'
date: 'August 2025'
---

`This technical analysis using SQL presents the findings of an A/B(C) testing experiment conducted by GeoSocial to evaluate the effectiveness of different mobile number verification methods.`

# Challenge

For GeoSocial's new phone number verification methods, we have three versions:

- **Version A:** SMS verification only
- **Version B:** SMS verification marked, followed by unmarked WhatsApp verification
- **Version C:** WhatsApp verification marked, followed by unmarked SMS verification

# Data description

- `verification.csv`
    - **userID:** unique identifier
    - **group:** screen shown (A, B, or C)
    - **method:** verification method used (SMS or WhatsApp)
    - **verified:** when completed the process
- `profiles.csv`
    - **userID:** unique identifier
    - **gender:** male or female
    - **dob:** date of birth
    - **country:** location
- `costs.csv`
    - **country:** location
    - **sms_usd:** cost of SMS verification in USD
    - **whatsapp_usd:** cost of WhatsApp verification in USD

# Findings of a/b/c testing and questions to address

## Which screen is recommended to proceed with?

I recommend we move forward with Version C, with marked WhatsApp verification. This option provides a balanced approach by offering both WhatsApp and SMS for verification, which could encourage more users to complete the process at a lower cost.

I hypothesise that when users have a choice, they will pick the method they prefer, which should result in a higher success rate. On average, Version C has the highest verification rate at 91.9% and the lowest cost per verification at $0.061, as WhatsApp costs are lower than SMS for most countries.

Note that the average cost is calculated only for customers who successfully completed verification, not for all attempts.

$$
\text{average cost}=\frac{\text{sum of costs for SMS and WhatsApp}}{\text{count of successful verifications}}
$$

: Verified rate and average cost (when successful) by group

| group | verified_rate | avg_cost |
|-------|---------------|----------|
| A     | 86.2          | 0.104    |
| B     | 91.8          | 0.088    |
| C     | 91.9          | 0.061    |

```SQL
-- SQL script for verified rate and average cost
SELECT
    v.`group`,
    ROUND((CAST(SUM(CASE WHEN v.verified = 1 THEN 1 ELSE 0 END) AS REAL) / COUNT(*)) * 100, 2) AS verified_rate,
    ROUND(AVG(CASE WHEN v.verified = 1 AND v.method = 'Sms' THEN c.sms_usd WHEN v.verified = 1 AND v.method = 'Whatsapp' THEN c.whatsapp_usd END), 3) AS avg_cost
FROM verification AS v
JOIN profiles AS p ON v.userID = p.userID
JOIN costs AS c ON p.country = c.country
GROUP BY v.`group`;
```

## What success metrics are considered and why?

The main success metrics considered are the verification rate and the average cost per successful verification. The verification rate shows how effective a method is at completing the verification process, while the average cost provides an understanding of its financial efficiency. While a higher verification rate is desirable, it is equally important to ensure that the cost of achieving that rate is sustainable.

## Would additional data be incorporated if available?

Yes, it would help to include more data, such as:

- Time taken to complete verification
- Number of attempts per user
- Heat maps
- Customer feedback

These metrics work together to provide a clearer picture of where customers face friction. Longer completion times and multiple attempts can point to process bottlenecks, which may be caused by delays in message delivery or other technical issues. Heat maps then highlight the specific areas of the interface where customers struggle, while customer feedback adds valuable context by revealing their pain points and preferences.

## Why did the chosen screen perform best?

Version C performed best because it offers flexibility and is more cost-efficient, as WhatsApp is cheaper. Although the majority in Version B and about one-third in Version C preferred SMS, making it the most popular method, promoting WhatsApp verification will help the business operate more sustainably. Since verification rates are similarly high for both channels, this approach does not compromise performance.

: Percentage of chosen method and verified rate by group

| group | sms    | whatsapp | verified_sms | verified_whatsapp  |
|-------|--------|----------|--------------|--------------------|
| A     | 100.0  | NULL     | 86.1         | NULL               |
| B     | 87.9   | 12.1     | 91.7         | 90.8               |
| C     | 34.8   | 65.2     | 94.2         | 90.5               |

```sql
-- SQL script for chosen method and verified rate
SELECT
    `group`,
    ROUND((CAST(SUM(CASE WHEN method = 'Sms' THEN 1 ELSE 0 END) AS REAL) / COUNT(*)) * 100, 2) AS sms,
    ROUND((CAST(SUM(CASE WHEN method = 'Whatsapp' THEN 1 ELSE 0 END) AS REAL) / COUNT(*)) * 100, 2) AS whatsapp,
    ROUND(CASE WHEN SUM(CASE WHEN method = 'Sms' THEN 1 ELSE 0 END) = 0 THEN 0 ELSE (CAST(SUM(CASE WHEN method = 'Sms' AND verified = 1 THEN 1 ELSE 0 END) AS REAL) /
          SUM(CASE WHEN method = 'Sms' THEN 1 ELSE 0 END)) * 100 END, 2) AS verified_sms,
    ROUND(CASE WHEN SUM(CASE WHEN method = 'Whatsapp' THEN 1 ELSE 0 END) = 0 THEN 0 ELSE (CAST(SUM(CASE WHEN method = 'Whatsapp' AND verified = 1 THEN 1 ELSE 0 END) AS REAL) /
          SUM(CASE WHEN method = 'Whatsapp' THEN 1 ELSE 0 END)) * 100 END, 2) AS verified_whatsapp
FROM verification
GROUP BY `group`;
```

## Would additional changes to the screen be recommended?

Yes, further enhancements could be made:

- Usability testing (not directly related to changes but important to do)
- Make information icon more prominent
- Localise verification method
- Real-time feedback button
- Countdown timer for completion

Usability testing is important to understand how customers interact with the interface and their experience. For example, it can show whether the information icon is easy to find, if the instructions are clear, or why customers may prefer WhatsApp over SMS. Changing the primary method by location can help reduce costs, since in 20 out of 220 countries, WhatsApp is more expensive than SMS, including countries like the USA, UK, Turkey, and Pakistan. It is also helpful to give customers a way to ask for help or leave feedback if they face problems. Finally, the time allowed to enter the verification code is crucial, as most security codes are time-sensitive. We may need to include a timer on the screen.

# Segmentation

The sample is evenly divided among the three groups, making comparisons fair. Customer gender, age, and country are similarly distributed across all groups. There is no significant difference in completion rates between males and females. About 9% of countries have lower SMS fees than WhatsApp, as mentioned earlier, which could affect user preferences. Verification rates are below 70% in 23 countries, which is concerning. This suggests potential issues that should be investigated.

By age, the 25-34 group has the highest completion rate, while rates generally decline in older age groups, especially for those 65 and above. Interestingly, we do not see a drastic drop for the older groups in group C. To make our product more inclusive for all, we should take a closer look at the data by segments as needed.

: Verified rate below 70% by country

| country | verified_rate |
|---------|---------------|
| CG      | 0.00          |
| CU      | 0.00          |
| FO      | 0.00          |
| GD      | 0.00          |
| KM      | 0.00          |
| MN      | 0.00          |
| PR      | 0.00          |
| CI      | 6.1           |
| IR      | 10.2          |
| SY      | 16.3          |
| TD      | 32.8          |
| ZW      | 34.1          |
| GR      | 41.2          |
| BB      | 51.0          |
| BI      | 51.0          |
| GY      | 51.0          |
| MG      | 51.0          |
| VN      | 51.0          |
| AZ      | 55.2          |
| CN      | 61.1          |
| UA      | 62.3          |
| AL      | 67.1          |
| DO      | 67.1          |
| EE      | 67.1          |

```sql
-- SQL script for verified rate below 70%
SELECT
    country,
    ROUND((CAST(SUM(CASE WHEN v.verified = 1 THEN 1 ELSE 0 END) AS REAL) / COUNT(*)) * 100, 2) AS verified_rate
FROM verification AS v
JOIN profiles AS p ON v.userID = p.userID
GROUP BY country
HAVING verified_rate < 70
ORDER BY verified_rate, country;
```

: Distribution of age group along with age group count and verified rate by group

| group | age   | count_age | verified_rate |
|-------|-------|-----------|---------------|
| A     | 18-24 | 1802      | 84.2          |
| A     | 25-34 | 3251      | 87.7          |
| A     | 35-44 | 991       | 87.9          |
| A     | 45-54 | 229       | 85.9          |
| A     | 55-64 | 53        | 77.4          |
| A     | 65+   | 11        | 71.0          |
| B     | 18-24 | 1765      | 90.6          |
| B     | 25-34 | 2972      | 92.8          |
| B     | 35-44 | 872       | 91.9          |
| B     | 45-54 | 228       | 89.7          |
| B     | 55-64 | 41        | 93.2          |
| B     | 65+   | 13        | 67.2          |
| C     | 18-24 | 1708      | 90.1          |
| C     | 25-34 | 3128      | 92.9          |
| C     | 35-44 | 951       | 92.2          |
| C     | 45-54 | 237       | 90.8          |
| C     | 55-64 | 62        | 89.1          |
| C     | 65+   | 14        | 85.1          |

```sql
-- SQL script for age group distribution and verified rate
SELECT
    v.`group`,
    CASE 
        WHEN CAST(strftime('%Y','now') AS INTEGER) - CAST(SUBSTR(p.dob, INSTR(p.dob, ',') + 2, 4)
             AS INTEGER) BETWEEN 18 AND 24 THEN '18-24'
        WHEN CAST(strftime('%Y','now') AS INTEGER) - CAST(SUBSTR(p.dob, INSTR(p.dob, ',') + 2, 4)
             AS INTEGER) BETWEEN 25 AND 34 THEN '25-34'
        WHEN CAST(strftime('%Y','now') AS INTEGER) - CAST(SUBSTR(p.dob, INSTR(p.dob, ',') + 2, 4)
             AS INTEGER) BETWEEN 35 AND 44 THEN '35-44'
        WHEN CAST(strftime('%Y','now') AS INTEGER) - CAST(SUBSTR(p.dob, INSTR(p.dob, ',') + 2, 4)
             AS INTEGER) BETWEEN 45 AND 54 THEN '45-54'
        WHEN CAST(strftime('%Y','now') AS INTEGER) - CAST(SUBSTR(p.dob, INSTR(p.dob, ',') + 2, 4)
             AS INTEGER) BETWEEN 55 AND 64 THEN '55-64'
        WHEN CAST(strftime('%Y','now') AS INTEGER) - CAST(SUBSTR(p.dob, INSTR(p.dob, ',') + 2, 4)
             AS INTEGER) >= 65 THEN '65+'
    END AS age,
    COUNT(*) AS count_age,
    ROUND((CAST(SUM(CASE WHEN v.verified = 1 THEN 1 ELSE 0 END) AS REAL) / COUNT(*)) * 100, 2) AS verified_rate
FROM verification AS v
JOIN profiles AS p ON v.userID = p.userID
GROUP BY v.`group`, age
ORDER BY v.`group`, age;
```

# Statistical analysis and modelling

If time allows, we can expand the analysis to include:

- Chi-squared test to compare verification rates
- ANOVA test to assess differences in verification costs
- Logistic regression to predict verification success


## Chi-squared test for verification rate by group

I used Python to run a chi-squared test to see if the differences in verification rates between the three groups are real or just luck. The test shows that Groups B and C truly perform better than Group A, and the higher success rates are not due to chance.

- Chi-squared test results
    - Chi-squared statistic: 148.2
    - P-value: 1.2e-33
    - P-value is well below the significance level of 0.01
    - Effect size (Cramér's V): 0.48

: Verified rate by group

| group | verified_rate |
|-------|---------------|
| A     | 86.2          |
| B     | 91.8          |
| C     | 91.9          |

```python
# Python script for chi-squared test
print('Chi-squared Test for Verification Rate by Group')
contingency_table = pd.crosstab(verification_df['group'], verification_df['verified'])
chi2, p_chi2, dof, expected = chi2_contingency(contingency_table)

print(f'Contingency Table:\n{contingency_table}')
print(f'Chi-squared statistic: {chi2}')
print(f'P-value: {p_chi2}')

if p_chi2 < 0.01:
    print('Conclusion: The p-value is less than 0.01, indicating a highly statistically significant difference')
    print('in verification rates across the different groups (A, B, C).')
else:
    print('Conclusion: The p-value is greater than or equal to 0.05, indicating no statistically significant difference')
    print('in verification rates across the different groups (A, B, C).')
```

## ANOVA test for average verification cost by group

I used Python to run an ANOVA test to check if the average cost per successful verification is truly different across the three groups or just due to chance. The test shows a clear difference, confirming that Group C not only verifies well but also does so at a much lower cost than Groups A and B.

- ANOVA test results
    - F-statistic: 735.1
    - P-value: 2.1e-309
    - P-value is well below the significance level of 0.01
    - Effect size (eta-squared): 0.82

: Average cost when successful verification by group

| group | avg_cost |
|-------|----------|
| A     | 0.104    |
| B     | 0.088    |
| C     | 0.061    |

```python
# Python script for ANOVA test
print('ANOVA Test for Average Cost per Successful Verification by Group')

# Ensure there are successful verifications in each group before running ANOVA
if not successful_verifications_df.empty:
    # Get the costs for each group for successfully verified users
    group_a_costs = successful_verifications_df[successful_verifications_df['group'] == 'A']['cost_per_attempt'].dropna()
    group_b_costs = successful_verifications_df[successful_verifications_df['group'] == 'B']['cost_per_attempt'].dropna()
    group_c_costs = successful_verifications_df[successful_verifications_df['group'] == 'C']['cost_per_attempt'].dropna()

    # Perform ANOVA F-test
    # We need at least two groups with data for ANOVA.
    # Check if all groups have data
    groups_with_data = []
    if not group_a_costs.empty: groups_with_data.append(group_a_costs)
    if not group_b_costs.empty: groups_with_data.append(group_b_costs)
    if not group_c_costs.empty: groups_with_data.append(group_c_costs)

    if len(groups_with_data) >= 2:
        f_stat, p_anova = f_oneway(*groups_with_data)

        print(f'Average Cost by Group (Successful Verifications Only):')
        print(f'Group A Avg Cost: ${group_a_costs.mean():.3f}')
        print(f'Group B Avg Cost: ${group_b_costs.mean():.3f}')
        print(f'Group C Avg Cost: ${group_c_costs.mean():.3f}')
        print(f'F-statistic: {f_stat}')
        print(f'P-value: {p_anova}')

        if p_anova < 0.05:
            print('Conclusion: The p-value is less than 0.05, indicating a statistically significant difference')
            print('in the average cost per successful verification across the different groups (A, B, C).')
        else:
            print('Conclusion: The p-value is greater than or equal to 0.05, indicating no statistically significant')
            print('difference in the average cost per successful verification across the different groups (A, B, C).')
    else:
        print('Not enough groups with successful verification data to perform ANOVA.')
else:
    print('No successful verifications found to perform cost analysis.')
```

## Logistic Regression for verification success prediction

Due to time limits, I couldn't run a logistic regression. But, here is an idea:

- Quantify how gender, age, country, and other variables affect verification success
- Build coefficient and feature importance maps to identify key drivers
- Confirm Groups B and C outperform Group A, with Group C slightly ahead
- Determine which gender is more likely to verify
- Identify age groups needing extra support
- Highlight country-level variation for localisation

`Any questions, please reach out!`
