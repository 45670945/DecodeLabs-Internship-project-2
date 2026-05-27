# 📊 DecodeLabs — Data Analytics Project 2
### Exploratory Data Analysis (EDA) | Industrial Training Kit | Batch 2026

<br>

> **"You are the translator between data and decision. Transform a static table of numbers into meaningful insights through pure analytical logic."**
> — DecodeLabs

<br>

---

## 📋 Project Overview

This project demonstrates **Exploratory Data Analysis (EDA)** on a real-world e-commerce orders dataset. Using Python (Pandas, Matplotlib, Seaborn) in Google Colab, the dataset was analyzed to uncover hidden patterns, trends, distributions, and outliers — converting raw numbers into actionable business intelligence.

| Detail | Info |
|--------|------|
| 🏢 **Organization** | DecodeLabs |
| 📦 **Project** | Project 2 — Exploratory Data Analysis (EDA) |
| 🐍 **Tools** | Python — Pandas, NumPy, Matplotlib, Seaborn |
| 🌐 **Environment** | Google Colab |
| 📊 **Dataset** | E-commerce Orders — 1,200 rows, 14 columns |
| 🔍 **Analysis Areas** | Revenue, Payment, Order Status, Referral, Trends, Outliers |
| 📅 **Batch** | 2026 |

---

## 📁 Repository Structure

```
decodelabs-data-analytics-project2/
│
├── 📄 README.md                               ← You are here
├── 📓 Decodelabs_Project_2.ipynb              ← EDA notebook (Google Colab)
└── 📊 Dataset_for_Data_Analytics.xlsx         ← E-commerce orders dataset
```

---

## 🗂️ Dataset Overview

**File:** `Dataset_for_Data_Analytics.xlsx` — **1,200 rows × 14 columns**

| Column | Type | Description |
|--------|------|-------------|
| `OrderID` | object | Unique order identifier |
| `Date` | datetime64 | Order date (2023–2025) |
| `CustomerID` | object | Unique customer ID |
| `Product` | object | Laptop, Phone, Tablet, Monitor, Printer, Desk, Chair |
| `Quantity` | int64 | Units ordered (1–5) |
| `UnitPrice` | float64 | Price per unit (11.39–699.93) |
| `ShippingAddress` | object | Delivery address |
| `PaymentMethod` | object | Cash, Credit Card, Debit Card, Gift Card, Online |
| `OrderStatus` | object | Delivered, Shipped, Pending, Cancelled, Returned |
| `TrackingNumber` | object | Shipment tracking code |
| `ItemsInCart` | int64 | Cart size at checkout (1–10) |
| `CouponCode` | object | SAVE10, FREESHIP, WINTER15 (309 nulls → No Coupon) |
| `ReferralSource` | object | Google, Facebook, Instagram, Email, Referral |
| `TotalPrice` | float64 | Final order value |

---

## 🔢 Descriptive Statistics

| Metric | Quantity | UnitPrice | TotalPrice |
|--------|----------|-----------|------------|
| **Count** | 1,200 | 1,200 | 1,200 |
| **Mean** | 2.95 | 356.41 | 1,053.97 |
| **Median** | 3.00 | 364.21 | 823.62 |
| **Std Dev** | 1.41 | 197.18 | 819.86 |
| **Min** | 1.00 | 11.39 | 11.39 |
| **Max** | 5.00 | 699.93 | 3,456.40 |

> 💡 **Insight:** Mean TotalPrice (1,053.97) is significantly higher than Median (823.62), indicating the data is **right-skewed** — a few high-value orders are pulling the average up.

---

## 📊 EDA — Analysis Sections

### 1️⃣ Product Revenue Analysis

```python
product_sales = df.groupby('Product')['TotalPrice'].sum().sort_values(ascending=False)
product_sales.plot(kind='bar')
```

| Product | Total Revenue |
|---------|--------------|
| 🥇 Chair | $195,620.11 |
| 🥈 Printer | $195,612.61 |
| 🥉 Laptop | $192,126.56 |
| Tablet | $186,568.95 |
| Monitor | $175,651.41 |
| Desk | $167,459.93 |
| Phone | $151,722.39 |

