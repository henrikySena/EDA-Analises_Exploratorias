## Bloco C - Demanda / Carga

### ⚠️ Descoberta Crítica: Descontinuidade Metodológica em Dados de Consumo

---

### 🚨 Problema Identificado

Foi detectada uma **mudança abrupta e significativa na escala dos valores de dados** a partir do ano de **2003**, indicando uma provável **descontinuidade metodológica** ou **erro de unidade** na coleta ou registro.

### Comparação de Valores Típicos

| Período | Exemplo de Valor Típico | Unidade (Suspeita) | Plausibilidade |
| :--- | :--- | :--- | :--- |
| **1960-2002** | 402 kW (e.g., 1989) | kW | **✅ Alta** (Estáveis e realistas) |
| **2003 em diante** | 32,66 MW (e.g., 2009) | MW | **❌ Questionável** (2-3 ordens de grandeza acima) |

---

### 📊 Evidências da Discrepância

1.  **Estabilidade Histórica (1960-2002):**
    * Os valores anteriores a 2003 mostram uma **linha de base estável e realista** em gráficos, condizente com o consumo típico em **quilowatts (kW)**.
2.  **Salto de Escala (2003):**
    * Ocorre um **salto de 2 a 3 ordens de magnitude** (passando de kW para MW) exatamente no ano de **2003**.
3.  **Incompatibilidade com a Realidade:**
    * Um valor de **32,66 MW** para uma **Unidade Consumidora (UC) típica** é fisicamente e economicamente implausível.
    * Considerando a capacidade total do Sistema Elétrico Brasileiro (aproximadamente **~200 GW**), esse valor implicaria um número total de UCs de apenas **~6.000**, o que contradiz a existência de **milhões de UCs** no país.

---

### 🧪 Hipóteses Investigadas para o Desvio

| Hipótese | Descrição e Teste | Evidências / Resultado |
| :--- | :--- | :--- |
| **1. Erro de Unidade** | O manual indica `kW`, mas os dados pós-2003 poderiam estar em **Watts (W)**. | **Teste:** $32.660.000 \text{ W} = 32.660 \text{ kW} = 32,66 \text{ MW}$. **Resultado:** O valor de 32,66 MW permanece alto demais, descartando W como a unidade correta. |
| **2. Acumulação Histórica** | Os dados pós-2003 estariam representando o **consumo acumulado** desde a data de conexão da UC. | **Evidência:** Os valores **crescem consistentemente com a idade da UC**. Uma UC conectada em 1980 apresenta um valor maior que uma conectada em 2005, entretanto, o gráfico não possui formato típico de acúmlo, descartando tal hipótese. |
| **3. Mudança Metodológica ANEEL** | Em 2003, houve uma **nova sistemática de coleta ou relatório** da ANEEL. | **Problema:** Essa mudança pode ter resultado na **conversão incorreta ou despadronizada** de dados históricos, gerando a incompatibilidade entre períodos. |
| **4. Preenchimento Retroativo** | Cenário de **preenchimento de "buracos"** no histórico: uma UC conectada em 2015 possui valores desde 1990. | **Efeito:** Isso inflaciona artificialmente os valores nos anos mais recentes, especialmente se o método de preenchimento foi falho - o que faz sentido quando nos deparamos com dados que iniciaram na casa das centenas e terminaram na casa de bilhões. |

