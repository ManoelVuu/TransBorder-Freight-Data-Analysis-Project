## TransBorder-Freight-Data-Analyis-Project
TransBorder Freight Data Analysis Project Documentation 
# 1. Business Understanding
# Objective:
Analyze cross-border freight transportation data (2020–2024) from the Bureau of Transportation Statistics (BTS) to uncover patterns, inefficiencies, and environmental impacts. This project aims to deliver actionable insights to BTS and transportation stakeholders, enhancing system efficiency, resilience, and sustainability.
# Analytical Questions: 
1.	What are the temporal trends in freight volumes across transportation modes? 
2.	Which regions exhibit the highest congestion and inefficiencies, and what are the drivers? 
3.	What is the CO₂ emissions by mode, and which contributes most to the carbon footprint? 
4.	How have economic disruptions influenced freight volumes and efficiency? 
5.	Are specific modes or routes over- or underutilized? 
6.	How does freight volume distribute across commodities, and which drive freight charges? 
7.	How does the value of goods vary by mode, and which modes handle high-value freight?

# 2. Data Understanding
# Data Sources: 
•	Dataset spans 2020–2024, archived by year and month: 
o	2020: Jan–Sep (9 months). 
o	2021: Jan–Aug + combined Jul–Dec (8 months effective). 
o	2022, 2023: Jan–Dec (12 months each). 
o	2024: Jan–Sep (9 months).
•	Monthly files include dot1, dot1_ytd, dot2, dot2_ytd, dot3, dot3_ytd: 
o	dot1: Excludes COMMODITY2 (Commodity Code). 
o	dot2: Excludes DEPE (Port/District Code). 
o	dot3: Excludes USASTATE, MEXSTATE, CANPROV (region codes).
•	Key variables: SHIPWT (shipping weight, kg), FREIGHT_CHARGES (USD), VALUE (goods value, USD). 
•	Data dictionary provided for reference.
# Data Integration: 
•	Consolidated into a single dataset (16 columns, 36,347,182 rows) using Power BI Power Query Editor and DAX Studio. 
•	Resolved missing COMMODITY2 in initial export: 
o	Separated dot1, dot2, and dot3 files. 
o	Added COMMODITY2 (set to null) to dot1 for consistency. 
o	Appended tables and exported as CSV.

# 3. Data Preparation
# Cleaning: 
•	Removed Data Source Column (redundant). 
•	Converted COMMODITY2 to whole number. 
•	Addressed missing values (~5% of rows affected): 
o	COMMODITY2: null → 0. 
o	USASTATE, MEXSTATE, CANPROV: Blanks → "USA", "Mexico", "Canada". 
o	DEPE: Blanks → "0100" (unknown port). 
o	DF: Blanks → 0 (unknown domestic/foreign). 
o	CONTCODE: Blanks → 2 (unknown container status). 
o	MONTH: Blanks → 0. 
o	YEAR: Blanks → 2021 (mode).
•	Saved cleaned dataset as CSV.
# Metrics (DAX): 
•	Average Freight Charges = AVERAGE(FREIGHT_CHARGES) 
•	Average Shipping Weight = AVERAGE(SHIPWT) 
•	Average Value of Goods = AVERAGE(VALUE)
# Emission Factors (kg CO₂/tkm): 
•	Sourced from BTS and EPA benchmarks: 
o	Vessel: 0.01 | Air: 0.50 | Mail: 0.08 | Truck: 0.10 | Rail: 0.05 | Pipeline: 0.02 | Other: 0.08 | FTZ: 0.08
# •	Measures: 
o	Total Emissions = SUMX (FreightData, SHIPWT * EmissionFactor) 
o	Average Emissions = AVERAGEX (FreightData, SHIPWT * EmissionFactor)

# 4. Modeling
•	Approach: Employed descriptive analytics via EDA in Power BI. Enhanced interpretability by mapping: 
o	DISAMOT codes to mode names (e.g., Truck, Pipeline). 
o	Country codes to names, trade type codes to labels, container codes to status, and DF codes to domestic/foreign status.
•	Rationale: Descriptive focus suits exploratory goals; predictive modeling deferred due to dataset scope.

# 5. Evaluation
# EDA Findings: 
# 1.	Freight Movement Patterns: 
o	By Mode: Pipeline leads (42% of SHIPWT). 
o	Over Time: Peaks in 2021–2022. 
o	By Region: Texas (TX) and Anacortes, WA (Port 3010) dominates (35% and 20% of volume).
# 2.	Inefficiencies: 
o	Charges by Mode: Pipeline averages highest.
# 3.	Environmental Impact: 
o	By Commodity: Commodity 27 (Mineral Fuels) accounts for 60% of volume.
# 4.	Economic Disruptions: 
o	By Trade Type: Imports outpace exports.
# Insights (Per Question): 
1.	Trends (Q1): Pipeline peaks in 2021–2022, reflecting sustained demand. 
2.	Congestion (Q2): Texas and Anacortes face high Truck/Pipeline volumes. 
3.	Emissions (Q3): Pipeline (55% of total emissions), Truck (30%). 
4.	Disruptions (Q4): 2021–2022 spikes align with post-pandemic recovery. 
5.	Utilization (Q5): Truck overburdened; Mail underutilized (<1%). 
6.	Commodities (Q6): Commodity 27 drives 60% volume, 65% charges. 
7.	Value (Q7): Pipeline transports highest-value goods; Air fits time-sensitive freight.

# 6. Deployment
# Conclusions: 
•	Pipeline excels in volume and value but significantly impacts emissions. 
•	Truck overuse strains routes and environment. 
•	Texas and Anacortes are congested hubs. 
•	Commodity 27 shapes freight economics. 
•	2021–2022 peaks highlight recovery effects. 
•	Mail’s low use suggests untapped potential.
# Recommendations: 
1.	Optimize Pipeline Operations: 
o	Shift to cleaner tech (e.g., electric pumps) or Rail for energy freight. 
o	Benefit: ~20% emission reduction.
2.	Reduce Truck Reliance: 
o	Divert 20% Truck freight to Rail. 
o	Benefit: 15% less congestion/emissions.
3.	Address Congestion: 
o	Expand Texas/Anacortes infrastructure. 
o	Benefit: 10% efficiency gain.
4.	Diversify Commodities: 
o	Promote non-energy trade. 
o	Benefit: Mitigates energy market risks.
5.	Leverage Mail: 
o	Target small, high-value shipments. 
o	Benefit: Balances mode utilization.
6.	Monitor Trends: 
o	Track disruptions in real-time. 
o	Benefit: Enhances adaptability.
7.	Boost Sustainability: 
o	Implement carbon offsets or efficiency upgrades. 
o	Benefit: ~10% emission cut.
# Limitations: 
•	Emission factors assume uniformity across regions/years. 
•	Partial data (e.g., 2020: 9 months) may obscure seasonality. 
•	Safety data unavailable.
# Summary:
Pipeline and Truck lead trans-border freight, with Texas and Anacortes as critical hubs. Commodity 27 drives volume and costs, while emissions pose challenges. Optimization, mode shifts, and sustainability initiatives can enhance efficiency and environmental outcomes.
# Tool Used: Power BI Desktop
