---
title: "Analysing Students' Mental Health with SQL"
author: 'Chiawei Wang'
date: 'August 2025'
---

`This document presents a technical case study using SQL to analyse the mental health of international students at a Japanese university, focusing on the relationship between length of stay and mental health outcomes.`

# Technical Report: Analysing Students' Mental Health

## Background

International students often face unique challenges when studying abroad, including adapting to new cultures, languages, and social environments. These factors can impact their mental health, making it important to understand the risks and predictors of mental health difficulties in this population.

A Japanese international university conducted a survey in 2018, later approved by multiple ethical and regulatory boards, to investigate the mental health of its students. The study focused on three key measures:

- **Depression** (PHQ-9 test)
- **Social Connectedness** (SCS test)
- **Acculturative Stress** (ASISS test)

## Data Overview

The dataset includes the following columns:

| Field Name      | Description                                       |
| --------------- | ------------------------------------------------- |
| `inter_dom`     | Student type (international or domestic)          |
| `japanese_cate` | Japanese language proficiency                     |
| `english_cate`  | English language proficiency                      |
| `academic`      | Academic level (undergraduate or postgraduate)    |
| `age`           | Age of student                                    |
| `stay`          | Length of stay in years                           |
| `todep`         | Depression score (PHQ-9)                          |
| `tosc`          | Social connectedness score (SCS)                  |
| `toas`          | Acculturative stress score (ASISS)                |

## Research Questions

1. Does the length of stay in a foreign country influence the mental health of international students?
2. Specifically, are longer stays associated with higher levels of depression and acculturative stress?

## Approach

We use PostgreSQL to analyse the relationship between length of stay and mental health outcomes among international students. The following SQL query summarises the average scores for depression, social connectedness, and acculturative stress by length of stay:

```sql
-- Count international students and average scores by length of stay, longest stay first
SELECT stay, 
       COUNT(*) AS count_int,
       ROUND(AVG(todep), 2) AS average_phq, 
       ROUND(AVG(tosc), 2) AS average_scs, 
       ROUND(AVG(toas), 2) AS average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC;
```

## Results

| Length of Stay (years) | International Student Count | Average PHQ-9 (Depression) | Average SCS (Social Connectedness) | Average ASISS (Acculturative Stress) |
|------------------------|-----------------------------|----------------------------|------------------------------------|--------------------------------------|
| 10                     | 1                           | 13                         | 32                                 | 50                                   |
| 8                      | 1                           | 10                         | 44                                 | 65                                   |
| 7                      | 1                           | 4                          | 48                                 | 45                                   |
| 6                      | 3                           | 6                          | 38                                 | 58.67                                |
| 5                      | 1                           | 0                          | 34                                 | 91                                   |
| 4                      | 14                          | 8.57                       | 33.93                              | 87.71                                |
| 3                      | 46                          | 9.09                       | 37.13                              | 78                                   |
| 2                      | 39                          | 8.28                       | 37.08                              | 77.67                                |
| 1                      | 95                          | 7.48                       | 38.11                              | 72.8                                 |

## Insights

- **Depression and Acculturative Stress:** Students with longer stays (10 years) report the highest average depression scores, while those with shorter stays (1–3 years) have lower scores. Acculturative stress also tends to be higher among students with longer stays.
- **Social Connectedness:** Scores for social connectedness do not show a clear trend with length of stay, suggesting other factors may influence this aspect of mental health.

## Conclusion

The analysis indicates that international students who remain longer in a foreign country may experience increased depression and acculturative stress. These findings highlight the importance of targeted support for international students, especially those who have been abroad for extended periods.

`Any questions, please reach out!`

Chiawei Wang, PhD\
Data & Product Analyst\
<chiawei.w@outlook.com>
