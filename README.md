# market-solution-Power-bi-Script-Python-

# 📊 Customer Support Dashboard in Power BI

This project delivers a comprehensive **Power BI report** to analyze customer support data. The report is based on a dataset obtained from Kaggle ([Ecommerce Customer Service Satisfaction](https://www.kaggle.com/datasets/ddosad/ecommerce-customer-service-satisfaction)) and enriched with custom Python scripts and a calculated calendar table. The report focuses on performance metrics, team evaluations, and contact category insights. Below is a detailed overview.

---

## 🗂️ Project Structure

### 1. **Input Data**  
- **Source:** Kaggle dataset ([link](https://www.kaggle.com/datasets/ddosad/ecommerce-customer-service-satisfaction)).  
- **File Format:** Single Excel file.  
- **Data Transformations:** Python script executed within Power BI cleansed and formatted the data.  
  - Removed null values from date fields.
  - Standardized column names to lowercase and replaced spaces with underscores.
  - Converted columns to appropriate data types (e.g., `datetime`, `numeric`).
 


### 2. **Data Model**  
The report consists of two interconnected tables:  
1. **Customer Support Data:** Main dataset.  
2. **Calendar Table:** Custom table created to enable time-based filtering (day, week, month, year).  
   - Generated using DAX functions in Power BI.  
   - Includes calculated columns for `Week Number`, `Year`, `Month`, etc.  

> **Note:** Due to the limited scope of the dataset, a star schema was not implemented.

---

## 🎥 Report Overview

The report is divided into four key sections, each accessible through a **Landing Page** with video tutorials.  

### 4. **Landing Page**  
**Purpose:** Serve as an entry point to navigate the report.  
- Includes descriptions of each page to guide the user.  
- Quick links to all pages for easy access.


### 2. **Performance Overview**  
**Purpose:** Provide a summary of the main KPIs.  
- **KPIs:**  
  - **AHT Issues:** Average handling time for customer issues.  
  - **Resolved Issues % Goal:** Percentage of resolved issues meeting predefined goals.  
  - **Number of Orders Handled:** Total orders processed.  
  - **CSAT Score:** Customer satisfaction score.  
- **Filters:**  
  - Time-based filters (Year, Month, Day).  
  - Channel filter (**Channel Name**).  
- **Visualizations:**  
  - Weekly comparison of **Number of Orders Handled** vs. **CSAT Score**.  
  - Multi-selection slicer for employees by shift (also acts as a filter).  



https://github.com/user-attachments/assets/2ab7bea4-8768-4d68-a668-c7fa5074020a



### 3. **Team Performance**  
**Purpose:** Evaluate team performance at different organizational levels.  
- **KPIs:** Consistent with **Performance Overview**.  
- **Focus:**  
  - Analysis by **Manager**, **Supervisor**, and **Agent**.  
- **Visualizations:**  
  - Table summarizing KPIs by individual/team.  
  - Line charts showing trends over time for each hierarchical level.
 
      


https://github.com/user-attachments/assets/9bfb1477-b1bd-48e1-838e-d00ef573d09c


### 4. **Contact Category Insights**  
**Purpose:** Examine KPIs based on customer-selected categories and subcategories.  
- **KPIs:** Same as **Performance Overview**.  
- **Focus:**  
  - Understanding performance across different contact categories.  
- **Visualizations:**  
  - Stacked bar charts and detailed tables displaying KPIs segmented by category and subcategory.  



https://github.com/user-attachments/assets/412a62fd-5074-4950-bc86-f0ebf16674c3


---

## 💻 Python Script for Data Transformation  

Below is the Python script used in Power BI for data cleansing:  

```python
import pandas as pd

# Data cleansing steps

# Replace spaces in column names with underscores (_)
dataset.columns = dataset.columns.str.replace(' ', '_')

# Convert all column names to lowercase
dataset.columns = dataset.columns.str.lower()

# Remove spaces from the beginning and end of column names
dataset.columns = dataset.columns.str.strip()

# Drop rows where any of the specified date columns are null
dataset.dropna(subset=['order_date_time', 'issue_responded', 'survey_response_date', 'issue_reported_at'], inplace=True)

# Specify date columns to process
date_columns = ['order_date_time', 'issue_responded', 'survey_response_date', 'issue_reported_at']
for col in date_columns:
    if col in dataset.columns:
        # Convert to datetime
        dataset[col] = pd.to_datetime(dataset[col], errors='coerce')
        # Convert back to string, replace nulls with empty strings
        dataset[col] = dataset[col].dt.strftime('%d/%m/%Y %H:%M:%S').fillna('')

# Fill missing values with 0
dataset['item_price'].fillna(0, inplace=True)

# Convert numeric columns
if 'item_price' in dataset.columns:
    dataset['item_price'] = pd.to_numeric(dataset['item_price'], errors='coerce')
    # Format numbers with commas as decimal separators
    dataset['item_price'] = dataset['item_price'].apply(
        lambda x: f"{x:,.2f}".replace(",", "X").replace(".", ",").replace("X", ".") if pd.notnull(x) else x
    )