> 💡 **Insight:** Revenue is fairly evenly distributed across all products. Chair and Printer are the top earners, while Phone generates the least revenue — worth investigating with targeted promotions.

---

### 2️⃣ Payment Method Analysis

```python
payment = df['PaymentMethod'].value_counts()
payment.plot(kind='pie', autopct='%1.1f%%')
```

| Payment Method | Count |
|---------------|-------|
| Online | 258 (21.5%) |
| Cash | 246 (20.5%) |
| Credit Card | 234 (19.5%) |
| Debit Card | 232 (19.3%) |
| Gift Card | 230 (19.2%) |

> 💡 **Insight:** Payment methods are **almost equally distributed**, meaning no single payment type dominates. Online payment leads slightly, suggesting customers prefer digital transactions.

---

### 3️⃣ Order Status Analysis

```python
status = df['OrderStatus'].value_counts()
sns.countplot(x='OrderStatus', data=df)
```

| Order Status | Count |
|-------------|-------|
| Cancelled | 250 (20.8%) |
| Returned | 247 (20.6%) |
| Pending | 237 (19.8%) |
| Shipped | 235 (19.6%) |
| Delivered | 231 (19.3%) |

> 💡 **Insight:** ⚠️ **Critical Finding** — Only **19.3% of orders are Delivered**. Cancelled + Returned orders account for **41.4%** of all orders. This is a significant business problem requiring immediate attention.

---

### 4️⃣ Referral Source Analysis

```python
referral = df['ReferralSource'].value_counts()
```

| Referral Source | Count |
|----------------|-------|
| Instagram | 259 (21.6%) |
| Email | 250 (20.8%) |
| Google | 241 (20.1%) |
| Facebook | 228 (19.0%) |
| Referral | 222 (18.5%) |

> 💡 **Insight:** Instagram drives the most traffic, but only marginally. All channels are nearly equal — the business has a **healthy multi-channel acquisition strategy**.

---

### 5️⃣ Monthly Sales Trend

```python
df['Date'] = pd.to_datetime(df['Date'])
monthly_sales = df.groupby(df['Date'].dt.to_period('M'))['TotalPrice'].sum()
monthly_sales.plot(figsize=(10,5))
```

| Top Month | Revenue |
|-----------|---------|
| 🥇 June 2024 | $68,068.54 |
| 🥈 May 2023 | $63,836.84 |
| 🥉 January 2023 | $56,685.75 |

> 💡 **Insight:** June 2024 is the peak revenue month. Monthly revenue fluctuates across the 2023–2025 range — seasonal patterns can be explored for promotional planning.

---

### 6️⃣ Outlier Detection

```python
sns.boxplot(x=df['TotalPrice'])
```

| Metric | Value |
|--------|-------|
| Q1 (25th percentile) | $410.52 |
| Q3 (75th percentile) | $1,578.48 |
| IQR | $1,167.96 |
| Lower Bound | -$1,341.42 |
| Upper Bound | $3,330.42 |
| **Outliers Detected** | **8 orders** |

> 💡 **Insight:** 8 high-value orders exceed the IQR upper bound ($3,330). These are likely VIP or bulk orders — they are **signals, not noise**, and should be investigated for upselling opportunities.

---

## 🔗 Correlation Analysis

| | Quantity | UnitPrice | TotalPrice | ItemsInCart |
|--|----------|-----------|------------|-------------|
| **Quantity** | 1.00 | 0.01 | **0.62** | **0.65** |
| **UnitPrice** | 0.01 | 1.00 | **0.72** | 0.00 |
| **TotalPrice** | **0.62** | **0.72** | 1.00 | 0.39 |
| **ItemsInCart** | **0.65** | 0.00 | 0.39 | 1.00 |

> 💡 **Key Correlations:**
> - **UnitPrice → TotalPrice (0.72):** Strong positive — higher-priced items drive more revenue
> - **Quantity → TotalPrice (0.62):** Strong positive — bulk orders significantly increase revenue
> - **Quantity → ItemsInCart (0.65):** Customers who order more units also browse more items

---

## 🧠 Key Business Insights Summary

