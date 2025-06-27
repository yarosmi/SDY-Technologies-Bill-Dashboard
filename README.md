# SDY-Technologies-Bill-Dashboard
This dashboard visualizes bill data and finds top performers within Doctors, Insurance Providers and Medical Facilities.

### Table of Content
- [Project Overview](#project-overview)
- [Data Source](#data-source)
- [Tools](#tools)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Dashboard](#dashboard)
- [Results and Findings](#results-and-findings)
- [Challenges in Analysis](#challenges-in-analysis)

### Project Overview
The purpose of this dashboard is to visualize the bill data into easy to understand visualizations and find top performers within Doctors, Insurance Providers and Medical Facilities while solving data related anomalies/outliers.

### Data Source
The source for this project was provided by SDY Technologies through a MySQL database. I had a sample of their database which included tables for Bills, Doctors, Medical Facilities, Insurance Providers. **Please note:** The final dashboard will have doctor names as Doctor 1, Doctor 2... to hide the identity of real doctors due to privacy request from the client.

### Tools
The tools I used to complete this project are:
- **DBeaver Community**:
  - Access the database server.
  - Use SQL scripts to organize, find outliers, join tables and analyze data.
- **Power BI**:
  - Import tables from DBeaver through MySQL server using SQL scripts 
  - Clean data through Power Query.
  - Create measures and tables to make calculations.
  - Visualize data through tables and charts with SDY brand colors.
  - Construct the final dashboard for presentation.
  - Using final visualized data, find outliers and areas for improvement to create a narative that not only explains the dashboard but shows concrete examples and creates an impact.
- **MS Excel**:
  - Used to create a mapping table to mask real doctor names with generalized names.
 
### Data Cleaning and Preparation
  - Changed date type to MM/DD/YYYY from YYYY/MM/DD HH/MM/SS format
  - Analyzed the column quality score, column distribution and profile and adjusted values to increase score.
  - Removed duplicate values.
  - Double checked each column format to correspond with their data values.
  - Included an SQL script for each table to ignore deleted values from the database (by law, data newer than 7 years old cannot be deleted) so the tables in Power BI ignore the values that are not valid anymore. This keep the data in the charts relevant.
  - Example of one of the commands. "WHERE [table].deleted IS NULL" had to be used for every table.
```
SELECT 
bpi.bill_id AS bpi_bill_id,
bpi.insurance AS bpi_insurance,
bpi.billed AS bpi_billed,
bp.insurance_id AS bp_insurance,
i.id AS i_id,
i.name AS i_name 
FROM bill_payment_info bpi 
LEFT JOIN BillPayment bp ON bpi.bill_id = bp.id
LEFT JOIN Insurance i ON bp.insurance_id = i.id
WHERE 
    bpi.deleted IS NULL AND
    bp.deleted IS NULL AND
    i.deleted IS NULL;
```
 
### Dashboard
The SDY Technologies Dashboard can be [viewed](https://app.powerbi.com/links/vGVpHpf6hr?ctid=9bfa5866-930f-47e6-8d3c-1afc43c89e23&pbi_source=linkShare) on the official Power Bi web app viewer, and is interactable with the visualizations to filter data by Doctor, Doc. Specialties, Medical Facilities and Insurnace Providers. It contains 3 main pages that is organized by data segments and a few separate ToolTip pages that aid in providing more context in some chart visualizations.
![sdy dashbaord pg1](https://github.com/user-attachments/assets/69e9084c-abf4-4432-95ad-b2c3b24fddc4)
![sdy dashbaord pg2](https://github.com/user-attachments/assets/06adf576-c25d-42bb-b7ae-b825ca6f9507)
![sdy dashbaord pg3](https://github.com/user-attachments/assets/dd1a7316-1298-4a32-8ff3-607110c79b05)

### Results and Findings
**Bill Overview**
- DedCop accounts for 21% of the whole bill value.
- In 2021, medical bills accounted between 13-25% of adults in the US up to $10,000 balance.
- Pain Management makes up 81% of the whole bill count.
- Doctor 245 has the highest revenue.
- Top 10 doctors specialize in Pain Management, Orthopedic Surgery and Podiatry.
- Seton Medical facility income is higher than IMPC's2 locations combined.
- Highest income were covid years.

**Doctor Analysis**
- 3 doctors have more than 50% recovery rate while Doctor 439 leads with 68% recovery rate but takes 2 years to heal patients.
- Doctor 221 has a $7.7K billed/paid difference with 19 bills created.
- Doctor 479 is within the top 2 docs to heal patients in under a month.
- Doctor 245 has a $13.5M diff. between billing and paid in bills.
- Lumbar/anesthetics + C/T scans are the most common procedures.

**Insurance Providers**
- Berkshire Hathaway and Broadspire Claims have under $100 diff in Billed vs Reimbursed.
- It takes almost a month on average to reimburse bill with Packard Claims leading with 2 days.
- Intergrative Medical PC leads medical facility payout amount with 55.74% of all payouts. They have advantage of having 2 locations.
- GEICO, NY State Insurance Fund and Progressive Ins have paid out over $1M each overtime.

### Challenges in Analysis
- Not having enough data about patients was hard to analyze the Arbitration rate and DedCop % of the bill. I had to utilize extranl but reliable sources such as the USA census data and statistics from Peterson KFF Health System Tracker to get the medical debt amongst US adults numbers.
![sdy dashbaord med debt](https://github.com/user-attachments/assets/e6731182-fce4-410f-bdce-7710f4df96e6)

- The data I was working with was set between the cover-19 era and the income and bill count was skewed a lot by the pandemic cases so the whole data I was working with was made out of mostly outliers when comaring to the year before and after i had available which was pre and post cover era. Had I had more past data to work with, the visualization could be different.
- When creating table relationship between imported tabled in Power Bi, there were ambiguous paths between "BillPayment", "Cases" and "Insurance" tables. I needed BillPayment and Cases to be joined to Insurance to a few tables but there would be too many connections to the "insruance_id" column so I had to create a new table through SQL and left join "bill_payment_info", "BillPayment" and "Insurance" together so some charts would display the correct data. (I joined the new table from bill_payment_info becasue it was the fact table)
![table relationship error](https://github.com/user-attachments/assets/c241a574-5521-4dc2-a1f5-60ce72063d8f)
