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

## 📉 **3. Dashboards**
*[em breve]*

<br>

## 📌 **4. KPIs do Projeto**

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

## 📈 **5. Principais Insights**
1. **A produtividade média geral é boa, mas ainda há grande variação entre culturas:**
     - A produtividade média geral se mantém em um nível adequado, porém a dispersão entre as diferentes culturas é considerável. Isso indica que algumas produções são consistentemente mais eficientes, enquanto outras apresentam rendimento abaixo da média. Essa variação pode estar associada a fatores como exigências nutricionais específicas, condições climáticas ou manejo inadequado.

<br>
   
2. **Apenas um terço dos solos possui pH ideal, indicando necessidade de correção:**
   - Apenas cerca de um terço das amostras está dentro da faixa considerada ideal (pH 6–7). A maior parte dos solos tende a ser levemente ácida, o que pode reduzir a absorção de nutrientes pelas plantas. Isso sugere a necessidade de práticas corretivas, como calagem, para melhorar a fertilidade do solo e potencializar a produtividade.

<br>

3. **O ciclo médio de 166 dias é coerente para culturas de longa duração:**
   - O ciclo médio encontrado indica que o conjunto de dados está mais alinhado com culturas que possuem um período de desenvolvimento mais extenso. Esse número é consistente com culturas como milho safrinha, trigo e soja, reforçando a coerência do dataset.

<br>

4. **Apesar da alta receita total, o lucro por hectare é negativo, sugerindo desequilíbrios entre custo e retorno:**
   - Embora a receita total estimada seja elevada, o lucro por hectare permanece negativo. Isso revela um descompasso entre custos e retorno, indicando que despesas com insumos, mão de obra ou logística podem estar inflacionadas no cenário simulado. Esse desequilíbrio reforça a importância de otimizar processos produtivos e controlar gastos.

<br>

5. **O trigo se destaca como a cultura mais produtiva no dataset:**
   - Entre todas as culturas analisadas, o trigo apresentou os maiores níveis médios e máximos de produtividade. Esse desempenho indica boa adaptação às condições simuladas e maior eficiência no uso da área disponível.

<br>

6. **Os custos médios por cultura são muito altos, influenciando diretamente a rentabilidade:**
   - Os custos médios calculados no dataset são elevados e representam um dos principais fatores responsáveis pelo prejuízo médio. Esse comportamento sugere que certas práticas ou insumos podem estar com valores superestimados ou sendo utilizados de maneira pouco eficiente.

<br>

7. **A qualidade não é afetada pelo uso de pesticida:**
   - A comparação entre lotes que utilizaram pesticidas e os que não utilizaram mostra que o grau de qualidade permanece semelhante. Isso indica que, neste conjunto de dados, o uso de pesticidas não influencia diretamente a classificação final de qualidade — possivelmente porque variáveis relacionadas a pragas e doenças não foram modeladas em profundidade.

<br>

8. **Não existe uma relação clara entre quantidade de chuva e pH do solo:**
   - A análise entre pH e volume de chuva não apresentou correlação significativa. As médias de chuva permanecem estáveis entre as diferentes faixas de pH, indicando que, neste dataset, o regime de chuvas não exerce influência relevante sobre a acidez do solo. Outros fatores, como composição do solo e fertilizantes, parecem ser mais determinantes.


<br>

## 🛠️ **6. Tecnologias e Ferramentas**
- Excel
  - Limpeza de dados
  - Criação de variáveis derivadas
  - Tabelas e cálculos estatísticos
  - Gráficos e análises
- (Opcional futuramente: Power BI, Python, SQL)

<br>

## 📝 **7. Próximos Passos**
- Migrar o projeto para Power BI com visualizações mais ricas
- Criar ranking por cultura, região e qualidade
- Explorar elasticidade de preço e simulações de lucro
- Incluir modelos preditivos (futuro)

<br>

## 🧾 **8. Conclusão do Projeto**

A análise revelou desafios importantes no desempenho agrícola, como a baixa proporção de solos com pH ideal e os elevados custos médios de produção — fatores que contribuem para o prejuízo médio por hectare observado no dataset. Apesar disso, a produtividade geral se mantém em um patamar razoável, com destaque para o trigo como cultura mais eficiente em termos de rendimento.

Os insights também mostram que o uso de pesticidas não afeta diretamente a qualidade, e que não há relação clara entre os níveis de chuva e o pH do solo, indicando que esses fatores podem depender de variáveis externas ou de análises mais específicas.

De forma geral, o estudo fornece um panorama sólido para compreender fatores agronômicos, operacionais e financeiros que influenciam os resultados no setor agrícola. O projeto ainda abre espaço para evoluções futuras, como visualizações em Power BI, análises por região, simulações econômicas e implementação de modelos preditivos.

<br>
