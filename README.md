# COVID-19-Data-Analysis
## Global Insights into COVID-19: A Comprehensive Data Analysis

### Project Objective:
To analyze the COVID-19 pandemic's impact using publicly available data from Our World in Data. This project provides insights into global trends, regional impacts, and infection percentages to aid in understanding the spread and severity of the pandemic.

### Dataset Overview:
-	Source: Our World in Data
-	Table Name: coviddeaths
-	Columns Used:
    -continent
    -location
    -date
    -total_cases
    -total_deaths
    population

###  Analysis Goals:
#### 1.	Global Numbers:
o	Calculate total cases worldwide.
o	Calculate total deaths worldwide.
o	Calculate the death percentage worldwide 
#### 2.	Total Deaths per Continent:
o	Aggregate total deaths for each continent.
#### 3.	Percentage of Population Infected per Country:
o	Calculate the percentage of each country's population that was infected.
#### 4.	Percentage of Population Infected by Date per Continent:
o	Track how the infection percentage evolved over time for each continent.

### Tools Used:
- SQL: For data querying and aggregation.
- Tableau/Power BI: To create visualizations and dashboards.
- Excel: For quick data transformations and summaries.

### Exploratory Data Analysis (EDA) for COVID-19 Project
#### •	Check column names and data types:
- Purpose: Ensure that the structure of the coviddeaths table is correct. This step is important to understand the available columns, data types, and check if there are any missing values that need to be handled.
```
select * 
from PortofolioProject.dbo.coviddeaths;
```
#### •	Global Number
- Purpose: Provide global figures on the pandemic, including the total new cases (new_cases), total new deaths (new_deaths), and the global death percentage. This information is crucial for understanding the overall impact of the pandemic worldwide.
```
SELECT
    SUM(new_cases) AS total_case,
    SUM(CAST(new_deaths AS INT)) AS total_death,
    (SUM(CAST(new_deaths AS INT)) * 1.0 / NULLIF(SUM(new_cases), 0)) * 100 AS deathpercentage
FROM PortofolioProject.dbo.coviddeaths
WHERE continent IS NOT NULL
ORDER BY total_case;
```
#### •	Looking total cases vs total deaths and percentage
- Purpose: Calculate the total cases and total deaths for each location and date, and compute the death percentage. This percentage shows how severe the outbreak is in a specific location during a given period.
```
SELECT 
    location, 
    date, 
    total_cases, 
    total_deaths, 
    CASE 
        WHEN total_cases = 0 THEN 0 
        ELSE (total_deaths * 100.0 / total_cases) 
    END AS deathpercentage
FROM 
    PortofolioProject.dbo.coviddeaths
ORDER BY 
   location, date;
```
#### •	Looking total population vs total deaths and percentage
- Purpose: Measure the percentage of the population infected with COVID-19 (case_percentage) based on the total population in each location. This helps understand the spread of the disease across different regions.
```
select
		location, 
   		date, 
		population,
   		total_cases,  
    	CASE 
        WHEN total_cases = 0 THEN 0 
        ELSE (total_cases * 100.0 / population) 
    		END AS casepercentage
FROM 
    		PortofolioProject.dbo.coviddeaths
ORDER BY location, date;
```
#### •	Looking at countries with highest infection rated compared to population 
- Purpose: Identify countries or regions with the highest infection rates relative to their total population. This provides insights into areas that are most severely impacted by the pandemic in terms of case numbers compared to population size.
  ```
  select
	location,
	population,
	date,
	max(total_cases) as higehstinfectioncount,
	max(total_cases/population)*100 as percentpopulationinfected
  from  PortofolioProject.dbo.coviddeaths
  group by location, population, date 
  order by percentpopulationinfected desc;
  ```
#### •	Showing countries with highest death count per population
- Purpose: Find countries or regions with the highest death counts relative to their total population. This percentage highlights the deadliness of the pandemic in each location.
```
  select
	location,
	population,
	max(total_deaths) as totaldeath,
	max(total_deaths/population)*100 as percentpopulationdeath
  from  PortofolioProject.dbo.coviddeaths
  where continent is not null
  group by location, population 
  order by totaldeath desc
  ```
## Findings and Results
### Global Numbers
![GLOBAL NUMBER](https://github.com/user-attachments/assets/ce532909-4436-4201-86d2-87a5cb062232)

Insight: The global pandemic has resulted in nearly 776 million confirmed cases of COVID-19, with a total death toll of approximately 7.06 million, reflecting a global death rate of around 1%. This highlights the severe impact of the pandemic worldwide, with 1 in every 100 confirmed cases resulting in death.
### Total Deaths per Continent
![Total Death Count per Continent](https://github.com/user-attachments/assets/9ca2b891-6b09-4ddd-99ae-ad3a42aba536)

Insight:
•	Europe has the highest death toll, surpassing 2.1 million deaths, followed by North America (1.67 million) and Asia (1.64 million).
•	Africa and Oceania have comparatively lower death counts, with Africa having around 259,000 deaths and Oceania only 32,918.
•	The distribution of deaths highlights regional disparities, with continents like Europe and North America being disproportionately affected by the pandemic's fatality.
### Percentage of Population Infected per Country
![Percent Population Infected per Country](https://github.com/user-attachments/assets/85d0562a-d37b-42cc-bd47-3db9c827d023)

Insight:
•	The map visualization indicates that European countries are the most affected, with several nations experiencing high infection rates compared to their population. This suggests that Europe had some of the highest infection rates during the peak of the pandemic.
•	Countries with higher population densities and major urban centers were particularly impacted, explaining the dominance of Europe in infection rates.
### Percent Population Infected by Date per Continent
![Percent Population Infected by Date per Continent](https://github.com/user-attachments/assets/fa94b486-6cd4-4060-b4f5-270564783939)

Insight:
•	Oceania experienced a significant spike in early 2021, reflecting the rapid spread of COVID-19 in countries like Australia and New Zealand, which had relatively lower infection rates earlier in the pandemic.
•	The infection rates over time show a consistent upward trend across all continents, with notable surges in late 2020 and 2021.
•	The order of continents based on the highest percentage of the population infected is:
1.	Europe
2.	Oceania
3.	North America
4.	South America
5.	South Africa
6.	Asia
7.	Africa

This indicates that Europe had the highest proportion of its population infected, followed by Oceania, which saw rapid growth in infection rates after initial lower numbers.













