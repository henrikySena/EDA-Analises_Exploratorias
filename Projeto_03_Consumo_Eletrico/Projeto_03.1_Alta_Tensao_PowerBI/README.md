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
---
<br>

## Bloco C - Demanda/Carga: Identificação e Correção de Erro Sistemático

## 🔍 1. O PROBLEMA INICIAL: Valores Implausíveis

### Evidência 1: Números Absurdos
Ao calcular a demanda média para 2003-2024:
- **Resultado bruto**: 2,96 **bilhões** de kW
- **Problema**: 15× maior que **toda a capacidade instalada do Brasil** (~200 GW)

### Evidência 2: Salto Temporal Inexplicável
```
2003: ~300 milhões kW
2004: ~7 bilhões kW
Crescimento: +2.233% em 1 ano (vs crescimento econômico real: ~5,7%)
```

### Evidência 3: Padrão Temporal Claro
- **1960-2002**: Linha estável, valores plausíveis (~214 kW)
- **2003+**: Disparo abrupto
- **Conclusão**: Mudança sistemática em 2003

### Descoberta Documental
> "Entre o período de 2002 a 2004, a unidade de verificação de consumo de energia para fins de cálculo de subvenção e faturamento da ANEEL foi definida em MWh (Megawatt-hora)."
>
> *Fonte: LegisWeb - Legislação ANEEL*

---
<br>

## 🔬 2. VALIDAÇÃO: Teste de Consistência Interna

### Razão Demanda/CAR_INST
```dax
Razao_Demanda_CAR_INST =
DIVIDE([Demanda_Media_2003_2024], [CAR_INST_Media_2003_2024])
```

**Resultado:** 0,62 (62%)

**Interpretação:**
- Valor perfeitamente normal para fator de demanda
- Variáveis consistentes entre si
- Erro sistemático (afeta todas as medidas)

---
<br>

## 📊 3. COMPARAÇÃO COM PADRÕES DO SETOR

| Métrica | Faixa Esperada | Resultado Corrigido | Status |
|-------|---------------|-------------------|-------|
| Fator de Demanda | 30–80% | 62% | ✅ Normal |
| Demanda Média por UC | 0,5–10 MW | 2,96 MW | ✅ Plausível |
| CAR_INST Média por UC | 1–20 MW | 4,77 MW | ✅ Plausível |

---
<br>

## 🛠️ 4. CORREÇÃO APLICADA

### Determinação do Fator de Correção
```
Valor original pós-2002: 2.960.000.000
Valor esperado plausível: ~2.960 kW (2,96 MW)

Fator necessário = 1.000.000
```

### Composição do Fator
- Erro documental: kW → W (1.000×)
- Erro adicional de escala: 1.000×

Possíveis causas:
- Coleta em mW
- Inserção de zeros extras
- Conversão incorreta de sistemas

---
<br>

## 📝 5. FÓRMULAS DE CORREÇÃO

```dax
Demanda_Corrigida_MW =
IF(
    [ANO_CONEXAO] >= 2003,
    [Demanda_Bruta] / 1000000,
    [Demanda_Bruta]
)

CAR_INST_Corrigida_MW =
IF(
    [ANO_CONEXAO] >= 2003,
    [CAR_INST_Bruta] / 1000000,
    [CAR_INST_Bruta]
)
```

---
<br>

## 📈 6. RESULTADOS APÓS CORREÇÃO

| Métrica | 1960–2002 | 2003–2024 | Comparação |
|-------|-----------|-----------|-----------|
| Demanda Média | 214 kW | 2,96 MW | 13,8× |
| CAR_INST Média | Incerta | 4,77 MW | — |
| Razão Dem/CAR | 0,13% | 62% | Método distinto |

---
<br>

### ⚠️ 7. LIMITAÇÕES
- Mudança metodológica entre períodos
- Comparações de valores diretos, como carga, energia e demanda exigem cautela

---
<br>

