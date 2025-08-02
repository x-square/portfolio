---
title: 'Technical case study on funnel analysis'
author: 'Chiawei Wang'
date: 'July 2025'
date-format: "MMMM YYYY"
---

# Task 1 Analysis review: Understanding the impact of speed on growth

As a mentor at FinTech, you are reviewing Alex's analysis linking transfer speed to the likelihood of a customer making another transfer within 30 days.

| **Speed of transfer** | **Conversion to next transfer in 30 days** |
| --------------------- | ------------------------------------------ |
| 0 - 1 h               | 87%                                        |
| 1 - 12 h              | 76%                                        |
| 12 - 24 h             | 66%                                        |
| 24+ h                 | 61%                                        |

: Impact of speed on growth

**1.1. How do you think Alex would interpret these results?**

Alex would interpret these results as indicating a strong relationship between transfer speed and customer retention. The data suggests that customers who experience faster transfers are more likely to return for another transfer within 30 days. The highest conversion rate is seen in transfers completed in 0 to 1 hour, with a conversion rate of 87%. As the time taken for transfers increases, the conversion rate decreases, with transfers taking longer than 24 hours having a conversion rate of only 61%. This indicates that transfer speed is a significant factor influencing customer behaviour and retention.

**1.2. What insights can you draw from this data?**
   
The data shows a negative correlation between transfer speed and conversion rates. The longer a transfer takes, the less likely customers are to make another one within 30 days. Therefore, optimising transfer speed could be a key strategy to enhance user satisfaction and drive growth. But what statistical tests do we have to back this up?

**1.3. What would you advise Alex and the Exchange team to do based on these findings?**

I would recommend Alex and the Exchange team to focus on strategies that can further reduce transfer times, especially for those currently taking longer than 24 hours. This could involve optimising backend processes, improving system performance, or enhancing partnerships with financial teams to expedite transfers. Additionally, they should consider conducting user research to understand customer expectations regarding transfer speed and identify areas for improvement. Finally, it would be beneficial to monitor the impact of any changes made on conversion to ensure that efforts are effective in driving growth.

# Task 2 Data analysis: Understanding conversion dynamics and anomalies

This follow-up to Alex's analysis explores conversion dynamics and anomalies in transfer data to highlight areas needing product team attention.

**2.1. Do you notice any meaningful patterns in the conversion data? If so, what might be driving them?**

As most transfers from created to transferred are completed on the same day, we cannot conclude that the transfer speed is the root cause of the conversion rates. However, we can observe that 44.85% of transfers created are actually funded, and 71.34% of funded transfers are successfully transferred. This means there's a big drop-off between someone starting a transfer and actually funding it. The root cause of this drop-off could be due to several factors, such as customers not having sufficient funds, encountering issues during the funding process, or changing their minds about the transfer.

The SQL query below is used to calculate overall conversion rates from transfer creation to funding and then to completion:

```sql
SELECT
    -- Count unique users who initiated a transfer
    COUNT(DISTINCT CASE WHEN event_name = 'Transfer Created' THEN user_id END) AS users_created,
    -- Count unique users who funded a transfer
    COUNT(DISTINCT CASE WHEN event_name = 'Transfer Funded' THEN user_id END) AS users_funded,
    -- Count unique users who completed a transfer
    COUNT(DISTINCT CASE WHEN event_name = 'Transfer Transferred' THEN user_id END) AS users_transferred,
    -- Calculate the conversion rate from 'Transfer Created' to 'Transfer Funded' as a percentage
    ROUND(CAST(COUNT(DISTINCT CASE WHEN event_name = 'Transfer Funded' THEN user_id END) AS REAL) * 100.0 /
    NULLIF(COUNT(DISTINCT CASE WHEN event_name = 'Transfer Created' THEN user_id END), 0), 2) AS conversion_created_to_funded,
    -- Calculate the conversion rate from 'Transfer Funded' to 'Transfer Transferred' as a percentage
    ROUND(CAST(COUNT(DISTINCT CASE WHEN event_name = 'Transfer Transferred' THEN user_id END) AS REAL) * 100.0 /
    NULLIF(COUNT(DISTINCT CASE WHEN event_name = 'Transfer Funded' THEN user_id END), 0), 2) AS conversion_funded_to_transferred
FROM
    "25-07-fintech-funnel-events";
```

| **users created** | **users funded** | **users transferred** | **conversion created to funded** | **conversion funded to transferred** |
| ----------------- | ---------------- | --------------------- | -------------------------------- | ------------------------------------ |
| 40,782            | 18,292           | 13,057                | 44.85%                           | 71.34%                               |

: Overall conversion rates

**2.2. Are there any irregularities in the data that the product team should investigate? What could be causing them from the user's perspective?**

