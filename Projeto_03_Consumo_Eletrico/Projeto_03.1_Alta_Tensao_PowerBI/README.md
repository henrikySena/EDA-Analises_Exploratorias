## Bloco A — Identificação da UC

- Criamos mapas de **Total de UCs por CEP** e **por UF**  
- Tratamos o campo **UF** a partir do código do município (MUN)  
- Observamos que **SP e MG concentram ~80% das UCs**  

---
<br>

## Bloco B — Tipo de Sistema (TIP_SIST) e LIV
- Visualizamos **TIP_SIST** com gráfico de rosca → 93,78% de Rede Interligada
- Adicionamos **LIV** (Consumidor Livre / Cativo)
  - Criamos a coluna **LIV_Status** para legenda legível  
  - Gráfico de rosca agora **interativo** com outros gráficos  
- Criamos base para futuros gráficos de **TIP_SIST × LIV** ou distribuição por UF

---
<br>

## Bloco C - Demanda / Carga

### ⚠️ Descoberta Crítica: Descontinuidade Metodológica em Dados de Consumo

---

### 🚨 PROBLEMA IDENTIFICADO: DESCONTINUIDADE METODOLÓGICA CRÍTICA

Foi detectada uma **mudança abrupta e significativa na escala dos valores de dados** a partir do ano de **2003**, indicando uma provável **descontinuidade metodológica, erro sistemático de unidade** e/ou **acumulação histórica indevida** na coleta ou registro.

---

### 📈 COMPARAÇÃO ESTATÍSTICA CONFIRMATÓRIA

| Período | Métrica Analisada | Resultado | Interpretação |
| :--- | :--- | :--- | :--- |
| **1960-2002** | Demanda média por UC (período confiável) | 214.360 unidades | ~214 **kW** ⚡ (Valor **plausível** para consumidores de alta tensão) |
| **2003-2024** | Demanda média por UC (período questionável) | 2,96 bilhões de unidades | ~2,96 **GW** ⚡ (Valor **absurdo** – equivalente a 30 usinas médias) |
| **Razão (Pós/Pré)** | Fator de multiplicação | **≈ 13.800:1** | 🚨 **Confirma** erro sistemático de escala |

---
<br>

### 🔍 EVIDÊNCIAS CONCRETAS DA DESCONTINUIDADE

### 1. Análise por Exemplos Pontuais

| Período | Exemplo de Valor Típico | Unidade (Suspeita) | Plausibilidade |
| :--- | :--- | :--- | :--- |
| **1960-2002** | 402 kW (e.g., 1989) | kW | ✅ **ALTA** (Estáveis e realistas) |
| **2003 em diante** | 32,66 MW (e.g., 2009) | MW | ❌ **QUESTIONÁVEL** (2-3 ordens de grandeza acima) |

### 2. Análise Agregada (Médias)

| Período | Demanda Média por UC | Comparação com Realidade | Conclusão |
| :--- | :--- | :--- | :--- |
| **1960-2002** | 214 kW | Consistente com consumidores industriais médios | **DADOS VÁLIDOS** |
| **2003-2024** | 2,96 GW | 30-300× maior que o esperado | **DADOS COMPROMETIDOS** |

### 3. Análise Gráfica

* **Gráfico temporal:** Linha **plana e estável** (1960-2002) → **disparo abrupto** (2003) .
* **Mediana vs Média:** Diferença de **337×** em 2009 (indica *outliers* extremos).
* **Distribuição:** Assimetria extrema no período pós-2003.

---
<br>

### 🧪 HIPÓTESES INVESTIGADAS E CONFIRMADAS

| Hipótese | Evidência | Status |
| :--- | :--- | :--- |
| **✅ Hipótese 1: Mudança de Unidade (kW → W)** | Fator ~1.000× entre kW e W | **Forte Indicação** (explica parte do fator) |
| **✅ Hipótese 2: Acumulação Histórica** | Fator adicional ~14× além da mudança de unidade | **Razoavelmente provável** |
| **✅ Hipótese 3: Preenchimento Retroativo** | Valores aumentam com idade da UC | **Provável em partes** |
| **✅ Hipótese 4: Erro Sistêmico na Migração** | Mudança abrupta em ano específico (2003) | **Consistente com os dados** |

*Cenário plausível: UC conectada em 2010 recebeu dados acumulados desde 1980.*
*Contexto histórico: Atualizações de sistemas governamentais em ~2003.*

---
<br>

### 📊 IMPACTO NAS ANÁLISES DO BLOCO C:

### ✅ O que ainda é válido:

* **Tendências relativas** dentro de cada período homogêneo.
* **Proporções categóricas** (LIV=1 vs LIV=0, classes, etc.).
* **Padrões qualitativos** (ponta vs fora de ponta).
* **Crescimento da base** (contagem de UCs).

### ❌ O que está comprometido:

* **Valores absolutos** de demanda/consumo.
* **Comparações temporais diretas** (ex: 2009 vs 2024).
* **Totais do sistema** (inflados por erros).
* **Análises econométricas** que dependam de magnitudes.

--- 
<br>


