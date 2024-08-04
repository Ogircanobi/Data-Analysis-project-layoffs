# Global Layoff Data Analysis: Exploratory Data Analysis (EDA)

## Project Overview

This project focuses on performing exploratory data analysis (EDA) on the cleaned layoff data from the previous project. Using MySQL, I analyzed various aspects of global layoffs during the COVID-19 pandemic to uncover key trends and insights. This analysis provides a deeper understanding of the impact of layoffs across different dimensions, such as companies, industries, countries, and time periods.

## Table of Contents
1. [Introduction](#introduction)
2. [Exploratory Data Analysis](#exploratory-data-analysis)
    - [Maximum Layoffs and Percentage Laid Off](#maximum-layoffs-and-percentage-laid-off)
    - [Companies with Complete Layoffs](#companies-with-complete-layoffs)
    - [Companies with Most Total Layoffs](#companies-with-most-total-layoffs)
    - [Date Range of Data](#date-range-of-data)
    - [Most Affected Industries](#most-affected-industries)
    - [Total Layoffs per Country](#total-layoffs-per-country)
    - [Global Layoffs per Year](#global-layoffs-per-year)
    - [Layoffs per Company Stage](#layoffs-per-company-stage)
    - [Monthly Layoffs](#monthly-layoffs)
    - [Rolling Sum of Global Layoffs](#rolling-sum-of-global-layoffs)
    - [Layoffs by Company and Year](#layoffs-by-company-and-year)
    - [Top Five Companies per Year by Layoffs](#top-five-companies-per-year-by-layoffs)
3. [Technologies Used](#technologies-used)
4. [Getting Started](#getting-started)
5. [Conclusion](#conclusion)

## Introduction

In this project, I conducted a comprehensive exploratory data analysis (EDA) on the layoff data that was cleaned and prepared in the previous project. The analysis aims to provide actionable insights into the layoff trends during the COVID-19 pandemic by exploring various dimensions such as company performance, industry impact, regional effects, and temporal patterns.

## Exploratory Data Analysis

### Maximum Layoffs and Percentage Laid Off
```sql
SELECT MAX(total_laid_off), MAX(percentage_laid_off)
FROM layoffs_staging2;
```

### Companies with Complete Layoffs
```sql
SELECT * 
FROM layoffs_staging2
WHERE percentage_laid_off = 1
ORDER BY funds_raised_millions DESC;
```

### Companies with Most Total Layoffs
```sql
SELECT company, SUM(total_laid_off) 
FROM layoffs_staging2
GROUP BY company
ORDER BY 2 DESC;
```

### Date Range of Data
```sql
SELECT MIN(`date`), MAX(`date`)
FROM layoffs_staging2;
```

### Most Affected Industries
```sql
SELECT industry, SUM(total_laid_off) 
FROM layoffs_staging2
GROUP BY industry
ORDER BY 2 DESC;
```

### Total Layoffs per Country
```sql
SELECT country, SUM(total_laid_off) 
FROM layoffs_staging2
GROUP BY country
ORDER BY 2 DESC;
```

### Global Layoffs per Year
```sql
SELECT YEAR(`date`), SUM(total_laid_off) 
FROM layoffs_staging2
GROUP BY YEAR(`date`)
ORDER BY 1 DESC;
```

### Layoffs per Company Stage
```sql
SELECT stage, SUM(total_laid_off) 
FROM layoffs_staging2
GROUP BY stage
ORDER BY 2 DESC;
```

### Monthly Layoffs
```sql
SELECT SUBSTRING(`date`, 1,7) AS `MONTH`, SUM(total_laid_off)
FROM layoffs_staging2
WHERE SUBSTRING(`date`, 1,7) IS NOT NULL
GROUP BY `MONTH`
ORDER BY 1 ASC;
```

### Rolling Sum of Global Layoffs
```sql
WITH Rolling_Total AS
(
    SELECT SUBSTRING(`date`, 1,7) AS `MONTH`, SUM(total_laid_off) AS total_off
    FROM layoffs_staging2
    WHERE SUBSTRING(`date`, 1,7) IS NOT NULL
    GROUP BY `MONTH`
    ORDER BY 1 ASC
)
SELECT `MONTH`, total_off, 
SUM(total_off) OVER(ORDER BY `MONTH`) AS rolling_total
FROM Rolling_Total;
```

### Layoffs by Company and Year
```sql
SELECT company, YEAR(`date`), SUM(total_laid_off) 
FROM layoffs_staging2
GROUP BY company, YEAR(`date`)
ORDER BY 3 DESC;
```

### Top Five Companies per Year by Layoffs
```sql
WITH Company_year AS
(
    SELECT company, YEAR(`date`), SUM(total_laid_off) 
    FROM layoffs_staging2
    GROUP BY company, YEAR(`date`)
    ORDER BY 3 DESC
),
Company_Year_Rank AS
(
    SELECT *, 
    DENSE_RANK() OVER (PARTITION BY years ORDER BY total_laid_off DESC) AS Ranking
    FROM Company_year
    WHERE years IS NOT NULL
)
SELECT * 
FROM Company_Year_Rank
WHERE Ranking <= 5;
```

## Technologies Used

- **MySQL**: Utilized for querying and analyzing the cleaned layoff data.

## Getting Started

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/yourusername/layoff-data-analysis.git
   ```

2. **Set Up the Database:**
   - Import the `layoffs_staging2` table into your MySQL server if not already available.

3. **Run the Analysis Queries:**
   - Execute the provided SQL queries to perform exploratory data analysis on the `layoffs_staging2` table.

## Conclusion

This exploratory data analysis project showcases my ability to derive meaningful insights from complex datasets. By analyzing global layoff trends across various dimensions, I demonstrated my analytical skills and proficiency in using MySQL to handle and explore large-scale data.

---
