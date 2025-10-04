---
description: Criar ou atualizar a especificação de funcionalidade a partir de uma descrição de funcionalidade em linguagem natural.
---

A entrada do usuário pode ser fornecida diretamente pelo agente ou como argumento de comando - você **DEVE** considerá-la antes de prosseguir com o prompt (se não estiver vazia).

Entrada do usuário:

$ARGUMENTS

O texto que o usuário digitou após `/specify` na mensagem acionadora **é** a descrição da funcionalidade. Assuma que você sempre tem isso disponível nesta conversa mesmo que `$ARGUMENTS` apareça literalmente abaixo. Não peça ao usuário para repetir a menos que ele tenha fornecido um comando vazio.

Dada essa descrição de funcionalidade, faça isso:

1. Execute o script `.specify/scripts/bash/create-new-feature.sh --json "$ARGUMENTS"` da raiz do repositório e analise sua saída JSON para BRANCH_NAME e SPEC_FILE. Todos os caminhos de arquivo devem ser absolutos.
   **IMPORTANTE** Você deve executar este script apenas uma vez. O JSON é fornecido no terminal como saída - sempre se refira a ele para obter o conteúdo real que você está procurando.
2. Carregue `.specify/templates/spec-template.md` para entender seções obrigatórias.
3. Escreva a especificação para SPEC_FILE usando a estrutura do template, substituindo placeholders com detalhes concretos derivados da descrição da funcionalidade (argumentos) preservando ordem de seções e cabeçalhos.
4. Reporte conclusão com nome do branch, caminho do arquivo de especificação e prontidão para a próxima fase.

Nota: O script cria e faz checkout do novo branch e inicializa o arquivo de especificação antes de escrever.
