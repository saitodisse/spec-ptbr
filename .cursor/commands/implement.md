---
description: Executar o plano de implementação processando e executando todas as tarefas definidas em tasks.md
---

A entrada do usuário pode ser fornecida diretamente pelo agente ou como argumento de comando - você **DEVE** considerá-la antes de prosseguir com o prompt (se não estiver vazia).

Entrada do usuário:

$ARGUMENTS

1. Execute `.specify/scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks` da raiz do repositório e analise a lista FEATURE_DIR e AVAILABLE_DOCS. Todos os caminhos devem ser absolutos.

2. Carregue e analise o contexto de implementação:

   - **OBRIGATÓRIO**: Leia tasks.md para a lista completa de tarefas e plano de execução
   - **OBRIGATÓRIO**: Leia plan.md para stack tecnológico, arquitetura e estrutura de arquivos
   - **SE EXISTIR**: Leia data-model.md para entidades e relacionamentos
   - **SE EXISTIR**: Leia contracts/ para especificações de API e requisitos de teste
   - **SE EXISTIR**: Leia research.md para decisões técnicas e restrições
   - **SE EXISTIR**: Leia quickstart.md para cenários de integração

3. Analise a estrutura de tasks.md e extraia:

   - **Fases de tarefas**: Configuração, Testes, Núcleo, Integração, Polimento
   - **Dependências de tarefas**: Regras de execução sequencial vs paralela
   - **Detalhes de tarefas**: ID, descrição, caminhos de arquivo, marcadores paralelos [P]
   - **Fluxo de execução**: Ordem e requisitos de dependência

4. Execute implementação seguindo o plano de tarefas:

   - **Execução fase por fase**: Complete cada fase antes de mover para a próxima
   - **Respeite dependências**: Execute tarefas sequenciais em ordem, tarefas paralelas [P] podem executar juntas
   - **Siga abordagem TDD**: Execute tarefas de teste antes de suas tarefas de implementação correspondentes
   - **Coordenação baseada em arquivo**: Tarefas afetando os mesmos arquivos devem executar sequencialmente
   - **Pontos de verificação de validação**: Verifique a conclusão de cada fase antes de prosseguir

5. Regras de execução de implementação:

   - **Configuração primeiro**: Inicialize estrutura do projeto, dependências, configuração
   - **Testes antes do código**: Se você precisar escrever testes para contratos, entidades e cenários de integração
   - **Desenvolvimento do núcleo**: Implemente modelos, serviços, comandos CLI, endpoints
   - **Trabalho de integração**: Conexões de banco de dados, middleware, logging, serviços externos
   - **Polimento e validação**: Testes unitários, otimização de performance, documentação

6. Rastreamento de progresso e tratamento de erros:

   - Reporte progresso após cada tarefa completada
   - Pare execução se qualquer tarefa não-paralela falhar
   - Para tarefas paralelas [P], continue com tarefas bem-sucedidas, reporte as que falharam
   - Forneça mensagens de erro claras com contexto para depuração
   - Sugira próximos passos se implementação não puder prosseguir
   - **IMPORTANTE** Para tarefas completadas, certifique-se de marcar a tarefa como [X] no arquivo de tarefas.

7. Validação de conclusão:
   - Verifique que todas as tarefas obrigatórias estão completadas
   - Verifique que funcionalidades implementadas correspondem à especificação original
   - Valide que testes passam e cobertura atende requisitos
   - Confirme que implementação segue o plano técnico
   - Reporte status final com resumo do trabalho completado

Nota: Este comando assume que uma quebra completa de tarefas existe em tasks.md. Se tarefas estão incompletas ou ausentes, sugira executar `/tasks` primeiro para regenerar a lista de tarefas.
