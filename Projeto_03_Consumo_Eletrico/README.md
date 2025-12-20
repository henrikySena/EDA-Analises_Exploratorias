## 🔌 Projeto 03 — Distribuição e Consumo de Energia Elétrica no Brasil (BDGD / ANEEL)

**Análise da Distribuição e Consumo de Energia Elétrica no Brasil com base na Base de Dados Geográfica da Distribuidora (BDGD), disponibilizada pela Agência Nacional de Energia Elétrica - ANEEL.**

<br>

## Visão Geral do Projeto

Este projeto tem como objetivo explorar, analisar e compreender o consumo e a distribuição de energia elétrica no Brasil por meio dos dados oficiais fornecidos pela **ANEEL**.

A Base de Dados Geográfica da Distribuidora (BDGD) reúne informações espaciais e operacionais das concessionárias e permissionárias de distribuição de energia elétrica no país.
Os dados foram divididos em três níveis de tensão: **alta**, **média** e **baixa**, cada um analisado com uma ferramenta diferente.

<br>



## 📄 Estrutura do Projeto

O projeto foi dividido em três análises independentes, cada uma explorando um nível de tensão e utilizando uma ferramenta específica:

- ### **3.1 – Alta Tensão → Power BI**
    Visualização, painéis interativos e análise geográfica.

- ### **3.2 – Média Tensão → PostgreSQL**
    Consultas otimizadas, modelagem relacional e análise via SQL.

- ### **3.3 – Baixa Tensão → Python**
    Limpeza avançada, manipulação de grandes volumes de dados e análises escaláveis.
<br>

---

## 📂 Estrutura da Pasta

```
Projeto_03_Consumo_Eletrico/
│
├── Docs/
│   ├── Manual_de_Instruções_da_BDGD.pdf
│   └── Modulo10_PRODIST.pdf
│   └── URL_Dataset_Oficial_BDGD.txt
│
├── Projeto_03.1_Alta_Tensao_PowerBI/
│   └── Dataset/
│       └── ucat_pj.csv
│       └── ucat_pj.txt
|   └── Docs/
│       └── dicionario_das_siglas_das_colunas-ucat_pj.txt
|   └── EDA/
│       └── BDGD_AltaTensao_PBI.pbix
│
├── Projeto_03.2_Media_Tensao_PostgreSQL/
│   └── Dataset/
│       └── ucmt_pj.txt
│
└── Projeto_03.3_Baixa_Tensao_Python/
    └── Dataset/
        └── ucbt_pj.txt
```

---

## 📉 Fonte dos Dados

Os dados foram obtidos no portal de dados abertos da ANEEL:

> **BDGD – Base de Dados Geográfica da Distribuidora**
> URL oficial disponível em: `docs/URL_Dataset_Oficial_BDGD.txt`

A base contém informações relacionadas a:

* Unidades consumidoras
* Infraestrutura elétrica
* Concessionárias/permissionárias
* Redes de alta, média e baixa tensão
* Dados geográficos do sistema elétrico


<br>


## ✔️ Objetivos do Projeto

* Analisar o comportamento do consumo elétrico no Brasil por diferentes níveis de tensão.
* Explorar visualmente grandes conjuntos de dados energéticos.
* Construir consultas robustas utilizando PostgreSQL e PostGIS.
* Desenvolver scripts Python capazes de lidar com datasets de grande volume.
* Criar um pipeline completo e multiplataforma de análise de dados.

<br>

## 🛠️ Ferramentas Utilizadas

* **Projeto 03.1 [Power BI]** – análise visual e dashboards.
* **Projeto 03.2 [PostgreSQL] + PostGIS** – consultas espaciais e manipulação relacional.
* **Projeto 03.3 [Python]** – limpeza, tratamento e análise de grandes datasets.

<br>


