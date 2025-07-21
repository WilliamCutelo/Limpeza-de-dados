# 🧹 Layoffs 2022 - Data Cleaning Project

Este repositório contém um projeto de limpeza de dados utilizando SQL no MySQL Workbench. O dataset utilizado foi retirado do Kaggle:

📦 [Layoffs 2022 - Kaggle Dataset](https://www.kaggle.com/datasets/swaptr/layoffs-2022)

## 📊 Objetivo

Realizar o processo completo de **ETL (Extract, Transform, Load)** para transformar o dataset bruto de layoffs em um conjunto de dados limpo, pronto para análise.

---

## 🛠️ Tecnologias Utilizadas

- MySQL 8+
- MySQL Workbench
- SQL (CTEs, Window Functions, Updates, Deletes)
- Dataset do Kaggle (`layoffs-2022.csv`)

---

## 🔄 Etapas do Processo de Limpeza

### 1. 🗑️ Remoção de Duplicatas
- Utilização da função `ROW_NUMBER()` com `PARTITION BY` para identificar registros repetidos.
- Exclusão de duplicatas mantendo apenas a primeira ocorrência.

### 2. 🧼 Padronização de Dados
- Padronização de nomes de empresas com `TRIM()`.
- Agrupamento de setores similares (ex: "Crypto Finance" → "Crypto").
- Padronização de nomes de países (remoção de pontos, espaços).
- Conversão da coluna `date` de texto para o tipo `DATE` usando `STR_TO_DATE()`.

### 3. 🚫 Tratamento de Nulls e Campos Vazios
- Substituição de strings vazias por `NULL`.
- Preenchimento inteligente de colunas ausentes com base em registros semelhantes via `JOIN`.

### 4. 🧹 Remoção de Dados e Colunas Irrelevantes
- Exclusão de registros sem dados relevantes (`total_laid_off IS NULL AND percentage_laid_off IS NULL`).
- Remoção da coluna auxiliar `row_num` após a deduplicação.
