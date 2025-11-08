---
title: "Analyzing Students' Mental Health"
author: 'Chiawei Wang, PhD'
role: 'Data & Product Analyst'
email: 'chiawei.w@outlook.com'
date: 'August 2025'
---

`This document presents a technical case study using SQL to analyze the mental health of international students at a Japanese university, focusing on the relationship between length of stay and mental health outcomes.`

# Challenge

International students often face unique challenges when studying abroad, including adapting to new cultures, languages, and social environments. These factors can impact their mental health, making it important to understand the risks and predictors of mental health difficulties in this population.

A Japanese international university conducted a survey in 2018, later approved by multiple ethical and regulatory boards, to investigate the mental health of its students. The study focused on three key measures:

- **Depression** (PHQ-9 test)
- **Social Connectedness** (SCS test)
- **Acculturative Stress** (ASISS test)

# Research Questions

1. Does the length of stay in a foreign country influence the mental health of international students?
2. Specifically, are longer stays associated with higher levels of depression and acculturative stress?

# Data Overview

| Index | Column          | Type    | Description                                    |
| ----- | --------------- | ------- | ---------------------------------------------- |
| 0     | `inter_dom`     | VARCHAR | Student type (international or domestic)       |
| 1     | `japanese_cate` | VARCHAR | Japanese language proficiency                  |
| 2     | `english_cate`  | VARCHAR | English language proficiency                   |
| 3     | `academic`      | VARCHAR | Academic level (undergraduate or postgraduate) |
| 4     | `age`           | INT     | Age of student                                 |
| 5     | `stay`          | INT     | Length of stay in years                        |
| 6     | `todep`         | INT     | Depression score (PHQ-9)                       |
| 7     | `tosc`          | INT     | Social connectedness score (SCS)               |
| 8     | `toas`          | INT     | Acculturative stress score (ASISS)             |
| 9     | `inter_dom`     | VARCHAR | Student type (international or domestic)       |
| 10    | `japanese_cate` | VARCHAR | Japanese language proficiency                  |
| 11    | `english_cate`  | VARCHAR | English language proficiency                   |
| 12    | `academic`      | VARCHAR | Academic level (undergraduate or postgraduate) |
| 13    | `age`           | INT     | Age of student                                 |
| 14    | `stay`          | INT     | Length of stay in years                        |
| 15    | `todep`         | INT     | Depression score (PHQ-9)                       |
| 16    | `tosc`          | INT     | Social connectedness score (SCS)               |
| 17    | `toas`          | INT     | Acculturative stress score (ASISS)             |

# Approach

1. Performing the calculations
2. Filter and group the data
3. Ordering records

```sql
SELECT stay, 
       COUNT(*) AS student_count,
       ROUND(AVG(todep), 2) AS average_phq, 
       ROUND(AVG(tosc), 2) AS average_scs, 
       ROUND(AVG(toas), 2) AS average_asiss
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC;
```

# Results

| index | stay | student_count | average_phq | average_scs | average_asiss |
| ----- | ---- | ------------- | ----------- | ----------- | ------------- |
| 0     | 10   | 1             | 13          | 32          | 50            |
| 1     | 8    | 1             | 10          | 44          | 65            |
| 2     | 7    | 1             | 4           | 48          | 45            |
| 3     | 6    | 3             | 6           | 38          | 58.67         |
| 4     | 5    | 1             | 0           | 34          | 91            |
| 5     | 4    | 14            | 8.57        | 33.93       | 87.71         |
| 6     | 3    | 46            | 9.09        | 37.13       | 78            |
| 7     | 2    | 39            | 8.28        | 37.08       | 77.67         |
| 8     | 1    | 95            | 7.48        | 38.11       | 72.8          |

# Insights

- **Depression and Acculturative Stress:** Students with longer stays (10 years) report the highest average depression scores, while those with shorter stays (1–3 years) have lower scores. Acculturative stress also tends to be higher among students with longer stays.
- **Social Connectedness:** Scores for social connectedness do not show a clear trend with length of stay, suggesting other factors may influence this aspect of mental health.

# Summary

The analysis indicates that international students who remain longer in a foreign country may experience increased depression and acculturative stress. These findings highlight the importance of targeted support for international students, especially those who have been abroad for extended periods.

`Any questions, please reach out!`
