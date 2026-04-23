---
title: 'Price Matching Strategy'
subtitle: 'Targeted Interventions in the Canary Islands Market'
author: | 
    Chiawei Wang\
    Analytics Professional\
    <chiawei.w@outlook.com>
date: 'December 2025'
geometry: margin=1in
urlcolor: blue
numbersections: true
header-includes: # LaTeX packages and settings
    - \usepackage{charter} # charter for better readability or [default]{carlito} for a modern look
    - \usepackage{inconsolata} # Font for code
    - \usepackage{fvextra} # Code formatting
    - \DefineVerbatimEnvironment{Highlighting}{Verbatim}{fontsize = \footnotesize, breaklines, commandchars = \\\{\}} # Code block
    - \usepackage{enumitem} # List formatting
    - \renewcommand{\labelitemi}{$\bullet$} # 1st-level solid circle
    - \renewcommand{\labelitemii}{$\circ$} # 2nd-level hollow circle
    - \renewcommand{\labelitemiii}{$\diamond$} # 3rd-level hollow diamond
---

`This analysis uses SQL and data-driven insights to assess TravelAgent's price matching strategy in the Canary Islands, especially against OnTheBeach. It highlights where we are competitive, where we are vulnerable, and proposes a targeted pricing test to balance profit and market share growth.`

# Executive summary

From 8,388 Canary Islands searches, `TravelAgent` is the **dominant price leader** in **88.58%** of cases. The average price gap is **-£161.04**, a clear advantage that supports **market share growth** by consistently undercutting competitors.

However, we face a critical threat from **OnTheBeach**. They account for the highest volume of total searches, 4,476 in total, and against them, our win rate drops to **84.9%**. Our price gap narrows to a **small -£63.82 advantage**, which is insufficient to convert this high volume of potential customers and directly undermines our **profit optimisation** goals.

: Key metrics and business alignment

| Metric                    | Finding   | Business alignment                                                  |
| ------------------------- | --------- | ------------------------------------------------------------------- |
| Overall win rate          | 88.58%    | Supports market share growth (winning the majority of searches)     |  
| Overall average price gap | -£161.04  | Strong price leadership (significant price advantage overall)       |
| OTB win rate              | 84.9%     | Threat to market share growth (losing significant volume)           |
| OTB average price gap     | -£63.82   | Weak price signalling (insufficient to convert high-value shoppers) |

Shift from broad price competition to targeted, data-driven actions on the biggest losses versus OnTheBeach. We identified three key levers that will deliver the most value:

1. Product: **Bed & Breakfast packages**
2. Supplier: **Ryanair flights**
3. Geography: **Tenerife**

Proposed test plan:

- Apply a focused **5% price intervention** only on searches we currently lose to OnTheBeach for these **Tenerife B&B packages with Ryanair flights**.
- Run a 4‑week A/B test. Primary metric is **total profit per session**.
- Monitor competitor prices and supplier capacity. Use an **automated kill switch** to pause if OnTheBeach retaliates or costs spike.
- Success criteria include a wider price gap of **~-£160** versus OnTheBeach and a win rate of **~88%**, resulting in a **net positive profit uplift**.

# Where we are competitive: Strengths aligned with market share growth

Our pricing gives us a clear lead against the major UK rivals. Average gaps of more than £300 create a strong buffer that protects market share and margins. That buffer lets us run a small, targeted intervention against OnTheBeach without risking our wider position.

## Price dominance over legacy operators

We have a clear price advantage over **TUI** and **Jet2**. Our **dynamic packaging engine** bundles flights and hotels more efficiently, so we can offer lower prices on comparable full‑service packages. TUI and Jet2 likely have **higher operating costs**, so they cannot match our prices on like‑for‑like deals.

![](win_rate.png)

## Strategic buffer and freedom to act

Because we have big price cushions against legacy operators, we can act without risking our overall position. On average we are about **£347 cheaper** than TUI with a win rate of **96.3%** and about **£292** cheaper than Jet2 with a win rate of **93.0%**. This wide gap gives us the space to safely make a small, focused change to our prices against OnTheBeach, while still keeping our big lead in the market.

# Key threats and opportunities: Aligned with profit optimisation

The strategic focus shifts from broad market‑share competition to protecting and maximising profit in the highest‑value segments. Instead of across‑the‑board price cuts, we will concentrate limited margin investment only on the searches we currently lose to OnTheBeach (Tenerife B&B with Ryanair), where **volume and margin together** deliver the largest net profit uplift. This preserves margins on existing wins while directing interventions to the places that move the profit needle most.

