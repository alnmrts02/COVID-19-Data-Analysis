# COVID 19 Data Analysis Project

## Project Overview
The COVID-19 Data Analytics Project aims to analyze and visualize COVID-19 data to uncover trends, patterns, and insights about the pandemic's impact. The analysis includes case counts, death rates and vaccination progress across different regions and time periods. The project utilizes various data analytics and visualization techniques to present the findings effectively.

## Tools Used
1. Excel
2. MS SQL server
3. Tableau 

## Process
### Data Collection
- The COVID-19 data was taken from [Our World In Data](https://ourworldindata.org/covid-deaths) website.
- The dataset was downloaded in CSV format [file]().
- The dataset was loaded into excel and the data was divided into 2 datasets [COVID Deaths]() and [COVID Vaccinations]().
- Both the datasets were loaded into Microsoft SQL Server.

### Exploratory Data Analysis
- Various EDA process was done to get insights from the data.
- Total cases vs Total Deaths
```sql
SELECT Location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 AS DeathPercentage
FROM [Covid Project]..CovidDeaths
ORDER BY 1,2;
```

- Death Percentage according to Country
```sql
SELECT Location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 AS DeathPercentage
FROM [Covid Project]..CovidDeaths
WHERE location like 'India'
ORDER BY 1,2;
```

- Total cases vs population
```sql
SELECT Location, date, total_cases, population, (total_cases/population)*100 AS PercentPopulationInfected
FROM [Covid Project]..CovidDeaths
ORDER BY 1,2;
```

- Countries with highest infection rate compared to population
```sql
SELECT Location, population, MAX(total_cases) AS HighestInfectionCount, MAX((total_cases/population))*100 AS PercentPopulationInfected
FROM [Covid Project]..CovidDeaths
GROUP BY location, population
ORDER BY PercentPopulationInfected DESC;
```

- Highest death count by location
```sql
SELECT Location, MAX(total_deaths) AS TotalDeathCount
FROM [Covid Project]..CovidDeaths
WHERE continent is not null
GROUP BY location
ORDER BY TotalDeathCount DESC;
```
  
- Highest death count by continent
```sql
SELECT continent, MAX(CAST(total_deaths AS INT)) AS TotalDeathCount
FROM [Covid Project]..CovidDeaths
WHERE continent is not null
GROUP BY continent
ORDER BY TotalDeathCount DESC;
```

- Rolling people vaccinated
```sql
SELECT cd.continent, cd.location, cd.date, cd.population, MAX(cv.total_vaccinations) as RollingPeopleVaccinated
FROM [Covid Project]..CovidDeaths cd
JOIN [Covid Project]..CovidVaccinations cv
ON cd.location = cv.location
AND cd.date = cv.date
WHERE cd.continent is not null 
GROUP BY cd.continent, cd.location, cd.date, cd.population
ORDER BY 1,2,3
```

### Data Vizualization
- After Exploratory data analysis we use the findings to graphically represent it.
- The EDA data is put in multiple excel files and all the NULL values are converted into 0, since NULL value is interpreted as a String in Tableau
- Tableau Public is used to create a dashboard. [Link]()

