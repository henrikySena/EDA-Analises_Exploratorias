# 🔌 Projeto 03.2 — Média Tensão [PostgreSQL]

## Resumo Executivo

O Projeto 03.2 — Média Tensão integra o Projeto 03 — Consumo Elétrico (BDGD/ANEEL) e tem como objetivo a ingestão, validação, estruturação, análise e modelagem dos dados de Unidades Consumidoras de Média Tensão, cobrindo todo o ciclo analítico — da ingestão e engenharia de dados à geração de insumos analíticos.

O projeto adota uma abordagem incremental e metodologicamente rigorosa, iniciando pela construção de uma base sólida de ingestão e governança e evoluindo progressivamente para validação semântica, tipagem, exploração analítica, modelagem e integração com camadas analíticas superiores, em um contexto caracterizado por maior volume de registros, heterogeneidade histórica de layout, alta incidência de campos condicionais e dados ausentes legítimos, além de um schema significativamente mais amplo e heterogêneo, com conjunto de colunas distinto e mais complexo em relação ao subprojeto 03.1 — Alta Tensão.

Este documento funciona como um relatório vivo, alinhado ao padrão do Projeto 03, documentando decisões técnicas, aprendizados e a evolução do pipeline de dados ao longo de todas as etapas do projeto.

---
<br>

## Ambiente Técnico

### Banco de Dados

- **SGBD:** PostgreSQL 18.1
- **Sistema Operacional:** Windows x64
- **Ferramenta de administração:** pgAdmin 4
- **Database dedicado:** `bdgd_media_tensao`

<br>

### Configurações Gerais

- Porta: `5432`
- Usuário: `postgres`
- Conexão local validada

<br>

## Arquitetura de Ingestão — Visão Geral

A arquitetura adotada segue o princípio de **separação explícita entre dado bruto, estrutura e semântica**:

1. **RAW `(bdgd_media_tensao_raw)`** — preservação integral do CSV
2. **RAW de Validação `(bdgd_media_tensao_raw_v2)`** — verificação controlada do header
3. **STAGE `(bdgd_media_tensao_stage)`** — estruturação com schema explícito baseado no layout oficial

Nenhuma camada assume significado semântico sem validação documental.

---
<br>

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

<br>

### Resultado

- **312.074 registros** carregados
- Primeira linha já corresponde a dados válidos (UC)
- Header não ingerido como dado

---
<br>

## Seção Legacy — Experimento Exploratório Inicial (Descontinuado)

### Contexto

Foi testada inicialmente uma abordagem baseada em:
- ingestão com delimitador fictício;
- coluna única (`linha`);
- quebra posicional em colunas genéricas.

<br>

### Motivo do Abandono

Apesar de válida como experimento exploratório, a abordagem foi **formalmente descartada**, pois:
- não escala para ambientes profissionais;
- introduz ambiguidade semântica;
- dificulta governança e manutenção;
- gera dívida técnica evitável.

Essa estratégia permanece apenas como **registro histórico (legacy)**.

---
<br>

## Etapa 1 — Validação Controlada do Header (RAW Paralela)

### Motivação

Para eliminar qualquer dúvida sobre:
- existência do header no CSV;
- comportamento do pgAdmin durante a importação;
- integridade do arquivo original;

foi executado um **teste controlado, documentado e reproduzível**.

<br>

### Procedimento

- Criação da tabela `bdgd_media_tensao_raw_v2` (1 coluna `linha`)
- Importação do **mesmo CSV**, com:
  - Delimitador: `;`
  - Encoding: `LATIN1`
  - **HEADER = NO**

<br>

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

<br>

### Conclusão Técnica

- O CSV **possui header explícito**
- O comportamento anterior do pgAdmin foi **correto e esperado**
- O header foi tratado como **metadado**, não como dado

Essa validação fundamenta toda a estruturação posterior.

---
<br>

## Etapa 2 — Criação da Camada STAGE (Schema Explícito)

### Princípio Central

> Em projetos de dados reais, **colunas genéricas (`col_1`, `col_2`, …)** não são aceitáveis quando o schema original existe.

A camada *stage* deve **preservar semântica, governança e rastreabilidade**.

<br>

### Abordagem Adotada

- Criação da tabela *stage* **diretamente a partir do header validado do CSV**
- Correspondência **1:1** entre colunas da *stage* e colunas oficiais do arquivo
- Tipos definidos inicialmente como `TEXT`

<br>

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
<br>

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

<br>

### Exemplo de Resultado (recorte ilustrativo)

| COD_ID_ENCR | DIST | PN_CON | PAC | CTMT | MUN | GRU_TAR | SIT_ATIV | DAT_CON | DEM_01 | ENE_01 | DATA_BASE |
|------------|------|--------|-----|------|-----|---------|----------|---------|--------|--------|-----------|
| D485CC53… | 385 | 2105244401 | SRP092105244401 | SRP01_AL009 | 3547502 | A4 | AT | 30/09/2017 | 115.584 | 104877 | 31DEC2024 |

> *Valores apresentados apenas para fins ilustrativos; o dataset completo contém centenas de milhares de registros.

<br>