## Threats: Deep dive into OnTheBeach performance

OnTheBeach poses the most immediate threat. While our overall win rate is solid, the sheer volume and deep weakness against OnTheBeach demand a segmented approach. They make up more than 50% of the sampled searches and our average price gap versus them is -£63.82. They are **often undercutting us**.

At a typical package value of £1,200, losing about 15% of searches to OnTheBeach means we are missing **thousands of bookings** and potentially **millions of pounds** in annual revenue and profit. We need a quick, focused response. A segmented intervention starting with Tenerife B&B with Ryanair is the most effective way to stop further losses and protect profit.

### Segment analysis & strategic focus areas

We will get the **biggest gains** by fixing our weakest areas in our largest market, **Tenerife**. Our main focus is to fix the issues with **board types** and **airline** segments together. More specifically, by fixing the B&B and Ryanair packages in Tenerife, we will be solving our core weaknesses and can immediately start winning back customers from our main competitor.

The data show why we struggle against OnTheBeach in these lowest-performing segments. Our price gap is narrow, so OnTheBeach can easily undercut us and take the sale:

: Performance against OnTheBeach by focus segments

| Focus segment   | Total searches | Total wins | Win rate  | Price gap |
| --------------- | -------------- | ---------- | --------- | --------- |
| Bed & Breakfast | 2,020          | 1,682      | 83.27%    | -£69.1    |
| Ryanair         | 4,846          | 4,047      | 83.51%    | -£57.28   |
| Tenerife        | 1,936          | 1,635      | 84.45%    | -£53.67   |

The Tenerife data show a similar win rate and narrow price gap that allow OnTheBeach to win against us:

: Performance against OnTheBeach by focus segments in Tenerife

| Focus segment   | Total searches | Total wins | Win rate  | Price gap |
| --------------- | -------------- | ---------- | --------- | --------- |
| Bed & Breakfast | 1,019          | 848        | 83.22%    | -£51.99   |
| Ryanair         | 1,672          | 1,399      | 83.67%    | -£48.98   |

### High-risk, low-search-volume segments

We will not change prices for these segments now but will watch them closely. They are low volume, but they can cause outsized harm if they move against us.

- **5-star hotels:** A low win rate of **79.63%** with an average price gap of **-£97.88**. Losing luxury bookings hurts revenue and **brand reputation**.
- **Seasonality (June/July):** Win rate drop dramatically to about **77.4%** in peak season. OnTheBeach is more competitive then, so we need better **forecasting and early countermeasures**.
- **Children-friendly packages:** A very low win rate of **73.28%** with a price gap of **-£62.16**. Families are high-value customers. Losing them impacts long-term growth.

## Opportunities: Targeted profit optimisation

The opportunity is not simply to lower prices, but to execute a **surgical counter-strategy** that avoids eroding margins where we already win. Instead, we will focus our investment only on the high-volume bookings lost to OnTheBeach, thereby protecting our profit margins across all of our other products.

### Immediate operational fixes

We must translate our existing advantage (proven against our largest competitors) into the simpler segments targeted:

- **B&B contract weakness:** We will use our volume to secure **exclusive, dynamic B&B hotel-only rates** in Tenerife, creating a definitive price gap in our favour.
- **Ryanair optimisation:** We must ensure the Ryanair component is **consistently competitive** by either negotiating better direct net fares or dynamically adjusting our flight packaging logic.

### Strategic pillars for growth and profit

These are the core principles that support our long-term success, each with a clear objective.

- **Sustainable growth:** We will drive controlled, profitable market share growth by using our current surgical pricing success as a blueprint for entry into **new markets and high-volume segments** we currently lose. For example, we can quickly replicate the Tenerife B&B with Ryanair strategy in **Lanzarote**.
- **Margin optimisation and ancillary products:** We will achieve **margin optimisation per transaction** by using targeted competitive pricing on the main package and aggressively **boosting high-margin ancillary product sales** immediately post-booking. For example, car hire and insurance upsells can significantly increase total profit per booking. A £20 insurance sale on a £1,200 package is a 1.67% margin boost.
- **User journey optimisation:** We must actively study and fix customer **pain points** along the user journey to ensure a seamless, high-conversion booking experience that supports a high, sustainable win rate. For example, optimising page load times and simplifying checkout can reduce drop-offs.

