---
description: Realizar uma análise de consistência e qualidade não destrutiva entre spec.md, plan.md e tasks.md após a geração de tarefas.
---

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Objetivo

Identificar inconsistências, duplicações, ambiguidades e itens subespecificados nos três artefatos principais (`spec.md`, `plan.md`, `tasks.md`) antes da implementação. Este comando DEVE ser executado apenas após `/tasks` ter produzido com sucesso um `tasks.md` completo.

## Restrições Operacionais

**ESTRITAMENTE SOMENTE LEITURA**: **Não** modifique nenhum arquivo. Produza um relatório de análise estruturado. Ofereça um plano de remediação opcional (o usuário deve aprovar explicitamente antes que qualquer comando de edição subsequente seja invocado manualmente).

**Autoridade da Constituição**: A constituição do projeto (`.specify/memory/constitution.md`) é **inegociável** dentro deste escopo de análise. Conflitos com a constituição são automaticamente CRÍTICOS e requerem ajuste da especificação, plano ou tarefas—não diluição, reinterpretação ou silenciamento do princípio. Se um princípio em si precisa mudar, isso deve ocorrer em uma atualização de constituição separada e explícita fora do `/analyze`.

## Etapas de Execução

### 1. Inicializar Contexto de Análise

Execute `.specify/scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks` uma vez a partir da raiz do repositório e analise o JSON para FEATURE_DIR e AVAILABLE_DOCS. Derive caminhos absolutos:

- SPEC = FEATURE_DIR/spec.md
- PLAN = FEATURE_DIR/plan.md
- TASKS = FEATURE_DIR/tasks.md

Aborte com uma mensagem de erro se algum arquivo necessário estiver ausente (instrua o usuário a executar o comando de pré-requisito ausente).
Para aspas simples em argumentos como "I'm Groot", use sintaxe de escape: por exemplo 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

### 2. Carregar Artefatos (Divulgação Progressiva)

Carregue apenas o contexto mínimo necessário de cada artefato:

**De spec.md:**

- Visão Geral/Contexto
- Requisitos Funcionais
- Requisitos Não Funcionais
- Histórias de Usuário
- Casos Extremos (se presentes)

**De plan.md:**

- Escolhas de arquitetura/stack
- Referências do Modelo de Dados
- Fases
- Restrições técnicas

**De tasks.md:**

- IDs de Tarefa
- Descrições
- Agrupamento por fase
- Marcadores paralelos [P]
- Caminhos de arquivos referenciados

**Da constituição:**

- Carregue `.specify/memory/constitution.md` para validação de princípios

### 3. Construir Modelos Semânticos

Crie representações internas (não inclua artefatos brutos na saída):

- **Inventário de requisitos**: Cada requisito funcional + não funcional com uma chave estável (derive slug baseado na frase imperativa; por exemplo, "Usuário pode fazer upload de arquivo" → `usuario-pode-fazer-upload-arquivo`)
- **Inventário de histórias de usuário/ações**: Ações discretas do usuário com critérios de aceitação
- **Mapeamento de cobertura de tarefas**: Mapeie cada tarefa para um ou mais requisitos ou histórias (inferência por palavra-chave / padrões de referência explícitos como IDs ou frases-chave)
- **Conjunto de regras da constituição**: Extraia nomes de princípios e declarações normativas DEVE/DEVERIA

### 4. Passos de Detecção (Análise Eficiente em Tokens)

Foque em descobertas de alto sinal. Limite a 50 descobertas no total; agregue o restante em resumo de overflow.

#### A. Detecção de Duplicação

- Identifique requisitos quase duplicados
- Marque fraseado de qualidade inferior para consolidação

#### B. Detecção de Ambiguidade

- Sinalize adjetivos vagos (rápido, escalável, seguro, intuitivo, robusto) sem critérios mensuráveis
- Sinalize placeholders não resolvidos (TODO, TKTK, ???, `<placeholder>`, etc.)

#### C. Subespecificação

- Requisitos com verbos mas sem objeto ou resultado mensurável
- Histórias de usuário sem alinhamento de critérios de aceitação
- Tarefas referenciando arquivos ou componentes não definidos em spec/plan

#### D. Alinhamento com a Constituição

