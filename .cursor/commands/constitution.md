---
description: Criar ou atualizar a constituição do projeto a partir de entradas de princípios interativas ou fornecidas, garantindo que todos os templates dependentes permaneçam sincronizados.
---

A entrada do usuário pode ser fornecida diretamente pelo agente ou como argumento de comando - você **DEVE** considerá-la antes de prosseguir com o prompt (se não estiver vazia).

Entrada do usuário:

$ARGUMENTS

Você está atualizando a constituição do projeto em `.specify/memory/constitution.md`. Este arquivo é um TEMPLATE contendo tokens de placeholder entre colchetes (ex. `[PROJECT_NAME]`, `[PRINCIPLE_1_NAME]`). Seu trabalho é (a) coletar/derivar valores concretos, (b) preencher o template precisamente, e (c) propagar quaisquer emendas através de artefatos dependentes.

Siga este fluxo de execução:

1. Carregue o template de constituição existente em `.specify/memory/constitution.md`.

   - Identifique cada token de placeholder da forma `[ALL_CAPS_IDENTIFIER]`.
     **IMPORTANTE**: O usuário pode requerer menos ou mais princípios do que os usados no template. Se um número for especificado, respeite isso - siga o template geral. Você atualizará o documento adequadamente.

2. Colete/derive valores para placeholders:

   - Se entrada do usuário (conversa) fornecer um valor, use-o.
   - Caso contrário, infira do contexto do repositório existente (README, docs, versões constitucionais anteriores se incorporadas).
   - Para datas de governança: `RATIFICATION_DATE` é a data de adoção original (se desconhecida pergunte ou marque TODO), `LAST_AMENDED_DATE` é hoje se mudanças forem feitas, caso contrário mantenha anterior.
   - `CONSTITUTION_VERSION` deve incrementar de acordo com regras de versionamento semântico:
     - MAJOR: Remoções ou redefinições de governança/princípio incompatíveis com versões anteriores.
     - MINOR: Novo princípio/seção adicionado ou orientação materialmente expandida.
     - PATCH: Esclarecimentos, redação, correções de erro de digitação, refinamentos não-semânticos.
   - Se tipo de bump de versão ambíguo, proponha raciocínio antes de finalizar.

3. Rascunhe o conteúdo constitucional atualizado:

   - Substitua cada placeholder com texto concreto (nenhum token entre colchetes deixado exceto slots de template intencionalmente retidos que o projeto escolheu não definir ainda—justifique explicitamente qualquer deixado).
   - Preserve hierarquia de cabeçalhos e comentários podem ser removidos uma vez substituídos a menos que ainda adicionem orientação esclarecedora.
   - Garanta que cada seção de Princípio: linha de nome sucinta, parágrafo (ou lista de bullets) capturando regras não-negociáveis, justificativa explícita se não óbvia.
   - Garanta que seção de Governança liste procedimento de emenda, política de versionamento, e expectativas de revisão de conformidade.

4. Lista de verificação de propagação de consistência (converta lista de verificação anterior em validações ativas):

   - Leia `.specify/templates/plan-template.md` e garanta que qualquer "Verificação Constitucional" ou regras se alinhem com princípios atualizados.
   - Leia `.specify/templates/spec-template.md` para alinhamento de escopo/requisitos—atualize se constituição adiciona/remove seções obrigatórias ou restrições.
   - Leia `.specify/templates/tasks-template.md` e garanta que categorização de tarefas reflita tipos de tarefas impulsionados por princípios novos ou removidos (ex., observabilidade, versionamento, disciplina de teste).
   - Leia cada arquivo de comando em `.specify/templates/commands/*.md` (incluindo este) para verificar que nenhuma referência desatualizada (nomes específicos de agente como CLAUDE apenas) permaneça quando orientação genérica for requerida.
   - Leia qualquer documentação de orientação de runtime (ex., `README.md`, `docs/quickstart.md`, ou arquivos de orientação específicos de agente se presentes). Atualize referências a princípios alterados.

5. Produza um Relatório de Impacto de Sincronização (prepend como comentário HTML no topo do arquivo constitucional após atualização):

   - Mudança de versão: antiga → nova
   - Lista de princípios modificados (título antigo → novo título se renomeado)
   - Seções adicionadas
   - Seções removidas
   - Templates requerendo atualizações (✅ atualizado / ⚠ pendente) com caminhos de arquivo
   - TODOs de acompanhamento se quaisquer placeholders intencionalmente adiados.

6. Validação antes da saída final:

   - Nenhum token de colchete não explicado restante.
   - Linha de versão corresponde ao relatório.
   - Datas no formato ISO YYYY-MM-DD.
   - Princípios são declarativos, testáveis, e livres de linguagem vaga ("deveria" → substitua com justificativa MUST/SHOULD onde apropriado).

7. Escreva a constituição completada de volta para `.specify/memory/constitution.md` (sobrescrever).

8. Produza um resumo final para o usuário com:
   - Nova versão e justificativa de bump.
   - Quaisquer arquivos sinalizados para acompanhamento manual.
   - Mensagem de commit sugerida (ex., `docs: emenda constituição para vX.Y.Z (adições de princípio + atualização de governança)`).

Requisitos de Formatação e Estilo:

- Use cabeçalhos Markdown exatamente como no template (não promova/rebaixe níveis).
- Quebre linhas de justificativa longas para manter legibilidade (<100 chars idealmente) mas não force rigidamente com quebras estranhas.
- Mantenha uma única linha em branco entre seções.
- Evite espaços em branco no final.

Se o usuário fornecer atualizações parciais (ex., apenas uma revisão de princípio), ainda execute etapas de validação e decisão de versão.

Se informação crítica ausente (ex., data de ratificação verdadeiramente desconhecida), insira `TODO(<FIELD_NAME>): explicação` e inclua no Relatório de Impacto de Sincronização sob itens adiados.

Não crie um novo template; sempre opere no arquivo existente `.specify/memory/constitution.md`.
