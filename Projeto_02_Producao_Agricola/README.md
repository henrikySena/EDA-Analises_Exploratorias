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
### 🔹 BLOCO 1 — Produtividade (Desempenho Agrícola)

![Dashboard](https://raw.githubusercontent.com/henrikySena/EDA-Analises_Exploratorias/main/Projeto_02_Producao_Agricola/BLOCO-01_Dashboard.png)
![KPI](https://raw.githubusercontent.com/henrikySena/EDA-Analises_Exploratorias/main/Projeto_02_Producao_Agricola/BLOCO-01_KPI.png)


### Este bloco apresenta uma análise do desempenho produtivo das culturas, considerando diferenças regionais, manejo e características agronômicas.

<br>

- Embora o trigo tenha sido a cultura **mais produtiva**, ele não foi a **mais lucrativa**. Esse contraste pode estar relacionado a fatores como manejo eficiente, resistência natural a pragas e variações climáticas, adaptação a solos mais ácidos ou secos, e maior capacidade de transformar fertilizantes e recursos hídricos em biomassa.

- É importante ressaltar que produtividade elevada nem sempre se converte em lucratividade, já que custos operacionais, preço de mercado, sensibilidade logística e nível de mecanização podem reduzir o retorno financeiro.

Em um dataset didático, essa dinâmica aparece de forma simplificada, mas ancora tendências presentes no setor agrícola real.

<br>

---

### 🔹 BLOCO 2 — Qualidade do Produto

![Dashboard](https://raw.githubusercontent.com/henrikySena/EDA-Analises_Exploratorias/main/Projeto_02_Producao_Agricola/BLOCO-02_Dashboard.png)
![KPI](https://raw.githubusercontent.com/henrikySena/EDA-Analises_Exploratorias/main/Projeto_02_Producao_Agricola/BLOCO-02_KPI.png)

### Este bloco examina elementos associados à qualidade das safras e fatores que poderiam influenciá-la, como uso de pesticidas, método de irrigação e condições ambientais.

<br>

- A análise da relação entre a média qualitativa (A, B e C) e o uso de pesticidas não mostrou associação significativa no dataset. É importante reforçar que estes dados são didáticos e não refletem comportamentos reais já que na agricultura prática, o controle de pragas tem impacto direto sobre perdas, padrão do grão e qualidade final.

- O estudo também avaliou a influência dos métodos de irrigação na produtividade, comparando sequeiro, pivô central, gotejamento e aspersão. De forma inesperada, o sequeiro apresentou o melhor desempenho, resultado que contraria evidências agronômicas reais - já que o gotejamento tende a ser o método mais eficiente em aproveitamento hídrico e estabilidade produtiva. A explicação está na própria natureza do dataset, construído para fins de EDA e sem compromisso com tendências agronômicas reais.

Por fim, foi investigada a relação entre quantidade de chuvas e pH do solo, considerando a hipótese de que maior precipitação poderia aumentar a *lixiviação* e alterar a acidez. Não foi observada correlação entre essas variáveis, sugerindo que, no dataset, o pH depende mais de características do solo do que de fatores climáticos.

<br>

---

### 🔹 BLOCO 3 — Receita, Lucro e Custos
![Dashboard](https://raw.githubusercontent.com/henrikySena/EDA-Analises_Exploratorias/main/Projeto_02_Producao_Agricola/BLOCO-03_Dashboard.png)
![KPI](https://raw.githubusercontent.com/henrikySena/EDA-Analises_Exploratorias/main/Projeto_02_Producao_Agricola/BLOCO-03_KPI.png)

### O bloco em questão avalia os resultados financeiros do dataset, explorando a relação entre custos operacionais, rendimento e retorno econômico das culturas.

<br>

- O gráfico de **Lucro por Região e Tipo de Cultura** mostra que a maioria das culturas apresentou lucro negativo, com exceção do feijão, que ficou próximo do ponto de equilíbrio, e do café, que se destacou como a cultura mais lucrativa — ainda que com valores discretos, condizentes com o caráter didático dos dados.

- A receita estimada total foi de **R$ 852.912.851,51**, mas o lucro médio por hectare atingiu **R$ –217,23**, evidenciando inconsistências operacionais, possível mau manejo ou subaproveitamento de recursos. Esse desempenho negativo pode estar ligado a custos excessivos, perdas elevadas, baixa eficiência no uso de insumos ou problemas de planejamento agronômico. Tal ocorrêcia, em um cenário realístico, necessitaria de averiguação e pesquisa imediata.

- O custo médio por cultura, de **R$ 191.937,50**, reforça a necessidade de investigar desequilíbrios entre investimento e retorno. Em um cenário real, valores tão altos combinados com produtividade moderada tendem a comprometer a margem de lucro. Aqui, esse comportamento é um reflexo da natureza do dataset, destacando a importância de análises criteriosas de custo-benefício.

Mesmo em caráter fictício, o café permanece como a cultura mais lucrativa, o que acompanha tendências reais de mercado em que o produto costuma manter alta valorização devido à forte demanda e preço agregado.

<br>

---

### 🔹 BLOCO 4 — Eficiência Operacional
![KPI](https://raw.githubusercontent.com/henrikySena/EDA-Analises_Exploratorias/main/Projeto_02_Producao_Agricola/BLOCO-04_KPI.png)

### Por fim, este bloco aborda variáveis relacionadas ao desempenho operacional das fazendas, com foco em tempo e uso do espaço.

<br>

- O ciclo médio de cultivo, estimado em **6 meses**, indica um período relativamente uniforme entre plantio e colheita, sugerindo estruturas produtivas com ritmos semelhantes entre culturas. Em análises reais, essa métrica é essencial para planejamento de safras, rotação de culturas e otimização do calendário agrícola.

A área média plantada reflete o porte médio das operações representadas no dataset. Esse indicador é relevante para entender a escala produtiva, avaliar a viabilidade econômica e comparar resultados entre propriedades de diferentes tamanhos.

<br>

---

## 📈 **4. Principais Insights**   
1. **Apenas um terço dos solos possui pH ideal, indicando necessidade de correção:**
   - A maioria dos solos é levemente ácida, o que reduz a eficiência na absorção de nutrientes. Isso indica necessidade de correção, como calagem, para melhorar a fertilidade e elevar a produtividade.

<br>

2. **Apesar da alta receita total, o lucro por hectare é negativo, sugerindo desequilíbrios entre custo e retorno:**
   - Mesmo com receita elevada, os custos superam o retorno. Isso sugere gastos inflacionados com insumos, logística ou mão de obra, reforçando a necessidade de otimização operacional.

<br>

3. **O trigo se destaca como a cultura mais produtiva no dataset:**
   - Apresenta os maiores valores médios e máximos de produtividade, indicando bom desempenho e maior eficiência no uso da área.

<br>

4. **A qualidade não é afetada pelo uso de pesticida:**
   - A qualidade permanece semelhante entre lotes com e sem pesticidas, indicando ausência de impacto direto neste dataset — possivelmente pela falta de variáveis sobre pragas/doenças.

<br>

5. **Não existe uma relação clara entre quantidade de chuva e pH do solo:**
   - A média de chuva se mantém estável entre diferentes faixas de pH, mostrando ausência de correlação. Outros fatores, como tipo de fertilizante, parecem mais determinantes.

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
- Migrar o projeto para Power BI com visualizações e filtragens mais ricas
- Explorar elasticidade de preço e simulações de lucro
- Incluir modelos preditivos (futuro)

<br>

## 🧾 **7. Conclusão do Projeto**

A análise exploratória permitiu compreender os principais aspectos produtivos, qualitativos e financeiros do dataset agrícola. Mesmo sendo um conjunto de dados didático, as etapas de limpeza, criação de métricas e visualizações revelaram padrões importantes, seguindo os preceitos fundamentais das etapas aplicadas em uma análise de dados."

- O trigo destacou-se em produtividade, enquanto o café apresentou a maior lucratividade. Por outro lado, a maior parte das culturas registrou lucro negativo, evidenciando desequilíbrios entre custos e retorno — um ponto que reforça a importância de avaliar manejo, uso de insumos e planejamento operacional.

- As análises de qualidade e irrigação não mostraram relações consistentes, refletindo a natureza sintética do dataset, mas servindo para ilustrar como esse tipo de investigação é essencial em cenários reais.

- Iindicadores de eficiência, como ciclo médio de cultivo e área plantada, complementaram a visão geral do sistema produtivo.

O projeto demonstra como uma EDA estruturada pode apoiar decisões no contexto agropecuário, integrando produtividade, qualidade e resultados econômicos de forma clara e acessível.

<br>