A execução da camada stage resultou em um schema estruturado, semanticamente explícito e fiel ao layout original do CSV, com as seguintes garantias técnicas:

- ✔ Header validado empiricamente e utilizado como fonte única de definição do schema
- ✔ Estratégia de ingestão consolidada, com separação clara entre RAW e STAGE
- ✔ Dado bruto preservado integralmente na camada RAW
- ✔ Tabela STAGE criada com schema explícito, sem uso de colunas genéricas
- ✔ Número de colunas e ordem validados, garantindo correspondência 1:1 com o CSV
- ✔ Integridade da carga confirmada, sem perdas ou desalinhamentos

Com isso, a camada stage passa a atuar como base estrutural confiável para as próximas etapas de tipagem, validação semântica e modelagem analítica.

---
<br>


## Etapa 3 — Validação Semântica Oficial (Layout UCMT — PRODIST / ANEEL)

### Motivação

Após a validação empírica do header e a consolidação da camada *stage* com schema explícito, tornou-se necessário **confirmar formalmente a semântica de cada campo** presente no dataset, garantindo aderência total ao **modelo regulatório oficial da ANEEL**.

Essa etapa visa eliminar ambiguidades interpretativas, especialmente em campos codificados, métricas elétricas e variáveis temporais de demanda e energia.

<br>

### Referencial Normativo Utilizado

Foi identificado e isolado o **layout oficial da entidade UCMT (Unidade Consumidora de Média Tensão)**, conforme definido nos **Procedimentos de Distribuição (PRODIST)** da ANEEL:

- Entidade: **UCMT — Unidade Consumidora de Média Tensão**
- Modelagem: **UCMT**
- Tipo geométrico: **Ponto**
- Documento: **Estrutura da Base de Dados Geográfica da Distribuidora — BDGD**
- Seção: **Anexo I**
- Revisão: **2**
- Vigência: **01/01/2021**

Esse layout constitui o **dicionário de dados oficial** para interpretação semântica do conjunto analisado.

<br>

### Estrutura Conceitual da Entidade UCMT

O layout UCMT define **53 campos oficiais**, organizáveis conceitualmente nos seguintes blocos funcionais:

- **Identificação e Topologia Elétrica**  
  Campos responsáveis pela identificação única da unidade consumidora e sua associação à rede de distribuição (ex.: `COD_ID`, `DIST`, `CTMT`, `SUB`, `CONJ`).

- **Localização Geográfica e Enquadramento Territorial**  
  Informações de município, endereço e área regulatória, utilizadas para análises espaciais e critérios de continuidade (ex.: `MUN`, `LGRD`, `BRR`, `CEP`, `ARE_LOC`).

- **Perfil Econômico, Tarifário e Técnico**  
  Campos que caracterizam o enquadramento econômico, tarifário e elétrico da unidade consumidora (ex.: `CNAE`, `CLAS_SUB`, `GRU_TAR`, `GRU_TEN`, `TEN_FORN`, `TIP_CC`, `FAS_CON`, `LIV`).

- **Características Elétricas Declaradas**  
  Informações de carga instalada conforme cadastro da distribuidora (ex.: `CAR_INST`).

- **Séries Temporais de Demanda e Energia**  
  Conjuntos de campos mensais (`DEM_01` a `DEM_12`, `ENE_01` a `ENE_12`) que representam, respectivamente:
  - demanda ativa máxima (kW);
  - energia ativa consumida (kWh);  
  conforme valores medidos, faturados ou estimados, seguindo regras explícitas do PRODIST.

- **Indicadores de Continuidade do Serviço**  
  Métricas regulatórias anuais de interrupção individual (ex.: `DIC`, `FIC`).

- **Campos de Controle e Observação**  
  Indicadores auxiliares e descrição livre do registro (ex.: `SEMRED`, `DESCR`).

<br>

### Introdução Visual — Evidência Documental do Layout Oficial

Para reforçar a rastreabilidade metodológica e destacar o alinhamento com o modelo regulatório, foram extraídos os **dois primeiros quadros descritivos do layout UCMT (Unidades Consumidoras de Média Tensão) ** diretamente do manual oficial, convertidos em imagens e incorporados ao README como evidência visual do dicionário de dados adotado.

![Dicionário UCMT 01](./Images/ucmt_dicionario1.jpg)
![Dicionário UCMT 01](./Images/ucmt_dicionario2.jpg)


Esses registros visuais documentam:
- a definição formal da entidade UCMT;
- a listagem oficial dos campos e seus significados;
- o enquadramento regulatório utilizado como base semântica do projeto.

<br>

### Conclusão Técnica

A validação semântica confirmou que:

- o dataset analisado está conceitualmente alinhado à entidade **UCMT** definida pela ANEEL;
- a camada *stage* preserva integralmente a semântica oficial dos campos;
- o manual PRODIST passa a atuar como **fonte normativa explícita**, complementando a validação empírica realizada nas etapas anteriores.

Com isso, o projeto passa a dispor de **base estrutural e semântica sólida**, apta para as próximas fases de:
- tipagem definitiva;
- validação de domínios;
- modelagem analítica e exploração dos dados.
