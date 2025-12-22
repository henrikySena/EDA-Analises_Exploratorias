# Projeto 03 - Alta Tensão [PowerBI]

Este repositório documenta uma análise exploratória de dados (EDA) sobre o consumo elétrico no Brasil,
com foco em Unidades Consumidoras de alta tensão, a partir da base BDGD/ANEEL.

A análise foi desenvolvida utilizando o **Power BI** e corresponde ao **primeiro de três conjuntos de dados**
que compõem o **Projeto 03 — Consumo Elétrico**, o qual foi estruturado em etapas distintas conforme o
nível de tensão das Unidades Consumidoras.

Atualmente, este dataset encontra-se organizado e analisado por meio de quatro blocos analíticos:

- **Bloco A** — Identificação das Unidades Consumidoras  
- **Bloco B** — Distribuição geográfica e características gerais  
- **Bloco C** — Demanda/Carga e identificação de erro sistemático nos dados  
- **Bloco D** — Contratação e modalidade tarifária  

Cada bloco possui objetivos específicos e documentação própria, compondo de forma progressiva
a compreensão da estrutura, qualidade e comportamento da base de dados analisada.

---
<br>

## Bloco A — Identificação da Unidade Consumidora (UC)

### 🔍 Objetivo Analítico
O Bloco A tem como objetivo **compreender a distribuição espacial das Unidades Consumidoras (UCs)** presentes na base de dados, estabelecendo o contexto geográfico necessário para as análises posteriores de perfil, sistema elétrico e demanda.

<br>

### ⚙️ Tratamentos Realizados

- Utilização do **CEP** como principal identificador espacial, dada a ausência de um campo explícito de município em parte da base
- Derivação do campo **UF** a partir do código do município (**MUN**), garantindo consistência territorial
- Padronização dos campos geográficos para uso em mapas e segmentações

Esses tratamentos permitiram preservar o máximo de granularidade espacial possível sem introduzir inferências artificiais.

<br>

### 🗺️ Visualizações Desenvolvidas

- Mapa de **Total de UCs por CEP**
- Mapa de **Total de UCs por UF**

Essas visualizações possibilitam identificar rapidamente padrões de concentração regional e servem como base para cruzamentos posteriores com variáveis técnicas e de consumo.

<br>

### 🧠 Principais Achados

- Observa-se uma **forte concentração de UCs nos estados de São Paulo (SP) e Minas Gerais (MG)**
- Em conjunto, esses dois estados concentram aproximadamente **80% das Unidades Consumidoras analisadas**

Esse resultado é coerente com o perfil histórico de industrialização e densidade econômica dessas regiões, reforçando a validade espacial da base de dados.

<br>

---

## Bloco B — Tipo de Sistema (TIP_SIST) e Regime de Contratação (LIV)

### 🔍 Objetivo Analítico
O Bloco B busca caracterizar as Unidades Consumidoras quanto ao **tipo de sistema elétrico ao qual estão conectadas** e ao **regime de contratação**, fornecendo o pano de fundo institucional e operacional para a análise de demanda.

<br>

### 🛠️ Tratamentos e Modelagem

- Análise do campo **TIP_SIST**, que classifica o tipo de sistema elétrico
> **Nota metodológica**: o campo **TIP_SIST não consta no dicionário oficial de dados** disponibilizado para o conjunto analisado. Ainda assim, optou-se por incluí-lo no estudo devido à sua **alta relevância analítica**, uma vez que permite distinguir estruturalmente o tipo de sistema elétrico ao qual as Unidades Consumidoras estão conectadas. Sua utilização mostrou-se consistente ao longo das análises exploratórias e agregou valor interpretativo ao EDA.

- Criação da coluna derivada **LIV_Status**, traduzindo o campo técnico **LIV** em categorias semanticamente legíveis:
  - Consumidor Livre
  - Consumidor Cativo

Esse tratamento permitiu maior clareza interpretativa nas visualizações e facilitou a interação entre filtros e gráficos.

<br>

### 📉 Visualizações Desenvolvidas

- Gráfico de rosca do **TIP_SIST**
- Gráfico de rosca do **LIV_Status**, totalmente interativo com os demais elementos do relatório

<br>

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
- **1960–2002 (Bloco C1)**: valores estáveis e fisicamente plausíveis
- **2003 em diante (Bloco C2)**: mudança abrupta de escala, sugerindo alteração cadastral, metodológica ou semântica

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

