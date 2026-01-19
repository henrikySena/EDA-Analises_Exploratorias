# 🔌 Projeto 03.2 — Média Tensão [PostgreSQL]

## Resumo Executivo

O **Projeto 03.2 — Média Tensão** integra o **Projeto 03 — Consumo Elétrico (BDGD/ANEEL)** e tem como objetivo a **ingestão, validação estrutural e preparação analítica** dos dados de **Unidades Consumidoras de Média Tensão**, com foco em **engenharia de dados, rastreabilidade e governança**.

Nesta fase, o projeto está **deliberadamente restrito à camada de ingestão e estruturação**, evitando qualquer modelagem prematura. Todas as decisões técnicas são documentadas para garantir **reprodutibilidade**, **auditabilidade** e **clareza metodológica**.

Este documento funciona como um **relatório vivo**, alinhado ao padrão do **Projeto 03.1 — Alta Tensão**, incorporando aprendizados práticos específicos do contexto de Média Tensão.

---

## Contexto do Projeto

O Projeto 03 foi estruturado em três frentes analíticas complementares:

- **03.1 — Alta Tensão** (Power BI) ✔️
- **03.2 — Média Tensão** (PostgreSQL) 🔄 *(este projeto)*
- **03.3 — Baixa Tensão** (planejado)

O subprojeto **03.2** aprofunda a abordagem de engenharia de dados, lidando com:
- maior volume de registros;
- maior heterogeneidade histórica de layout;
- maior incidência de campos condicionais e dados ausentes legítimos.

---

## Ambiente Técnico

### Banco de Dados

- **SGBD:** PostgreSQL 18.1
- **Sistema Operacional:** Windows x64
- **Ferramenta de administração:** pgAdmin 4
- **Database dedicado:** `bdgd_media_tensao`

### Configurações Gerais

- Porta: `5432`
- Usuário: `postgres`
- Conexão local validada

---

## Arquitetura de Ingestão — Visão Geral

A arquitetura adotada segue o princípio de **separação explícita entre dado bruto, estrutura e semântica**:

1. **RAW** — preservação integral do CSV
2. **RAW de Validação** — verificação controlada do header
3. **STAGE** — estruturação com schema explícito baseado no layout oficial

Nenhuma camada assume significado semântico sem validação documental.

---

## Etapa 0 — Ingestão RAW (Versão Oficial)

### Objetivo

Garantir a ingestão **integral e fiel** do dataset de Média Tensão, sem transformação, limpeza ou inferência estrutural.

### Implementação

- Criação da tabela `bdgd_media_tensao_raw`
- Estrutura: **1 coluna (`linha` TEXT)**
- Importação do CSV com:
  - Delimitador real: `;`
  - Encoding: `LATIN1`
  - **HEADER = YES**

### Resultado

- **312.074 registros** carregados
- Primeira linha já corresponde a dados válidos (UC)
- **Possível perda de informação (Headers)**

---

## Seção Legacy — Abordagem Inicial (Descontinuada)

### Contexto

Foi testada inicialmente uma abordagem baseada em:
- ingestão com delimitador fictício;
- coluna única (`linha`);
- quebra posicional em colunas genéricas.

### Motivo do Abandono

Apesar de válida como experimento exploratório, a abordagem foi **formalmente descartada**, pois:
- não escala para ambientes profissionais;
- introduz ambiguidade semântica;
- dificulta governança e manutenção;
- gera dívida técnica evitável.

Essa estratégia permanece apenas como **registro histórico (legacy)**.

---

## Etapa 1 — Validação Controlada do Header (RAW Paralela)

### Motivação

Para eliminar qualquer dúvida sobre:
- existência do header no CSV;
- comportamento do pgAdmin durante a importação;
- integridade do arquivo original;

foi executado um **teste controlado, documentado e reproduzível**.

### Procedimento

- Criação da tabela `bdgd_media_tensao_raw_v2` (1 coluna `linha`)
- Importação do **mesmo CSV**, com:
  - Delimitador: `;`
  - Encoding: `LATIN1`
  - **HEADER = NO**

### Evidência Empírica

Consulta executada:

```sql
SELECT *
FROM bdgd_media_tensao_raw_v2
LIMIT 2;
```

Resultado observado:
- Primeira linha contendo os nomes das colunas (`COD_ID_ENCR;DIST;PN_CON;...;POINT_Y`)
- Segunda linha iniciando os dados reais

### Conclusão Técnica

- O CSV **possui header explícito**
- O comportamento anterior do pgAdmin foi **correto e esperado**
- O header foi tratado como **metadado**, não como dado

Essa validação fundamenta toda a estruturação posterior.

---

