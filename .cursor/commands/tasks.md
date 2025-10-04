---
description: Gerar um tasks.md acionável e ordenado por dependências para a funcionalidade baseado em artefatos de design disponíveis.
---

A entrada do usuário pode ser fornecida diretamente pelo agente ou como argumento de comando - você **DEVE** considerá-la antes de prosseguir com o prompt (se não estiver vazia).

Entrada do usuário:

$ARGUMENTS

1. Execute `.specify/scripts/bash/check-prerequisites.sh --json` da raiz do repositório e analise a lista FEATURE_DIR e AVAILABLE_DOCS. Todos os caminhos devem ser absolutos.
2. Carregue e analise documentos de design disponíveis:

   - Sempre leia plan.md para stack tecnológico e bibliotecas
   - SE EXISTIR: Leia data-model.md para entidades
   - SE EXISTIR: Leia contracts/ para endpoints de API
   - SE EXISTIR: Leia research.md para decisões técnicas
   - SE EXISTIR: Leia quickstart.md para cenários de teste

   Nota: Nem todos os projetos têm todos os documentos. Por exemplo:

   - Ferramentas CLI podem não ter contracts/
   - Bibliotecas simples podem não precisar de data-model.md
   - Gere tarefas baseado no que está disponível

3. Gere tarefas seguindo o template:

   - Use `.specify/templates/tasks-template.md` como base
   - Substitua tarefas de exemplo com tarefas reais baseadas em:
     - **Tarefas de configuração**: Inicialização do projeto, dependências, linting
     - **Tarefas de teste [P]**: Uma por contrato, uma por cenário de integração
     - **Tarefas principais**: Uma por entidade, serviço, comando CLI, endpoint
     - **Tarefas de integração**: Conexões de DB, middleware, logging
     - **Tarefas de polimento [P]**: Testes unitários, performance, docs

4. Regras de geração de tarefas:

   - Cada arquivo de contrato → tarefa de teste de contrato marcada [P]
   - Cada entidade em data-model → tarefa de criação de modelo marcada [P]
   - Cada endpoint → tarefa de implementação (não paralela se arquivos compartilhados)
   - Cada história de usuário → teste de integração marcado [P]
   - Arquivos diferentes = podem ser paralelos [P]
   - Mesmo arquivo = sequencial (sem [P])

5. Ordene tarefas por dependências:

   - Configuração antes de tudo
   - Testes antes da implementação (TDD)
   - Modelos antes de serviços
   - Serviços antes de endpoints
   - Núcleo antes de integração
   - Tudo antes de polimento

6. Inclua exemplos de execução paralela:

   - Agrupe tarefas [P] que podem executar juntas
   - Mostre comandos reais do agente Task

7. Crie FEATURE_DIR/tasks.md com:
   - Nome correto da funcionalidade do plano de implementação
   - Tarefas numeradas (T001, T002, etc.)
   - Caminhos de arquivo claros para cada tarefa
   - Notas de dependência
   - Orientação de execução paralela

Contexto para geração de tarefas: $ARGUMENTS

O tasks.md deve ser imediatamente executável - cada tarefa deve ser específica o suficiente para que um LLM possa completá-la sem contexto adicional.
