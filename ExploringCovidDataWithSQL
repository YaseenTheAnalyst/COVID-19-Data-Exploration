-- EXPLORING DATA

SELECT * 
FROM CovidDeaths
WHERE continent IS NOT NULL
ORDER BY 3,4;

Select *
FROM CovidVaccinations
WHERE continent IS NOT NULL
ORDER BY 3,4;

-- SELECT DATA THAT I'M GOING TO USE

Select Location, date, total_cases, new_cases, total_deaths, population
From CovidDeaths
WHERE continent IS NOT NULL
ORDER BY 1,2;


-- LOOKING AT TOTAL CASES VS TOTAL DEATHS IN MY COUNTRY

Select Location, date, total_cases, total_deaths, (total_deaths/total_cases)*100 as Death_rate
From CovidDeaths
WHERE location like 'Egypt'
ORDER BY 1,2;


-- LOOKING AT TOTAL CASES VS POPULATION IN MY COUNTRY
-- SHOWING WHAT PERCENTAGE OF POPULATION GOT COVID

Select Location, date, total_cases, population, (total_cases/population)*100 as InfectionRate
From CovidDeaths
WHERE location like 'Egypt'
ORDER BY 1,2;


-- LOOKING AT COUNTRIES WITH HIGHEST INFECTION RATE COMPARED TO POPULATION


Select Location, MAX(total_cases) HighestInfectionCount, population, MAX((total_cases/population))*100 as InfectionRate
From CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location, population
ORDER BY 4 DESC;

-- SHOWING AT COUNTRIES WITH HIGHEST DEATH RATE COMPARED TO POPULATION

Select Location, MAX(CAST(total_deaths AS INT)) HighestDeathsCount, population, MAX((total_cases/population))*100 as Death_Rate
From CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location, population
ORDER BY 4 DESC;

-- BREAKING THINGS DOWN TO CONTINENT
-- SHOWING CONTINENTS WITH HIGHEST DEATH COUNT
Select location, MAX(CAST(total_deaths AS INT)) HighestDeathsCount
From CovidDeaths
WHERE continent IS NULL
GROUP BY location
ORDER BY 2 DESC;

-- LET'S SEE GLOBAL NUMBERS

SELECT SUM(new_cases) as total_cases, SUM(CAST(new_deaths as int)) as total_deaths, SUM(CAST(new_deaths as int))/SUM(new_cases)*100 as DeathPercentage
FROM CovidDeaths
WHERE continent IS NOT NULL
-- GROUP BY date
order by 1;


-- LOOKING AT TOTAL POPULATION VS VACCINATION

-- USING CTE

WITH popvsvac (continent, Location, Date, Population, New_Vaccinations, RollingPeopleVaccinated)
as
(
SELECT DEA.continent, DEA.location, DEA.date, DEA.population, VAC.new_vaccinations, 
SUM(CONVERT(INT, VAC.new_vaccinations)) OVER (PARTITION BY DEA.location, DEA.date) as RollingPeopleVaccinated
FROM CovidDeaths DEA
JOIN CovidVaccinations VAC
ON DEA.location = VAC.location
AND DEA.date = VAC.date
WHERE DEA.continent IS NOT NULL
)
SELECT * , (RollingPeopleVaccinated/Population)*100
FROM popvsvac;



-- CREATING VIEW TO STORE DATA FOR LATER VISUALIZATIONS

CREATE VIEW POPULATIONvsVACCINATION as
SELECT DEA.continent, DEA.location, DEA.date, DEA.population, VAC.new_vaccinations, 
SUM(CONVERT(INT, VAC.new_vaccinations)) OVER (PARTITION BY DEA.location, DEA.date) as RollingPeopleVaccinated
FROM CovidDeaths DEA
JOIN CovidVaccinations VAC
ON DEA.location = VAC.location
AND DEA.date = VAC.date
WHERE DEA.continent IS NOT NULL;


CREATE VIEW INFECTIONRATE as
Select Location, MAX(total_cases) HighestInfectionCount, population, MAX((total_cases/population))*100 as InfectionRate
From CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location, population;


CREATE VIEW DEATHRATE as
Select Location, MAX(CAST(total_deaths AS INT)) HighestDeathsCount, population, MAX((total_cases/population))*100 as Death_Rate
From CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location, population;

CREATE VIEW CONTINENTSDEATHS as
Select location, MAX(CAST(total_deaths AS INT)) HighestDeathsCount
From CovidDeaths
WHERE continent IS NULL
GROUP BY location;
