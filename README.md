
# Bengaluru Restaurants Analysis

An Excel-based market analysis of 9,291 Bengaluru restaurant listings: what exists, where it is, how it is rated, and which services matter. The project ships as one workbook (`Bengaluru_restaurant_analysis.xlsx`) with a clickable Home page, three themed dashboards and one fully interactive dashboard with filters.

## Objective
Understand Bengaluru's restaurant market and give owners and investors a single, filterable Excel dashboard to explore it.

| # | Goal | What it answers |
|---|------|-----------------|
| 1 | **Supply** | Map restaurants by pincode and cuisine |
| 2 | **Quality** | Measure ratings and review volume |
| 3 | **Services** | Find the popular meal slots, diets and features |
| 4 | **Decisions** | Put it all in one dashboard that owners and investors can filter themselves |

### The 15 business questions

**Dashboard 1: Market Overview**
1. How many restaurants are listed in Bengaluru?
2. How many restaurants have a rating, and how many are unrated?
3. How many reviews are there in total, and how many per restaurant on average?
4. Which 10 pincodes have the most restaurants?
5. How are ratings distributed from 1 to 5, and what is the share by rating band?

**Dashboard 2: Cuisine & Quality**

6. What is the average rating across all rated restaurants?
7. What percentage of rated restaurants score 4.5 or higher?
8. How many restaurants are popular (100 or more reviews)?
9. Which cuisines have the most restaurants?
10. Which cuisines have the best average rating and the most reviews?

**Dashboard 3: Services & Features**

11. What percentage of restaurants are vegetarian friendly?
12. What percentage of restaurants accept reservations?
13. What percentage of restaurants offer delivery or takeout?
14. Which meal slots (breakfast, lunch, dinner, brunch) are covered most?
15. Which dietary options and features are most common?

---

## Data

| Item | Detail |
|------|--------|
| Source file | `Bengaluru_Restaurants.csv` (kept untouched on the `Raw data` sheet) |
| Size | 9,291 listings, 20 original columns |
| Key fields | name, address, pincode, latitude/longitude, cuisine, dietary restrictions, features, meal type, number of reviews, rating (1 to 5) |
| Rated listings | 5,481 (3,810 or 41% have no rating) |
| Reviews | 135,084 in total |

### Data cleaning (full detail on the `Cleaning_Log` sheet)

| Area | What was done | Rows |
|------|---------------|------|
| Whitespace | Trimmed leading, trailing and repeated spaces in every text column | 9,291 |
| Pincode | Fixed typos and spacing (for example "560 083", "5600034") | 15 |
| Pincode | Filled missing or invalid pincodes from the address text | 10 |
| Pincode | Inferred from the 5 nearest listings by coordinates (flagged in `Pincode_Source`) | 1,155 |
| Pincode | Still unknown after all attempts | 97 |
| Coordinates | Removed values outside Bengaluru; filled gaps with the pincode centre | 57 / 279 |
| Blank text fields | Cuisine to "Not Specified"; diet, features and meal type to "None Listed" / "Not Specified" | 3,954 / 8,422 |
| Ratings | Unrated listings left blank (not invented) and labelled `Rating_Status = "Unrated"` | 3,810 |
| Duplicates | No exact duplicates; 58 possible duplicates (same name, phone and pincode) flagged in `Possible_Duplicate`, not deleted | 58 |

Multi-value fields (cuisine, dietary options, features, meal types) are stored as semicolon-separated lists, so one restaurant can count towards several cuisines or features.

---

## Workbook structure

| Sheet | Purpose |
|-------|---------|
| **Home** | Landing page with three clickable cards that open Dashboards 1, 2 and 3, plus headline numbers |
| **Dashboard 1** | Market Overview: Q1 to Q5 (KPI cards, top 10 pincodes, rating distribution, rating band doughnut, auto-updating insights) |
| **Dashboard 2** | Cuisine & Quality: Q6 to Q10 (KPI cards, top cuisines, best rated cuisines, most reviewed cuisines, insights) |
| **Dashboard 3** | Services & Features: Q11 to Q15 (KPI cards, meal slots, dietary options, top 10 features, insights) |
| **Interactive dashboard** | One-page explorer with a filter panel: Cuisine, Meal, Rating band, Serves Alcohol, Vegetarian Friendly, Min reviews, Pincode. KPI cards, charts, a 24-pincode tile map, a "Most reviewed" top 10 and a restaurant lookup all update together. Active filters turn light yellow. |
| **Dash_Calc** | Calculations behind Home and Dashboards 1 to 3 (also holds the two editable settings) |
| **Calc** | Calculations behind the interactive dashboard |
| **Lists** | Dropdown values for the filter boxes |
| **Cleaned_Data** | The analysis table (`tblRest`) with helper formula columns AC to AL |
| **Cleaning_Log** | Every cleaning step and the rows it affected |
| **Guide** | Short how-to for end users |
| **Raw data** | Original CSV, unchanged |

All sheets use a gray, white, light yellow and light blue palette. There are no macros; every number is a plain Excel formula.


## Headline results

| Measure | Value |
|---------|-------|
| Restaurants listed | 9,291 |
| Rated / unrated | 5,481 / 3,810 (41% unrated) |
| Total reviews / average per restaurant | 135,084 / 14.5 |
| Busiest pincode | 560001 with 411 listings (4.4%) |
| Top 10 pincodes share of listings | 34.2% (3,174 listings) |
| Average rating | 3.95 out of 5 |
| Rated 4.5 or higher | 26.9% (1,474 restaurants) |
| Most common rating | 4.0 (44.1% of rated restaurants) |
| Popular (100+ reviews) | 272 restaurants (2.9%) |
| Most listed cuisine | Indian (2,265), then Chinese (1,373) |
| Best rated cuisine (30+ rated) | Healthy at 4.34, then Japanese and Brew Pub |
| Most reviewed cuisine | Indian (80,016 reviews) |
| Vegetarian friendly | 10.1% (936 restaurants) |
| Accept reservations | 12.5% (1,165 restaurants) |
| Delivery or takeout | 9.8% (912 restaurants) |
| Meal slots | Dinner 2,313 > Lunch 2,223 > Breakfast 703 > Brunch 491 |
| Most common feature | Seating (1,369), then Reservations (1,165) |

---

## Definitions and assumptions

- **Rated restaurant:** has a rating value. Rating metrics (average, 4.5+ share, bands) use rated restaurants only.
- **Rating bands:** below 3.0, 3.0 to 3.9, 4.0 to 4.4, 4.5 to 5.0 on the three themed dashboards. The interactive dashboard uses its own labelled bands (Excellent, Good, Average, Below 3, Unrated).
- **Popular:** at least 100 reviews (editable).
- **Service percentages** (vegetarian, reservations, delivery or takeout, meal slots, features) are shares of **all** listings. Many listings leave these fields blank, so treat them as lower bounds, not the true share of restaurants that offer them.
- **Cuisine counts** use every cuisine a restaurant lists, so totals across cuisines exceed the number of restaurants. Restaurants with no cuisine are not counted in any cuisine. "Pub", "Bar" and "Brew Pub" appear because the source data lists them as cuisines.
- **Inferred pincodes** (1,155 listings) are estimates from nearby listings and are flagged in `Pincode_Source`.
---

## Deliverables

| File | Description |
|------|-------------|
| `Bengaluru_restaurant_analysis.xlsx` | The workbook described above |
| `Bengaluru_Restaurants_Insights.pptx` | Companion presentation answering the 15 questions |
| `README.md` | This file |

## Tools

Microsoft Excel (formulas, named ranges, table, charts, conditional formatting; no macros).
