# TTC-Subway-Delay-Analysis
<img width="1426" height="800" alt="TTC Delay Dashboard Tutorial" src="https://github.com/user-attachments/assets/c2e3aada-147f-4001-a538-f90d1bb2b4ff" />

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
<img width="895" height="501" alt="Screenshot 2026-06-05 173805" src="https://github.com/user-attachments/assets/eb039c35-e03a-426a-b251-b8193149a4fe" />
<img width="890" height="500" alt="Screenshot 2026-06-05 173832" src="https://github.com/user-attachments/assets/a4feaff2-82b1-4cd7-955d-8bbff2f6fd3f" />
<img width="893" height="499" alt="Screenshot 2026-06-05 173914" src="https://github.com/user-attachments/assets/55708388-4a6b-459d-ad31-199074745985" />
<img width="894" height="501" alt="Screenshot 2026-06-05 174553" src="https://github.com/user-attachments/assets/a80629b0-9ba8-4711-9cae-e0163d4164d7" />







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