- Qualquer requisito ou elemento do plano conflitando com um princípio DEVE
- Seções obrigatórias ou controles de qualidade ausentes da constituição

#### E. Lacunas de Cobertura

- Requisitos com zero tarefas associadas
- Tarefas sem requisito/história mapeada
- Requisitos não funcionais não refletidos em tarefas (por exemplo, desempenho, segurança)

#### F. Inconsistência

- Deriva terminológica (mesmo conceito nomeado diferentemente entre arquivos)
- Entidades de dados referenciadas no plano mas ausentes na spec (ou vice-versa)
- Contradições de ordenação de tarefas (por exemplo, tarefas de integração antes de tarefas de configuração fundamentais sem nota de dependência)
- Requisitos conflitantes (por exemplo, um requer Next.js enquanto outro especifica Vue)

### 5. Atribuição de Severidade

Use esta heurística para priorizar descobertas:

- **CRÍTICO**: Viola DEVE da constituição, artefato principal da spec ausente, ou requisito com zero cobertura que bloqueia funcionalidade base
- **ALTO**: Requisito duplicado ou conflitante, atributo de segurança/desempenho ambíguo, critério de aceitação não testável
- **MÉDIO**: Deriva terminológica, cobertura de tarefa não funcional ausente, caso extremo subespecificado
- **BAIXO**: Melhorias de estilo/redação, redundância menor não afetando ordem de execução

### 6. Produzir Relatório de Análise Compacto

Produza um relatório em Markdown (sem gravação de arquivos) com a seguinte estrutura:

## Relatório de Análise de Especificação

| ID  | Categoria  | Severidade | Local(is)        | Resumo                        | Recomendação                               |
| --- | ---------- | ---------- | ---------------- | ----------------------------- | ------------------------------------------ |
| A1  | Duplicação | ALTO       | spec.md:L120-134 | Dois requisitos similares ... | Mesclar fraseado; manter versão mais clara |

(Adicione uma linha por descoberta; gere IDs estáveis prefixados pela inicial da categoria.)

**Tabela de Resumo de Cobertura:**

| Chave do Requisito | Tem Tarefa? | IDs das Tarefas | Notas |
| ------------------ | ----------- | --------------- | ----- |

**Problemas de Alinhamento com a Constituição:** (se houver)

**Tarefas Não Mapeadas:** (se houver)

**Métricas:**

- Total de Requisitos
- Total de Tarefas
- % de Cobertura (requisitos com >=1 tarefa)
- Contagem de Ambiguidades
- Contagem de Duplicações
- Contagem de Problemas Críticos

### 7. Fornecer Próximas Ações

No final do relatório, produza um bloco de Próximas Ações conciso:

- Se existirem problemas CRÍTICOS: Recomende resolver antes do `/implement`
- Se apenas BAIXO/MÉDIO: Usuário pode prosseguir, mas forneça sugestões de melhoria
- Forneça sugestões de comando explícitas: por exemplo, "Execute /specify com refinamento", "Execute /plan para ajustar arquitetura", "Edite manualmente tasks.md para adicionar cobertura para 'metricas-desempenho'"

### 8. Oferecer Remediação

Pergunte ao usuário: "Gostaria que eu sugerisse edições de remediação concretas para os N principais problemas?" (NÃO as aplique automaticamente.)

## Princípios Operacionais

### Eficiência de Contexto

- **Tokens mínimos de alto sinal**: Foque em descobertas acionáveis, não documentação exaustiva
- **Divulgação progressiva**: Carregue artefatos incrementalmente; não despeje todo o conteúdo na análise
- **Saída eficiente em tokens**: Limite a tabela de descobertas a 50 linhas; resuma overflow
- **Resultados determinísticos**: Reexecutar sem mudanças deve produzir IDs e contagens consistentes

### Diretrizes de Análise

- **NUNCA modifique arquivos** (esta é uma análise somente leitura)
- **NUNCA alucine seções ausentes** (se ausentes, reporte-as com precisão)
- **Priorize violações da constituição** (estas são sempre CRÍTICAS)
- **Use exemplos em vez de regras exaustivas** (cite instâncias específicas, não padrões genéricos)
- **Reporte zero problemas com elegância** (emita relatório de sucesso com estatísticas de cobertura)

## Contexto

$ARGUMENTS
