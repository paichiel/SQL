# SQL


-- DATA CLEANING

 -- 1. Remove Duplicates
 -- 2. Standardize the Date
 -- 3. Null values or blank values 
 -- 4. Remove Any Columns


select *
from layoffs_raw
;

create table layoffs_stage
like layoffs_raw
;


select *
from layoffs_stage
;


insert layoffs_stage
select *
from layoffs_raw
;

select *
from layoffs_stage;

-- Duplicates
 
 with duplicate_cte as
 (
 select *,
row_number() over(
partition by Company, location, Industry, total_laid_Off, Percentage_laid_off, `date`, stage,
 country, funds_raised_millions) as row_num
 from layoffs_stage
 )
 select *
 from duplicate_cte
 where row_num > 1;



select *
from layoffs_stage
where company = 'casper';



with duplicate_cte as
 (
 select *,
row_number() over(
partition by Company, location, Industry, total_laid_Off, Percentage_laid_off, `date`, stage,
 country, funds_raised_millions) as row_num
 from layoffs_stage
 )
delete
 from duplicate_cte
 where row_num > 1;
 
 create table `layoffs_stage2` (
 `company` text,
 `location` text,
 `industry` text,
 `total_laid_off` int default null,
 `percentage_laid_off` text,
 `date` text,
 `stage` text,
 `country` text,
`funds_raised_millions` int default null,
`row_num` int
)ENGINE=INnoDB default charset=utf8mb4 collate=utf8mb4_0900_ai_ci;




select *
from layoffs_stage2;


select *
from layoffs_stage2;

-- INSERT

 
insert into layoffs_stage2
 select *,
row_number() over(
partition by Company, location, Industry, total_laid_Off, Percentage_laid_off, `date`, stage,
 country, funds_raised_millions) as row_num
 from layoffs_stage
;

-- DELETE DUPLICATE
delete
from layoffs_stage2
where row_num > 1;


select *
from layoffs_stage2;

-- STANDARDIZING DATA

select company, trim(company)
from layoffs_stage2;

update layoffs_stage2
set company = trim(company);

select distinct industry
from layoffs_stage2
order by 1
;

-- what we did here is, we had both crypto and 
-- crypto currency as is industry name but we only need one, so we changed it to just crypto.

select *
from layoffs_stage2
where industry like 'crypto%'
;

update layoffs_stage2
set industry = 'crypto'
where industry like 'crypto%';


select distinct location
from layoffs_stage2
;

select distinct industry
from layoffs_stage2
;

select distinct country
from layoffs_stage2
;


select *
from layoffs_stage2
where country like 'united state%'
order by 1;


select distinct country, trim(country)
from layoffs_stage2
order by 1;


select distinct country, trim(trailing '.' from country)
from layoffs_stage2
order by 1;

update layoffs_stage2
set country = trim(trailing '.' from country)
where country like 'united state%';


-- in this place we were changing the date format into an actual date format and not a text

select *
from layoffs_stage2
;

select `date`,
str_to_date(`date`, '%m/%d/%Y')
from layoffs_stage2
;

update layoffs_stage2
set `date` = str_to_date(`date`, '%m/%d/%Y')
;

select `date`
from layoffs_stage2;


alter table layoffs_stage2
modify column `date` date;

select *
from layoffs_stage2
;

select *
from layoffs_stage2
where total_laid_off is null
;

select *
from layoffs_stage2
where total_laid_off is null
and percentage_laid_off is null
;

update layoffs_stage2 
set industry = null
where industry = '';

select distinct industry
from layoffs_stage2;

select *
from layoffs_stage2
where industry is null 
or industry = '';
;

select *
from layoffs_stage2
where company = 'Airbnb'
;

 
select t1.industry, t2.industry
from layoffs_stage2  t1
join layoffs_stage2  t2
   on t1.company = t2.company
where (t1.industry is null or t1.industry = '')
and t2.industry is not null   
;


 update layoffs_stage2  t1
join layoffs_stage2  t2
   on t1.company = t2.company
 set t1.industry = t2.industry
where t1.industry is null 
and t2.industry is not null   
;

select *
from layoffs_stage2
where company like 'Bally%'
;

select *
from layoffs_stage2;

-- delete
select *
from layoffs_stage2
where total_laid_off is null
and percentage_laid_off is null
;


delete
from layoffs_stage2
where total_laid_off is null
and percentage_laid_off is null
;


select *
from layoffs_stage2;

alter table layoffs_stage2
drop column row_num;

select *
from layoffs_stage2;