### Scale and defence for long-term security

We must ensure our strategy is scalable, safe and creates a long-lasting competitive advantage.

- **Scalable framework:** Success in Tenerife (our largest market in the Canary Islands) creates a proven, repeatable framework for rapid expansion with minimal effort.
- **Automated safety:** We will manage the enormous package optimisation space using **smart filtering** to find the best price quickly, and simultaneously protect this strategy with an **automated kill switch** that instantly pauses pricing if competitor retaliation or a cost spike is detected.
- **Defensive shield:** Establishing a dominant price position acts as a strong defensive shield, making it **financially unviable** for OTB to profitably challenge us in the high-volume, low-cost package space moving forward.

## Pricing experiment: A targeted profit growth plan

The pricing environment is highly dynamic, demanding a sophisticated, learning approach to pricing. The goal of this test is to validate, with minimal commercial delay, that guaranteeing a specific price advantage in our most vulnerable segment drives a **profitable conversion uplift**.

### Strategy and method

We will use technology to find the absolute best price, not just guess one.

- **Surgical intervention:** We will test a **5% surgical price intervention** as our starting hypothesis for our most vulnerable segment (**Tenerife B&B with Ryanair**).
- **Learning engine:** An A/B test powered by reinforcement learning will allow the system to learn and dynamically optimise the **absolute best price point** (which may be greater or less than 5%). This ensures the price is set to balance margin and volume for **sustainable profit growth**.

### Formal hypothesis and metrics

We are focusing on the precise change required to fix our weakness and the expected results.

- **Target change:** We hypothesise that by increasing our average price advantage against OnTheBeach from the current -£63.82 to a target advantage of -£160, we will increase our win rate to 88% within the next quarter.
- **Success metric:** We predict this guaranteed price signal will cause a **15% profitable conversion uplift**. Success will be formally measured by **total profit uplift per session** (including ancillary sales), ensuring we drive profitable market share gains, not just volume.

# Measurement of success: Aligned with profit optimisation and market share growth

Success will be defined by validating our core hypothesis that increasing booking volume through price adjustment leads to a net increase in profit. This validation must be conducted through a controlled, statistically rigorous **A/B test** over a minimum of four consecutive weeks, focusing on our targeted segment: **Tenerife B&B packages with Ryanair flights**.

## Primary metric: Total profit per session

This north star metric is essential to confirm that the increased volume and resulting ancillary sales (**volume effect**) decisively **outweigh** the direct cost of the 5% price adjustment (**margin effect**), thereby ensuring **profit optimisation**.

$$
\text{total profit per session}=\frac{(\text{booking volume} \times \text{package margin}) + \text{ancillary revenue} - \text{total cost}}{\text{total sessions}}
$$

| KPI | Success | Rationale |
| - | --- | --- |
| OTB win rate | Increase from 84.9% to a target of 88% | Primary measure of securing market share against the key competitor |
| OTB average price gap | Widen from -£63.82 to -£160 or greater | Shows we are successfully established as the clear price leader in targeted areas |

## Secondary metrics and context

1. **Conversion rate:** Confirms **market share growth**. It tracks the direct behavioural impact of the price change, confirming the hypothesis's volume goal (e.g. 15% increase as a target uplift to validate the action).
2. **Ancillary uplift:** Essential for **profit optimisation**. We need to confirm that a lower package price drives an uptake in high-margin extras (e.g. insurance, car hire) which will directly offset the cost of the price adjustment.
3. **Customer lifetime value:** Directly supports **market share growth**. Family customers are often higher-value repeat bookers. Securing their initial booking, even at a lower immediate profit, may be a strategic win, if it secures a customer with a high customer lifetime value.

# Other factors and data sources to be considered

To ensure the A/B test is valid and the strategy is effective, several external factors and data dependencies must be integrated. 

## External factors and assumptions

