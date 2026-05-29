# ✈️ Indigo Airlines - Customer Booking Analysis

A complete end-to-end data analysis project on 50,000 Indigo Airlines customer booking records.  
The project covers data cleaning, exploratory data analysis, metric building, hypothesis testing, and business recommendations - delivered across **Python (Jupyter Notebook)**, **Microsoft Excel**, and **Tableau**.

---

## 📁 Repository Structure

```
indigo-booking-analysis/
│
├── data/
│   └── Indigo-customer-booking-raw-data.csv              # Raw dataset (50,000 rows, 18 columns)
│
├── notebook/
│   └── Indigo_Customer_Booking_Analysis_Final.ipynb      # Python analysis notebook
│
├── excel/
│   └── Indigo_Data_Analysis_Final.xlsx                   # Excel workbook (4 sheets)
│
├── tableau/
│   └── Indigo_Customer_Booking_Analysis.twb              # Tableau workbook (10 sheets + 1 dashboard + story telling)
│
├── charts/                                               # Exported chart images from notebook
│   ├── chart_01_seat_class.png
│   ├── chart_02_sales_channel.png
│   ├── chart_03_flight_day_of_week.png
│   ├── chart_04_booking_amount_by_total_add_ons.png
│   ├── chart_05_h1_purchase_lead_time_by_seat_class.png
│   ├── chart_06_h2_booking_amount_vs_length_of_stay.png
│   ├── chart_07_h2_booking_amount_vs_length_of_stay_regression.png
│   ├── chart_08_h3_behavioral_comparison_by_sales_channel.png
│   ├── chart_09_h4_combined_add_on_analysis.png
│   └── chart_10_h5_combined_flight_day_analysis.png
│
└── README.md
```

---

## 📊 Dataset Overview

| Property | Detail |
|---|---|
| Source | Indigo Airlines Customer Booking System |
| Records | 50,000 rows (after removing 1,201 junk rows) |
| Columns | 18 original + 3 helper columns added |
| Booking completion rate | 14.9% (7,442 completed out of 50,000) |
| Booking amount range | ₹0 - ₹91,651 |
| Seat classes | Economy, Premium Economy, Business |
| Sales channels | Internet, Mobile |
| Trip types | RoundTrip, OneWay, CircleTrip |
| Unique routes | 799 |
| Unique booking origins | 103 countries |

### Column Reference

| Column | Type | Description |
|---|---|---|
| `Flight number` | Numerical Discrete | Unique flight identifier |
| `num_passengers` | Numerical Discrete | Number of passengers per booking |
| `sales_channel` | Nominal Categorical | Internet or Mobile |
| `trip_type` | Nominal Categorical | RoundTrip / OneWay / CircleTrip |
| `purchase_lead` | Numerical Discrete | Days between booking and flight |
| `length_of_stay` | Numerical Discrete | Number of nights at destination |
| `flight_hour` | Numerical Discrete | Departure hour (0-23) |
| `flight_day` | Ordinal Categorical | Day of week (Mon-Sun) |
| `route` | Nominal Categorical | Origin–destination airport pair |
| `booking_origin` | Nominal Categorical | Country of booking |
| `wants_extra_baggage` | Binary (0/1) | Add-on: extra baggage selected |
| `wants_preferred_seat` | Binary (0/1) | Add-on: preferred seat selected |
| `wants_in_flight_meals` | Binary (0/1) | Add-on: in-flight meal selected |
| `flight_duration_planned` | Numerical Continuous | Scheduled flight duration (hours) |
| `flight_duration_actual` | Numerical Continuous | Actual flight duration (hours) |
| `booking_complete` | Binary (0/1) | 1 = booking completed, 0 = abandoned |
| `Seat class` | Nominal Categorical | Economy / Premium Economy / Business |
| `booking amount` | Numerical Continuous | Total booking value in ₹ |

---

## 🐍 Python Notebook

**File:** `notebook/Indigo_Customer_Booking_Analysis_Final.ipynb`

### Libraries Required

```bash
pip install pandas numpy matplotlib seaborn
```

### Notebook Structure

