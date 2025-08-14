# Uncovering-the-World-s-Oldest-Businesses
In my latest SQL analysis project, I explored datasets from BusinessFinancing.co.uk that detail the oldest still-operating business in nearly every country.

# 📜 Analyzing the World’s Oldest Businesses – SQL Project

## 📌 Project Overview

This project explores **the oldest still-operating businesses** around the world, with a deep dive into their origins, historical resilience, and geographic distribution.  
The analysis uses datasets compiled by **BusinessFinancing.co.uk**, which lists the oldest companies in *almost* every country, along with their founding years and other details.

Using **PostgreSQL**, I explored questions about business longevity, resilience, and missing data coverage across continents.

---

## 🎯 Key Questions

- What is the **oldest business** on each continent?
- How many countries lack data on their oldest business?
- Does including newly recorded businesses change the results?

---

## 🗂️ Dataset Description

The project uses two primary datasets:

1. **`businesses`** – Contains historical businesses by country  
   - `business` – Name of the business  
   - `year_founded` – Founding year  
   - `country_code` – Country identifier  

2. **`countries`** – Metadata for countries  
   - `country` – Country name  
   - `continent` – Continent name  
   - `country_code` – Country identifier (for joins)  

An additional dataset **`new_businesses`** is also used to see if including newer companies affects the analysis.

---

## 🧪 Tools Used

- **PostgreSQL** for querying and joining datasets
- **SQL Joins, Aggregation, and Filtering** for analysis
- **DataCamp Datalab** as the execution environment

---

## 🧠 Example Queries

### Oldest Business per Continent
```sql
SELECT c1.continent, c1.country, b1.business, b1.year_founded
FROM public.businesses AS b1
INNER JOIN public.countries AS c1
ON b1.country_code = c1.country_code
WHERE (c1.continent, b1.year_founded) IN (
    SELECT c1.continent, MIN(b1.year_founded)
    FROM public.businesses AS b1
    INNER JOIN public.countries AS c1
    ON b1.country_code = c1.country_code
    GROUP BY c1.continent
)
ORDER BY b1.year_founded;