## Etapa 2 — Criação da Camada STAGE (Schema Explícito)

### Princípio Central

> Em projetos de dados reais, **colunas genéricas (`col_1`, `col_2`, …)** não são aceitáveis quando o schema original existe.

A camada *stage* deve **preservar semântica, governança e rastreabilidade**.

### Abordagem Adotada

- Criação da tabela *stage* **diretamente a partir do header validado do CSV**
- Correspondência **1:1** entre colunas da *stage* e colunas oficiais do arquivo
- Tipos definidos inicialmente como `TEXT`

### Query — Criação da Tabela *Stage*

```sql
CREATE TABLE bdgd_media_tensao_stage (
    COD_ID_ENCR TEXT,
    DIST TEXT,
    PN_CON TEXT,
    PAC TEXT,
    CTMT TEXT,
    UNI_TR_AT TEXT,
    SUB TEXT,
    CONJ TEXT,
    MUN TEXT,
    CEG_GD TEXT,
    LGRD TEXT,
    BRR TEXT,
    CEP TEXT,
    CLAS_SUB TEXT,
    CNAE TEXT,
    TIP_CC TEXT,
    FAS_CON TEXT,
    GRU_TEN TEXT,
    TEN_FORN TEXT,
    GRU_TAR TEXT,
    SIT_ATIV TEXT,
    DAT_CON TEXT,
    CAR_INST TEXT,
    LIV TEXT,
    ARE_LOC TEXT,
    TIP_SIST TEXT,
    DEM_CONT TEXT,
    DEM_01 TEXT,
    DEM_02 TEXT,
    DEM_03 TEXT,
    DEM_04 TEXT,
    DEM_05 TEXT,
    DEM_06 TEXT,
    DEM_07 TEXT,
    DEM_08 TEXT,
    DEM_09 TEXT,
    DEM_10 TEXT,
    DEM_11 TEXT,
    DEM_12 TEXT,
    ENE_01 TEXT,
    ENE_02 TEXT,
    ENE_03 TEXT,
    ENE_04 TEXT,
    ENE_05 TEXT,
    ENE_06 TEXT,
    ENE_07 TEXT,
    ENE_08 TEXT,
    ENE_09 TEXT,
    ENE_10 TEXT,
    ENE_11 TEXT,
    ENE_12 TEXT,
    DIC_01 TEXT,
    DIC_02 TEXT,
    DIC_03 TEXT,
    DIC_04 TEXT,
    DIC_05 TEXT,
    DIC_06 TEXT,
    DIC_07 TEXT,
    DIC_08 TEXT,
    DIC_09 TEXT,
    DIC_10 TEXT,
    DIC_11 TEXT,
    DIC_12 TEXT,
    FIC_01 TEXT,
    FIC_02 TEXT,
    FIC_03 TEXT,
    FIC_04 TEXT,
    FIC_05 TEXT,
    FIC_06 TEXT,
    FIC_07 TEXT,
    FIC_08 TEXT,
    FIC_09 TEXT,
    FIC_10 TEXT,
    FIC_11 TEXT,
    FIC_12 TEXT,
    SEMRED TEXT,
    DESCR TEXT,
    DATA_BASE TEXT,
    POINT_X TEXT,
    POINT_Y TEXT
);
```

### Validações Estruturais Executadas

```sql
SELECT COUNT(*) 
FROM information_schema.columns
WHERE table_name = 'bdgd_media_tensao_stage';
```

```sql
SELECT
    ordinal_position,
    column_name
FROM information_schema.columns
WHERE table_name = 'bdgd_media_tensao_stage'
ORDER BY ordinal_position;
```

```sql
SELECT COUNT(*) FROM bdgd_media_tensao_stage;
```

```sql
SELECT *
FROM bdgd_media_tensao_stage
LIMIT 5;
```

Essas queries confirmaram:
- número correto de colunas;
- ordem fiel ao CSV original;
- carga íntegra dos registros;
- alinhamento estrutural perfeito.

---

## Status Atual do Projeto

- ✔ Header validado empiricamente
- ✔ Estratégia de ingestão consolidada
- ✔ RAW preservada
- ✔ STAGE criada com schema explícito
- ✔ Ordem e integridade conferidas

---

## Próximos Passos

1. Tipagem progressiva das colunas
2. Validação semântica via dicionário BDGD
3. Criação de camadas analíticas
4. Integração com análises posteriores

---

## Observações Metodológicas Finais

Este projeto adota princípios de **engenharia de dados profissional**:
- nada é assumido sem evidência;
- erros são documentados, não escondidos;
- decisões são rastreáveis;
- pipelines são defensáveis.

O ambiente está estável e pronto para evolução.
