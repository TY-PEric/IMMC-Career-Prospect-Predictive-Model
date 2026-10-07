# Emerging Careers: Prospect and Trend Model (IMMC 2025)

This repository contains the research paper and modeling framework for the **International Mathematical Modeling Challenge (IMMC) 2025** qualifying round, which qualified our team for the international round. The model scores the short-term prospects and 10-year development trends of 19 new professions in the Chinese labor market.

---

## Project Overview
In July 2024, China's Ministry of Human Resources and Social Security introduced 19 emerging professions (e.g., Generative AI Specialists, Bioengineering Technicians). With youth unemployment high, we built a career selection framework for college graduates in three stages:
1. **Short-Term Career Prospect Model:** Quantifies the immediate market attractiveness across 5 representative careers using objective weighting.
2. **Long-Term Development Trend Model:** Forecasts 10-year industry viability and AI fungibility across all 19 professions using subjective hierarchical weighting.
3. **Personalized Career Recommendation:** A survey (the graduate's top 5 picks among the 19 professions, expected salary, and background) recommends the three best-trending careers that fit.

---

## Mathematical Modeling & Algorithmic Workflow

### 1. Data Normalization & Objective Weighting (CV Method)
* **Harmonic Mean for Skewed Salaries:** Handled right-skewed wage distributions by computing the harmonic mean of lower ($I_{low}$) and upper ($I_{high}$) bounds:
  $$I_{income} = \frac{2 \cdot I_{low} \cdot I_{high}}{I_{low} + I_{high}}$$
* **Dual Normalization:** Benefit indicators (Baidu search volume, CIER job vacancy index) were scaled via ratio transformation ($x / \max(x)$); cost indicators (daily working hours) were inverted ($\min(x) / x$).
* **Coefficient of Variation (CV) Method:** Assigned objective dispersion weights based on relative variation:
  $$CV_j = \frac{\sigma_j}{\mu_j}, \quad w_j = \frac{CV_j}{\sum_{k=1}^m CV_k}$$
  *Resulting Primary Weights:* Market Supply-Demand (43%), Developing Potential (28%), Income Level (22%), Working Conditions & Stability (7%).

### 2. Hierarchical Weight Allocation (AHP)
To model macro-level 10-year trends, we constructed a 3-layer Analytic Hierarchy Process (AHP):
* **Hierarchy:** Evaluated Primary Indicators (Market Supply-Demand, Industry Potential, Industrial Transformation) and corresponding secondary metrics (R&D investment, market size growth, PPP policy subsidies, and replaceability).
* **Consistency Verification:** Principal eigenvalues and eigenvectors were calculated ($AW = \lambda_{max}W$). Every comparison matrix passed the consistency check ($CR < 0.10$):
  $$CI = \frac{\lambda_{max} - n}{n - 1}, \quad CR = \frac{CI}{RI}$$

### 3. Multi-Criteria Ranking via TOPSIS
We implemented the Technique for Order Preference by Similarity to Ideal Solution (TOPSIS) on vector-normalized decision matrices:
* Calculated Euclidean distances to Positive-Ideal ($A^+$) and Negative-Ideal ($A^-$) solutions:
  $$D_i^+ = \sqrt{\sum_{j=1}^n (v_{ij} - v_j^+)^2}, \quad D_i^- = \sqrt{\sum_{j=1}^n (v_{ij} - v_j^-)^2}$$
* Scored alternatives based on relative closeness:
  $$C_i^+ = \frac{D_i^-}{D_i^+ + D_i^-}$$

---

## Results

**Short-term prospects (5 careers):** Cloud Network Intelligent Maintenance Technician scored highest (56.39), followed by Live Streamer (52.55).

### Top 5 Long-Term Emerging Careers (10-Year Trend Score)
| Rank | Emerging Profession | Trend Score (TOPSIS) |
| :---: | :--- | :---: |
| **1** | **Non-Ferrous Metal Spot Trader** | **60.23** |
| **2** | **User Growth Operations Specialist** | **58.51** |
| **3** | **Intelligent Manufacturing System Maintenance** | **52.26** |
| **4** | **Industrial Internet Maintenance Technician** | **50.71** |
| **5** | **Cybersecurity Grading & Assessment Specialist**| **46.48** |

The paper attributes the top rank to strong industry potential and a low replaceability score; the next places go to internet-related roles, in line with growing digitalization. Exhibition Builder scored lowest (**8.08**) because of its small market and unfavorable supply-demand balance.

---

## Repository Contents
* `IMMC_Qualify_Paper.pdf`: the full paper, with data tables, AHP comparison matrices, score distributions, and sensitivity analyses.
