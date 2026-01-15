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

## 0️⃣ Etapa Inicial — Preparação de Ambiente e Ingestão RAW

### Objetivo
Preparar o ambiente PostgreSQL e realizar a ingestão **segura e integral** do dataset de média tensão, sem qualquer transformação estrutural.

### Atividades Executadas

- Instalação e configuração do PostgreSQL 18.1 (Windows x64)
- Instalação e validação do pgAdmin 4
- Criação do database dedicado `bdgd_media_tensao`
- Validação de conexão local (porta 5432, usuário `postgres`)
- Criação da tabela de ingestão bruta:
  - `bdgd_media_tensao_raw`
- Importação integral do arquivo CSV de média tensão

📊 **Total de registros carregados:** **312.074 linhas**

### Decisões Técnicas

- Opção por ingestão em formato RAW (uma única coluna `linha`)
- Uso de delimitador fictício (`|`) para evitar quebra incorreta de colunas
- Encoding definido como `LATIN1`

### Justificativa

Essa abordagem garantiu:
- preservação total do dado original;
- ingestão sem perda de informação;
- base confiável para inspeção e estruturação posterior.

---

## 1️⃣ Primeira Etapa — Inspeção Inicial da Estrutura do Dataset

### Objetivo
Inspecionar mecanicamente o conteúdo do dado bruto para identificar o delimitador real e estimar a estrutura do CSV **sem assumir headers ou semântica**.

### Atividades Executadas

- Abertura do Query Tool no database `bdgd_media_tensao`
- Inspeção visual do conteúdo da tabela RAW
- Identificação do delimitador real do arquivo
- Contagem de delimitadores para estimativa do número de colunas

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

#### Contagem de delimitadores e estimativa de colunas

```sql
SELECT
    length(linha) - length(replace(linha, ';', '')) AS qtd_delimitadores,
    (length(linha) - length(replace(linha, ';', '')) + 1) AS qtd_colunas_estimadas
FROM bdgd_media_tensao_raw
LIMIT 1;
```

### Resultados Obtidos

- Delimitador identificado: **`;` (ponto e vírgula)**
- **79 delimitadores por linha**
- **80 colunas estimadas**

### Observações Importantes

- Nenhum nome de coluna, tipo de dado ou significado semântico foi assumido
- A estrutura completa dependerá de:
  - validação da consistência do número de campos em todas as linhas;
  - identificação de header (se existente);
  - consulta ao dicionário oficial da BDGD

---

## Próxima Etapa Planejada — Estruturação (Stage)

### Objetivos

- Validar a consistência estrutural do dataset completo
- Identificar oficialmente os campos do CSV
- Quebrar a coluna `linha` em colunas reais
- Criar tabela estruturada (`bdgd_media_tensao_stage`)

---

## Status Geral

🟢 **Projeto ativo**  
🧱 **Fase atual:** Inspeção inicial concluída — pronto para estruturação

---

*README organizado como relatório vivo, com rastreabilidade temporal das decisões e atividades executadas.*