To dive deeper, we break down the data by the available dimensions to examine anomalies. We do not see sudden declines or raises on a particular day, but we do observe disproportionate changes in segments of region, platform, and experience. The metric anomaly likely comes from issues in the customer experience. Some regions may have payment barriers, iOS users might face bugs after funding, and new users could find the process confusing. These problems may lead to uneven conversion rates across different groups.

Starting with regions, we see that the Asian region has a significantly lower conversion rate from transfer created to funded at 26.91% compared to Europe at 48.09% and North America at 43.87%. This suggests that users in the Asian region may face unique challenges during the payment stage, such as limited payment options, high fees, or strict regulations. However, once they manage to fund their transfer, they have a very high conversion rate to transferred at 90.71%, indicating that the issue lies primarily in the funding stage.

Looking at platforms, we find that iOS users have the highest conversion rate from created to funded at 55.01%, but their conversion rate from funded to transferred is the lowest at 65.89%. This could indicate potential issues with the iOS app, such as bugs or poor user experience, that prevent users from completing their transfers after funding. In contrast, Web users have the lowest conversion rate from created to funded at 36.42%, suggesting that the payment process may be more complex or confusing for them. However, once they fund their transfer, they have the highest conversion rate to transferred at 73.04%, indicating that those who manage to fund their transfers are likely to complete them.

When we look at experience, existing customers have a much higher conversion rate from created to funded at 60.14%, compared to new customers at only 23.71%. This suggests that existing users are more familiar with the process and find it easier to fund their transfers. However, once new users do fund their transfer, they have a high completion rate of 75.62%, indicating that the onboarding process is generally effective once they get past the initial hurdles.

The SQL query below calculates conversion rates for each dimension, using region as an example:

```sql
SELECT
    -- Region for which the conversion rates are calculated
    region,
    -- Count unique users who initiated a transfer
    COUNT(DISTINCT CASE WHEN event_name = 'Transfer Created' THEN user_id END) AS users_created,
    -- Count unique users who funded a transfer
    COUNT(DISTINCT CASE WHEN event_name = 'Transfer Funded' THEN user_id END) AS users_funded,
    -- Count unique users who completed a transfer
    COUNT(DISTINCT CASE WHEN event_name = 'Transfer Transferred' THEN user_id END) AS users_transferred,
    -- Calculate the conversion rate from 'Transfer Created' to 'Transfer Funded' as a percentage
    ROUND(CAST(COUNT(DISTINCT CASE WHEN event_name = 'Transfer Funded' THEN user_id END) AS REAL) * 100.0 /
    NULLIF(COUNT(DISTINCT CASE WHEN event_name = 'Transfer Created' THEN user_id END), 0), 2) AS conversion_created_to_funded,
    -- Calculate the conversion rate from 'Transfer Funded' to 'Transfer Transferred' as a percentage
    ROUND(CAST(COUNT(DISTINCT CASE WHEN event_name = 'Transfer Transferred' THEN user_id END) AS REAL) * 100.0 /
    NULLIF(COUNT(DISTINCT CASE WHEN event_name = 'Transfer Funded' THEN user_id END), 0), 2) AS conversion_funded_to_transferred
FROM "25-07-fintech-funnel-events"
GROUP BY region
ORDER BY region ASC;
```

| **region** | **users created** | **users funded** | **users transferred** | **conversion created to funded** | **conversion funded to transferred** |
| ---------- | ----------------- | ---------------- | --------------------- | -------------------------------- | ------------------------------------ |
| Europe     | 22,309            | 10,729           | 7,003                 | 48.09%                           | 65.28%                               |
| South Am   | 11,904            | 5,222            | 3,715                 | 43.87%                           | 71.13%                               |
| Asia       | 6,736             | 1,865            | 1,733                 | 27.69%                           | 92.92%                               |

: Conversion rates by region

| **platform** | **users created** | **users funded** | **users transferred** | **conversion created to funded** | **conversion funded to transferred** |
| ------------ | ----------------- | ---------------- | --------------------- | -------------------------------- | ------------------------------------ |
| Android      | 15,423            | 6,149            | 4,386                 | 39.86%                           | 71.32%                               |
| Web          | 11,450            | 4,173            | 3,047                 | 36.42%                           | 73.04%                               |
| iOS          | 13,909            | 7,970            | 5,624                 | 55.01%                           | 65.89%                               |

: Conversion rates by platform

| **experience** | **users created** | **users funded** | **users transferred** | **conversion created to funded** | **conversion funded to transferred** |
| -------------- | ----------------- | ---------------- | --------------------- | -------------------------------- | ------------------------------------ |
| Existing       | 23,957            | 14,404           | 9,689                 | 60.14%                           | 67.25%                               |
| New            | 16,825            | 3,888            | 2,939                 | 23.71%                           | 75.62%                               |

: Conversion rates by experience

`Any questions, please reach out`

Chiawei Wang PhD\
Senior Product Analyst\
<chiawei.w@outlook.com>
