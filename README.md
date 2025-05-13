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
- `Average Freight Charges = AVERAGE(FREIGHT_CHARGES)`
- `Average Shipping Weight = AVERAGE(SHIPWT)`
- `Average Value of Goods = AVERAGE(VALUE)`

### Emission Factors (kg CO₂/tkm)
- Sourced from BTS and EPA benchmarks:
  - Vessel: 0.01 | Air: 0.50 | Mail: 0.08 | Truck: 0.10 | Rail: 0.05 | Pipeline: 0.02 | Other: 0.08 | FTZ: 0.08
- Measures:
  - `Total Emissions = SUMX(FreightData, SHIPWT * EmissionFactor)`
  - `Average Emissions = AVERAGEX(FreightData, SHIPWT * EmissionFactor)`

## EDA

- **Approach**: Employed descriptive analytics via Exploratory Data Analysis (EDA) in Power BI.
- **Enhancements**: Mapped codes for interpretability:
  - `DISAMOT` codes to mode names (e.g., Truck, Pipeline).
  - Country codes to names, trade type codes to labels, container codes to status, and `DF` codes to domestic/foreign status.
- **Rationale**: Descriptive focus suits exploratory goals; predictive modeling deferred due to dataset scope.

## Evaluation

### EDA Findings ![EDA TransBorder Freight Analysis](https://github.com/user-attachments/assets/b87db774-811b-4254-a794-50e597bdd40c)

1. **Freight Movement Patterns**:
   - **By Mode**: Pipeline leads (42% of `SHIPWT`)
   - **Over Time**: Peaks in 2021–2022
   - **By Region**: Texas (TX) and Anacortes, WA (Port 3010) dominate (35% and 20% of volume)
2. **Inefficiencies**:
   - **Charges by Mode**: Pipeline averages highest
3. **Environmental Impact**:
   - **By Commodity**: Commodity 27 (Mineral Fuels) accounts for 60% of volume
4. **Economic Disruptions**:
   - **By Trade Type**: Imports outpace exports (55% vs. 45%)

### Insights (Per Analytical Question) ![TransBorder Freight Analysis](https://github.com/user-attachments/assets/6baed55d-b1c9-4065-96ca-a4f328df0f49)

1. **Trends (Q1)**: Pipeline peaks in 2021–2022, reflecting sustained demand.
2. **Congestion (Q2)**: Texas and Anacortes face high Truck/Pipeline volumes.
3. **Emissions (Q3)**: Pipeline (55% of total emissions), Truck (30%)
4. **Disruptions (Q4)**: 2021–2022 spikes align with post-pandemic recovery.
5. **Utilization (Q5)**: Truck overburdened; Mail underutilized (<1%).
6. **Commodities (Q6)**: Commodity 27 drives 60% volume, 65% charges.
7. **Value (Q7)**: Pipeline transports highest-value goods; Air fits time-sensitive freight.

### Conclusions
- Pipeline excels in volume and value but significantly impacts emissions.
- Truck overuse strains routes and environment.
- Texas and Anacortes are congested hubs.
- Commodity 27 shapes freight economics.
- 2021–2022 peaks highlight recovery effects.
- Mail’s low use suggests untapped potential.

### Recommendations
1. **Optimize Pipeline Operations**:
   - Shift to cleaner tech (e.g., electric pumps) or Rail for energy freight.
   - *Benefit*: 20% emission reduction.
2. **Reduce Truck Reliance**:
   - Divert 20% Truck freight to Rail.
   - *Benefit*: 15% less congestion/emissions.
3. **Address Congestion**:
   - Expand Texas/Anacortes infrastructure.
   - *Benefit*: 10% efficiency gain.
4. **Diversify Commodities**:
   - Promote non-energy trade.
   - *Benefit*: Mitigates energy market risks.
5. **Leverage Mail**:
   - Target small, high-value shipments.
   - *Benefit*: Balances mode utilization.
6. **Monitor Trends**:
   - Track disruptions in real-time.
   - *Benefit*: Enhances adaptability.
7. **Boost Sustainability**:
   - Implement carbon offsets or efficiency upgrades.
   - *Benefit*: 10% emission cut.

### Limitations
- Emission factors assume uniformity across regions/years.
- Partial data (e.g., 2020: 9 months) may obscure seasonality.
- Safety data unavailable.

## Tools Used
- **Power BI Desktop**: Data Integration, Data Cleaning, Exploratory Data Analysis (EDA), Data Visualization, DAX Programming, Problem-Solving
- **DAX Studio**: Combining consolidated data into a CSV file.

## Contact
For questions or collaboration, reach out via:
- **Email**: [manoelvuu@gmail.com](mailto:manoelvuu@gmail.com)
- **Medium**: Read the full story at [Unpacking Cross-Border Freight](https://medium.com/@manoelvuu/transborder-freight-data-analysis-d9ec94a6c568)
