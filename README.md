# Predição de Preços de Imóveis com Machine Learning

Este projeto tem como objetivo prever o preço de imóveis utilizando técnicas de Machine Learning, a partir do dataset **Flats Price Dataset** disponível no Kaggle.

Dataset: https://www.kaggle.com/datasets/sumanbera19/flats-price-dataset

---

## Objetivo

Construir um modelo preditivo capaz de estimar o preço de imóveis com base em suas características estruturais e contextuais, explorando desde abordagens simples até modelos mais avançados.

---

## Etapas do Projeto

O projeto foi estruturado nas seguintes etapas:

1. Exploração e limpeza de dados  
2. Feature engineering  
3. Modelagem com algoritmos de regressão  
4. Tuning de hiperparâmetros  
5. Avaliação de desempenho  
6. Interpretação dos resultados  

---

## Data Cleaning

Durante a análise inicial, foram identificados diversos desafios na base:

- Valores numéricos misturados com texto (ex: `bedRoom`, `bathroom`, `balcony`)
- Coluna `price` com múltiplas unidades (**Lakh / Crore**)
- Variáveis compostas em texto (`areaWithType`, `floorNum`, etc.)

### Principais transformações:

- Padronização da variável `price` para uma única unidade (Lakh)
- Extração de valores numéricos de colunas textuais
- Separação de colunas compostas:
  - `areaWithType` → `area_type`, `area_sqft`, `area_sqm`
  - `floorNum` → `floor`, `total_floors`

---

## Feature Engineering

Foram criadas novas variáveis com base em conhecimento de domínio:

  #### Estrutura do imóvel
  
  - `floor_category`  
    Representa a posição relativa do imóvel no prédio:
    - low (0–33%)
    - mid (33–66%)
    - high (66–100%)
  
  - `is_high_rise`  
    Indica se o imóvel está em prédio alto (≥ 15 andares)
  
  ---
  
  #### Status do imóvel
  
  A variável `agePossession` foi transformada em múltiplas features:
  
  - `construction_status`:
    - ready
    - under_construction
    - future_possession
  
  - Faixas de idade:
    - `0_to_1_years_old`
    - `1_to_5_years_old`
    - `5_to_10_years_old`
    - `10_more_years_old`
  
  - `expected_possession_date` (extraída quando aplicável)
  
  ---
  
  #### Localização
  
  Foram criadas variáveis binárias para capturar regiões relevantes com base no endereço:
  
  - `loc_gurgaon`
  - `loc_delhi`
  - `loc_dwarka`
  - etc.
  
  (One-hot encoding manual baseado em palavras-chave)

---

### Transformação do Target

A variável `price` foi transformada utilizando log:

y = log(1 + price)

Objetivo:
- Reduzir skewness
- Melhorar performance dos modelos

<img width="572" height="453" alt="image" src="https://github.com/user-attachments/assets/cb78c4d3-6769-456d-9dce-d91ae83631c1" />


---

## Modelos Testados

Foram avaliados os seguintes modelos:

- Linear Regression  
- Ridge Regression  
- Random Forest  
- Gradient Boosting  

---

## Resultados

| Modelo | Versão | Conjunto | RMSE | MAE | R² |
|--------|--------|----------|------|------|------|
| Linear Regression | default | validação | 94.64 | 55.70 | 0.552 |
| Ridge Regression | default | validação | 94.22 | 55.23 | 0.556 |
| Random Forest | default | validação | 59.73 | 23.67 | 0.821 |
| Gradient Boosting | default | validação | 59.73 | 25.54 | 0.821 |
| Random Forest | tuned | validação | 72.87 | 30.80 | 0.734 |
| Gradient Boosting | tuned | validação | 56.31 | 23.27 | 0.841 |
| **Gradient Boosting** | **tuned** | **teste** | **36.04** | **19.56** | **0.930** |

---

## Modelo Final

O melhor modelo foi:

**Gradient Boosting Regressor (tuned)**

Desempenho no conjunto de teste:

- RMSE: 36.04  
- MAE: 19.56  
- R²: 0.93  

O modelo conseguiu explicar aproximadamente **93% da variância dos preços**, indicando excelente capacidade preditiva.

---

## Principais Insights

- Modelos lineares não foram suficientes para capturar a complexidade dos dados  
- Modelos baseados em árvores performaram significativamente melhor  
- Feature engineering teve impacto direto na performance  
- O tuning melhorou o Gradient Boosting, mas prejudicou o Random Forest  
- Garantir consistência entre treino, validação e teste foi essencial  

---

## Tecnologias Utilizadas

- Python  
- Pandas  
- NumPy  
- Scikit-learn  
- Seaborn  
- Matplotlib  


