# 🚇 TTC Subway Delay & Ridership Analysis

<img width="1426" height="800" alt="TTC Delay Dashboard Tutorial" src="https://github.com/user-attachments/assets/c2e3aada-147f-4001-a538-f90d1bb2b4ff" />

---

## 📌 Project Overview

This project is an end-to-end Power BI data analysis of Toronto Transit Commission (TTC) subway delays from 2023 to early 2026. 

The goal of this dashboard is to move beyond simply counting total delays by evaluating **station efficiency**. By combining raw operational delay logs with static station ridership data, this project calculates a **"Delay Rate per 100k Commuters"** to identify which stations truly experience the highest frequency of incidents relative to their foot traffic.

👉 **Live Dashboard:** [Click here to interact with the dashboard on NovyPro](Insert your link here)

---

## 📂 The Data Sources

*   **TTC Delay Logs (2023 - 2026):**  
    Raw operational CSV/Excel logs detailing incident dates, times, delay lengths, and operational codes.

*   **TTC Subway Ridership (2023 - 2024):**  
    A static PDF report published by the TTC detailing the average weekday passenger boardings per station.

---

## 🛠️ Architecture & Workflow

### 1. Data Engineering & ETL (Power Query / M Code)

*   **PDF Extraction & Unpivoting:**  
    Extracted ridership tables embedded inside a PDF, split multi-column layouts, and appended them into a single, clean vertical lookup table.

*   **Data Cleansing & Standardization:**  
    Built M-code logic to clean legacy operational data drift. Consolidated over 30+ inconsistent transit codes (e.g., `YUS`, `BD/YU`, `LINE 1`) into clean, standardized categories using `Text.Contains` logic.

*   **Appending Historical Data:**  
    Harmonized column structures across multiple yearly delay files and appended them into a single Master Fact Table (75,000+ rows).

---

### 2. Data Modeling

*   **Star Schema (1-to-Many):**  
    Architected an optimized database model connecting the transactional delay data to the station ridership lookup table.

*   **Rolling Calendar:**  
    Generated a custom, rolling calendar table natively in Power Query to support continuous timelines and time-intelligence logic.

---

### 3. DAX & Advanced Analytics

*   **Rate Calculations:**  
    Engineered a custom `Delay Rate per 100k Commuters` metric using `DIVIDE` to safely handle blanks and adjust delay volume by station ridership scale.

*   **Time Intelligence:**  
    Utilized `CALCULATE` and `SAMEPERIODLASTYEAR` to enable dynamic Year-over-Year (YoY) comparisons.

*   **Dynamic Targets:**  
    Authored logic to automatically generate a monthly delay reduction goal (5% reduction vs. prior year), including a baseline handler for the 2023 cold-start year.

*   **UI/UX Logic:**  
    Created visual filter helpers and dynamic warning labels to handle incomplete periods (e.g., masking future blank months and alerting users when viewing incomplete 2026 data).

---

## 📊 Dashboard Features

1.  **Executive Summary:**  
    High-level KPIs, system-wide trend analysis with forecasting, and Top 10 delay breakdowns.

2.  **Station Deep Dive (Drillthrough):**  
    Interactive Bing Maps integration mapped to specific Toronto coordinates, alongside a matrix sorting stations by operational delay rate rather than raw volume.

---

## 📈 Dashboard Previews
<img width="893" height="504" alt="Screenshot 2026-06-05 174818" src="https://github.com/user-attachments/assets/c1a1c91c-3c52-423b-8e4f-ec03f62ddffb" />
<img width="890" height="500" alt="Screenshot 2026-06-05 173832" src="https://github.com/user-attachments/assets/a4feaff2-82b1-4cd7-955d-8bbff2f6fd3f" />
<img width="893" height="499" alt="Screenshot 2026-06-05 173914" src="https://github.com/user-attachments/assets/55708388-4a6b-459d-ad31-199074745985" />
<img width="894" height="501" alt="Screenshot 2026-06-05 174553" src="https://github.com/user-attachments/assets/a80629b0-9ba8-4711-9cae-e0163d4164d7" />

---

## 📂 Repository Contents

*   `TTC_Subway_Analysis.pbix`: The complete Power BI Desktop project file.
*   `Subway Ridership 20232024.pdf`: The raw ridership report used as a lookup table.
*   `TTC Subway Delay Data 2024.xlsx` & `TTC Subway Delay Data since 2025.csv`: The raw delay files.

---

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
