# TransBorder Freight Data Analysis

![Truck crossing Border3](https://github.com/user-attachments/assets/acf7b86b-6fcf-414c-ad24-f806eb0da7fc)

*Freight truck at a key U.S.-Canada border hub.*

## Overview

The **TransBorder Freight Data Analysis Project** analyzes cross-border freight transportation data (2020–2024) from the Bureau of Transportation Statistics (BTS) to uncover patterns, inefficiencies, and environmental impacts. This project aims to deliver actionable insights to BTS and transportation stakeholders, enhancing system efficiency, resilience, and sustainability. By processing 36,347,182 rows of data using Power BI Desktop, it addresses critical business questions to optimize freight operations.

## Business Objective

Analyze cross-border freight transportation data (2020–2024) from the Bureau of Transportation Statistics (BTS) to uncover patterns, inefficiencies, and environmental impacts. This project aims to deliver actionable insights to BTS and transportation stakeholders, enhancing system efficiency, resilience, and sustainability.

## Analytical Questions

1. What are the temporal trends in freight volumes across transportation modes?
2. Which regions exhibit the highest congestion and inefficiencies, and what are the drivers?
3. What is the CO₂ emissions by mode, and which contributes most to the carbon footprint?
4. How have economic disruptions influenced freight volumes and efficiency?
5. Are specific modes or routes over- or underutilized?
6. How does freight volume distribute across commodities, and which drive freight charges?
7. How does the value of goods vary by mode, and which modes handle high-value freight?

## Data Description

### Data Sources
- **Dataset**: BTS TransBorder Freight Data (2020–2024), archived by year and month:
  - 2020: Jan–Sep (9 months).
  - 2021: Jan–Aug + combined Jul–Dec (8 months effective).
  - 2022, 2023: Jan–Dec (12 months each).
  - 2024: Jan–Sep (9 months).
- **Files**: `dot1`, `dot1_ytd`, `dot2`, `dot2_ytd`, `dot3`, `dot3_ytd`.
  - `dot1`: Excludes `COMMODITY2` (Commodity Code).
  - `dot2`: Excludes `DEPE` (Port/District Code).
  - `dot3`: Excludes `USASTATE`, `MEXSTATE`, `CANPROV` (region codes).
- **Key Variables**:
  - `SHIPWT`: Shipping weight (kg).
  - `FREIGHT_CHARGES`: Cost (USD).
  - `VALUE`: Goods value (USD).
- **Reference**: Data dictionary provided.

### Data Integration
- Consolidated into a single dataset (16 columns, 36,347,182 rows) using Power BI Power Query Editor and DAX Studio.
- Resolved missing `COMMODITY2`:
  - Separated `dot1`, `dot2`, and `dot3` files.
  - Added `COMMODITY2` (set to `null`) to `dot1` for consistency.
  - Appended tables and exported as CSV.

## Data Preparation

### Cleaning
- Removed redundant `Data Source Column`.
- Converted `COMMODITY2` to whole number.
- Addressed missing values (~5% of rows):
  - `COMMODITY2`: `null` → 0.
  - `USASTATE`, `MEXSTATE`, `CANPROV`: Blanks → "USA", "Mexico", "Canada".
  - `DEPE`: Blanks → "0100" (unknown port).
  - `DF`: Blanks → 0 (unknown domestic/foreign).
  - `CONTCODE`: Blanks → 2 (unknown container status).
  - `MONTH`: Blanks → 0.
  - `YEAR`: Blanks → 2021 (mode).
- Saved cleaned dataset as CSV.

### Metrics (DAX)
- Sum Freight Charges = SUM(FREIGHT_CHARGES)
- Sum Shipping Weight = SUM(SHIPWT)
- Sum Value of Goods = SUM(VALUE)

### Emission Factors (kg CO₂/tkm)
- Sourced from BTS and EPA benchmarks:
  - Vessel: 0.01 | Air: 0.50 | Mail: 0.08 | Truck: 0.10 | Rail: 0.05 | Pipeline: 0.02 | Other: 0.08 | FTZ: 0.08
- Measures:
  - `Total Emissions = SUMX(FreightData, SHIPWT * EmissionFactor)`
  - `Average Emissions = AVERAGEX(FreightData, SHIPWT * EmissionFactor)`

## Modelling

- **Approach**: Employed descriptive analytics via Exploratory Data Analysis (EDA) in Power BI.
- **Enhancements**: Mapped codes for interpretability:
  - `DISAMOT` codes to mode names (e.g., Truck, Pipeline).
  - Country codes to names, trade type codes to labels, container codes to status, and `DF` codes to domestic/foreign status.
- **Rationale**: Descriptive focus suits exploratory goals; predictive modeling deferred due to dataset scope.

## Evaluation
### EDA Findings

<img width="1156" height="644" alt="EDA TransBorder Freight Analysis" src="https://github.com/user-attachments/assets/91ddd64f-182c-4f0c-86a8-969790fee659" />

- **By Mode:** Vessel leads with 33% of shipping weight (SHIPWT), followed by Pipeline at 30%.
- **Over Time:** Freight volume peaked in 2021–2022 at 15 trillion kg, reflecting economic recovery.
- **Charges by Mode:** Trucks lead with $0.66 trillion USD in freight charges.
- **By Commodity:** Commodity 27 (Mineral Fuels) drives 56% of freight volume.
- **By Trade Type:** Imports dominate at 86%, compared to exports at 14%.

### Insights (Per Analytical Question) 

<img width="1151" height="649" alt="TransBorder Freight Analysis" src="https://github.com/user-attachments/assets/cbb9bafd-c9f6-4ff3-b96a-ce9fff6176a0" />

- **Trends (Q1):** Freight volume peaked in 2021–2022, with Vessel leading at 33% of shipping weight.
- **Congestion (Q2):** Texas handles 14% of U.S. freight volume, primarily driven by Vessel.
- **Emissions (Q3):** Trucks account for 53% of CO2 emissions, followed by Rail (21%), Pipeline (16%), and Vessel (9%).
- **Disruptions (Q4):** Volume spikes in 2021–2022 align with post-pandemic recovery.
- **Utilization (Q5):** Trucks handle 65% of freight movements, while Mail is underused (<1%).
- **Commodities (Q6):** Commodity 27 (Mineral Fuels) contributes 56% of volume and 37% of charges.
- **Value (Q7):** Trucks carry 62% of goods’ value, based on freight charges and value in USD.

## Deployment
### Conclusion
- Vessel moves the most freight volume (33%), but Trucks dominate emissions (53%) and economic value (62%).
- High Truck usage strains infrastructure and increases environmental impact.
- Texas, handling 14% of volume, faces congestion, largely due to Vessel traffic.
- Commodity 27 (Mineral Fuels) drives over half of freight volume (56%) and a significant share of costs (37%).
- The 2021–2022 volume surge reflects post-pandemic demand.
- Mail’s minimal use (<1%) offers opportunities for optimization.

### Recommendations
- **Adopt Cleaner Truck Technologies:** Encourage the adoption of electric and hybrid trucks to significantly reduce greenhouse gas emissions and align with evolving environmental standards.
- **Divert Truck Freight to Rail:** Shift a portion of long-haul truck freight to rail to lower emissions and alleviate roadway congestion.
- **Expand Infrastructure in TX:** With over 14% of U.S. freight volume flowing through Texas, strategic investment in ports and highways can ease congestion and boost efficiency.
- **Diversify Commodity Types:** Reduce overreliance on Commodity 27 (Mineral Fuels), which dominates freight volume, by promoting trade in diversified, non-energy goods to increase economic stability.
- **Leverage Mail and Air for High-Value Freight:** Optimize underused modes like Mail for small, high-value goods and utilize Air for time-sensitive shipments, improving balance and responsiveness in mode utilization.
- **Implement Real-Time Freight Monitoring:** Adopt live data systems to monitor freight flows, detect bottlenecks, and respond proactively to disruptions enhancing operational resilience.
- **Promote Sustainable Logistics Practices:** Introduce carbon offset programs and system-wide efficiency upgrades with a target of reducing overall freight-related emissions by at least 10%.

### Limitations
- Emission factors may vary by region and year, limiting precision.
- Partial data for 2020 and 2024 (e.g., 9 months in 2020) may miss seasonal trends.
- Lack of safety data restricts risk analysis.

## Tools Used
- **Power BI Desktop**: Data Integration, Data Cleaning, Exploratory Data Analysis (EDA), Data Visualization, DAX Programming, Problem-Solving
- **DAX Studio**: Combining consolidated data into a CSV file.

## Contact
For questions or collaboration, reach out via:
- **Email**: [manoelvuu@gmail.com](mailto:manoelvuu@gmail.com)
- **Medium**: Read the full story at [Unpacking Cross-Border Freight](https://medium.com/@manoelvuu/transborder-freight-data-analysis-d9ec94a6c568)
