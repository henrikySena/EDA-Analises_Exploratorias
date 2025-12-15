## 🗺️ Bloco A — Identificação da Unidade Consumidora (UC)

### 🔍 Objetivo Analítico
O Bloco A tem como objetivo **compreender a distribuição espacial das Unidades Consumidoras (UCs)** presentes na base de dados, estabelecendo o contexto geográfico necessário para as análises posteriores de perfil, sistema elétrico e demanda.

<br>

### ⚙️ Tratamentos Realizados

- Utilização do **CEP** como principal identificador espacial, dada a ausência de um campo explícito de município em parte da base
- Derivação do campo **UF** a partir do código do município (**MUN**), garantindo consistência territorial
- Padronização dos campos geográficos para uso em mapas e segmentações

Esses tratamentos permitiram preservar o máximo de granularidade espacial possível sem introduzir inferências artificiais.

<br>

### 📉 Visualizações Desenvolvidas

- Mapa de **Total de UCs por CEP**
- Mapa de **Total de UCs por UF**

Essas visualizações possibilitam identificar rapidamente padrões de concentração regional e servem como base para cruzamentos posteriores com variáveis técnicas e de consumo.

<br>

### 🧠 Principais Achados

- Observa-se uma **forte concentração de UCs nos estados de São Paulo (SP) e Minas Gerais (MG)**
- Em conjunto, esses dois estados concentram aproximadamente **80% das Unidades Consumidoras analisadas**

Esse resultado é coerente com o perfil histórico de industrialização e densidade econômica dessas regiões, reforçando a validade espacial da base de dados.

<br>

## 🔌 Bloco B — Tipo de Sistema (TIP_SIST) e Regime de Contratação (LIV)

### 🔍 Objetivo Analítico
O Bloco B busca caracterizar as Unidades Consumidoras quanto ao **tipo de sistema elétrico ao qual estão conectadas** e ao **regime de contratação**, fornecendo o pano de fundo institucional e operacional para a análise de demanda.

<br>

### 🛠️ Tratamentos e Modelagem

- Análise do campo **TIP_SIST**, que classifica o tipo de sistema elétrico
- Criação da coluna derivada **LIV_Status**, traduzindo o campo técnico **LIV** em categorias semanticamente legíveis:
  - Consumidor Livre
  - Consumidor Cativo

Esse tratamento permitiu maior clareza interpretativa nas visualizações e facilitou a interação entre filtros e gráficos.

<br>

### 📉 Visualizações Desenvolvidas

- Gráfico de rosca do **TIP_SIST**
- Gráfico de rosca do **LIV_Status**, totalmente interativo com os demais elementos do relatório



### 🧠 Principais Achados

- Aproximadamente **93,78% das UCs estão conectadas à Rede Interligada**, confirmando a predominância do Sistema Interligado Nacional (SIN)
- A segmentação por **Consumidor Livre / Cativo** permite análises comparativas relevantes nos blocos seguintes, especialmente no estudo de demanda e carga

---

**[NOTA]:** Esses dois blocos estabelecem a **base estrutural e institucional** do projeto, garantindo que as análises de demanda realizadas no Bloco C sejam interpretadas à luz da distribuição geográfica, do tipo de sistema elétrico e do regime de contratação das Unidades Consumidoras.

---

## Bloco C — Demanda / Carga: Análise Histórica, Ruptura Estrutural e Avaliação de Unidade

## 🔍 1. Contexto e Problema Inicial

Durante a análise da demanda das Unidades Consumidoras (UCs), foram identificados **valores implausíveis e uma ruptura estrutural clara na série histórica**, concentrada no início dos anos 2000.

### Evidência 1 — Ordens de Grandeza Incompatíveis
- **Período 2003–2024 (análise observada)**: a Demanda Média por UC atinge valores da ordem de **bilhões de kW**, incompatíveis com a capacidade instalada do sistema elétrico brasileiro e com limites físicos conhecidos.

### Evidência 2 — Salto Temporal Inexplicável

```
2003: ~300 milhões
2004: ~7 bilhões
Variação: +2.233% em 1 ano
```

Esse crescimento não encontra respaldo em variáveis macroeconômicas, expansão de infraestrutura ou mudanças conhecidas no perfil de carga do período.

### Evidência 3 — Quebra de Padrão Histórico
- **1960–2002 (C1)**: valores estáveis e fisicamente plausíveis
- **2003 em diante (C2)**: mudança abrupta de escala, sugerindo alteração cadastral, metodológica ou semântica

