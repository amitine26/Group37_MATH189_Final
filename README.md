# San Diego Short-Term Residential Occupancy (STRO) Ordinance Analysis

**Group 37 MATH 189 Final Project** 
**Group Members:** Anthony Mitine, Alan Tran, Jimmy Ying, James Xu

---

## Project Overview

San Diego’s rental market is currently navigating a severe affordability crisis, recently ranked as the 4th most unaffordable city in the United States. A widely debated cause of this crisis is the rapid expansion of commercial short-term rental units (STRs), which critics argue depletes the local housing supply in popular tourist neighborhoods.

International evidence supports this concern; for example, Airbnb saturation raised average neighborhood rents by nearly 2% (spiking to 7% in dense areas) in Barcelona, and displaced 0.23 to 0.37 rental units per listing in Berlin while increasing rent per square meter by 1.3 to 2.4 percent.

In May 2023, San Diego implemented the Short-Term Residential Occupancy (STRO) ordinance, capping whole-home short-term rental units at 1% of the city's housing stock and effectively eliminating nearly 50% of the short-term rentals across the city. This project investigates whether this massive policy shock caused a statistically significant decrease in long-term rent prices and whether the effect was stronger in areas with a higher pre-ordinance density of Airbnb listings.

---

## Hypotheses

* **Null Hypothesis:** The implementation of short-term rental caps did not have a differential effect on the average Airbnb listing price or long-term rent prices between high-density and low-density areas.
* **Alternative Hypothesis:** The implementation of short-term rental caps did have a differential effect on the average Airbnb listing price and long-term rent prices between high-density and low-density areas.

---

## Data Sources

| Source | Description | Timeframes | Key Features Used |
| :--- | :--- | :--- | :--- |
| **InsideAirbnb** | Point-in-time snapshot data of Airbnb listings in San Diego. | March 2023 & June 2025 | Price, neighborhood, latitude, longitude, room type |
| **Zillow (ZORI)** | Zillow Observed Rent Index for measuring long-term residential housing market trends. | January 2023 & January 2025 | ZIP code, average rent price |

---

## Methodology

### Exploratory Data Analysis (EDA)
* Filtered the InsideAirbnb dataset to isolate "Entire home/apt" properties, aligning with the specific targets of the STRO ordinance.
* Classified neighborhoods into "High-Density" and "Low-Density" groups based strictly on pre-ordinance (March 2023) listing counts to avoid post-treatment bias.
* Mapped neighborhood densities to broader ZIP codes to integrate with the ZORI dataset.
* Removed the top 1% of extreme price outliers (listings above $1999 per night) to handle severe right-skewness.
* Applied natural logarithm transformations to both Airbnb listing prices and ZORI rent prices to address heavy tails observed in Quantile-Quantile (QQ) plots and satisfy linear modeling assumptions.
* Plotted Empirical Cumulative Distribution Functions (ECDFs) to establish baseline differences in cumulative probability between density groups.

### Statistical Analyses
* **Difference-in-Differences Framework:** Evaluated the differential impact of the ordinance using Multiple Linear Regression (Ordinary Least Squares).
* **Short-Term Market Model:** Modeled the natural log of Airbnb prices against `Density_Group` and `Time_Period`, evaluating the interaction term.
* **Long-Term Market Model:** Modeled the natural log of ZORI rent prices against the same interaction effect.
* **Diagnostics:** Verified regression assumptions using Cook's Distance for influential outliers and Autocorrelation Function (ACF) plots for residual independence.
* **Nonparametric Fallback:** Executed a Two-Sample Kolmogorov-Smirnov (KS) test to evaluate if empirical distribution functions were statistically distinct due to slight non-normality in residuals.

---

## Results & Conclusions

### Short-Term Market (Airbnb Prices)
The interaction term between density and time yielded a coefficient of 0.0297 with a p-value of 0.444. Since this is well above the 0.05 threshold, we fail to reject the null hypothesis. While the nonparametric KS test confirmed a structural pricing difference exists between high and low-density markets (p-value = 1.82e-15), the regression confirms this gap did not differentially shift due to the policy. Capping supply did not cause remaining Airbnbs in saturated markets to disproportionately raise rates compared to less saturated areas.

### Long-Term Market (ZORI Rent Prices)
The ZORI regression yielded an interaction coefficient of -0.0122 with a p-value of 0.93. We fail to reject the null hypothesis. There is no statistical evidence that long-term rents decreased disproportionately in high-density Airbnb ZIP codes after the ordinance was enacted. The influx of former short-term rentals back into the residential market was likely absorbed instantly by San Diego's massive overarching housing deficit.

---

## Limitations

* **Geographic Resolution:** The ZORI long-term rent dataset was aggregated at the ZIP code level, which is much broader than the coordinate-level data provided by InsideAirbnb. This discrepancy may have diluted highly localized rent changes on specific streets or blocks.
* **Macroeconomic Confounders:** Observing market changes over a two-year period introduces confounding variables (e.g., inflation, shifting Federal Reserve interest rates, post-pandemic travel trends) that the current models could not fully isolate.
