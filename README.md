# 🛒 E-commerce Funnel Analysis Dashboard – Conversion & Drop-off Analytics

[![Power BI](https://img.shields.io/badge/Power_BI-Desktop-blue?logo=powerbi)](https://powerbi.microsoft.com/) 
[![DAX](https://img.shields.io/badge/Language-DAX-purple)](https://learn.microsoft.com/en-us/dax/)
[![Analytics](https://img.shields.io/badge/Type-Funnel_Analytics-green)](https://github.com)

A comprehensive Power BI solution built to analyze the complete customer conversion funnel from website visits to cart additions and purchases. It transforms raw customer journey data into actionable insights — empowering management to identify drop-off points, measure conversion efficiency, and optimize the customer experience at each funnel stage.

---

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Project Objective](#project-objective)
- [Dataset Description](#dataset-description)
- [Data Model Architecture](#data-model-architecture)
- [DAX Measures](#dax-measures)
- [Dashboard Features](#dashboard-features)
- [Installation & Prerequisites](#installation--prerequisites)
- [How to Use](#how-to-use)
- [Key Insights](#key-insights)
- [Tech Stack](#tech-stack)

---

## 📊 Project Overview

This Power BI project provides an **interactive view of customer conversion funnel and journey metrics** using multi-table relational data model. It enables monitoring of:

- 📈 **Funnel conversion rates** (visits to cart, cart to purchase, overall)
- 🚫 **Drop-off analysis** (stage-wise user abandonment)
- 👥 **User flow tracking** (visitors, cart users, purchase users)
- 🌍 **Regional and device insights** (conversion by region and device type)
- 💳 **Payment preferences** (payment method analysis)
- 📊 **Conversion efficiency** (stage-by-stage performance)

---

## 🎯 Project Objective

To analyze the complete customer conversion funnel and identify where potential buyers drop off:

✅ **Track stage-by-stage conversion** from visits to purchase  
✅ **Identify major drop-off points** in the funnel  
✅ **Evaluate conversion efficiency** of each stage  
✅ **Assess payment preferences** and regional differences  
✅ **Measure user flow** across funnel stages  
✅ **Data-driven optimization** for improving conversion rates  

---

## ⚙️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| **Power BI Desktop** | Data modeling, visualization, and report design |
| **DAX (Data Analysis Expressions)** | Created calculated measures and KPIs |
| **Tabular Editor** | Advanced DAX measure creation and optimization |

---

## 📋 Dataset Description

### Tables Used

**1. Customer_360_Visits**

| Column | Description |
|--------|-------------|
| **Customer_ID** | Visitor identifier |
| **Visit_ID** | Unique visit ID |
| **Visit_Date** | Date of website visit |
| **Region** | Region of the user |
| **Device** | User device type |
| **Source** | Traffic source (Organic, Paid, etc.) |

**2. Customer_360_Carts**

| Column | Description |
|--------|-------------|
| **Customer_ID** | User who added items to cart |
| **Cart_ID** | Unique cart ID |
| **Cart_Date** | Cart creation date |
| **Items_Count** | Number of items |
| **Cart_Value** | Total cart amount |
| **Converted_to_Purchase** | Whether purchase was completed |

**3. Customer_360_Purchases**

| Column | Description |
|--------|-------------|
| **Customer_ID** | Buyer identifier |
| **Purchase_ID** | Unique purchase ID |
| **Purchase_Date** | Transaction date |
| **Payment_Method** | Payment mode |
| **Items_Count** | Number of items purchased |
| **Purchase_Value** | Total order value |

**4. Supporting Tables**

- **Customers** – Contains unique customer IDs
- **DateTable** – Supports date relationships for time-based analysis
- **Funnel_Stage** – Defines funnel stages and calculates drop-offs
- **Funnel_Measures** – Stores aggregate funnel conversion KPIs

---

## 🏗️ Data Model Architecture

The dashboard uses a **multi-table relational model** connecting customer journey stages:

```
Customer_360_Visits ──→ Customers
         ↓                   ↓
    DateTable ←────────────┐ │
         ↑                   │ │
         │              Funnel_Stage
         │                   │
Customer_360_Carts ──────────┤
         ↓                    │
    DateTable ←──────────────┘
         ↑
         │
Customer_360_Purchases
         ↓
    DateTable ←──────────────┐
                              │
                        Funnel_Measures
```

---

## 🧮 DAX Measures

All measures are built using DAX calculations for real-time KPI computation.

### Stage-wise User Measures

#### 1️⃣ Total_Visit_Users
```dax
Total_Visit_Users = DISTINCTCOUNT(Customer_360_Visits[Customer_ID])
```
**Description:** Total unique visitors  
**Use Case:** Funnel base, visitor tracking

#### 2️⃣ Total_Cart_Users
```dax
Total_Cart_Users = DISTINCTCOUNT(Customer_360_Carts[Customer_ID])
```
**Description:** Total unique users who added to cart  
**Use Case:** Cart conversion tracking

#### 3️⃣ Total_Purchase_Users
```dax
Total_Purchase_Users = DISTINCTCOUNT(Customer_360_Purchases[Customer_ID])
```
**Description:** Total unique users who made a purchase  
**Use Case:** Purchase conversion tracking

### Funnel Conversion Metrics

#### 4️⃣ Visit_to_Cart_Conversion
```dax
Visit_to_Cart_Conversion = DIVIDE([Total_Cart_Users], [Total_Visit_Users], 0)
```
**Description:** % of users who added items to cart after visiting  
**Use Case:** Visit-to-cart conversion rate

#### 5️⃣ Cart_to_Purchase_Conversion
```dax
Cart_to_Purchase_Conversion = DIVIDE([Total_Purchase_Users], [Total_Cart_Users], 0)
```
**Description:** % of users who completed purchase after adding to cart  
**Use Case:** Cart-to-purchase conversion rate

#### 6️⃣ Overall_Conversion
```dax
Overall_Conversion = DIVIDE([Total_Purchase_Users], [Total_Visit_Users], 0)
```
**Description:** % of total visitors who completed purchase  
**Use Case:** Overall conversion rate

### Funnel Drop-off Metrics

#### 7️⃣ Visit_to_Cart_Dropoff
```dax
Visit_to_Cart_Dropoff = [Total_Visit_Users] - [Total_Cart_Users]
```
**Description:** Users who did not move from visit to cart stage  
**Use Case:** Drop-off volume tracking

#### 8️⃣ Visit_to_Cart_Dropoff_Percent
```dax
Visit_to_Cart_Dropoff_Percent = DIVIDE([Total_Visit_Users] - [Total_Cart_Users], [Total_Visit_Users], 0)
```
**Description:** % of visitors who didn't add to cart  
**Use Case:** Drop-off rate analysis

#### 9️⃣ Cart_to_Purchase_Dropoff
```dax
Cart_to_Purchase_Dropoff = [Total_Cart_Users] - [Total_Purchase_Users]
```
**Description:** Users who didn't purchase after cart  
**Use Case:** Cart abandonment tracking

#### 🔟 Cart_to_Purchase_Dropoff_Percent
```dax
Cart_to_Purchase_Dropoff_Percent = DIVIDE([Total_Cart_Users] - [Total_Purchase_Users], [Total_Cart_Users], 0)
```
**Description:** % of cart users who didn't purchase  
**Use Case:** Cart abandonment rate

#### 1️⃣1️⃣ Overall_Dropoff
```dax
Overall_Dropoff = [Total_Visit_Users] - [Total_Purchase_Users]
```
**Description:** Total users lost through funnel  
**Use Case:** Total drop-off volume

#### 1️⃣2️⃣ Overall_Dropoff_Percent
```dax
Overall_Dropoff_Percent = DIVIDE([Total_Visit_Users] - [Total_Purchase_Users], [Total_Visit_Users], 0)
```
**Description:** Total funnel drop-off %  
**Use Case:** Overall funnel efficiency

---

## 📈 Dashboard Features

### 1️⃣ Funnel Performance Metrics
- Total Visitors KPI card
- Total Cart Users KPI card
- Total Purchase Users KPI card
- Overall Conversion Rate metric

### 2️⃣ Conversion Analysis
- Visit-to-Cart Conversion Rate
- Cart-to-Purchase Conversion Rate
- Overall Conversion Rate
- Conversion efficiency by stage

### 3️⃣ Drop-off Analysis
- Visit-to-Cart drop-off count and percentage
- Cart-to-Purchase drop-off count and percentage
- Overall funnel drop-off count and percentage
- Stage-wise drop-off visualization

### 4️⃣ Regional & Device Insights
- Conversion by region
- Conversion by device type
- Drop-off analysis by region
- Traffic source analysis

### 5️⃣ Payment Preferences
- Purchase distribution by payment method
- Payment method performance

### 6️⃣ Interactive Filters & Slicers
- Filter by region
- Filter by device type
- Filter by traffic source
- Filter by payment method
- Filter by date range
- Cross-filtering across all visuals

---

## ⚙️ Installation & Prerequisites

### System Requirements
- **Microsoft Power BI Desktop** (Latest version recommended)
- **Windows 10** or later
- **2GB RAM** minimum (4GB+ recommended)
- **1GB** free disk space

### Installation Steps

```
1. Download Power BI Desktop
   → https://powerbi.microsoft.com/downloads/

2. Download the Dashboard File
   → E-commerce_Funnel_Analysis.pbix

3. Open Power BI Desktop

4. File → Open → Select E-commerce_Funnel_Analysis.pbix

5. Wait for data model to load

6. Click "Refresh" to load latest data
```

---

## 📊 How to Use

### Viewing the Dashboard

**Step 1: Open the File**
```
Power BI Desktop → File → Open → E-commerce_Funnel_Analysis.pbix
```

**Step 2: Navigate Between Pages**
- Switch between different dashboard views using tabs
- Each tab shows different funnel analysis perspectives

**Step 3: Use Interactive Elements**

| Feature | Action |
|---------|--------|
| **Slicers** | Click to filter by Region, Device, Source, Payment Method, Date |
| **KPI Cards** | View summary metrics at a glance |
| **Charts & Graphs** | Hover to see tooltips with detailed values |
| **Cross-Filtering** | Click on chart elements to filter other visuals |

### Interpreting the Data

- **KPI Cards** → Quick summary of funnel stage metrics
- **Funnel Charts** → Visual representation of conversion stages
- **Column Charts** → Drop-off comparison by stage
- **Line Charts** → Conversion trends over time
- **Tables** → Detailed transaction-level data

---

## 💡 Key Insights

### Conversion Analysis
✅ Track **stage-by-stage conversion** from visits to purchase  
✅ Monitor **visit-to-cart conversion rate** effectiveness  
✅ Analyze **cart-to-purchase conversion rate** performance  
✅ Measure **overall conversion rate** across all visitors  

### Drop-off Analysis
✅ Identify **major drop-off points** in the funnel  
✅ Track **visit-to-cart drop-off** volume and percentage  
✅ Measure **cart abandonment** rate  
✅ Calculate **overall funnel drop-off** percentage  

### Funnel Optimization
✅ Evaluate **conversion efficiency** of each stage  
✅ Identify **high-drop-off segments** for optimization  
✅ Assess **regional and device** impact on conversion  
✅ Analyze **payment method preferences** by region  

### Strategic Decision-Making
✅ **Optimize conversion funnel** based on drop-off insights  
✅ **Improve user experience** at critical drop-off points  
✅ **Enhance cart abandonment** recovery strategies  
✅ **Data-driven improvements** for conversion rate optimization  

---

## 🧰 Tech Stack

| Component | Technology |
|-----------|------------|
| **Platform** | Power BI Desktop |
| **Language** | DAX (Data Analysis Expressions) |
| **Data Model** | Multi-table Relational Model |
| **Data Source** | Synthetic E-commerce Data |
| **Visualization** | Power BI native visuals |

---

## 📁 Project Files

**File:** `E-commerce_Funnel_Analysis.pbix`
- Power BI report file
- Contains all data model, measures, and visuals
- Ready for immediate use
- Scalable for production environments

---

## 🎓 Learning Outcomes

By working with this dashboard, you'll gain proficiency in:

✅ **Multi-table relational data modeling** in Power BI  
✅ **DAX formula creation** for funnel and conversion metrics  
✅ **KPI development** for funnel analysis  
✅ **Interactive dashboard design** with cross-filtering  
✅ **Customer journey analytics** principles and techniques  
✅ **Drop-off analysis** and optimization strategies  
✅ **Business intelligence** fundamentals  
✅ **E-commerce analytics** best practices  

---

## 📝 Notes

- Dataset uses **synthetic E-commerce data** for demonstration/learning purposes
- All **DAX measures are production-ready** and optimized
- **Multi-table relational model** structure enables efficient funnel analysis
- **Multiple filter dimensions** allow flexible conversion analysis
- Dashboard provides actionable insights for **funnel optimization**
- **DateTable** supports time-based trend analysis

---

*This Power BI E-commerce Funnel Analysis Dashboard project demonstrates practical business intelligence expertise in conversion analytics, combining multi-table relational data modeling with DAX calculations and interactive visualizations to enable data-driven decisions through comprehensive monitoring of customer conversion funnels, drop-off points, and optimization opportunities across regions, devices, traffic sources, and payment methods.*