- **Product parity validation:** We must assume and, where possible, validate that the comparison packages from OnTheBeach are truly like-for-like, covering identical baggage allowances, flight times and room types. Significant deviations could skew the effectiveness of the price adjustment.
- **Competitor reaction:** The dynamic pricing environment means OnTheBeach may react. We must have an **automated monitoring trigger** in place to detect if OnTheBeach retaliates by lowering its price in turn, which would require pausing the test (**the kill switch**) and alerting the Trading team for re-evaluation. Finding a new strategy such as focusing on value-added services may be necessary if a price war ensues.
- **Capacity constraints:** The hypothesis relies on the assumption that our flight and hotel suppliers have the capacity to handle a **15% increase in volume** without spiking costs, which would negate the profit gain. If capacity is tight, we may need to adjust the expected uplift or negotiate better rates with suppliers in advance.
- **Qualitative feedback:** Collect customer feedback to find out why people choose OnTheBeach even when our price is lower. This will show non-price reasons e.g. website ease of use, trust, or brand preference that affect bookings and long‑term loyalty.
- **Loyalty programme impact:** If a loyalty programme is in place, we must consider how points or rewards influence customer choice, potentially skewing results if OnTheBeach offers a more attractive loyalty proposition.

## Data and system requirements

- **Live competitor pricing feed:** Mandatory for dynamic intervention.
- **Ancillary sales data integration:** Crucial for calculating **total profit** accurately and linking it to the test group.
- **Customer ID and booking history data:** Essential for calculating and monitoring customer lifetime value of the newly acquired customers.
- **Product match audit:** Necessary to periodically verify that the compared OnTheBeach package components (e.g. flights, board basis, baggage) are truly like-for-like.

# Appendix: Data overview and example SQL codes for key analysis

: Data overview

| Column                        | Type    | Description                                      |
| ----------------------------- | ------- | ------------------------------------------------ |
| departure_date                | DATE    | Year, month and day the trip starts             |
| hotel_name                    | VARCHAR | Name of the hotel                                |
| star_rating                   | INTEGER | Hotel's rating                                   |
| board_type                    | VARCHAR | Type of food plan (e.g. all-inclusive)           |
| duration                      | INTEGER | Number of nights                                 |
| num_adults                    | INTEGER | Count of adults                                  |
| num_children                  | INTEGER | Count of children                                |
| website                       | VARCHAR | Agency for the deal                              |
| origin_airport                | VARCHAR | Airport code where the journey begins            |
| destination_airport           | VARCHAR | Airport code near the holiday spot               |
| outbound_airline_travelagent | VARCHAR | travelagent' airline name for the first flight  |
| outbound_airline_competitor   | VARCHAR | Competitor's airline name for the first flight   |
| inbound_airline_competitor    | VARCHAR | Competitor's airline name for the return flight  |
| inbound_airline_travelagent  | VARCHAR | travelagent' airline name for the return flight |
| destination                   | VARCHAR | City or area name where the holiday is located   |
| region                        | VARCHAR | Larger geographical region name                  |
| resort                        | VARCHAR | Specific small resort town name                  |
| price_travelagent            | INTEGER | travelagent' total cost                         |
| price_competitor              | INTEGER | Competitor's total cost                          |

: Win rate and average price gap by board type against OTB

| board_type      | total_searches | total_wins | win_rate | avg_price_lh | avg_price_comp | avg_price_diff |
| --------------- | -------------- | ---------- | -------- | ------------ | -------------- | -------------- |
| Bed & Breakfast | 2020           | 1682       | 83.27    | 1320.10      | 1389.21        | -69.11         |
| Self Catering   | 1362           | 1156       | 84.88    | 969.79       | 1027.10        | -57.31         |
| All Inclusive   | 468            | 407        | 86.97    | 1421.79      | 1479.79        | -57.99         |
| Half Board      | 415            | 358        | 86.27    | 1365.71      | 1426.41        | -60.70         |
| Room Only       | 211            | 197        | 93.36    | 1057.30      | 1131.52        | -74.22         |

: Win rate and average price gap by Ryanair against OTB

| flight_leg | total_searches | total_wins | win_rate | avg_price_lh | avg_price_comp | avg_price_diff |
| ---------  | -------------- | ---------- | -------- | ------------ | -------------- | -------------- |
| Round      | 4846           | 4047       | 83.51    | 1203.53      | 1260.81        | -57.28         |
| Inbound    | 2457           | 2053       | 83.56    | 1203.26      | 1260.85        | -57.59         |
| Outbound   | 2389           | 1994       | 83.47    | 1203.79      | 1260.77        | -56.98         |

: Win rate and average price gap by region against OTB

