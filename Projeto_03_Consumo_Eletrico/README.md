## ⚡ Projeto 03 — Consumo Elétrico (BDGD / ANEEL)

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

* **Power BI** – análise visual e dashboards.
* **PostgreSQL + PostGIS** – consultas espaciais e manipulação relacional.
* **Python** – limpeza, tratamento e análise de grandes datasets.

<br>

## 🔹 Progresso do Projeto

Dia 08 dezembro, 2025
- Criada tabela duplicada para tratamento: 01_AltaTensao_Tratamento
- Tabela original preservada como: 00_AltaTensao_Original
- Tipos de dados revisados no Power Query
- Todas as colunas apresentam tipos corretos (numéricos, texto, data, etc.)
- Valores nulos identificados como válidos, sem necessidade de substituição
- Base considerada limpa o suficiente para seguir para a fase de modelagem

Próximo passo: mapear colunas categóricas, entender possibilidades de análise e iniciar primeiras medidas no Power BI

<br>

Dia 09 de dezembro, 2025
- Organização das variáveis do dataset em blocos lógicos, incluindo o campo **TIP_SIST**, que será utilizado nas análises e no relatório final.

---

## 🔵 A — Identificação da Unidade Consumidora (UC)

Variáveis que identificam **quem** e **onde** é a Unidade Consumidora.

| Sigla     | Descrição                                      |
|-----------|------------------------------------------------|
| COD_ID    | Identificador único da UC                      |
| PN_CON    | Ponto notável (ex.: rural, urbano)             |
| COD_LOC   | Código do local / município                    |
| NOM_MUN   | Nome do município                              |
| COD_UF    | Estado                                          |
| SUBSIST   | Subsistema elétrico do SIN (SE/NE/S/NO)        |

---

## 🟣 B — Tipo de Sistema (TIP_SIST)

Campo incluído mesmo sem constar no manual oficial ANEEL, pois agrega valor analítico.

| Sigla     | Descrição                                      |
|-----------|------------------------------------------------|
| TIP_SIST  | Tipo de sistema: **RED_INTERLIG** ou **RED_ISOLADA** |

---

## 🟢 C — Demanda / Carga

Variáveis relacionadas ao uso e capacidade de energia.

| Sigla     | Descrição                                      |
|-----------|------------------------------------------------|
| DEMANDA   | Demanda registrada (kW)                        |
| CARGA     | Carga instalada (kW)                           |

---

## 🟠 D — Contratação / Modalidade Tarifária

| Sigla       | Descrição                                                |
|-------------|----------------------------------------------------------|
| MOD_CONTR   | Modalidade de contratação (convencional, horo-sazonal)   |
| TENS_NOM    | Tensão nominal (kV)                                      |
| CLASSE      | Classe de consumo (industrial, comercial, rural, etc.)   |

---

## 🔴 E — Informações de Fornecimento

| Sigla     | Descrição                                      |
|-----------|------------------------------------------------|
| FASES     | Número de fases (mono / bi / trifásico)        |
| TIP_FORN  | Tipo de fornecimento (aéreo, subterrâneo, etc.)|
| TP_ATR    | Tipo de atendimento                            |

---

## 🟡 F — Consumo / Energia

| Sigla     | Descrição                                      |
|-----------|------------------------------------------------|
| CONS_BI   | Consumo faturado (kWh)                         |
| CONS_MES  | Consumo do mês                                 |
| FAT_MES   | Faturamento do mês                             |

---

## 🧩 Resumo dos Blocos

1. Identificação da UC  
2. **Tipo de Sistema (TIP_SIST)**  
3. Demanda e Carga  
4. Modalidade Tarifária  
5. Fornecimento  
6. Consumo e Faturamento  

---


<br>





