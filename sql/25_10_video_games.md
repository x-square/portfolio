---
title: 'When Was the Golden Era of Video Games?'
author: 'Chiawei Wang, PhD'
role: 'Data & Product Analyst'
email: 'chiawei.w@outlook.com'
date: 'October 2025'
---

`This case study explores the golden era of video games by analyzing sales data and review scores from both users and critics. By examining trends over the years, we aim to identify periods of exceptional quality and popularity in the gaming industry.`

# Challenge

Video games have evolved significantly over the years, with various genres, platforms, and technologies emerging. The challenge is to identify the golden era of video games by analyzing critical and user reception, as well as sales performance.

# Research questions

1. What are the top ten best-selling video games of all time?
2. What are the top ten years with the highest average critic ratings?
3. What are the golden years of video games based on user and critic reviews?

# Data overview

### game_sales

| Index | Column       | Type    | Description                      |
| ----- | ------------ | ------- | -------------------------------- |
| 0     | `name`       | varchar | Name of the video game           |
| 1     | `platform`   | varchar | Gaming platform                  |
| 2     | `publisher`  | varchar | Game publisher                   |
| 3     | `developer`  | varchar | Game developer                   |
| 4     | `games_sold` | float   | Number of copies sold (millions) |
| 5     | `year`       | int     | Release year                     |

### game_reviews

| Index | Column         | Type    | Description                          |
| ----- | -------------- | ------- | ------------------------------------ |
| 0     | `name`         | varchar | Name of the video game               |
| 1     | `critic_score` | float   | Critic score according to Metacritic |
| 2     | `user_score`   | float   | User score according to Metacritic   |

### game_users

| Index | Column           | Type   | Description                                          |
| ------| ---------------- | ------ | ---------------------------------------------------- |
| 0     | `year`           | int    | Release year of the games reviewed                   |
| 1     | `num_games`      | int    | Number of games released that year                   |
| 2     | `avg_user_score` | float  | Average score of all the games' ratings for the year |

### game_critics

| Index | Column             | Type  | Description                                                 |
| ------| ------------------ | ----- | ----------------------------------------------------------- |
| 0     | `year`             | int   | Release year of the games reviewed                          |
| 1     | `num_games`        | int   | Number of games released that year                          |
| 2     | `avg_critic_score` | float | Average critic score of all the games' ratings for the year |

# Approach

1. Find the top ten best selling games of all time
2. Find the top ten years with the highest rated average from critics
3. Find the golden years of video games, based on the user and critic reviews

```sql
-- best_selling_games
SELECT *
FROM game_sales
ORDER BY games_sold DESC
LIMIT 10;
```

```sql
-- critics_top_ten_years
SELECT g.year, COUNT(g.name) AS num_games, ROUND(AVG(r.critic_score),2) AS avg_critic_score
FROM game_sales AS g
INNER JOIN reviews AS r
ON g.name = r.name
GROUP BY g.year
HAVING COUNT(g.name) >= 4
ORDER BY avg_critic_score DESC
LIMIT 10;
```

```sql
-- golden_years
SELECT u.year, u.num_games, c.avg_critic_score, u.avg_user_score, c.avg_critic_score - u.avg_user_score AS diff
FROM critics_avg_year_rating AS c
INNER JOIN users_avg_year_rating AS u
ON c.year = u.year
WHERE c.avg_critic_score > 9 OR u.avg_user_score > 9
ORDER BY year ASC;
```

# Results

### Best-selling games

| index | name                                      | platform | publisher        | developer         | games_sold | year |
| ----- | ----------------------------------------- | -------- | ---------------- | ----------------- | ---------- | ---- |
| 0     | Wii Sports for Wii                        | Wii      | Nintendo         | Nintendo EAD      | 82.9       | 2006 |
| 1     | Super Mario Bros. for NES                 | NES      | Nintendo         | Nintendo EAD      | 40.24      | 1985 |
| 2     | Counter-Strike: Global Offensive for PC   | PC       | Valve            | Valve Corporation | 40.0       | 2012 |
| 3     | Mario Kart Wii for Wii                    | Wii      | Nintendo         | Nintendo EAD      | 37.32      | 2008 |
| 4     | PLAYERUNKNOWN'S BATTLEGROUNDS for PC      | PC       | PUBG Corporation | PUBG Corporation  | 36.6       | 2017 |
| 5     | Minecraft for PC                          | PC       | Mojang           | Mojang AB         | 33.15      | 2010 |
| 6     | Wii Sports Resort for Wii                 | Wii      | Nintendo         | Nintendo EAD      | 33.13      | 2009 |
| 7     | Pokemon Red / Green / Blue Version for GB | GB       | Nintendo         | Game Freak        | 31.38      | 1998 |
| 8     | New Super Mario Bros. for DS              | DS       | Nintendo         | Nintendo EAD      | 30.8       | 2006 |
| 9     | New Super Mario Bros. Wii for Wii         | Wii      | Nintendo         | Nintendo EAD      | 30.3       | 2009 |

### Top ten years with highest average critic ratings

| index | year | num_games | avg_critic_score |
| ----- | ---- | --------- | ---------------- |
| 0     | 1998 | 10        | 9.32             |
| 1     | 2004 | 11        | 9.03             |
| 2     | 2002 | 9         | 8.99             |
| 3     | 1999 | 11        | 8.93             |
| 4     | 2001 | 13        | 8.82             |
| 5     | 2011 | 26        | 8.76             |
| 6     | 2016 | 13        | 8.67             |
| 7     | 2013 | 18        | 8.66             |
| 8     | 2008 | 20        | 8.63             |
| 9     | 2017 | 13        | 8.62             |


### Golden years of video games

| index | year | num_games | avg_critic_score | avg_user_score | diff  |
| ----- | ---- | --------- | ---------------- | -------------- | ----- |
| 0     | 1997 | 8         | 7.93             | 9.50           | -1.57 |
| 1     | 1998 | 10        | 9.32             | 9.40           | -0.08 |
| 2     | 2004 | 11        | 9.03             | 8.55           | 0.48  |
| 3     | 2008 | 20        | 8.63             | 9.03           | -0.40 |
| 4     | 2009 | 20        | 8.55             | 9.18           | -0.63 |
| 5     | 2010 | 23        | 8.41             | 9.24           | -0.83 |

# Insights

- **Top ten best-selling games:** Wii Sports for Wii, Super Mario Bros. for NES, Counter-Strike: Global Offensive for PC, Mario Kart Wii for Wii, PLAYERUNKNOWN'S BATTLEGROUNDS for PC, Minecraft for PC, Wii Sports Resort for Wii, Pokemon Red / Green / Blue Version for GB, New Super Mario Bros. for DS, New Super Mario Bros. Wii for Wii.
- **Top ten years with highest average critic ratings:** 1998, 2004, 2002, 1999, 2001, 2011, 2016, 2013, 2008, 2017
- **Golden years of video games based on user and critic reviews:** 1997, 1998, 2004, 2008, 2009, 2010

# Summary

The analysis of video game sales and reviews reveals significant insights into the industry's evolution. The top-selling games highlight the enduring popularity of franchises like Nintendo's Mario series and the impact of multiplayer games like Counter-Strike and PUBG. The years 1998 and 2004 stand out as peak periods for critical acclaim, suggesting a golden era of innovation and quality in game development. Additionally, the identified golden years based on user and critic reviews underscore the dynamic nature of gaming preferences, with notable peaks in 1997, 1998, 2004, 2008, 2009, and 2010. These findings provide valuable insights for developers and publishers aiming to understand market trends and consumer preferences in the gaming industry.

`Any questions, please reach out!`