KPIs aplicados em todos os períodos:
- Demanda Média por UC
- Demanda Mediana por UC
- Capacidade Instalada Média por UC (CAR_INST)
- Razão Demanda (DEM_P e DEM_F) / Carga Instalada (CAR_INST)

### Teste de Consistência Estrutural

```DAX
Razao_Demanda_CAR_INST = DIVIDE([Demanda Média por UC], [CAR_INST Média por UC])
```

**Resultados:**
- **C1 (1960–2002)**: razão ≈ **0,01**, significativamente inferior à observada em C2, com valor extremamente baixo para uma razão dessa natureza, sugerindo erro estrutural na forma de registro dos dados
- **C2 (2003–2024)**: razão ≈ **0,85**, indicando elevada utilização e coerência interna

A preservação dessa razão ao longo das análises indica que a inconsistência observada não está na relação entre as variáveis, mas na escala absoluta dos valores registrados.

<br>

## 🧠 4. C2.1 — Hipótese de Unidade e Rotulagem (W → kW)

A hipótese central deste trabalho é que os valores de demanda registrados a partir de 2003 **permaneceram numericamente em watts (W)**, porém passaram a ser **rotulados como quilowatts (kW)** no dicionário de dados, sem que a conversão numérica correspondente (divisão por 1.000) fosse aplicada.

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

Essa assimetria observada sugere a presença de valores extremos distribuídos ao longo do período analisado, cuja natureza, requer investigação específica posterior.

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

## 8. Conclusão Técnica do Bloco

A análise do Bloco C evidencia uma **ruptura estrutural clara** na série de demanda a partir de 2003. Os resultados indicam que os valores registrados mantêm coerência interna, mas apresentam ordens de grandeza incompatíveis quando interpretados diretamente como kW.

A hipótese de erro de rotulagem de unidade (W → kW) mostrou-se capaz de restaurar a plausibilidade física dos indicadores sem alterar suas relações estruturais, permitindo uma leitura mais realista do comportamento da carga no período recente.

Essa abordagem reforça a importância da validação semântica de unidades em análises de dados históricos de grande escala, especialmente em contextos de transição metodológica.

<br>

---

## Bloco D — Contratação / Modalidade Tarifária

Este bloco tem como objetivo analisar o perfil de contratação das Unidades Consumidoras,
explorando a distribuição e a evolução das modalidades tarifárias ao longo do tempo.

- Neste contexto, a modalidade tarifária é tratada como a **forma contratual sob a qual a
Unidade Consumidora se conecta ao sistema elétrico**, refletindo regras de acesso,
faturamento e enquadramento regulatório, e não seu nível de consumo ou desempenho energético.

- A análise é de caráter exploratório, buscando identificar padrões, mudanças estruturais
e diferenças na distribuição das modalidades ao longo do tempo, considerando recortes
por tipo de sistema e geográficos apenas como lentes de observação, sem inferência causal
ou avaliação econômica nesta etapa.

Os resultados deste bloco servem como base para análises comparativas posteriores e para a
compreensão do contexto regulatório e operacional da base de dados.

<br>

### 📃 Grupos Tarifários — Referência (Manual ANEEL)

A tabela abaixo foi construída a partir da documentação oficial `Manual_de_Instruções_da_BDGD.pdf`, sendo utilizada
como referência conceitual ao longo do projeto.

| Código | Descrição |
|------|----------|
| 0 | Não informado |
| A1 | Subgrupo A1 – tensão de fornecimento igual ou superior a 230 kV |
| A2 | Subgrupo A2 – tensão de fornecimento de 88 kV a 138 kV |
| A3 | Subgrupo A3 – tensão de fornecimento de 69 kV |
| A3A | Subgrupo A3a – tensão de fornecimento de 30 kV a 44 kV |
| A4 | Subgrupo A4 – tensão de fornecimento de 2,3 kV a 25 kV |
| AS | Subgrupo AS |
| B1 | Subgrupo B1 – residencial |
| B1BR | Subgrupo B1 – residencial baixa renda |
| B2RU | Subgrupo B2 – rural |
| B2CO | Subgrupo B2 – cooperativa de eletrificação rural |
| B2SP | Subgrupo B2 – serviço público de irrigação |
| B3 | Subgrupo B3 – demais classes |
| B4A | Subgrupo B4 – iluminação pública – propriedade do poder público |
| B4B | Subgrupo B4 – iluminação pública – propriedade da distribuidora |

