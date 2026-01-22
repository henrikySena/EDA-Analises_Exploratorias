## 🔌 Projeto 03 — Distribuição e Consumo de Energia Elétrica no Brasil  
### BDGD / ANEEL | Projeto de Aprendizagem Intencional em Ferramentas de Dados

---

## 📌 Resumo Executivo

Este projeto utiliza a **Base de Dados Geográfica da Distribuidora (BDGD)**, disponibilizada pela **Agência Nacional de Energia Elétrica (ANEEL)**, como **objeto de estudo técnico** para o desenvolvimento e consolidação de competências em **Power BI, PostgreSQL e Python**, aplicadas a dados reais, complexos e normativos.

O **Projeto 03 não tem como objetivo principal responder perguntas de negócio específicas**, mas sim **maximizar o aprendizado profundo das ferramentas**, respeitando as particularidades estruturais, históricas e técnicas de uma base pública de grande escala.

A estratégia adotada consiste em **isolar o aprendizado de cada ferramenta**, aplicando-a a um subconjunto coerente da base (segmentado por nível de tensão), antes de avançar para projetos futuros com **integração completa de tecnologias e foco em resolução de problemas**.

---

## 🧠 Contexto e Motivação

A BDGD é uma base extensa, heterogênea e fortemente regulada, que combina:

- informações geográficas
- dados contratuais e operacionais
- registros históricos de longa duração
- normas técnicas do setor elétrico (ex.: PRODIST)

Bases com esse nível de complexidade **não são adequadas para abordagens superficiais ou análises exploratórias rápidas**.  
Dessa forma, este projeto foi concebido como um **ambiente controlado de estudo**, no qual cada ferramenta é explorada de forma isolada, consciente e documentada.

---

## 🎯 Objetivo Central do Projeto

> **Aprender ferramentas de dados em profundidade, utilizando uma base real e complexa, antes de integrá-las em projetos orientados à resolução de problemas.**

O foco do Projeto 03 é:

- compreender como cada ferramenta se comporta em cenários reais
- identificar limites, vantagens e desafios técnicos
- desenvolver um **modelo mental sólido**, e não apenas gerar outputs finais
- criar uma base técnica confiável para projetos futuros mais integrados

---

## 🧩 Estratégia de Segmentação por Nível de Tensão

A divisão da base por **alta, média e baixa tensão** não é apenas organizacional, mas **estrutural**:

- cada nível de tensão apresenta:
  - volumes distintos
  - estruturas de dados diferentes
  - campos específicos
  - lógicas operacionais próprias

Essa segmentação permitiu associar **uma ferramenta principal a cada nível**, maximizando o aprendizado técnico sem mascarar dificuldades por meio de soluções híbridas prematuras.

---

## 🏗️ Arquitetura do Projeto

O Projeto 03 é composto por três subprojetos independentes, porém conceitualmente conectados:

### 🔹 Projeto 03.1 — Alta Tensão | Power BI  
Foco em:
- exploração visual
- dashboards interativos
- análise geográfica
- entendimento do comportamento visual dos dados energéticos

🔗 Acesse:  
[Projeto 03.1 – Alta Tensão [Power BI]](https://github.com/henrikySena/EDA-Analises_Exploratorias/tree/main/Projeto_03_Consumo_Eletrico/Projeto_03.1_Alta_Tensao_PowerBI)

---

### 🔹 Projeto 03.2 — Média Tensão | PostgreSQL  
Foco em:
- ingestão segura de dados
- preservação do dado bruto
- modelagem relacional
- consultas SQL documentadas
- validação estrutural da base

🔗 Acesse:  
[Projeto 03.2 – Média Tensão [PostgreSQL]](https://github.com/henrikySena/EDA-Analises_Exploratorias/blob/main/Projeto_03_Consumo_Eletrico/Projeto_03.2_Media_Tensao_PostgreSQL/README.md)

---

### 🔹 Projeto 03.3 — Baixa Tensão | Python  
Foco em:
- limpeza avançada
- manipulação de grandes volumes de dados
- tratamento programático
- construção de scripts reutilizáveis

---

## 📂 Estrutura da Pasta

