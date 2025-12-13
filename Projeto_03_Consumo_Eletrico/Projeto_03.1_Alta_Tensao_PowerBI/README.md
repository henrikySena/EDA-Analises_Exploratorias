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


# Análise de Correção de Demanda e CAR_INST — BDGD / ANEEL

## 1. Razão Demanda / CAR_INST

**Medida DAX**
```dax
Razao_Demanda_CAR_INST =
DIVIDE([Demanda_Media_2003_2024], [CAR_INST_Media_2003_2024])
```

**Resultado:** 0,62 (62%)

**Interpretação:**
- Valor perfeitamente normal para fator de demanda  
- Indica que as variáveis são consistentes entre si  
- O problema é sistemático (afeta todas as medidas igualmente)

---

## 2. Comparação com Padrões do Setor

| Métrica | Faixa Esperada | Resultado Obtido (Corrigido) | Status |
|------|--------------|-----------------------------|-------|
| Fator de Demanda | 30–80% | 62% | ✅ Normal |
| Demanda Média por UC | 0,5–10 MW | 2,96 MW | ✅ Plausível |
| CAR_INST Média por UC | 1–20 MW | 4,77 MW | ✅ Plausível |

---

## 3. Correção Aplicada

### Determinação do Fator de Correção

- Valor original pós-2002: **2.960.000.000 unidades**
- Valor esperado plausível: **~2.960 kW (2,96 MW)**

**Fator necessário:**  
`2.960.000.000 ÷ 2.960 = 1.000.000`

### Composição do Fator 1.000.000

- Erro documental ANEEL: kW → W (**1.000×**)
- Documentação oficial especifica kW  
- Dados reais estão em W  
- Erro adicional de escala: **1.000×**

**Possíveis causas:**
- Sistema de coleta em mW (milivatt)
- Inserção sistemática de zeros extras
- Problema na conversão entre sistemas

---

## 4. Fórmulas de Correção Implementadas

```dax
// Demanda corrigida para período pós-2002
Demanda_Corrigida_MW =
IF(
    [ANO_CONEXAO] >= 2003,
    [Demanda_Bruta] / 1000000,
    [Demanda_Bruta]
)

// CAR_INST corrigida
CAR_INST_Corrigida_MW =
IF(
    [ANO_CONEXAO] >= 2003,
    [CAR_INST_Bruta] / 1000000,
    [CAR_INST_Bruta]
)
```

---

## 5. Resultados Após Correção

### Valores Corrigidos e Validados

| Métrica | 1960–2002 | 2003–2024 (Corrigido) | Comparação |
|------|----------|---------------------|-----------|
| Demanda Média | 214 kW | 2,96 MW | 13,8× maior |
| CAR_INST Média | Unidade incerta* | 4,77 MW | — |
| Razão Demanda/CAR_INST | 0,13% (anômalo) | 62% (típico) | Métodos diferentes |

\* CAR_INST pré-2002 apresenta unidade não confirmada — uso com ressalva.

### Evolução Real Revelada

- Consumidores pré-2002: **~214 kW** demanda média
- Consumidores pós-2002: **~2,96 MW** demanda média
- Crescimento real: **~14×** em capacidade média

**Interpretação:** reflete maior industrialização e plantas modernas.

---

## 6. Limitações Identificadas

- Razão Demanda/CAR_INST pré-2002: **0,13% (anômalo)**
- Razão Demanda/CAR_INST pós-2002: **62% (normal)**

**Conclusão:** métodos de medição e definição mudaram fundamentalmente.

---

## 7. Implicações para Análise

| Tipo de Análise | 1960–2002 | 2003–2024 |
|---------------|----------|-----------|
| Valores absolutos | ✅ Demanda válida / ⚠️ CAR_INST questionável | ✅ Ambos válidos |
| Comparações temporais | ⚠️ Com ressalvas | ✅ Válidas |
| Relações entre variáveis | ❌ Não confiável | ✅ Confiável |

---

## 8. Abordagem Analítica Recomendada

### Análises Quantitativas
```dax
Demanda_Analise = [Demanda_Corrigida_MW]
CAR_INST_Analise = [CAR_INST_Corrigida_MW]
```

### Análises Qualitativas / Tendências
- Dados brutos podem ser usados para proporções
- Padrões temporais relativos permanecem válidos
- Crescimento da base de UCs não é afetado

### Análises por Período Separado
- **Pré-2002:** tendências históricas
- **Pós-2002:** análises técnicas detalhadas

---

## 9. Cartões Essenciais para Dashboard

- ⚠️ Aviso metodológico (correção aplicada)
- Demanda média pré-2002: **214 kW**
- Demanda média pós-2002: **2,96 MW**
- Fator de crescimento: **13,8×**
- Razão Demanda/CAR_INST pós-2002: **62%**
- Salto metodológico 2003→2004: **+2.233% bruto**

---

## 10. Conclusões e Recomendações

### Para Pesquisadores / Analistas
> Utilize divisor **1.000.000** para dados quantitativos pós-2002.  
> Analise períodos separadamente, reconhecendo mudanças metodológicas.

### Para a ANEEL (se aplicável)
- Revisão da documentação de unidades
- Publicação de nota técnica sobre a descontinuidade
- Correção oficial do dataset

### Para Próximos Blocos do Projeto
- Validar consistência interna
- Comparar com referências setoriais
- Documentar limitações

---

## 11. Valor do Trabalho Realizado

### Contribuições Técnicas
- Identificação de erro sistemático em dados oficiais
- Validação por múltiplas abordagens
- Correção com fator estatístico
- Documentação completa

### Habilidades Demonstradas
- Pensamento crítico em qualidade de dados
- Validação estatística cruzada
- Comunicação técnica clara
- Solução prática para dados imperfeitos

### Impacto Potencial
- Melhoria da qualidade analítica do setor elétrico
- Base para estudos mais confiáveis
- Caso de estudo em qualidade de dados

---

**Documento atualizado em:** Fevereiro de 2024  
*Baseado em análise do dataset ANEEL / BDGD (1960–2024)*  
*Correção validada por consistência interna (Razão Demanda/CAR_INST = 0,62)*
* **Análises econométricas** que dependam de magnitudes.

--- 
<br>



