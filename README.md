# 🛒 Segmentação de Clientes com RFM - Projeto E-Commerce

Este projeto realiza a segmentação de clientes com base na metodologia **RFM (Recência, Frequência e Valor Monetário)** utilizando dados de um e-commerce. O pipeline foi construído em três camadas (Bronze, Silver e Gold) seguindo a arquitetura de **Lakehouse**, e foi desenvolvido inteiramente no **Databricks com PySpark**.

---

## 🚀 Visão Geral

- **Bronze**: Ingestão dos dados crus no formato `.csv`.
- **Silver**: Limpeza e conversão dos dados para o formato Delta.
- **Gold**: Cálculo das métricas RFM, transformação em scores, classificação em segmentos e exportação para análise.

---

## 🧰 Tecnologias Utilizadas

- **Databricks / Apache Spark**
- **PySpark**
- **Delta Lake**
- **Pandas + NumPy**
- **SQL no Spark**
- **Lakehouse Architecture (Bronze, Silver, Gold)**

---

## 📂 Estrutura do Projeto

```
📁 Projeto1-Dados-Ecommerce/
│
└── 📁 Data_Layers/
    ├── [Bronze] Projeto1-Dados E-Commerce.ipynb   # Ingestão e armazenamento inicial
    ├── [Silver] Projeto1-Dados E-Commerce.ipynb   # Limpeza e formatação
    └── [Gold] Projeto1-Dados E-Commerce.ipynb     # Cálculo RFM e segmentação
```

---

## 🔢 Pipeline Detalhado

### 🔹 Bronze Layer
- Download dos dados via `wget`.
- Armazenamento bruto no DBFS como `.csv`.
- Conversão para formato **Parquet**.

### 🔸 Silver Layer
- Leitura dos dados Parquet.
- Limpeza: remoção de linhas nulas.
- Conversão para o formato **Delta Lake**.

### 🟡 Gold Layer
- Cálculo das métricas:
  - `Recency`: dias desde a última compra.
  - `Frequency`: número de pedidos distintos.
  - `Monetary`: soma dos valores pagos.
- Criação da tabela `RFM`.
- Normalização com `qcut` (quantis).
- Cálculo do score médio (F e M).
- Aplicação de regras para segmentação de clientes com `np.select`.

---

## 🎯 Segmentos Identificados

Os clientes foram classificados nos seguintes grupos:

- Champions
- Loyal Customers
- Potential Loyalist
- Customers Needing Attention
- About to Sleep
- At Risk
- Hibernating

---

## 📈 Exemplo de Regras de Segmentação

```python
regras = [
  (recency_score >= 4) & (FM_media >= 4),
  (recency_score >= 2) & (FM_media >= 3),
  ...
]
segmentos = ['Champions', 'Loyal Customers', ..., 'Hibernating']
