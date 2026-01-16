# 🔌 Projeto 03.2 — Média Tensão [PostgreSQL]

## Resumo Executivo

Este projeto corresponde ao **Projeto 03.2 — Média Tensão**, continuação direta do **Projeto 03 — Consumo Elétrico (BDGD)**, e tem como objetivo a **preparação, ingestão e estruturação analítica** dos dados de **Unidades Consumidoras de média tensão** da base **BDGD/ANEEL**.

Nesta etapa inicial, o foco está **exclusivamente na construção do ambiente técnico e na ingestão segura do dataset**, preservando integralmente o dado bruto e documentando todas as decisões metodológicas adotadas.

Este README segue o **mesmo padrão estrutural e narrativo do Projeto 03.1 (Alta Tensão)**, funcionando como um **relatório vivo**, que será progressivamente expandido conforme o avanço das análises.

<br>

**Principais pontos até o momento:**
- **Escopo e dados:** Unidades Consumidoras de média tensão — base BDGD/ANEEL.
- **Abordagem:** ingestão controlada do dado bruto, com preservação total da informação original.
- **Ferramentas:** PostgreSQL + pgAdmin 4 (preparação para análises posteriores).

---
<br>

## Contexto do Projeto

O Projeto 03 foi estruturado em três frentes analíticas distintas, conforme o nível de tensão das Unidades Consumidoras:

- **03.1 — Alta Tensão** (Power BI) ✔️
- **03.2 — Média Tensão** (PostgreSQL) 🔄
- **03.3 — Baixa Tensão** (planejado)

O subprojeto **03.2 — Média Tensão** surge como continuidade natural do 03.1, permitindo:
- reaproveitamento de aprendizados metodológicos;
- comparação estrutural entre níveis de tensão;
- identificação de similaridades e diferenças cadastrais e históricas na BDGD.

---
<br>

## Ambiente Técnico

### Banco de Dados

- **SGBD:** PostgreSQL
- **Versão:** 18.1
- **Sistema Operacional:** Windows x64
- **Ferramenta de administração:** pgAdmin 4
- **Database dedicado:** `bdgd_media_tensao`

### Configurações Gerais

- Porta padrão: **5432**
- Usuário: `postgres`
- Conexão local validada

---
<br>

## Etapas do Projeto (Rastreabilidade Temporal)

Esta seção organiza o avanço do Projeto 03.2 em **etapas cronológicas**, permitindo rastreabilidade clara do que foi feito, quando e com qual objetivo técnico.

---
<br>

## 0️⃣ Etapa Inicial — Preparação de Ambiente e Ingestão RAW

### Objetivo
Preparar o ambiente PostgreSQL e realizar a ingestão **segura e integral** do dataset de média tensão, sem qualquer transformação estrutural.

<br>

### Atividades Executadas

- Instalação e configuração do PostgreSQL 18.1 (Windows x64)
- Instalação e validação do pgAdmin 4
- Criação do database dedicado `bdgd_media_tensao`
- Validação de conexão local (porta 5432, usuário `postgres`)
- Criação da tabela de ingestão bruta:
  - `bdgd_media_tensao_raw`
- Importação integral do arquivo CSV de média tensão

📊 **Total de registros carregados:** **312.074 linhas**

<br>

### Decisões Técnicas

- Opção por ingestão em formato RAW (uma única coluna `linha`)
- Uso de delimitador fictício (`|`) para evitar quebra incorreta de colunas
- Encoding definido como `LATIN1`

<br>

### Justificativa

Essa abordagem garantiu:
- preservação total do dado original;
- ingestão sem perda de informação;
- base confiável para inspeção e estruturação posterior.

---
<br>

## 1️⃣ Primeira Etapa — Inspeção Inicial da Estrutura do Dataset

### Objetivo
Inspecionar mecanicamente o conteúdo do dado bruto para identificar o delimitador real e estimar a estrutura do CSV **sem assumir headers ou semântica**.

<br>

### Atividades Executadas

