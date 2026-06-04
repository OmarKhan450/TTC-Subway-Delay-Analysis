# TTC-Subway-Delay-Analysis
<img width="1304" height="708" alt="Power Bi Dashboard TTC Subway Delay" src="https://github.com/user-attachments/assets/8360ab3c-56ac-4da5-bd2b-441f1454f31a" />
An end-to-end Power BI dashboard analyzing Toronto transit delays, ridership efficiency, and spatial data.

# 🚇 TTC Subway Delay & Ridership Performance

## 📌 Objective
The goal of this project was to transform raw, unstructured operational transit logs from the Toronto Transit Commission (TTC) into an interactive Power BI dashboard. It evaluates system-wide delay volumes and engineers a custom efficiency metric to find which stations experience the worst delay rates relative to their passenger volume.

## 🛠️ Tools Used
* **Power Query (M Code):** Data extraction, cleaning, and appending.
* **Power BI & DAX:** Relational modeling, time-intelligence calculations, and data visualization.

## 🗂️ Data Engineering & Modeling
* **Data Cleansing:** Resolved 30+ legacy operational code variations (e.g., matching "Yonge-University And B" to "Bloor-Yonge") using custom M-code logic.
* **Geocoding:** Engineered location columns to properly map generic station names to Toronto coordinates via Bing Maps.
* **The Star Schema:** Built an optimized 1-to-Many relational model connecting the transactional delay table to a custom rolling calendar and a station dimension table.
*(Drag and drop your Data Model screenshot here)*

## 📈 Dashboard Previews
<img width="948" height="534" alt="Screenshot 2026-06-04 131428" src="https://github.com/user-attachments/assets/33c3f504-fed0-4222-8836-2c47e03452c3" />
<img width="893" height="537" alt="Screenshot 2026-06-04 115537" src="https://github.com/user-attachments/assets/6d4cc4fd-d67c-4530-af80-9456d291f962" />
<img width="893" height="539" alt="Screenshot 2026-06-04 115503" src="https://github.com/user-attachments/assets/1581ecba-3fd6-47db-a6e7-68977cb169a0" />
<img width="420" height="304" alt="Screenshot 2026-06-04 120714" src="https://github.com/user-attachments/assets/3c655b9e-d18f-4020-967f-f110deacb1dd" />



## 🧮 Key DAX Measures
Prior Year Delays = 
CALCULATE(
    [Total Delay Incidents], 
    SAMEPERIODLASTYEAR(Dim_Calendar[Date])
)

Monthly Target Delays = 
IF(
    ISBLANK([Prior Year Delays]), 
    BLANK(), 
    [Prior Year Delays] * 0.90
)

Total Delay Incidents = COUNTROWS(Fact_TTC_Delays)

Average Delay (Mins) = 
    AVERAGE
    (Fact_TTC_Delays[Min Delay]
    )

Authored custom DAX logic to evaluate station efficiency:
`Delay Rate per 10k Commuters = DIVIDE([Total Delay Incidents], [Total Daily Boardings]) * 10000`



