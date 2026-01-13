# ⚡ Projeto 03.2 — Média Tensão [PostgreSQL]

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

## Status Atual do Projeto

### ✅ Etapas Concluídas

- Criação do database dedicado `bdgd_media_tensao`
- Preparação completa do ambiente PostgreSQL
- Criação da tabela de ingestão bruta:
  - `bdgd_media_tensao_raw`
- Importação integral do dataset de média tensão

📊 **Total de registros carregados:** **312.074 linhas**

---
<br>

## Estado Atual dos Dados

- Os dados encontram-se armazenados em **formato RAW**
- Cada linha original do CSV foi preservada integralmente em uma única coluna (`linha`)
- Nenhuma tipagem, limpeza ou transformação foi aplicada até o momento

### Justificativa Metodológica

Essa estratégia foi adotada para:
- garantir ingestão completa do dataset;
- contornar problemas iniciais de delimitador, estrutura e encoding;
- preservar o dado original como referência primária;
- permitir tratamento posterior controlado e auditável.

---
<br>

## Desafios Técnicos Enfrentados

Durante a ingestão inicial do dataset, foram identificados e resolvidos os seguintes pontos:

- Delimitador do CSV incompatível com a configuração padrão (`;` vs `,`)
- Tentativa inicial de importação sem estrutura de colunas definida
- Encoding incompatível com UTF-8 (dataset em `LATIN1`)

### Solução Adotada

- Criação de tabela RAW com uma única coluna (`linha`)
- Importação utilizando:
  - delimitador fictício (`|`)
  - encoding `LATIN1`

Essa abordagem permitiu a ingestão completa dos dados **sem perda de informação**.

---
<br>

## Próximos Passos Planejados

### Curto Prazo

- Identificação e validação da estrutura real do CSV
- Quebra da coluna `linha` em colunas reais
- Criação de tabela estruturada (`bdgd_media_tensao_stage` ou equivalente)
- Validação inicial de qualidade dos dados (nulos, datas, domínios)

### Médio Prazo

- Análise Exploratória de Dados (EDA)
- Comparação estrutural com o dataset de alta tensão (Projeto 03.1)
- Identificação de padrões, inconsistências e possíveis rupturas históricas

---
<br>

## Observações Metodológicas

- O dado bruto é preservado integralmente
- Todas as decisões técnicas são documentadas
- Transformações futuras ocorrerão apenas em tabelas derivadas
- O projeto prioriza **clareza analítica, rastreabilidade e consistência metodológica**

---
<br>

## Status Geral

🟢 **Projeto ativo**  
🧱 **Fase atual:** Ingestão e preparação dos dados

---
<br>

*README em construção contínua — seguirá o mesmo padrão narrativo e analítico do Projeto 03.1 (Alta Tensão).*