| Section | Contents |
|---|---|
| 1 - Setup | Library imports, global chart styling |
| 2 - Data Loading & Cleaning | Load CSV, remove junk rows, fix placeholders, standardise trip types, set data types, create helper columns |
| 3 - Exploratory Data Analysis | Data type reference table, numerical descriptive stats, categorical value counts, outlier check |
| 4 - Top-Level Metrics & KPI Dashboard | Metrics tree, Total Revenue, Avg Booking Value, Completion Rate, Add-on attachment rates |
| 5 - Revenue Breakdown | Breakdown by seat class, sales channel, trip type, flight day, and add-ons with charts |
| 6 - Hypothesis Testing | 5 hypotheses with stats, visualisations, and verdicts |
| 7 - Summary & Recommendations | Hypothesis results table, 5 business recommendations |

### Key Data Cleaning Steps (Section 2)

```python
# 1. Remove 1,201 junk rows after row 50,000
df = pd.read_csv('Indigo-customer-booking-raw-data.csv')
df = df.dropna(subset=['Flight number']).reset_index(drop=True)

# 2. Replace placeholder text with NaN
df['sales_channel'] = df['sales_channel'].replace('{no data}', np.nan)
df['booking_origin'] = df['booking_origin'].replace('(not set)', np.nan)

# 3. Standardise inconsistent trip_type values
df['trip_type'] = df['trip_type'].replace({
    'RoundOnewayTrip': 'RoundTrip',
    'RoundTripRoundOnewayTrip': 'RoundTrip'
})

# 4. Order flight_day correctly (Mon - Sun)
day_order = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
df['flight_day'] = pd.Categorical(df['flight_day'], categories=day_order, ordered=True)
```

---

## 📗 Excel Workbook

**File:** `excel/Indigo_Data_Analysis_Final.xlsx`

### Workbook Structure

| Sheet | Purpose |
|---|---|
| **KPI Dashboard** | Top-level KPI cards, add-on rates, hypothesis snapshot, recommendations |
| **Metric Tree** | Total booking amount breakdown by Seat Class, Sales Channel, and Trip Type |
| **Data Dictionary** | Column data types reference, dataset statistics, missing value counts |
| **Pivot Tables** | Pivot-style tables by seat class, channel, flight day, and add-on analysis |

---

## 📊 Tableau Workbook

**File:** `tableau/Indigo_Customer_Booking_Analysis.twb`  
**Data Source:** `Indigo_Data_Set.xlsx`  
**Tool Version:** Tableau Desktop 2026.1

### Workbook Structure

The workbook contains **10 worksheets** assembled into **1 interactive dashboard**.

#### Worksheets

| Sheet | Chart Type | Purpose |
|---|---|---|
| KPI Sheet | Text / KPI Cards | Total Revenue, Completed Bookings, Completion Rate |
| Revenue by Seat Class | Bar + Line (Dual Axis) | Revenue and average booking amount by seat class |
| Revenue by Sales Channel | Pie Chart | Internet vs Mobile revenue split (91.7% vs 8.3%) |
| Add-on Heatmap | Colour Matrix | Avg booking amount by Seat Class × Total Add-ons |
| Add-on Impact | Bar Chart | How add-on count drives average booking amount |
| Booking by Flight Days | Bar + Line (Dual Axis) | Booking count and average amount by day of week |
| Top 10 Route by Revenue | Horizontal Bar | Top revenue-generating routes |
| Purchase Lead by Seat Class | Bar Chart | H1 - avg lead time per seat class |
| Sales Channel Behaviour | Multi-axis Chart | H3 - channel comparison (volume + value) |
| Stay vs Amount | Scatter + Trend Line | H2 - length of stay vs booking amount |

#### Dashboard

| Dashboard | Sheets Included | Interactive Features |
|---|---|---|
| Dashboard 1 | All 10 sheets | Slicers for Seat Class, Sales Channel, Flight Day · Filter actions between charts |

#### Calculated Fields

| Field Name | Formula | Purpose |
|---|---|---|
| `Total Revenue (Completed)` | `IF [booking_complete] = 1 THEN [booking_amount] ELSE 0 END` | Revenue from completed bookings only |
| `Total Completed Booking` | Count filtered to `booking_complete = 1` | Count of completed bookings |
| `Completion Rate` | `[Total Completed Booking] / COUNT(*)` | % of bookings that completed |
| `Is Complete` | `[booking_complete] = 1` | Boolean filter used across all revenue charts |
| `Number of Records` | `COUNT(*)` | Total booking count |

