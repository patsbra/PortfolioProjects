**Data Analyst**

**Education**
- Database Administration Certificate  *Austin Community College (2020)*	       		
- B.S. Geology  *University of Houston (2015)* 			        		

**Certification**
- Microsoft Technology Associate - Database Fundamentals (_2019_)

**Skills**
- Technical: SQL, Excel, Power BI, Tableau, Data Governance, Database Administration
- Professional: Team Leadership, Cross-Agency Collaboration, Stakeholder Communication, Customer Service, Project Coordination

**Work Experience**

**Lead Data Analyst & Data Analyst (_2022 - 2025_)**
* *U.S Department of Agriculture*
  - Led a regional data analytics team, coordinating with national and regional offices to execute joint analysis projects and manage schedules, workloads, and work sessions.  
  - Developed Power BI dashboards monitoring Child Nutrition Program appropriations across 7 state agencies, tracking authorizations, draws, timing trends, and compliance with federal requirements.  
  - Identified and corrected data quality issues in SNAP applications, discovering 6% were misclassified, ensuring compliance with federal regulations.  
  - Created a Power BI dashboard comparing regional breastfeeding participation rates for 200+ local agencies and tribal organizations to guide outreach initiatives.  
  
**Data Analyst (_2021 - 2022_)**
* *Texas Department of Agriculture* 
  - Developed and managed 24 high-value public datasets and interactive visualizations on the Open Data Portal, enhancing transparency and accessibility.
  - Designed and implemented a data governance framework, including standardized data definitions, metadata documentation,and automated update schedules for public datasets.
 
**Corrective Action Specialist & Environmental Permit Specialist (_2016 - 2021_)**
* *Texas Commission on Environmental Quality*
  - Leveraged SQL and Excel to analyze data and create reports over three hurricane seasons, improving site assessments and accelerating emergency response efforts.
  - Improved data reliability by reviewing database structures and enforcing referential integrity across key systems.


**Projects**
  - [World Layoffs](https://github.com/patsbra/PortfolioProjects/tree/68dbc766672890b44b868275760b1b18d1c1e321/World%20Layoffs)
    -  Transformed the World Layoffs dataset into a clean, analysis-ready table by removing duplicates, standardizing fields, and handling missing values.
    -  Explored global layoffs to identify trends—ranked companies and industries by total layoffs, tracked yearly changes, and highlighted the biggest single layoff events.
 - [Retail Store Sales: Dirty for Data Cleaning](https://github.com/patsbra/PortfolioProjects/tree/f2d0e9b39a0640f409794d6cb528cb03eb3b7721/Retail%20Sales-Dirty%20Data)
   - Cleaned Retail Store Sales data—fixed invalid dates, standardized fields, resolved mismatches, and filled missing values.
   - Performed SQL-based EDA using joins, aggregates, and window functions to analyze revenue by item, location, and month, identify top-selling products, and evaluate customer spending trends.
     

**Query Highlights**

* **World Layoffs Project**
  - Identify the top 5 companies with the highest layoffs each year. Highlights major workforce reductions and trends.
  - Aggregates layoffs by company and year using a CTE.
  - Calculates a ranking per year with `DENSE_RANK()` to handle ties.
  - Filters to only include the top 5 companies per year.

  <details>
  <summary>View full SQL query</summary>

```sql
-- Identify the top 5 companies with the most layoffs each year
WITH company_year_cte AS (
    SELECT 
        company, 
        SUBSTRING(`date`, 1, 4) AS `YEAR`,
        SUM(total_laid_off) AS total_off
    FROM layoffs2
    GROUP BY company, `YEAR`
), 
company_year_rank_cte AS (
    SELECT 
        *,
        DENSE_RANK() OVER (PARTITION BY `YEAR` ORDER BY total_off DESC) AS ranking
    FROM company_year_cte
    WHERE `YEAR` IS NOT NULL
)
SELECT *
FROM company_year_rank_cte
WHERE ranking <= 5;
```
  </details>

* **Retail Store Sales: Dirty for Data Cleaning**
  - Ensure all Item values are complete and accurate before analysis.
  - Pre-update validation and data preparation for missing Item values.

  <details>
  <summary>View full SQL query</summary>

```sql
-- Preview changes and prepare valid items for update
SELECT 
    target.Category,
    target.Price_Per_Unit,
    target.Item AS current_item,
    source.Item AS new_item,
    COUNT(*) AS rows_affected
FROM clean_retail_store_sales AS target
JOIN (
    SELECT 
        Category, 
        Price_Per_Unit, 
        Item
    FROM clean_retail_store_sales AS source
    WHERE Item IS NOT NULL
    GROUP BY Category, Price_Per_Unit, Item
) AS source
    ON target.Category = source.Category
    AND target.Price_Per_Unit = source.Price_Per_Unit
WHERE target.Item IS NULL
GROUP BY 
    target.Category, 
    target.Price_Per_Unit, 
    target.Item, 
    source.Item;

-- Notes:
-- target = the table being updated
-- source = subquery providing valid Item values
-- Subquery creates a reference of non-NULL Items per Category & Price_Per_Unit
-- JOIN matches target rows to source reference
-- Only updates rows where target.Item IS NULL
```
  </details>

