# PROJECT OVERVIEW
![etl_pipeline_for_data_warehouse](https://github.com/user-attachments/assets/5c13b9e0-396d-420b-94bd-3862373bfbd3)

## Project Title: Oil Data Warehouse Project
### Description
This project involves designing and building a **data warehouse for Oil financial and sales data**. The source data comes from an MS Access database, staged in SQL Server, transformed, and loaded into dimensional and fact tables using SSIS. Incremental loading and Slowly Changing Dimensions (SCD) logic were applied to maintain historical accuracy. The ETL process is automated via SQL Server Agent for daily execution.

## Tools & Technologies
- **Database:** SQL Server (SSMS)  
- **ETL:** SSIS (Lookup, Conditional Split, Derived Column, Row Count)  
- **Scheduling:** SQL Server Agent  
- **Version Control:** GitHub  
- **Languages:** SQL, T-SQL  
- **Source Database:** MS Access  

## Project Workflow

### 1. Data Source (MS Access)
Oil financial and sales data is profiled and extracted from MS Access.

### 2. Staging Database
Staging tables in SQL Server mirror the source Oil data for easier transformation.  
![image](https://github.com/user-attachments/assets/777057bf-b70d-4263-8b11-745e8031b8e4)

### 3. Dimension & Fact Tables
- **Dimensions:** Company_Dim, Currency_Dim, Date_Dim, Product_Dim (Oil products, companies, etc.)  
- **Fact Tables:** Fact_Finance, Fact_Sales (Oil financial transactions, sales volumes)  
![image](https://github.com/user-attachments/assets/e632e50a-fa70-4a36-a7c3-6e44c24a1ecd)

### 4. Incremental Loads & SCD
Lookups identify new/updated records. SCD Type 1 (overwrite) and Type 2 (historical) logic applied.  
![image](https://github.com/user-attachments/assets/0bc371f7-ae8c-4bc0-ab3d-789d474a2e9a)

### 5. Event Handling
SSIS logs errors, warnings, and execution events in `event_ssis` for debugging.  
![image](https://github.com/user-attachments/assets/914af0d6-0c3d-49e7-846b-2c26b6c1cb21)

### 6. Deployment to SSISDB
- Enabled SSISDB and deployed packages using SSDT Project Deployment Model  
- Configured environment variables for dev/prod connections  
![image](https://github.com/user-attachments/assets/972b6d68-f920-4717-95d0-e77c5bb8ddf9)  
![image](https://github.com/user-attachments/assets/3d969ad2-a02f-4f25-9e99-1239ab9eff86)

### 7. Execution & Monitoring
- **Execution View:** Tracks status, errors  
- **Performance View:** Identifies bottlenecks  
![image](https://github.com/user-attachments/assets/478dafdb-fe06-4dfd-a879-ab226ceb3f7c)  
![image](https://github.com/user-attachments/assets/5fe4a816-2fd2-4722-a851-6430ad661e31)

### 8. SQL Server Agent Job
Scheduled daily ETL execution at 1 PM.  
![image](https://github.com/user-attachments/assets/d28cd680-26b8-4861-a0fd-4a7e949ccb65)  
![image](https://github.com/user-attachments/assets/201912f2-dc3c-4ac5-a452-4956abf49ebe)

## Data Warehouse Design
- **Star Schema:** Fact tables store Oil transactions; dimension tables provide context  
- **Dimensions:** Company_Dim, Product_Dim (Oil products), Currency_Dim, Date_Dim (SCD Type 1 & 2)  
- **Fact Tables:** Fact_Finance, Fact_Sales (transaction-level granularity for Oil financials and sales)  
- **Normalization:** Staging normalized; DW denormalized for query efficiency  
- **Surrogate Keys:** Auto-increment PKs; natural keys retained for lookups  
- **Hierarchies:** Year, Quarter, Month, Day in Date_Dim  
![image](https://github.com/user-attachments/assets/c25fff28-4f32-4d94-a033-047d9e2f31d1)

## ETL Pipelines
### Phases
1. **Extract:** Pull Oil financial and sales data from Access to staging  
2. **Transform:** Clean, convert types, apply business rules, perform lookups  
3. **Load:** Insert into dimensions/facts, maintain SCDs, ensure referential integrity  

### Challenges & Solutions
- **Data Quality:** Handled nulls, duplicates, inconsistent formats via SSIS Derived Columns & Conditional Splits  
- **SCD Handling:** Type 1 overwrites, Type 2 creates historical records  
- **Performance:** Partial caching, indexing, parallel execution  
- **Date Formats:** `TRY_PARSE`/`TRY_CONVERT` for consistent DATETIME  
- **Incremental Loading:** LoadControl table tracks la