- Abertura do Query Tool no database `bdgd_media_tensao`
- Inspeção visual do conteúdo da tabela RAW
- Identificação do delimitador real do arquivo
- Contagem de delimitadores para estimativa do número de colunas

<br>

### Queries Executadas

#### Inspeção do conteúdo RAW

```sql
SELECT *
FROM bdgd_media_tensao_raw
LIMIT 5;
```

**Objetivo:**
- Confirmar a correta ingestão dos dados;
- Validar a existência de uma única coluna (`linha`);
- Observar o formato bruto das linhas do CSV original.

---
<br>

#### Contagem de delimitadores e estimativa de colunas

```sql
SELECT
    length(linha) - length(replace(linha, ';', '')) AS qtd_delimitadores,
    (length(linha) - length(replace(linha, ';', '')) + 1) AS qtd_colunas_estimadas
FROM bdgd_media_tensao_raw
LIMIT 1;
```
<br>

### Resultados Obtidos

- Delimitador identificado: **`;` (ponto e vírgula)**
- **79 delimitadores por linha**
- **80 colunas estimadas**

<br>

### Observações Importantes

- Nenhum nome de coluna, tipo de dado ou significado semântico foi assumido
- A estrutura completa dependerá de:
  - validação da consistência do número de campos em todas as linhas;
  - identificação de header (se existente);
  - consulta ao dicionário oficial da BDGD

---
<br>

## 2️⃣ Segunda Etapa — Validação Estrutural e Natureza dos Dados Ausentes

### Objetivo
Avaliar a **consistência estrutural global** do dataset e documentar corretamente a **natureza dos campos vazios**, evitando interpretações equivocadas durante as etapas de modelagem e análise.

<br>

### Atividades Executadas

- Validação do número de delimitadores em todo o dataset
- Identificação de variações estruturais no layout do arquivo
- Inspeção manual de linhas com número de colunas superior ao padrão esperado
- Análise contextual dos campos vazios à luz do domínio do problema (BDGD)

<br>

### Queries Executadas

#### Validação global de delimitadores

```sql
SELECT
    COUNT(*) AS total_linhas,
    MIN(length(linha) - length(replace(linha, ';', ''))) AS min_delimitadores,
    MAX(length(linha) - length(replace(linha, ';', ''))) AS max_delimitadores
FROM bdgd_media_tensao_raw;
```
<br>

#### Distribuição de padrões estruturais

```sql
SELECT
    length(linha) - length(replace(linha, ';', '')) AS qtd_delimitadores,
    COUNT(*) AS total_linhas
FROM bdgd_media_tensao_raw
GROUP BY qtd_delimitadores
ORDER BY qtd_delimitadores;
```
<br>

### Resultados Obtidos

- **Layout dominante:** 79 delimitadores (80 colunas) → ~98% do dataset
- Existência de um subconjunto minoritário com **81 a 84 delimitadores**
- As variações estruturais não são aleatórias, indicando **compatibilidade histórica de layouts**

<br>

### Interpretação Técnica

- A presença de delimitadores adicionais está associada a:
  - campos opcionais adicionados em versões posteriores do layout;
  - sequências de campos vazios (`;;;;;`);
  - evolução histórica do cadastro da BDGD.

> **Dada a natureza do dataset, campos vazios não devem ser interpretados como erro**, mas sim como:
  - ausência legítima de ocorrência (ex.: interrupções de fornecimento inexistentes);
  - atributos não aplicáveis àquela Unidade Consumidora;
  - informações condicionais dependentes de eventos específicos.

<br>

### Decisão Metodológica Importante

- Dados ausentes (**NULL / vazio**) são tratados como **informação semântica válida**, e não como falha de ingestão.
- Nenhuma linha será descartada nesta fase.
- O dado bruto permanece preservado integralmente na tabela `RAW`.

Essa interpretação é fundamental para evitar:
- exclusões indevidas de registros;
- distorções estatísticas futuras;
- erros conceituais durante análises de continuidade de serviço e qualidade.

---
<br>

## 3️⃣ Terceira Etapa — Criação da Tabela *Stage* e Estruturação Inicial

