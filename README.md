# Will the Customer Accept the Coupon?
**Dataset:** UCI Machine Learning Repository ; Driving Coupon Survey (Amazon Mechanical Turk)  ![`coupons.csv`](https://github.com/sahota-sudo/Portfolio-Project-1/blob/main/coupons.csv)

**Notebook:** ![`Practical_Application_Response.ipynb`](https://github.com/sahota-sudo/Portfolio-Project-1/blob/main/Practical_Application_Response.ipynb)

---

## Project Overview

This project explores what drives a customer to accept or reject a coupon delivered to their phone while driving. Survey respondents were shown a driving scenario and asked whether they would use a coupon for a nearby venue. Responses of "right away" or "later before it expires" were coded as **accepted (Y=1)**; "no" was coded as **rejected (Y=0)**.

The dataset contains **12,684 records** across five coupon types: Carry Out & Take Away, Inexpensive Restaurants (<$20), Coffee Houses, Restaurants ($20–$50), and Bars.

**Overall acceptance rate: 56.84%**

---

## Data Cleaning

Two sources of missing data were identified and addressed:

- **`car` column** : Over 99% of values were missing. This column was **dropped entirely**.
- **Frequency columns** (`Bar`, `CoffeeHouse`, `CarryAway`, `RestaurantLessThan20`, `Restaurant20To50`) — Each had 1–2% missing values, filled using the **column mode** (most common value) to preserve the ordinal distribution.

---

## Problem 1 : Coupon Type Distribution

![Coupon Distribution](https://github.com/sahota-sudo/Portfolio-Project-1/blob/main/01_coupon_distribution.png)

Coffee House coupons were offered most frequently (3,996), followed by inexpensive restaurants and carry-out. Bar coupons were among the least common at 2,017 offers.

---

## Problem 2: Temperature Distribution

![Temperature Distribution](https://github.com/sahota-sudo/Portfolio-Project-1/blob/main/02_temperature_histogram.png)

The overwhelming majority of survey scenarios took place at **80°F**, with far fewer at 55°F or 30°F. This skew means warm-weather conditions dominate the dataset and may inflate overall acceptance rates, since sunny/warm weather is associated with higher coupon uptake.

---

## Investigating Bar Coupons

Overall bar coupon acceptance was 41.0%. Bar coupons had the lowest acceptance rate of any coupon type. The analysis focused on which subgroups drove acceptance up or down.

Visit frequency tended to be the strongest predictor. Drivers who visit bars **more than 3 times a month** accepted bar coupons at nearly **77%** — more than double the rate of infrequent visitors (37%). The habit already exists; the coupon just reinforces it.

Age and frequency also compound the effect. Drivers who go to bars more than once a month **and** are over 25 accept at **69.5%**, versus just **33.5%** for everyone else. Older frequent bar-goers are a reliably receptive audience.

---

### Grouped Comparisons


Three groups were compared:

- **Group A** : Bar >1/month, no kids in car, not widowed: **71.3%**
- **Group B** : Bar >1/month, under age 30: **72.2%**
- **Group C** : Cheap restaurant >4/month, income under $50k: **60.1%**

Groups A and B confirm that social context (no kids, younger) and existing habits are the clearest markers. Group C is notable — even drivers who aren't bar regulars accept coupons at an elevated rate if they're budget-conscious frequent diners, suggesting value-sensitivity plays a secondary role.

### Hypothesis: Who Accepts Bar Coupons?

Bar coupon acceptance is driven primarily by **existing bar-going habits**, **younger age**, and **social context**. Drivers who already frequent bars don't need convincing, the coupon just makes the decision easier. Those traveling without kids and not widowed (i.e., likely younger and socially active) respond at rates above 70%. Budget-conscious diners also show elevated acceptance, pointing to a secondary value-seeking segment.

---

## Independent Investigation: Coffee House Coupons
![Coffee House Independent Investigation](https://github.com/sahota-sudo/Portfolio-Project-1/blob/main/03_coffee.png)

Coffee house coupons had an overall acceptance rate of **49.9%**; lower than carry-out or cheap restaurants, but with clear patterns that reveal who responds best.

The pattern mirrors bar coupons: drivers who already visit coffee houses regularly accept at dramatically higher rates. Regular visitors (4–8 times/month) accept at **68.6%**, while those who never go accept at just **18.9%**. Coupon delivery to non-regulars is largely wasted.

Acceptance peaks at **10AM (64.1%)** and drops through the day, falling to **41–44%** in the evening. This is intuitive, coffee is a morning behavior. Delivering coffee coupons at 6PM or 10PM is poorly timed.

Drivers with **friends** in the car accept at **59.7%**, compared to **43.8%** for solo drivers. Traveling with a partner (57.1%) also boosts acceptance. A passenger makes stopping feel more like a social activity than an errand.

### Key Findings: Coffee House Coupons

Coffee house coupon acceptance is strongly predicted by three factors: **existing visit habits**, **time of day**, and **who's in the car**. The ideal target is a regular coffee drinker, offered a coupon in the morning, while traveling with a friend or partner. Delivering coffee coupons to non-regulars in the evening is the least effective scenario by a wide margin.

---

## Summary of Key Findings

| Factor | Finding |
|---|---|
| Best coupon type | Carry Out & Take Away (74%), Inexpensive Restaurant (71%) |
| Worst coupon type | Bar (41%) |
| Strongest predictor  | Existing visit habits at the venue type |
| Best time to deliver | Afternoon (2PM) for most coupons; Morning (10AM) for coffee |
| Social context | Friends in car → highest acceptance across all types |
| Expiration window | 1-day coupons accepted at 63% vs 50% for 2-hour coupons |
| Bar coupon ideal profile | Frequent bar-goers, under 30, no kids in car |
| Coffee coupon ideal profile | Regular coffee drinkers, morning delivery, with friends |

---

## Next Steps
Based on the analysis, going forward coupon delivery should be targeted more directly given the findings. To do this, they should build a simple customer profile filter that prioritizes sending bar coupons to frequent bar-goers under 30 and coffee coupons to regular coffee drinkers in the morning, which should meaningfully improve acceptance rates over the current broad distribution. Additionally, underperforming coupon types should be investigated; upscale restaurants (44%) and bars (41%) both sit well below the overall average, and a deeper dive into what demographics do accept them could surface a niche audience worth targeting rather than writing those coupon types off entirely. It would also be beneficial to build a predictive model with the future in mind, with clean features like visit frequency, age, passenger type, and time of day already showing strong signal, the dataset is well-positioned for a classification model (e.g. logistic regression or a decision tree) that could predict acceptance probability for any given driver and coupon combination in real time.