<br>

### Evolução das Unidades Consumidoras por Subgrupo Tarifário (Alta Tensão)
![Gráfico de linhas — Subgrupos Tarifários (Bloco D)](Images/grafico_linhas_subgrupoD.png)

---

### 🔎 Descrição do Visual Analisado

- **Eixo X**: Ano de conexão da UC  
- **Eixo Y**: Quantidade de Unidades Consumidoras  
- **Legenda**:  
  - Subgrupo A1 (≥ 230 kV)  
  - Subgrupo A2 (88–138 kV)  
  - Subgrupo A3 (69 kV)

<br>

### 1️⃣ Subgrupo A1 (≥ 230 kV)

O subgrupo A1 permanece **residual ao longo de toda a série histórica**, com valores próximos
de zero e sem variações relevantes.

Este comportamento indica que, dentro da base analisada, as conexões em tensões iguais ou
superiores a 230 kV representam uma parcela muito pequena do total de UCs.

<br>

### 2️⃣ Subgrupo A2 (88–138 kV): volatilidade e picos

O subgrupo A2 passa a apresentar crescimento após os anos 1980, caracterizado por:

- picos abruptos em anos específicos,
- alta variabilidade ao longo do tempo,
- ausência de crescimento estritamente contínuo.

Esse padrão sugere que o comportamento do A2 pode estar associado a **eventos pontuais** ou
processos concentrados em determinados períodos, em vez de uma expansão gradual.

<br>

### 3️⃣ Subgrupo A3 (69 kV): crescimento mais gradual

Em contraste com o A2, o subgrupo A3 apresenta:

- crescimento mais progressivo,
- menor volatilidade relativa,
- aumento consistente principalmente nos anos mais recentes.

Adicionalmente, observa-se que, em determinados períodos, há um comportamento alternado entre
A2 e A3, em que a redução de um coincide com o aumento do outro.

Esse padrão sugere hipóteses exploratórias como:

- possíveis reclassificações tarifárias ao longo do tempo,
- mudanças nos critérios regulatórios de enquadramento,
- estratégias contratuais adotadas por consumidores e distribuidoras.

<br>

### 4️⃣ Período anterior aos anos 1980

O gráfico indica que, até o final da década de 1970, o volume de registros de UCs conectadas
em alta tensão é reduzido e apresenta pouca variação entre os subgrupos.

É importante ressaltar que este comportamento não implica necessariamente a inexistência de UCs nesse período, mas pode refletir limitações históricas relacionadas a:

- processos de regulamentação do setor elétrico;
- ausência de padronização nacional;
- registro incompleto das informações nas bases de dados;
- descontinuidade institucional de empresas responsáveis pela operação, registro ou regulação dessas Unidades Consumidoras, o que pode ter resultado na perda ou não incorporação de dados históricos aos sistemas atuais.

Assim, o baixo volume observado pode estar mais associado à formalização, consolidação institucional e documentação progressiva das Unidades Consumidoras do que à dinâmica real de conexão ao sistema elétrico.

<br>

### 5️⃣ Intensificação a partir dos anos 1980

A partir dos anos 1980, observa-se um aumento claro no volume de registros, indicando uma
mudança no padrão da série temporal.

Este comportamento pode estar associado, de forma exploratória, a processos de reorganização
institucional e regulatória do setor elétrico brasileiro, incluindo:

- avanços institucionais no setor elétrico;
- maior organização regulatória ao longo das décadas seguintes;
- fortalecimento progressivo dos mecanismos de registro e acompanhamento das Unidades Consumidoras;
- criação e consolidação de órgãos reguladores, como a Agência Nacional de Energia Elétrica (ANEEL), instituída em 1996, que contribuiu para a padronização e centralização das informações do setor.

Esta hipótese não é afirmada como causal, mas surge como um **contexto histórico plausível**
para a mudança observada no comportamento da série temporal.

<br>

### 6️⃣ Anos 2000 em diante: mudança estrutural visível

A partir dos anos 2000, o gráfico apresenta uma **mudança clara de patamar**, caracterizada por:

- aumento do volume médio de UCs conectadas,
- maior variabilidade na série,
- novo regime visual em relação ao período anterior.

Essa mudança estrutural é visível exclusivamente a partir do comportamento da série temporal.
Como contexto interpretativo, esse período é compatível com uma fase de maior dinamismo
econômico e tecnológico no Brasil, embora esta análise não estabeleça relações causais.

<br>

### 🧠 Considerações finais desta etapa

Esta interpretação inicial cumpre o papel de:

- descrever padrões observáveis no gráfico,
- levantar hipóteses plausíveis e testáveis,
- reconhecer limitações históricas e regulatórias da base de dados.

As conclusões aqui apresentadas servem como **base para análises comparativas posteriores**
e para eventual incorporação de contexto histórico-regulatório em etapas futuras do projeto.

---
<br>

## Bloco E — Informações de Fornecimento

O Bloco E tem como objetivo analisar **as características do fornecimento de energia elétrica** das Unidades Consumidoras (UCs), buscando compreender **como a energia é entregue**, sob quais condições técnicas e contratuais, e quais padrões emergem a partir dos dados disponíveis na BDGD.

Este bloco é **descritivo e exploratório**, funcionando como uma ponte entre os aspectos contratuais (Bloco D) e as análises mais técnicas e quantitativas do projeto.

> Enquanto o **Bloco D** analisa *como a energia é contratada*, o **Bloco E** analisa *como a energia é fornecida*.

<br>

### 🧱 Estrutura Analítica do Bloco E

#### **E1 — Visão Geral do Fornecimento**
Análise inicial da distribuição dos tipos de fornecimento presentes no dataset.

**Perguntas orientadoras:**
- Quais são os principais tipos de fornecimento cadastrados?
- Existe concentração em poucos padrões dominantes?
- O fornecimento é homogêneo ou diverso entre as UCs?

**Entregas esperadas:**
- Gráfico de distribuição (barra ou rosca)
- Breve descrição interpretativa

<br>

#### **E2 — Fornecimento × Localização**
Análise da relação entre os tipos de fornecimento e a distribuição geográfica das UCs.

**Cruzamentos sugeridos:**
- Fornecimento × UF
- Fornecimento × Região (se aplicável)
- Fornecimento × TIP_SIST

**Perguntas orientadoras:**
- Existem padrões regionais claros?
- Certos tipos de fornecimento são predominantes em áreas específicas?

**Entregas esperadas:**
- Gráficos comparativos (barras empilhadas ou matriz)
- Comentário analítico sobre padrões espaciais

<br>

#### **E3 — Fornecimento × Perfil da Unidade Consumidora**
Avaliação do fornecimento em relação ao perfil contratual e tarifário das UCs.

**Cruzamentos sugeridos:**
- Fornecimento × Grupo/Subgrupo Tarifário
- Fornecimento × LIV (Consumidor Livre / Cativo)

**Perguntas orientadoras:**
- Consumidores livres concentram determinados tipos de fornecimento?
- O perfil técnico do fornecimento é coerente com o perfil contratual da UC?

**Entregas esperadas:**
- Visualizações comparativas
- Análise crítica de coerência técnica

<br>

#### **E4 — Qualidade dos Dados e Limitações**
Avaliação da completude e representatividade das informações de fornecimento.

**Pontos de atenção:**
- Campos com alto volume de valores ausentes
- Categorias raras ou pouco representativas
- Necessidade de agrupamentos ou exclusões justificadas

**Entregas esperadas:**
- Lista sintética de limitações
- Visual simples (quando aplicável)

<br>

### Escopo Delimitado (O que não será analisado)
Para evitar extrapolações indevidas, este bloco **não contempla**:
- Inferências sobre eficiência energética
- Análises de consumo ou demanda (tratadas em blocos anteriores)
- Deduções técnicas não explicitamente presentes no dataset (ex.: espessura de condutores inferida)

<br>

### 🧠 Resultado Esperado
Ao final do Bloco E, espera-se responder de forma clara:

> “Quais são os padrões de fornecimento de energia elétrica para Unidades Consumidoras de alta tensão no Brasil, e como esses padrões se relacionam com fatores geográficos, sistêmicos e contratuais, respeitando os limites do dado analisado.”

Este bloco servirá como base interpretativa para análises posteriores e como documentação estruturada do perfil de fornecimento presente na BDGD.

