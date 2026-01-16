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

### Próxima Etapa Planejada

- Definição da estratégia de **estruturação em tabela STAGE**
- Criação de tabela estruturada com base no layout canônico (80 colunas)
- Garantia de rastreabilidade entre `RAW` e `STAGE`
- Posterior associação ao dicionário oficial da BDGD

