# ZoomRide SQL Analysis

## 📌 Project Overview
This project uses **SQL (MySQL)** to clean and analyze trip data for **ZoomRide**, a ride-hailing company operating in six cities: Lagos, Abuja, Port-Harcourt, Accra, Nairobi and Kampala. The dataset contains 300 trip records, linked to driver and customer tables.

**Business problem solved:** The raw trip data had entry errors (misspelled city names, extra spaces, duplicate trips and missing fares) that made city-level reporting unreliable. This project cleans the data first, then answers questions on revenue by city, month and vehicle type, and on customer value, so the business can see where its money comes from.

## ❓ Business Questions Answered
1. How many rows are in the trips table?
2. What are the 5 longest completed trips?
3. How many trips happened in each city?
4. Which trips are duplicates, and how many completed trips have a missing fare?
5. How can the data be cleaned (city spellings, extra spaces, duplicates)?
6. What is the revenue, trip count and average fare by city?
7. What is the revenue and trip count by month?
8. What is the revenue by vehicle type?
9. Which customers have never booked a trip?
10. Who are the top 3 customers by total spend on completed trips?

## 📊 Key Metrics
- Total completed trips: **248**
- Total revenue from completed trips: **568,470** (currency not specified in the dataset)
- Revenue by city, month and vehicle type
- Average fare per trip, by city
- Top customers by total spend

## 🧹 Data Cleaning Summary
| Issue found | How it was fixed |
|-------------|------------------|
| Misspelled city names (`Nairobbi`, `Kampla`) | Corrected with `UPDATE ... SET` |
| Inconsistent city labels (`PH`, `Port Harcourt`) | Standardized to `Port-Harcourt` |
| Leading/trailing spaces (e.g. `" Accra"`, `" Lagos"`) | Removed with `TRIM()` across all text columns |
| 2 duplicate trips (trip IDs 299 and 300 repeated trips 82 and 253) | Identified with `GROUP BY ... HAVING COUNT(*) > 1` and removed with `DELETE` |
| 9 completed trips with a missing (`NULL`) fare | Identified and counted. These were left in place, so they are excluded from revenue totals (see Notes) |

**Result:** The city column went from 12 inconsistent values down to the 6 real cities, and the table went from 300 rows to 298.

## 🗄️ Database Structure
| Table | Description |
|-------|-------------|
| `trips` | One row per trip: trip_id, customer_id, driver_id, city, trip_date, distance_km, fare, status, payment_method |
| `drivers` | Driver details, including vehicle_type (Economy, Comfort, Bike) |
| `customers` | Customer details, including customer_name |

## 🛠️ Tools & SQL Skills Used
- **MySQL**
- Data cleaning: `UPDATE`, `TRIM()`, `DELETE`
- Duplicate detection: `GROUP BY`, `HAVING`, `MIN()` / `MAX()`
- Aggregations: `COUNT()`, `SUM()`, `AVG()`, `ROUND()`
- Joins: `INNER JOIN` (trips to drivers) and `LEFT JOIN` (customers to trips)
- Filtering and sorting: `WHERE`, `ORDER BY`, `LIMIT`
- Date formatting: `DATE_FORMAT()`

## 💡 Key Insights
- **Lagos is the biggest market.** It generated about 38% of total revenue (218,890 from 93 completed trips), far ahead of Accra (92,640) and Abuja (88,720). Kampala was the smallest at 38,020.
- **Accra has the highest average fare** (2,807), while Nairobi has the lowest (1,902). Lagos wins on volume, not on fare size.
- **Comfort rides earn far more per trip.** Economy brings in the most revenue overall (262,550 from 121 trips), but Comfort is close behind (239,050) with only 76 trips, an average of about 3,145 per trip compared to about 2,170 for Economy. Bikes are the lowest earner (66,870 from 51 trips).
- **Revenue peaks in December 2025 and May 2026.** December 2025 was the best month (66,980 from 31 trips), followed by May 2026 (64,140). April 2026 was the weakest (21,160 from 14 trips).
- **A few customers are very valuable.** Chioma Nwosu is the top spender (38,950 over 21 trips). Tunde Bakare ranks second (37,610) with only 13 trips, which means he spends much more per trip.
- **4 registered customers have never booked a trip,** which is a clear target for re-engagement or onboarding campaigns.

## 📝 Notes
- The 9 completed trips with missing fares were not filled in or removed. Revenue totals use `SUM()`, which ignores `NULL` values, so true revenue is slightly higher than reported. The trip counts include these rows.
- The monthly revenue query is sorted by revenue (highest first), not by date. Sort by `month` instead to see the trend over time.

## 📁 Files in This Repository
- `Zoom_Ascent.sql` – All SQL queries (Q1 to Q10) with comments
- `Zoom_Ascent_Answer_Sheet.txt` – Query outputs
- `README.md` – Project documentation
-  Message_to_the_Manager.docx

## ✅ Conclusion
This project shows an end-to-end SQL workflow: cleaning messy data (spelling errors, whitespace, duplicates), validating the fixes, and using joins and aggregations to answer business questions. It highlights Lagos as ZoomRide's main revenue source, the higher value of Comfort rides, and a clear group of top customers and inactive sign-ups to focus on.
