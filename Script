-- SQL - Limpeza de dados

-- https://www.kaggle.com/datasets/swaptr/layoffs-2022

-- 1 Remover duplicatas
-- 2 Colocar um padrão e corrigir erros
-- 3 Ver sobre os valores nulls e vazios
-- 4 Remover colunas desnecessarias

-- Removendo duplicatas


SELECT *,
		ROW_NUMBER() OVER (
			PARTITION BY company, industry, total_laid_off,`date`) AS row_num
	FROM 
		world_layoff.layoffs_staging;
        
WITH duplicate_cte AS
( SELECT *,
		ROW_NUMBER() OVER (
			PARTITION BY company, location, industry, total_laid_off,`date`, stage, country, funds_raised_millions) AS row_num
	FROM 
		world_layoff.layoffs_staging
)
SELECT *
FROM duplicate_cte
where row_num >= 2;





CREATE TABLE `layoffs_staging2` (
  `company` text,
  `location` text,
  `industry` text,
  `total_laid_off` int DEFAULT NULL,
  `percentage_laid_off` text,
  `date` text,
  `stage` text,
  `country` text,
  `funds_raised_millions` int DEFAULT NULL,
  `row_num` int 
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;




SELECT *
FROM layoffs_staging2;

INSERT INTO layoffs_staging2
 SELECT *,
		ROW_NUMBER() OVER (
			PARTITION BY company, location, industry, total_laid_off,`date`, stage, country, funds_raised_millions) AS row_num
	FROM 
		world_layoff.layoffs_staging;
        
SET SQL_SAFE_UPDATES = 0;

DELETE 
FROM layoffs_staging2
WHERE row_num >= 2;

SELECT *
FROM layoffs_staging2
WHERE row_num >=2;



-- Padronizar

-- Nomes
SELECT company, trim(company)
FROM layoffs_staging2;

UPDATE layoffs_staging2
SET company = trim(company);

-- Industrias
SELECT industry
FROM layoffs_staging2
ORDER BY 1;

SELECT *
FROM layoffs_staging2
WHERE industry LIKE 'Crypto%';

UPDATE layoffs_staging2
SET industry = 'Crypto'
WHERE industry LIKE 'Crypto%';

-- Paises
SELECT DISTINCT Country
FROM layoffs_staging2
WHERE country LIKE 'United%';

UPDATE layoffs_staging2
SET country = TRIM(TRAILING '.' FROM country);

-- datas

Select `date`
from layoffs_staging2;

UPDATE layoffs_staging2
SET `date` = STR_TO_DATE(`date`, '%m/%d/%Y');

ALTER TABLE layoffs_staging2
MODIFY COLUMN `date` DATE;

-- Pontos vazios e nulls

SELECT *
FROM layoffs_staging2
WHERE total_laid_off IS NULL;

UPDATE layoffs_staging2
SET industry = null
WHERE industry = '';

SELECT *
FROM layoffs_staging2 
WHERE industry IS NULL 
or industry = '';

SELECT *
FROM layoffs_staging2 t1
JOIN layoffs_staging2 t2
	ON t1.company = t2.company
    AND t1.location = t2.location
WHERE (t1.industry IS NULL or t1.industry = '')
AND t2.industry IS NOT NULL;

UPDATE layoffs_staging2 t1
JOIN layoffs_staging2 t2
	ON t1.company = t2.company
SET t1.industry = t2.industry
WHERE t1.industry IS NULL
AND	t2.Industry IS NOT NULL;



-- DELETANDO DESNECESSARIOS
SELECT *
FROM layoffs_staging2
WHERE total_laid_off IS NULL
AND percentage_laid_off IS NULL;

ALTER TABLE layoffs_staging2
DROP COLUMN row_num;


DELETE
FROM layoffs_staging2
WHERE total_laid_off IS NULL
AND percentage_laid_off IS NULL;
