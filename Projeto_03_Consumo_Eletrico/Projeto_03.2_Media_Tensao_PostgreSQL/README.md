# 🔌 Projeto 03.2 — Média Tensão [PostgreSQL]

## Resumo Executivo

O **Projeto 03.2 — Média Tensão** integra o **Projeto 03 — Consumo Elétrico (BDGD/ANEEL)** e tem como objetivo a **ingestão, validação estrutural e preparação analítica** dos dados de **Unidades Consumidoras de Média Tensão**, com foco em **engenharia de dados, rastreabilidade e governança**.

Nesta fase, o projeto está **deliberadamente restrito à camada de ingestão e estruturação**, evitando qualquer modelagem prematura. Todas as decisões são documentadas para garantir **reprodutibilidade técnica** e **clareza metodológica**.

Este documento funciona como um **relatório vivo**, seguindo o mesmo padrão narrativo do **Projeto 03.1 — Alta Tensão**, porém incorporando aprendizados práticos específicos do contexto de Média Tensão.

---

## Contexto do Projeto

O Projeto 03 foi estruturado em três frentes analíticas complementares:

- **03.1 — Alta Tensão** (Power BI) ✔️
- **03.2 — Média Tensão** (PostgreSQL) 🔄 *(este projeto)*
- **03.3 — Baixa Tensão** (planejado)

O subprojeto **03.2** aprofunda a abordagem de engenharia de dados, pois lida com:
- maior volume de registros;
- maior heterogeneidade histórica de layout;
- maior presença de campos condicionais e dados ausentes legítimos.

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
2. **RAW de Validação** — verificação controlada de metadados (header)
3. **STAGE** — estruturação com schema explícito baseado no header real

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
- Nenhuma perda de informação

Essa tabela representa a **fonte de verdade primária** do projeto.

---

## Etapa 1 — Validação Controlada do Header (RAW Paralela)

### Motivação

Durante a inspeção inicial, observou-se que os nomes das colunas não estavam fisicamente presentes no banco. Para eliminar qualquer dúvida sobre:
- existência do header no CSV;
- comportamento do pgAdmin durante a importação;
- integridade do arquivo original;

foi realizado um **teste controlado e reproduzível**.

### Procedimento

- Criação da tabela `bdgd_media_tensao_raw_v2` (1 coluna `linha`)
- Importação do **mesmo CSV**, com:
  - **HEADER = NO**

### Evidência Obtida

- A primeira linha ingerida foi:
  ```
  COD_ID_ENCR;DIST;PN_CON;PAC;...;POINT_X;POINT_Y
  ```

### Conclusão Técnica

- O CSV **possui header explícito**
- O comportamento anterior foi **correto e esperado**
- O header foi tratado como metadado, não como dado

Essa validação fundamenta toda a estruturação posterior.

---

## Etapa 2 — Criação da Camada STAGE (Schema Explícito)

### Princípio Central

> Em projetos de dados reais, **colunas genéricas (`col_1`, `col_2`, …)** não são aceitáveis quando o schema original existe.

A camada *stage* deve **preservar semântica, governança e rastreabilidade**.

### Abordagem Adotada

- A tabela *stage* será criada **com base direta no header validado do CSV**
- Cada coluna da *stage* corresponde **1:1** ao nome oficial do arquivo
- Tipos inicialmente definidos como `TEXT`

### Benefícios

- Clareza estrutural imediata
- Facilidade de validação cruzada com dicionários BDGD
- Redução drástica de dívida técnica
- Pipeline defensável em contexto profissional

> A tipagem e normalização ocorrerão **apenas em camadas analíticas posteriores**.

---

## Seção Legacy — Abordagem Inicial (Descontinuada)

### Contexto

Na fase inicial do projeto, foi testada uma abordagem baseada em:
- contagem de delimitadores;
- criação de *stage* com colunas genéricas (`col_01` … `col_n`);
- estruturação puramente posicional.

### Motivo da Descontinuação

Embora tecnicamente válida como experimento exploratório, essa abordagem foi **formalmente abandonada** por:

- não refletir boas práticas de engenharia de dados;
- introduzir ambiguidade semântica;
- não escalar para ambientes de Big Data e governança;
- tornar o pipeline difícil de manter e auditar.

### Status

- Mantida apenas como **referência histórica (legacy)**
- **Não utilizada** em nenhuma análise futura

---

## 🧩 Apêndice Técnico — Comandos e Queries Utilizadas

Esta seção documenta **apenas os comandos que sustentam decisões arquiteturais e metodológicas** do Projeto 03.2. Não se trata de um tutorial de SQL, mas de um **registro técnico reproduzível** das validações realizadas.

### 1️⃣ Validação da Presença de Header no CSV

**Objetivo:** comprovar que o arquivo original possui header e entender o comportamento do pgAdmin durante a importação.

Importação realizada com:
- Delimitador: `;`
- Encoding: `LATIN1`
- **HEADER = NO** (forçando ingestão literal da primeira linha)

Consulta de validação:

```sql
SELECT *
FROM bdgd_media_tensao_raw_v2
LIMIT 2;
```

**Resultado esperado:**
- Primeira linha contendo os nomes das colunas (`COD_ID_ENCR;DIST;PN_CON;...`)
- Segunda linha iniciando os dados reais

Essa validação confirmou que o **header existe na origem** e que o comportamento observado anteriormente foi exclusivamente decorrente da configuração de importação.

---

### 2️⃣ Verificação de Integridade da Carga

**Objetivo:** garantir que nenhuma linha foi perdida durante a ingestão.

```sql
SELECT COUNT(*)
FROM bdgd_media_tensao_raw_v2;
```

O total de registros obtido é consistente com o volume esperado do dataset de média tensão da BDGD.

---

### 3️⃣ Criação da Tabela *Stage* com Schema Explícito (Abordagem Oficial)

**Objetivo:** estabelecer a camada *stage* já com os **nomes oficiais das colunas**, conforme definidos no header do CSV, evitando qualquer estratégia baseada em colunas genéricas.

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

Essa definição consolida a **estratégia oficial do projeto**, garantindo semântica explícita, governança e facilidade de evolução futura do modelo.

---

### Observação

Comandos triviais de infraestrutura (criação de database, usuários, permissões) foram deliberadamente omitidos deste apêndice por não contribuírem para a compreensão das decisões analíticas ou arquiteturais do projeto.
---

## Status Atual do Projeto

- ✔ Header do CSV validado empiricamente
- ✔ Estratégia de ingestão redefinida e consolidada
- ✔ RAW oficial preservada
- 🔄 Criação da STAGE com schema explícito (próximo passo)

---

## Próximos Passos

1. Criar tabela `bdgd_media_tensao_stage` com colunas oficiais
2. Inserir dados a partir da RAW
3. Validar alinhamento semântico com dicionário BDGD
4. Evoluir para camadas analíticas

---

## Observações Metodológicas Finais

Este projeto prioriza:
- decisões observáveis e testáveis;
- documentação explícita de erros e correções;
- abandono consciente de abordagens inadequadas;
- aderência a práticas profissionais de engenharia de dados.

Nada é assumido. Tudo é validado.


---
