# 🌾 **Projeto 02 — Análise de Produção Agrícola**

<br>

## **Relatório Técnico do Projeto**

Este projeto tem como objetivo analisar um conjunto de dados agrícolas fictício, explorando produtividade, ciclos, características do solo, custos, lucros e outros fatores relevantes para a tomada de decisão no contexto agropecuário. Foram realizadas etapas de limpeza, modelagem, criação de variáveis derivadas, análises estatísticas, construção de KPIs e visualizações.

<br>


## 🔍 **1. Objetivos do Projeto**
- Construir um dataset agrícola coerente para fins de estudo.
- Criar novas variáveis derivadas (ciclo em dias/meses, produtividade, faixa de pH, lucro por ha etc.).
- Realizar análises descritivas para identificar padrões e tendências.
- Criar KPIs operacionais, de produtividade e financeiros.
- Construir visualizações baseadas nos dados.
- Documentar os insights encontrados.

<br>

## 📁 **2. Estrutura do Dataset**
O dataset contém as seguintes colunas principais:
- ID_Fazenda
- Região
- Tipo_de_Cultura
- Area_ha
- Data_Plantio / Data_Colheita
- Ciclo_Dias / Ciclo_Meses
- Producao_ton
- Produtividade_ton_por_ha
- Tipo_Irrigacao
- pH_Solo / pH_Faixa
- Chuva_mm
- Temperatura_Media_C
- Fertilizante_kg
- Uso de Pesticida / Tipo_Pesticida
- Grau_Qualidade
- Preço por Tonelada
- Custos_Producao
- Lucro / Lucro_por_ha

<br>

## 📌 **3. KPIs do Projeto**

### 🌾 **Produtividade**
| Indicador | Valor |
|----------|-------|
| Produtividade Média Geral (ton/ha) | **3,97** |
| Produtividade Média por Região (ton/ha) | **3,93** |
| Produtividade Máxima | **6,5 ton/ha** |
| Produtividade Mínima | **1,50 ton/ha** |
| Cultura Mais Produtiva | **Trigo** |

### 🌱 **Operacionais**
| Indicador | Valor |
|----------|-------|
| Área Média Plantada | **152,35 ha** |
| Ciclo Médio (dias) | **166 dias** |
| Ciclo Médio (meses) | **6 meses** |
| Percentual de Solos com pH Ideal (6–7) | **33,2%** |

### 💰 **Financeiros**
| Indicador | Valor |
|----------|-------|
| Receita Total Estimada | **R$ 852.912.851,51** |
| Lucro Médio por ha | **–217,23** (prejuízo médio por hectare) |
| Custo Médio por Cultura | **R$ 191.937,50** |

<br>

## 📈 **4. Principais Insights**
- A produtividade média geral é boa, mas ainda há grande variação entre culturas.
- Apenas um terço dos solos possui pH ideal, indicando necessidade de correção.
- O ciclo médio de 166 dias é coerente para culturas de longa duração.
- Apesar da alta receita total, o lucro por hectare é negativo, sugerindo desequilíbrios entre custo e retorno.
- O trigo se destaca como a cultura mais produtiva no dataset.
- Os custos médios por cultura são muito altos, influenciando diretamente a rentabilidade.
- **A qualidade não é afetada pelo uso de pesticida.**
- **Não existe uma relação clara entre quantidade de chuva e pH do solo.**

<br>

## 🛠️ **5. Tecnologias e Ferramentas**
- Excel
  - Limpeza de dados
  - Criação de variáveis derivadas
  - Tabelas e cálculos estatísticos
  - Gráficos e análises
- (Opcional futuramente: Power BI, Python, SQL)

<br>

## 📝 **6. Próximos Passos**
- Migrar o projeto para Power BI com visualizações mais ricas
- Criar ranking por cultura, região e qualidade
- Explorar elasticidade de preço e simulações de lucro
- Incluir modelos preditivos (futuro)

<br>

## 🧾 **7. Conclusão do Projeto**

A análise revelou desafios importantes no desempenho agrícola, como a baixa proporção de solos com pH ideal e os elevados custos médios de produção — fatores que contribuem para o prejuízo médio por hectare observado no dataset. Apesar disso, a produtividade geral se mantém em um patamar razoável, com destaque para o trigo como cultura mais eficiente em termos de rendimento.

Os insights também mostram que o uso de pesticidas não afeta diretamente a qualidade, e que não há relação clara entre os níveis de chuva e o pH do solo, indicando que esses fatores podem depender de variáveis externas ou de análises mais específicas.

De forma geral, o estudo fornece um panorama sólido para compreender fatores agronômicos, operacionais e financeiros que influenciam os resultados no setor agrícola. O projeto ainda abre espaço para evoluções futuras, como visualizações em Power BI, análises por região, simulações econômicas e implementação de modelos preditivos.

<br>
