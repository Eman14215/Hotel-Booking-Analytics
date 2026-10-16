# 🏨 Hotel Booking Business Intelligence & Analytics

An end-to-end data analytics and business intelligence project analyzing **~117,000 hotel bookings** (2015–2017) across a City Hotel and a Resort Hotel. The project extracts actionable business insights regarding customer demographics, operational discrepancies, revenue streams, and cancellation patterns.

---

## 📌 Executive Summary & Key Insights
* **Revenue Realized:** Generated **$42.7M** across confirmed bookings, with **$16.7M lost** due to booking cancellations.
* **Overall Cancellation Rate:** Stood at **37.5%**, driven heavily by long lead times and non-refundable deposit policies.
* **Customer Demographics:** 
  * **Couples** represent the dominant segment at **52%** (60.9k bookings, avg. stay 3.4 nights).
  * **Families** represent **22%** (25.8k bookings, avg. stay 4.1 nights).
  * **Solo Travelers** account for **17%** (avg. stay 2.8 nights).
* **Operational Risk:** Identified an **18.14% room discrepancy rate** (reserved room ≠ assigned room), representing a major friction point impacting guest satisfaction.

---

## 🛠️ Tech Stack & Workflow
* **Data Processing & Prep:** Python (Pandas, NumPy) & Excel Power Query
* **Database & Querying:** SQL Server (Complex Aggregations, Window Functions, Case Statements)
* **Business Intelligence & Dashboards:** Power BI Desktop
📁 Repository Structure
├── data/
│   ├── hotel_booking.csv             # Raw data (~119k records)
│   └── hotel_bookings_cleaned.csv     # Analysis-ready clean dataset
├── sql/
│   ├── 1_Revenue_Query.sql           # Hotel revenue baselines & lost revenue
│   ├── 2_Market_Query.sql            # Market segments & booking share
│   ├── 3_Cancellation_Query.sql      # Lead time & cancellation risk drivers
│   └── 4_Customer_Query.sql          # Demographics, meal plans & room discrepancies
├── dashboards/
│   ├── Hotel_Performance.pbix        # Interactive Power BI report
│   └── dashboard_screenshots/        # High-resolution dashboard visuals
└── README.md
---

## 📊 Core Analytical Modules

### 1. Customer Behavior & Operational Analysis (Deep Dive)
* Segmented guests using conditional logic based on adults, children, and babies.
* Calculated distribution of meal plan preferences per property using SQL window functions (`PARTITION BY hotel`).
* Measured operational friction points:
  * **Average Special Requests:** 0.57 per booking.
  * **Booking Change Rate:** 14.5% of reservations modified prior to arrival.
  * **Room Assignment Discrepancy:** 18.14% assigned an alternative room type.

```sql
-- Sample: Service Intensity & Operational Discrepancy
SELECT 
    hotel,
    AVG(total_of_special_requests) AS avg_special_requests,
    100.0 * SUM(CASE WHEN booking_changes > 0 THEN 1 ELSE 0 END) / COUNT(*) AS change_rate_pct,
    100.0 * SUM(CASE WHEN reserved_room_type <> assigned_room_type THEN 1 ELSE 0 END) / COUNT(*) AS room_discrepancy_rate_pct
FROM hotel_bookings_cleaned
GROUP BY hotel;
2. Revenue & Cancellation Analysis
Analyzed seasonality across Average Daily Rates (ADR), identifying summer peak rates.

Evaluated lead-time correlation with cancellations (risk increases significantly when lead time exceeds 90 days).

Modeled revenue leakage showing cancellations as the primary opportunity for margin improvement.

📈 Dashboard Showcase
(Screenshots of the interactive Power BI dashboard displaying key operational and behavior metrics)

🚀 How to Run
Database: Import hotel_bookings_cleaned.csv into SQL Server and run queries in /sql.

Dashboard: Open the .pbix file inside Microsoft Power BI Desktop to interact with the visualizations.


---

### نصائح سريعة لترتيب الملفات داخل الـ Repository:
1. **لا ترفعي ملف البوربوينت الأصلي (`.pptx`)**: العرض التقديمي كان مخصصاً للمناقشة الجماعية ومكتوباً فيه أسماء الجميع في البداية والنهاية. بدلاً من ذلك، خذي منه صور الداشبورد فقط وضعيها داخل مجلد باسم `img` أو `dashboard_screenshots`.
2. **ارفعي ملف الداشبورد بصيغة Power BI (`.pbix`)**: هو الملف الأهم الذي يبحث عنه أي عميل لتحميله والاطلاع على التصميم والـ DAX.
3. **ملفات الـ SQL**: ضعي ملفات الاستعلامات الأربعة، وبالأخص ملف استعلامات سلوك العملاء (`4_Customer_Query.sql`)؛ لأنه يبرز مهاراتك في استخدام الـ Window Functions والتقسيم المنطقي.