### Objetivo  
Estabelecer uma **camada intermediária (*stage*)** entre o dado bruto (`RAW`) e as futuras tabelas analíticas, permitindo a **quebra controlada da linha textual em colunas**, sem perda de informação e sem assumir, prematuramente, significados semânticos incorretos.

Essa etapa tem como foco **organização estrutural**, não modelagem.

<br>

### Contexto Técnico

Após a validação estrutural do arquivo, foi identificado que:

- O **layout dominante possui 79 delimitadores**, resultando em **80 colunas**
- Linhas com delimitadores adicionais (≥ 80) representam **variações históricas do layout**
- O recorte seguro e consistente para estruturação inicial é de **80 colunas**

<br>

### Decisão Estrutural Importante

- A tabela *stage* foi criada com **80 colunas genéricas (`col_01` a `col_80`)**
- Nenhuma coluna recebe nome semântico nesta fase

Essa abordagem garante:

- **Neutralidade analítica**
- **Rastreabilidade total** entre posição no CSV e coluna no banco
- **Evita interpretações erradas** antes do cruzamento com dicionários oficiais da BDGD

> Nesta fase, **posição é mais importante que significado**.

<br>

### Atividades Executadas

- Criação da tabela `bdgd_media_tensao_stage`
- Definição explícita de **80 colunas do tipo `TEXT`**
- Preparação da estrutura para receber apenas o **layout dominante**
- Planejamento de tratamento posterior para linhas com colunas excedentes

<br>

### Query Executada — Criação da Tabela *Stage*

```sql
CREATE TABLE bdgd_media_tensao_stage (
    col_01 TEXT, col_02 TEXT, col_03 TEXT, col_04 TEXT, col_05 TEXT,
    col_06 TEXT, col_07 TEXT, col_08 TEXT, col_09 TEXT, col_10 TEXT,
    col_11 TEXT, col_12 TEXT, col_13 TEXT, col_14 TEXT, col_15 TEXT,
    col_16 TEXT, col_17 TEXT, col_18 TEXT, col_19 TEXT, col_20 TEXT,
    col_21 TEXT, col_22 TEXT, col_23 TEXT, col_24 TEXT, col_25 TEXT,
    col_26 TEXT, col_27 TEXT, col_28 TEXT, col_29 TEXT, col_30 TEXT,
    col_31 TEXT, col_32 TEXT, col_33 TEXT, col_34 TEXT, col_35 TEXT,
    col_36 TEXT, col_37 TEXT, col_38 TEXT, col_39 TEXT, col_40 TEXT,
    col_41 TEXT, col_42 TEXT, col_43 TEXT, col_44 TEXT, col_45 TEXT,
    col_46 TEXT, col_47 TEXT, col_48 TEXT, col_49 TEXT, col_50 TEXT,
    col_51 TEXT, col_52 TEXT, col_53 TEXT, col_54 TEXT, col_55 TEXT,
    col_56 TEXT, col_57 TEXT, col_58 TEXT, col_59 TEXT, col_60 TEXT,
    col_61 TEXT, col_62 TEXT, col_63 TEXT, col_64 TEXT, col_65 TEXT,
    col_66 TEXT, col_67 TEXT, col_68 TEXT, col_69 TEXT, col_70 TEXT,
    col_71 TEXT, col_72 TEXT, col_73 TEXT, col_74 TEXT, col_75 TEXT,
    col_76 TEXT, col_77 TEXT, col_78 TEXT, col_79 TEXT, col_80 TEXT
);
```

<br>

### Interpretação Metodológica

- A tabela *stage* **não representa o modelo final**
- Ela funciona como uma **zona de transição controlada**
- A semântica real das colunas será atribuída **somente após**:
  - cruzamento com dicionários oficiais da BDGD;
  - análise de conteúdo por posição;
  - validação por amostragem.

<br>

### Planejamento para Próximas Etapas

- Inserção dos dados do `RAW` na *stage* com **split controlado**
- Tratamento explícito das linhas com colunas excedentes
- Criação de uma tabela de **mapeamento coluna ↔ significado**
- Evolução da *stage* para tabelas analíticas normalizadas