<br>

##  2. Estrutura Analítica do Bloco C

Para tratar adequadamente essa ruptura, o Bloco C foi segmentado da seguinte forma:

- **C1 — Demanda / Carga (1960–2002)**: análise do período histórico pré-ruptura
- **C2 — Demanda / Carga (2003–2024)**: análise observada do período pós-ruptura
- **C2.1 — Hipótese de Unidade e Rotulagem (W × kW)**
- **C2.2 — Demanda Ajustada (Avaliação do Impacto da Hipótese)**

Essa estrutura permite separar claramente:
- o comportamento observado do dado
- a hipótese explicativa
- a avaliação quantitativa do impacto da interpretação correta da unidade

<br>

## 🔬 3. KPIs Utilizados e Validação Interna

KPIs aplicados de forma consistente em todos os períodos:
- Demanda Média por UC
- Demanda Mediana por UC
- Capacidade Instalada Média por UC (CAR_INST)
- Razão Demanda / CAR_INST

### Teste de Consistência Estrutural

```DAX
Razao_Demanda_CAR_INST = DIVIDE([Demanda Média por UC], [CAR_INST Média por UC])
```

**Resultados:**
- **C1 (1960–2002)**: razão ≈ **0,01**, compatível com utilização média histórica
- **C2 (2003–2024)**: razão ≈ **0,85**, indicando elevada utilização e coerência interna

A preservação dessa razão ao longo das análises sugere que o problema identificado não está na relação entre as variáveis, mas na **escala absoluta dos valores registrados**.

<br>

## 🧠 4. C2.1 — Hipótese de Unidade e Rotulagem (W → kW)

A hipótese central deste trabalho é que os valores de demanda registrados a partir de 2003 **permaneceram numericamente em watts (W)**, porém passaram a ser **rotulados como quilowatts (kW)** no dicionário de dados, sem que a conversão numérica correspondente (divisão por 1.000) fosse realizada.

Sob essa hipótese:
- os números registrados não estariam incorretos
- o problema residiria na **semântica da unidade**, e não na integridade do dado

Essa interpretação explica simultaneamente:
- os picos extremos observados
- a coerência interna entre demanda e capacidade instalada
- a ruptura abrupta a partir de 2003

<br>

## 5. C2.2 — Avaliação Quantitativa sob a Hipótese W → kW

Aplicando-se um fator de conversão uniforme (W → kW), exclusivamente para fins analíticos, obtêm-se os seguintes resultados ajustados para o período 2003–2024:

- **Demanda Média por UC (ajustada)**: **5,13 MW**
- **Demanda Mediana por UC (ajustada)**: **64,30 kW**
- **CAR_INST Média por UC (ajustada)**: **5,99 MW**
- **Razão Demanda / CAR_INST**: **0,85**

Esses valores apresentam **ordens de grandeza fisicamente plausíveis** para consumidores conectados em alta tensão e preservam integralmente as relações estruturais observadas nos dados originais.

A elevada diferença entre média e mediana evidencia uma distribuição fortemente assimétrica, na qual um subconjunto reduzido de grandes consumidores exerce influência significativa sobre a média do período.

<br>

## 6. 📉 Visualizações e Exploração

- Séries temporais de demanda média e mediana por UC
- Comparação entre valores observados e ajustados (C2 × C2.2)
- Segmentações por `ANO_CONEXAO`, `LIV_Status` e `TIP_SIST`
- Identificação e contextualização de outliers históricos

<br>

## 7. Limitações

- A conversão W → kW é tratada como **hipótese analítica**, não como correção oficial da base
- Mudanças metodológicas e cadastrais entre períodos limitam comparações diretas
- O estudo não substitui validação documental formal junto à ANEEL

<br>

## ✔ 8. Conclusão Técnica

A análise do Bloco C evidencia uma **ruptura estrutural clara** na série de demanda a partir de 2003. Os resultados indicam que os valores registrados mantêm coerência interna, mas apresentam ordens de grandeza incompatíveis quando interpretados diretamente como kW.

A hipótese de erro de rotulagem de unidade (W → kW) mostrou-se capaz de restaurar a plausibilidade física dos indicadores sem alterar suas relações estruturais, permitindo uma leitura mais realista do comportamento da carga no período recente.

Essa abordagem reforça a importância da validação semântica de unidades em análises de dados históricos de grande escala, especialmente em contextos de transição metodológica.





