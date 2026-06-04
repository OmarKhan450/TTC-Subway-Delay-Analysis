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
*(Drag and drop your Page 1 screenshot here)*
*(Drag and drop your Page 2 / Map screenshot here)*
*(Drag and drop your GIF here showing interactivity)*

## 🧮 Key DAX Measures
Authored custom DAX logic to evaluate station efficiency:
`Delay Rate per 10k Commuters = DIVIDE([Total Delay Incidents], [Total Daily Boardings]) * 10000`


