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
