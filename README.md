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
* *World Layoffs Project*
  - Identify the top 5 companies with the highest layoffs each year. This helps highlight major workforce reductions and trends over time.
  - Aggregates layoffs by company and year using a CTE.
  - Calculates a ranking per year with DENSE_RANK() to handle ties.
  - Filters to only include the top 5 companies per year.
     <details>
<summary>View full SQL query</summary>
#Filter the rankings by the top five per year
#Uses 2 CTEs and dense_rank
WITH company_year_cte as
(
select company, 
substring(`date`,1,4) as `YEAR`,
sum(total_laid_off) as total_off
from layoffs2
group by company, `YEAR`
), 
company_year_rank_cte as 
(select * ,
dense_rank()over(partition by `YEAR` order by total_off desc) as ranking
from company_year_cte 
where `YEAR` is not NULL

)
select * 
from company_year_rank_cte
where ranking <= 5
;
</details>

* Retail Store Sales: Dirty for Data Cleaning*
  - Ensure all Item values are complete and accurate in the Retail Store Sales table before analysis.
  - Performed a pre-update validation to check missing Item values and prepare a reference of valid items for each Category and Price_Per_Unit.
  - Used SQL joins, aggregations, and grouping to identify affected rows and safely update missing values.
  <details>
<summary>View full SQL query</summary>
  #Practice run before update
SELECT 
    target.Category,
    target.Price_Per_Unit,
    target.Item AS current_item,
    source.Item AS new_item,
    COUNT(*) AS rows_affected
FROM clean_retail_store_sales as target
JOIN (
    SELECT Category, Price_Per_Unit, Item
    FROM clean_retail_store_sales as source
    WHERE Item IS NOT NULL
    GROUP BY Category, Price_Per_Unit, Item
) source ON target.Category = source.Category 
    AND target.Price_Per_Unit = source.Price_Per_Unit
WHERE target.Item IS NULL
GROUP BY target.Category, target.Price_Per_Unit, target.Item, source.Item;

#target = the table we're updating
#source = the subquery that provides the correct Item values
#Subquery purpose is to create a list of valid Item values that are not 
#NULL for each combination of Category and Price_Per_Unit
#JOIN ON = matches rows based on Category AND Price_Per_Unit
#SET target.Item = source.Item = copies the Item from source to target
#WHERE target.Item IS NULL = only updates rows that currently have NULL#
</details>
