🏦 Bank Loan Lending Analysis

End-to-end data analytics project analyzing 38,000+ loan records using MySQL and Power BI — covering ETL pipeline, data cleaning, SQL analysis, and interactive dashboard.

🗂️ Project Overview
This project performs a complete analysis of bank loan lending data to monitor loan performance, track KPIs, and distinguish between Good Loans (Fully Paid / Current) and Bad Loans (Charged Off).
MetricValueTotal Loan Applications38,576Total Funded Amount$435.8MTotal Amount Received$473.1MAverage Interest Rate12.05%Average DTI13.33%Good Loan Rate86.2%Bad Loan Rate13.8%

🛠️ Tech Stack
ToolPurposeMySQL 8.4Database, ETL, Data CleaningPower BIDashboard & VisualizationMicrosoft Excel / CSVRaw Data SourceSQL (DDL/DML)Data transformation & validation

🔄 Project Architecture
CSV File (Financial_Loan_Data_Import.csv)
        ↓
MySQL Database (bankloandb)
        ↓
ETL + Data Cleaning (SQL DDL/DML)
        ↓
Power BI Dashboard (3 Pages)

📁 Repository Structure
bank-loan-lending-analysis/
│
├── data/
│   └── Financial_Loan_Data_Import.csv
│
├── sql/
│   └── SQL_Script_BankLoan_LendingDataAnalysis.sql
│
├── powerbi/
│   └── BankLoan_Dashboard.pbix
│
├── assets/
│   ├── dashboard_summary.png
│   ├── dashboard_overview.png
│   └── dashboard_details.png
│
└── README.md

⚙️ ETL Pipeline
1. Data Extraction

Source: Financial_Loan_Data_Import.csv (38K+ records)
Loaded into MySQL database bankloandb → table bank_loan_data

2. Data Transformation

Converted inconsistent date formats (dd/mm/yyyy, dd-mmm-yy) to standard MySQL DATE format using DML queries
Validated imported data for accuracy and UAT

3. Data Loading

Cleaned and transformed data loaded into Power BI for visualization


🧹 Data Cleaning Steps

✅ Data Type Conversion — Ensured correct data types for all columns
✅ Date Format Standardization — Resolved multiple inconsistent date formats via SQL
✅ Missing Value Treatment — Identified and handled null/missing data points
✅ Duplicate Removal — Detected and removed duplicate records
✅ Outlier Detection — Frequency analysis on ordinal data; no significant outliers found


📈 Dashboard Pages
Page 1 — Summary

KPI cards: Total Applications, Funded Amount, Amount Received, Avg Interest Rate, Avg DTI
Good Loan vs Bad Loan donut charts
Loan Status breakdown table (MTD + MoM metrics)

Page 2 — Overview

Total Loan Applications by Month (area chart)
Geographic distribution map (Mapbox)
Loan Applications by Term, Employee Length, Purpose, Home Ownership

Page 3 — Details

Full transactional table with loan-level details
Filters: Purpose, Grade, Verification Status


🔍 Key SQL Queries Performed
sql-- Total Loan Applications
SELECT COUNT(id) AS Total_Loan_Applications FROM bank_loan_data;

-- MTD Loan Applications
SELECT COUNT(id) AS MTD_Total_Loan_Applications 
FROM bank_loan_data
WHERE MONTH(issue_date) = 12 AND YEAR(issue_date) = 2021;

-- Good Loan Percentage
SELECT
    (COUNT(CASE WHEN loan_status = 'Fully Paid' OR loan_status = 'Current' 
     THEN id END) * 100) / COUNT(id) AS Good_Loan_Percentage
FROM bank_loan_data;

-- Bad Loan Funded Amount
SELECT SUM(loan_amount) AS Bad_Loan_Funded_Amount 
FROM bank_loan_data
WHERE loan_status = 'Charged Off';

📄 Full SQL script available in /sql/ folder


📌 Key Insights

86.2% of loans are Good Loans (Fully Paid + Current) worth $370.2M
13.8% are Bad Loans (Charged Off) worth $65.5M
Loan applications peaked in Q4 with consistent MoM growth
Debt Consolidation is the most common loan purpose
Employees with 10+ years of experience have the highest loan application rate
36-month term loans dominate at 80.48% vs 60-month at 19.52%


🚀 How to Run
SQL Setup
sql-- 1. Create Database
CREATE DATABASE bankloandb;
USE bankloandb;

-- 2. Run DDL script to create table
-- (refer sql/SQL_Script_BankLoan_LendingDataAnalysis.sql)

-- 3. Import CSV data
LOAD DATA INFILE 'Financial_Loan_Data_Import.csv'
INTO TABLE bank_loan_data
FIELDS TERMINATED BY ','
IGNORE 1 ROWS;
Power BI

Open powerbi/BankLoan_Dashboard.pbix in Power BI Desktop
Update MySQL connection string with your local credentials
Refresh data