| # | Finding | Business Action |
|---|---------|----------------|
| 1 | Only **19.3% of orders Delivered** | Investigate logistics & fulfilment issues |
| 2 | **41.4% Cancelled + Returned** | Identify root causes — product quality, delivery delays |
| 3 | **Chair & Printer** are top revenue products | Prioritize stock and marketing for these |
| 4 | **Phone** generates least revenue | Consider promotions or bundling |
| 5 | **June 2024** is peak revenue month | Plan seasonal campaigns around this period |
| 6 | **Instagram** is top referral source | Increase Instagram marketing budget |
| 7 | **8 high-value outlier orders** | Identify VIP customers for loyalty programs |
| 8 | **UnitPrice has 0.72 correlation with Revenue** | Focus on premium product listings |

---

## 🚀 How to Run This Notebook

### Option A — Google Colab (Recommended)

1. Go to [colab.research.google.com](https://colab.research.google.com)
2. **File → Upload notebook** → select `Decodelabs_Project_2.ipynb`
3. Click 📁 **Files** (left panel) → **Upload** → select `Dataset_for_Data_Analytics.xlsx`
4. Click **Runtime → Run all** (`Ctrl + F9`)
5. All charts and outputs render inline

### Option B — Jupyter Notebook (Local)

```bash
pip install pandas numpy matplotlib seaborn openpyxl

jupyter notebook Decodelabs_Project_2.ipynb
```

> Update the file path in the load step:
> ```python
> df = pd.read_excel("Dataset_for_Data_Analytics.xlsx")
> ```

---

## 🧠 Python Libraries Used

| Library | Purpose |
|---------|---------|
| `pandas` | Data loading, groupby, aggregation |
| `numpy` | Numerical operations |
| `matplotlib` | Bar charts, line charts, pie charts |
| `seaborn` | Count plots, boxplots |

---

## 📈 Charts Produced

| Chart | Type | Insight |
|-------|------|---------|
| Revenue by Product | Bar Chart | Chair & Printer lead |
| Payment Method Distribution | Pie Chart | Equal distribution across methods |
| Order Status Count | Count Plot | High cancellation rate |
| Monthly Revenue Trend | Line Chart | June 2024 peak |
| TotalPrice Outliers | Box Plot | 8 high-value orders |

---

## ✅ Project Requirements — Verified

| DecodeLabs Requirement | Method Used | Status |
|----------------------|-------------|--------|
| Calculate mean, median, count | `df.describe()`, `df.info()` | ✅ Done |
| Identify trends | Monthly sales line chart | ✅ Done |
| Identify outliers | IQR boxplot — 8 found | ✅ Done |
| Summarize key observations | Business Insights table (8 findings) | ✅ Done |
| Visualizations produced | 5 charts (bar, pie, count, line, box) | ✅ Done |

---

## ✅ Submission Checklist

- [x] Code is working properly
- [x] Project files are complete
- [x] GitHub Repository created
- [x] README file added
- [x] Screenshots / Documentation prepared
- [x] Final project tested properly
- [x] All key requirements from DecodeLabs brief met

---

## 🛠️ Tools Used

![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C?style=for-the-badge&logo=python&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-4C72B0?style=for-the-badge&logo=python&logoColor=white)
![Google Colab](https://img.shields.io/badge/Google_Colab-F9AB00?style=for-the-badge&logo=googlecolab&logoColor=black)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)

---

## 👨‍💻 Author

| | |
|--|--|
| 👤 **Name** | Thilani Senarath |
| 💼 **Role** | Data Analytics Intern |
| 🏢 **Organization** | DecodeLabs |
| 📅 **Batch** | 2026 |

---

## 📜 License

This project is created for **educational and learning purposes only**.

All code, analysis, and documentation in this repository are part of the DecodeLabs Industrial Training Program and are intended solely for skill development and portfolio building.

---

## 📞 About DecodeLabs

| | |
|--|--|
| 🌐 Website | [www.decodelabs.tech](https://www.decodelabs.tech) |
| 📧 Email | decodelabs.tech@gmail.com |
| 📞 Phone | +91 89330 06408 |
| 📍 Location | Greater Lucknow, India |

---

<div align="center">

**DecodeLabs Industrial Training Kit — Batch 2026**

*"Your journey to becoming a professional analyst begins right here, right now, with the very first pattern you discover today."*

</div>



