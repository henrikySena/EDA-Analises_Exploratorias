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

## Bloco C - Demanda/Carga: Análise e Comparação Histórica

## 🔍 1. Contexto e Problema Inicial

Durante a análise da demanda das Unidades Consumidoras (UCs), identificamos valores **implausíveis e discrepâncias históricas**:

### Evidência 1: Números Absurdos
- Período 2003–2024: Demanda média atingiu valores muito superiores ao esperado (~5,13 Bi kW), muito acima da capacidade instalada real do Brasil.

### Evidência 2: Salto Temporal Inexplicável
```
2003: ~300 milhões kW
2004: ~7 bilhões kW
Crescimento: +2.233% em 1 ano (vs crescimento econômico real: ~5,7%)
```

### Evidência 3: Padrão Histórico Diferente
- **1960–2002**: Valores estáveis e plausíveis (~820 mil kW)
- **2003+**: Disparo abrupto, indicando mudança de metodologia ou unidade

### Descoberta Documental Importante
> "Entre o período de 2002 a 2004, a unidade de verificação de consumo de energia para fins de cálculo de subvenção e faturamento da ANEEL foi definida em MWh (Megawatt-hora)."
>
> *Fonte: LegisWeb - Legislação ANEEL*

---

## 🔬 2. KPIs e Validação Interna

KPIs utilizados para ambos os períodos (1960–2002 e 2003–2024):
- Demanda Média por UC
- Demanda Mediana por UC
- Carga Instalada (CAR_INST) Média por UC
- Razão Demanda / CAR_INST

### Teste de Consistência
```dax
Razao_Demanda_CAR_INST = DIVIDE([Demanda_Media], [CAR_INST_Media])
```
**Interpretação:**
- C1 (1960–2002): 0,01 → baixa utilização histórica
- C2 (2003–2024): 0,85 → demanda próxima à capacidade instalada, valores plausíveis pós-ajuste

---

## 📊 3. Comparação Histórica e Setorial

| Métrica | 1960–2002 | 2003–2024 | Observação |
|---------|------------|-----------|------------|
| Demanda Média por UC | 820,64 mil kW | 5,13 Bi kW | Mudança de escala e metodologia |
| Demanda Mediana por UC | 54,85 mil kW | 64,30 mil kW | Valores mais consistentes no período recente |
| CAR_INST Média por UC | 168,41 Mi kW | 5,99 Bi kW | Aumento de capacidade instalada |
| Razão Demanda/CAR_INST | 0,01 | 0,85 | Diferença entre períodos históricos e recentes |

---

## 🛠️ 4. Observações sobre a Mudança de Unidade

- Dados históricos (C1) provavelmente estão em W ou kW conforme cadastro original
- Dados recentes (C2) estão em kW
- Alteração da unidade pela ANEEL entre 2002–2004 (de W/kW para MWh) deve ser considerada para interpretação
- Os dados foram mantidos **na forma original do dataset** para evidenciar possíveis inconsistências de registro

---

## 📈 5. Gráficos e Visualizações

- Linha temporal de Demanda Média e Mediana por UC
- Segmentação por `ANO_CONEXAO`, `LIV_Status` e `TIP_SIST`
- KPIs destacados no topo do relatório
- Outliers identificados e analisados para contexto histórico

---

## ⚠️ 6. Limitações

- Mudança metodológica entre períodos
- Comparações diretas devem considerar diferenças de unidade
- Razão Demanda/CAR_INST muito baixa em C1 indica **consumo histórico médio**, não máximo

---

## ✅ 7. Conclusão Técnica

- **C1**: dados históricos consistentes, referência para comparação
- **C2**: dados recentes plausíveis, evidenciam mudanças metodológicas e possíveis erros de captação
- Bloco C estruturado em **C1 e C2**, com KPIs unificados e comparação direta, permitindo **validação das hipóteses sobre inconsistências históricas**