| region        | total_searches | total_wins | win_rate | avg_price_lh | avg_price_comp | avg_price_diff |
| ------------- | -------------- | ---------- | -------  | -----------  | -------------- | -------------- |
| Tenerife      | 1936           | 1635       | 84.45    | 1227.68      | 1281.35        | -53.67         |
| Lanzarote     | 1228           | 1025       | 83.47    | 1237.83      | 1302.30        | -64.47         |
| Gran Canaria  | 707            | 597        | 84.44    | 1167.99      | 1249.48        | -81.49         |
| Fuerteventura | 603            | 542        | 89.88    | 1191.26      | 1265.87        | -74.61         |
| La Palma      | 2              | 1          | 50.00    | 885.00       | 875.00         | 10.00          |

```sql
-- Win rate and average price gap by board type against OTB
SELECT 
    board_type,
    COUNT(*) AS total_searches,
    SUM(CASE WHEN price_travelagent < price_competitor THEN 1 ELSE 0 END) AS total_wins,
    ROUND(SUM(CASE WHEN price_travelagent < price_competitor THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS win_rate,
    ROUND(AVG(price_travelagent), 2) AS avg_price_lh,
    ROUND(AVG(price_competitor), 2) AS avg_price_comp,
    ROUND(AVG(price_travelagent - price_competitor), 2) AS avg_price_diff
FROM search_results
WHERE competitor = 'OnTheBeach'
GROUP BY competitor, board_type 
ORDER BY total_searches DESC;
```

```sql
-- Win rate and average price gap by Ryanair against OTB
WITH CombinedLegs AS (
    SELECT 
        'Outbound' AS flight_leg,
        COUNT(*) AS total_searches,
        SUM(CASE WHEN price_travelagent < price_competitor THEN 1 ELSE 0 END) AS total_wins,
        ROUND(AVG(price_travelagent), 2) AS avg_price_lh,
        ROUND(AVG(price_competitor), 2) AS avg_price_comp,
        ROUND(AVG(price_travelagent - price_competitor), 2) AS avg_price_diff
    FROM search_results
    WHERE
        website = 'OnTheBeach' AND
        outbound_airline_travelagent = 'Ryanair' AND
        outbound_airline_competitor = 'Ryanair'
    UNION ALL
    SELECT 
        'Inbound' AS flight_leg,
        COUNT(*) AS total_searches,
        SUM(CASE WHEN price_travelagent < price_competitor THEN 1 ELSE 0 END) AS total_wins,
        ROUND(AVG(price_travelagent), 2) AS avg_price_lh,
        ROUND(AVG(price_competitor), 2) AS avg_price_comp,
        ROUND(AVG(price_travelagent - price_competitor), 2) AS avg_price_diff
    FROM search_results
    WHERE
        website = 'OnTheBeach' AND
        inbound_airline_travelagent = 'Ryanair' AND
        inbound_airline_competitor = 'Ryanair'
)
SELECT 
    'Round' AS flight_leg,
    SUM(total_searches) AS total_searches,
    SUM(total_wins) AS total_wins,
    ROUND(SUM(total_wins) * 100.0 / SUM(total_searches), 2) AS win_rate,
    ROUND(AVG(avg_price_lh), 2) AS avg_price_lh,
    ROUND(AVG(avg_price_comp), 2) AS avg_price_comp,
    ROUND(AVG(avg_price_diff), 2) AS avg_price_diff
FROM CombinedLegs
UNION ALL
SELECT 
    flight_leg,
    total_searches,
    total_wins,
    ROUND(total_wins * 100.0 / total_searches, 2) AS win_rate,
    avg_price_lh,
    avg_price_comp,
    avg_price_diff
FROM CombinedLegs
ORDER BY total_searches DESC;
```

```sql
-- Win rate and average price gap by region against OTB
SELECT 
    website AS competitor,
    region,
    COUNT(*) AS total_searches,
    SUM(CASE WHEN price_travelagent < price_competitor THEN 1 ELSE 0 END) AS total_wins,
    ROUND(SUM(CASE WHEN price_travelagent < price_competitor THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS win_rate,
    ROUND(AVG(price_travelagent), 2) AS avg_price_lh,
    ROUND(AVG(price_competitor), 2) AS avg_price_comp,
    ROUND(AVG(price_travelagent - price_competitor), 2) AS avg_price_diff
FROM search_results
WHERE competitor = 'OnTheBeach'
GROUP BY competitor, region 
ORDER BY total_searches DESC;
```

`Any questions, please reach out!`