#### How to Open

1. Download `Indigo_Customer_Booking_Analysis.twb` and `Indigo_Data_Set.xlsx` into the **same folder**
2. Open the `.twb` file in **Tableau Desktop 2021.1 or later**
3. If prompted to re-locate the data source, point it to `Indigo_Data_Set.xlsx`

> **Note:** The `.twb` file stores workbook structure only - it does not embed the data. The Excel file must be in the same folder for the workbook to load correctly.

---

## 🔬 Hypothesis Results

| # | Hypothesis | Verdict | Key Finding |
|---|---|---|---|
| H1 | Business class customers book further in advance than Economy | ❌ Not Supported | Avg lead times nearly identical: Business 84.6 days, Economy 85.0 days |
| H2 | Longer length of stay leads to higher booking amount | ❌ Not Supported | Pearson correlation ≈ 0.01 - scatter shows no relationship |
| H3 | Sales channel influences booking amount and behaviour | ⚠️ Partially Supported | Internet drives 92% of revenue by volume; average amounts are similar across channels |
| H4 | More add-ons selected leads to higher booking amount | ✅ Supported | 0 add-ons: ₹16,633 - 3 add-ons: ₹28,204 - ₹11,571 uplift confirmed |
| H5 | Flight day of week influences booking trends | ❌ Not Supported | No meaningful variation Mon-Fri; modest weekend premium only |

---

## 📈 Key Metrics at a Glance

| Metric | Value |
|---|---|
| Total Revenue (completed bookings) | ₹18,24,81,572 |
| Total Completed Bookings | 7,442 |
| Average Booking Value | ₹24,521 |
| Booking Completion Rate | 14.9% |
| Extra Baggage Attachment Rate | 66.9% |
| Preferred Seat Attachment Rate | 29.7% |
| In-flight Meal Attachment Rate | 42.7% |

---

## 💡 Business Recommendations

1. **Fix the booking completion rate** - only 14.9% of bookings are completed. Investigate drop-off points in the checkout funnel and implement abandoned-booking reminders. Even a 5% improvement adds ~2,500 completed bookings.
2. **Promote add-on bundles** - H4 is the only fully supported hypothesis. Bundling extra baggage, preferred seat, and in-flight meals at a small discount is a proven revenue lever with a confirmed ₹11,571 uplift per booking.
3. **Invest in Mobile conversion** - Internet drives 92% of completed bookings, yet Mobile customers pay nearly the same average amount. Improving the Mobile checkout experience would capture significant lost revenue.
4. **No day-of-week pricing needed** - H5 was not supported. A modest weekend premium exists but is not large enough to justify a dynamic pricing strategy by day of week.
5. **Fix data quality at source** - `{no data}` in `sales_channel` (59 rows), `(not set)` in `booking_origin`, and inconsistent `trip_type` values should be prevented through validated dropdowns in the booking system.

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Python | Data cleaning, EDA, hypothesis testing, visualisation |
| pandas | Data manipulation and analysis |
| numpy | Numerical operations |
| matplotlib | Chart creation |
| seaborn | Statistical visualisations |
| Jupyter Notebook | Interactive development environment |
| Microsoft Excel | Pivot tables, KPI dashboards, structured reporting |
| Tableau Desktop | Interactive dashboards and data storytelling |

---

## 📌 Notes

- All revenue analysis uses **completed bookings only** (`booking_complete = 1`, n = 7,442).
- Add-on attachment rates use the **full dataset** (n = 50,000) to capture customer intent regardless of whether the booking was completed.
- The raw CSV has 51,201 rows. The bottom 1,201 rows are empty junk rows and are removed at load time.
- Amounts are in **Indian Rupees (₹)**.
- The Tableau `.twb` file requires `Indigo_Data_Set.xlsx` in the same directory to load correctly.

---

## 👩‍💻 Author

**Vaishali Parmar**  
Data Analyst | Next Leap Program  
[GitHub](https://github.com/Vaishali-Parmar0801)

---

*This project is part of the Next Leap Data Analytics Program*
