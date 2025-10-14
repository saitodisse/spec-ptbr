---
description: Criar ou atualizar a constituição do projeto a partir de entradas de princípios interativas ou fornecidas, garantindo que todos os templates dependentes permaneçam em sincronia.
---

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Resumo

Você está atualizando a constituição do projeto em `.specify/memory/constitution.md`. Este arquivo é um TEMPLATE contendo tokens de placeholder entre colchetes (por exemplo `[PROJECT_NAME]`, `[PRINCIPLE_1_NAME]`). Seu trabalho é (a) coletar/derivar valores concretos, (b) preencher o template precisamente, e (c) propagar quaisquer emendas através de artefatos dependentes.

Siga este fluxo de execução:

1. Carregue o template de constituição existente em `.specify/memory/constitution.md`.

   - Identifique cada token de placeholder da forma `[IDENTIFICADOR_MAIUSCULAS]`.
     **IMPORTANTE**: O usuário pode requerer menos ou mais princípios do que os usados no template. Se um número for especificado, respeite isso - siga o template geral. Você atualizará o documento adequadamente.

2. Colete/derive valores para placeholders:

   - Se entrada do usuário (conversa) fornecer um valor, use-o.
   - Caso contrário, infira do contexto existente do repositório (README, docs, versões anteriores da constituição se incorporadas).
   - Para datas de governança: `RATIFICATION_DATE` é a data de adoção original (se desconhecida pergunte ou marque TODO), `LAST_AMENDED_DATE` é hoje se mudanças forem feitas, caso contrário mantenha anterior.
   - `CONSTITUTION_VERSION` deve incrementar de acordo com regras de versionamento semântico:
     - MAJOR: Remoções ou redefinições de governança/princípios incompatíveis com versões anteriores.
     - MINOR: Novo princípio/seção adicionado ou orientação materialmente expandida.
     - PATCH: Esclarecimentos, redação, correções de digitação, refinamentos não semânticos.
   - Se tipo de bump de versão ambíguo, proponha raciocínio antes de finalizar.

3. Rascunhe o conteúdo da constituição atualizada:

   - Substitua cada placeholder por texto concreto (sem tokens entre colchetes restantes exceto slots de template intencionalmente retidos que o projeto escolheu não definir ainda—justifique explicitamente qualquer restante).
   - Preserve hierarquia de cabeçalhos e comentários podem ser removidos uma vez substituídos a menos que ainda adicionem orientação esclarecedora.
   - Garanta cada seção de Princípio: linha de nome sucinto, parágrafo (ou lista bullet) capturando regras não negociáveis, justificativa explícita se não óbvia.
   - Garanta seção de Governança lista procedimento de emenda, política de versionamento e expectativas de revisão de compliance.

4. Checklist de propagação de consistência (converta checklist anterior em validações ativas):

   - Leia `.specify/templates/plan-template.md` e garanta que qualquer "Verificação de Constituição" ou regras se alinhem com princípios atualizados.
   - Leia `.specify/templates/spec-template.md` para alinhamento de escopo/requisitos—atualize se constituição adiciona/remove seções obrigatórias ou restrições.
   - Leia `.specify/templates/tasks-template.md` e garanta que categorização de tarefas reflita tipos de tarefas novos ou removidos impulsionados por princípios (por exemplo, observabilidade, versionamento, disciplina de testes).
   - Leia cada arquivo de comando em `.specify/templates/commands/*.md` (incluindo este) para verificar que nenhuma referência desatualizada (nomes específicos de agente como CLAUDE apenas) permanece quando orientação genérica é requerida.
   - Leia quaisquer docs de orientação de runtime (por exemplo, `README.md`, `docs/quickstart.md`, ou arquivos de orientação específicos de agente se presentes). Atualize referências a princípios alterados.

5. Produza um Relatório de Impacto de Sincronização (prefixe como um comentário HTML no topo do arquivo de constituição após atualização):

   - Mudança de versão: antiga → nova
   - Lista de princípios modificados (título antigo → título novo se renomeado)
   - Seções adicionadas
   - Seções removidas
   - Templates requerendo atualizações (✅ atualizado / ⚠ pendente) com caminhos de arquivo
   - TODOs de acompanhamento se quaisquer placeholders intencionalmente adiados.

6. Validação antes da saída final:

   - Nenhum token entre colchetes inexplicado restante.
   - Linha de versão corresponde ao relatório.
   - Datas no formato ISO AAAA-MM-DD.
   - Princípios são declarativos, testáveis e livres de linguagem vaga ("deveria" → substitua por justificativa DEVE/DEVERIA onde apropriado).

7. Grave a constituição completa de volta para `.specify/memory/constitution.md` (sobrescreva).

8. Produza um resumo final ao usuário com:
   - Nova versão e justificativa de bump.
   - Quaisquer arquivos sinalizados para acompanhamento manual.
   - Mensagem de commit sugerida (por exemplo, `docs: emendar constituição para vX.Y.Z (adições de princípio + atualização de governança)`).

Requisitos de Formatação e Estilo:

- Use cabeçalhos Markdown exatamente como no template (não rebaixe/promova níveis).
- Quebre linhas longas de justificativa para manter legibilidade (<100 caracteres idealmente) mas não force com quebras estranhas.
- Mantenha uma linha em branco única entre seções.
- Evite espaços em branco finais.

Se o usuário fornecer atualizações parciais (por exemplo, apenas uma revisão de princípio), ainda execute etapas de validação e decisão de versão.

Se informação crítica ausente (por exemplo, data de ratificação verdadeiramente desconhecida), insira `TODO(<NOME_CAMPO>): explicação` e inclua no Relatório de Impacto de Sincronização sob itens adiados.

Não crie um novo template; sempre opere no arquivo `.specify/memory/constitution.md` existente.
