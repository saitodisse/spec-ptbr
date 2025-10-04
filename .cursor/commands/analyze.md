---
description: Realizar análise não-destrutiva de consistência e qualidade entre os artefatos spec.md, plan.md e tasks.md após a geração de tarefas.
---

A entrada do usuário pode ser fornecida diretamente pelo agente ou como argumento de comando - você **DEVE** considerá-la antes de prosseguir com o prompt (se não estiver vazia).

Entrada do usuário:

$ARGUMENTS

Objetivo: Identificar inconsistências, duplicações, ambiguidades e itens subespecificados entre os três artefatos principais (`spec.md`, `plan.md`, `tasks.md`) antes da implementação. Este comando DEVE ser executado apenas após `/tasks` ter produzido com sucesso um `tasks.md` completo.

ESTRITAMENTE SOMENTE LEITURA: **NÃO** modifique nenhum arquivo. Produza um relatório de análise estruturado. Ofereça um plano de remediação opcional (o usuário deve aprovar explicitamente antes que comandos de edição subsequentes sejam invocados manualmente).

Autoridade da Constituição: A constituição do projeto (`.specify/memory/constitution.md`) é **não-negociável** dentro do escopo desta análise. Conflitos constitucionais são automaticamente CRÍTICOS e requerem ajuste da especificação, plano ou tarefas—não diluição, reinterpretação ou ignorar silenciosamente o princípio. Se um princípio em si precisar mudar, isso deve ocorrer em uma atualização constitucional separada e explícita fora de `/analyze`.

Etapas de execução:

1. Execute `.specify/scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks` uma vez a partir da raiz do repositório e analise o JSON para FEATURE_DIR e AVAILABLE_DOCS. Derive caminhos absolutos:

   - SPEC = FEATURE_DIR/spec.md
   - PLAN = FEATURE_DIR/plan.md
   - TASKS = FEATURE_DIR/tasks.md
     Aborte com uma mensagem de erro se algum arquivo obrigatório estiver ausente (instrua o usuário a executar o comando de pré-requisito ausente).

2. Carregue artefatos:

   - Analise seções do spec.md: Visão Geral/Contexto, Requisitos Funcionais, Requisitos Não-Funcionais, Histórias de Usuário, Casos Extremos (se presentes).
   - Analise plan.md: Escolhas de arquitetura/stack, referências do Modelo de Dados, Fases, Restrições técnicas.
   - Analise tasks.md: IDs de tarefas, descrições, agrupamento de fases, marcadores paralelos [P], caminhos de arquivos referenciados.
   - Carregue constituição `.specify/memory/constitution.md` para validação de princípios.

3. Construa modelos semânticos internos:

   - Inventário de requisitos: Cada requisito funcional + não-funcional com uma chave estável (derive slug baseado na frase imperativa; ex., "Usuário pode fazer upload de arquivo" -> `user-can-upload-file`).
   - Inventário de histórias/ações de usuário.
   - Mapeamento de cobertura de tarefas: Mapeie cada tarefa para um ou mais requisitos ou histórias (inferência por palavra-chave / padrões de referência explícita como IDs ou frases-chave).
   - Conjunto de regras constitucionais: Extraia nomes de princípios e quaisquer declarações normativas MUST/SHOULD.

4. Passes de detecção:
   A. Detecção de duplicação:

   - Identifique requisitos quase duplicados. Marque frases de menor qualidade para consolidação.
     B. Detecção de ambiguidade:
   - Sinalize adjetivos vagos (rápido, escalável, seguro, intuitivo, robusto) sem critérios mensuráveis.
   - Sinalize placeholders não resolvidos (TODO, TKTK, ???, <placeholder>, etc.).
     C. Subespecificação:
   - Requisitos com verbos mas sem objeto ou resultado mensurável.
   - Histórias de usuário sem alinhamento de critérios de aceitação.
   - Tarefas referenciando arquivos ou componentes não definidos em spec/plan.
     D. Alinhamento constitucional:
   - Qualquer requisito ou elemento de plano conflitando com um princípio MUST.
   - Seções obrigatórias ausentes ou portões de qualidade da constituição.
     E. Lacunas de cobertura:
   - Requisitos com zero tarefas associadas.
   - Tarefas sem requisito/história mapeada.
   - Requisitos não-funcionais não refletidos nas tarefas (ex., performance, segurança).
     F. Inconsistência:
   - Deriva terminológica (mesmo conceito nomeado diferentemente entre arquivos).
   - Entidades de dados referenciadas no plano mas ausentes na especificação (ou vice-versa).
   - Contradições na ordenação de tarefas (ex., tarefas de integração antes de tarefas de configuração fundamental sem nota de dependência).
   - Requisitos conflitantes (ex., um requer usar Next.js enquanto outro diz para usar Vue como framework).

5. Heurística de atribuição de severidade:

   - CRÍTICO: Viola MUST constitucional, artefato de especificação principal ausente, ou requisito com zero cobertura que bloqueia funcionalidade básica.
   - ALTO: Requisito duplicado ou conflitante, atributo de segurança/performance ambíguo, critério de aceitação não testável.
   - MÉDIO: Deriva terminológica, cobertura de tarefa não-funcional ausente, caso extremo subespecificado.
   - BAIXO: Melhorias de estilo/redação, redundância menor não afetando ordem de execução.

6. Produza um relatório Markdown (sem escrita de arquivos) com seções:

   ### Relatório de Análise de Especificação

   | ID  | Categoria  | Severidade | Local(ais)       | Resumo                        | Recomendação                              |
   | --- | ---------- | ---------- | ---------------- | ----------------------------- | ----------------------------------------- |
   | A1  | Duplicação | ALTO       | spec.md:L120-134 | Dois requisitos similares ... | Mesclar redação; manter versão mais clara |

   (Adicione uma linha por descoberta; gere IDs estáveis prefixados pela inicial da categoria.)

   Subseções adicionais:

   - Tabela de Resumo de Cobertura:
     | Chave do Requisito | Tem Tarefa? | IDs de Tarefas | Notas |
   - Problemas de Alinhamento Constitucional (se houver)
   - Tarefas Não Mapeadas (se houver)
   - Métricas:
     - Total de Requisitos
     - Total de Tarefas
     - % de Cobertura (requisitos com >=1 tarefa)
     - Contagem de Ambiguidade
     - Contagem de Duplicação
     - Contagem de Problemas Críticos

7. No final do relatório, produza um bloco conciso de Próximas Ações:

   - Se problemas CRÍTICOS existirem: Recomende resolver antes de `/implement`.
   - Se apenas BAIXO/MÉDIO: Usuário pode prosseguir, mas forneça sugestões de melhoria.
   - Forneça sugestões de comandos explícitos: ex., "Execute /specify com refinamento", "Execute /plan para ajustar arquitetura", "Edite manualmente tasks.md para adicionar cobertura para 'performance-metrics'".

8. Pergunte ao usuário: "Gostaria que eu sugerisse edições concretas de remediação para os N principais problemas?" (NÃO as aplique automaticamente.)

Regras de comportamento:

- NUNCA modifique arquivos.
- NUNCA alucine seções ausentes—se ausentes, reporte-as.
- MANTENHA descobertas determinísticas: se reexecutado sem mudanças, produza IDs e contagens consistentes.
- LIMITE total de descobertas na tabela principal a 50; agregue o restante em uma nota de overflow resumida.
- Se zero problemas encontrados, emita um relatório de sucesso com estatísticas de cobertura e recomendação de prosseguir.

Contexto: $ARGUMENTS
