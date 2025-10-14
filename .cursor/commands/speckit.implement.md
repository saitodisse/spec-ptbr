---
description: Executar o plano de implementação processando e executando todas as tarefas definidas em tasks.md
---

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Resumo

1. Execute `.specify/scripts/bash/check-prerequisites.sh --json --require-tasks --include-tasks` a partir da raiz do repositório e analise FEATURE_DIR e lista AVAILABLE_DOCS. Todos os caminhos devem ser absolutos. Para aspas simples em argumentos como "I'm Groot", use sintaxe de escape: por exemplo 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

2. **Verificar status dos checklists** (se FEATURE_DIR/checklists/ existir):

   - Escaneie todos os arquivos de checklist no diretório checklists/
   - Para cada checklist, conte:
     - Total de itens: Todas as linhas correspondendo a `- [ ]` ou `- [X]` ou `- [x]`
     - Itens completos: Linhas correspondendo a `- [X]` ou `- [x]`
     - Itens incompletos: Linhas correspondendo a `- [ ]`
   - Crie uma tabela de status:
     ```
     | Checklist | Total | Completo | Incompleto | Status |
     |-----------|-------|----------|------------|--------|
     | ux.md     | 12    | 12       | 0          | ✓ PASSOU |
     | test.md   | 8     | 5        | 3          | ✗ FALHOU |
     | security.md | 6   | 6        | 0          | ✓ PASSOU |
     ```
   - Calcule status geral:

     - **PASSOU**: Todos os checklists têm 0 itens incompletos
     - **FALHOU**: Um ou mais checklists têm itens incompletos

   - **Se algum checklist estiver incompleto**:

     - Exiba a tabela com contagens de itens incompletos
     - **PARE** e pergunte: "Alguns checklists estão incompletos. Deseja prosseguir com a implementação mesmo assim? (sim/não)"
     - Aguarde resposta do usuário antes de continuar
     - Se usuário disser "não" ou "aguarde" ou "pare", interrompa execução
     - Se usuário disser "sim" ou "prossiga" ou "continue", prossiga para etapa 3

   - **Se todos os checklists estiverem completos**:
     - Exiba a tabela mostrando que todos os checklists passaram
     - Prossiga automaticamente para etapa 3

3. Carregue e analise o contexto de implementação:

   - **OBRIGATÓRIO**: Leia tasks.md para a lista completa de tarefas e plano de execução
   - **OBRIGATÓRIO**: Leia plan.md para tech stack, arquitetura e estrutura de arquivos
   - **SE EXISTIR**: Leia data-model.md para entidades e relacionamentos
   - **SE EXISTIR**: Leia contracts/ para especificações de API e requisitos de teste
   - **SE EXISTIR**: Leia research.md para decisões técnicas e restrições
   - **SE EXISTIR**: Leia quickstart.md para cenários de integração

4. **Verificação de Configuração do Projeto**:

   - **OBRIGATÓRIO**: Criar/verificar arquivos ignore baseados na configuração real do projeto:

   **Lógica de Detecção e Criação**:

   - Verifique se o seguinte comando é bem-sucedido para determinar se o repositório é um repositório git (criar/verificar .gitignore se sim):

     ```sh
     git rev-parse --git-dir 2>/dev/null
     ```

   - Verifique se Dockerfile\* existe ou Docker em plan.md → criar/verificar .dockerignore
   - Verifique se .eslintrc* ou eslint.config.* existe → criar/verificar .eslintignore
   - Verifique se .prettierrc\* existe → criar/verificar .prettierignore
   - Verifique se .npmrc ou package.json existe → criar/verificar .npmignore (se publicando)
   - Verifique se arquivos terraform (\*.tf) existem → criar/verificar .terraformignore
   - Verifique se .helmignore necessário (charts helm presentes) → criar/verificar .helmignore

   **Se arquivo ignore já existir**: Verifique se contém padrões essenciais, anexe apenas padrões críticos ausentes
   **Se arquivo ignore ausente**: Crie com conjunto completo de padrões para tecnologia detectada

   **Padrões Comuns por Tecnologia** (do tech stack de plan.md):

   - **Node.js/JavaScript**: `node_modules/`, `dist/`, `build/`, `*.log`, `.env*`
   - **Python**: `__pycache__/`, `*.pyc`, `.venv/`, `venv/`, `dist/`, `*.egg-info/`
   - **Java**: `target/`, `*.class`, `*.jar`, `.gradle/`, `build/`
   - **C#/.NET**: `bin/`, `obj/`, `*.user`, `*.suo`, `packages/`
   - **Go**: `*.exe`, `*.test`, `vendor/`, `*.out`
   - **Universal**: `.DS_Store`, `Thumbs.db`, `*.tmp`, `*.swp`, `.vscode/`, `.idea/`

   **Padrões Específicos de Ferramenta**:

   - **Docker**: `node_modules/`, `.git/`, `Dockerfile*`, `.dockerignore`, `*.log*`, `.env*`, `coverage/`
   - **ESLint**: `node_modules/`, `dist/`, `build/`, `coverage/`, `*.min.js`
   - **Prettier**: `node_modules/`, `dist/`, `build/`, `coverage/`, `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`
   - **Terraform**: `.terraform/`, `*.tfstate*`, `*.tfvars`, `.terraform.lock.hcl`

5. Analise a estrutura de tasks.md e extraia:

   - **Fases de tarefas**: Setup, Tests, Core, Integration, Polish
   - **Dependências de tarefas**: Regras de execução sequencial vs paralela
   - **Detalhes de tarefas**: ID, descrição, caminhos de arquivo, marcadores paralelos [P]
   - **Fluxo de execução**: Ordem e requisitos de dependência

6. Execute implementação seguindo o plano de tarefas:

   - **Execução fase por fase**: Complete cada fase antes de mover para a próxima
   - **Respeite dependências**: Execute tarefas sequenciais em ordem, tarefas paralelas [P] podem executar juntas
   - **Siga abordagem TDD**: Execute tarefas de teste antes de suas tarefas de implementação correspondentes
   - **Coordenação baseada em arquivo**: Tarefas afetando os mesmos arquivos devem executar sequencialmente
   - **Pontos de validação**: Verifique conclusão de cada fase antes de prosseguir

7. Regras de execução de implementação:

   - **Setup primeiro**: Inicialize estrutura de projeto, dependências, configuração
   - **Testes antes de código**: Se você precisa escrever testes para contratos, entidades e cenários de integração
   - **Desenvolvimento central**: Implemente modelos, serviços, comandos CLI, endpoints
   - **Trabalho de integração**: Conexões de banco de dados, middleware, logging, serviços externos
   - **Polish e validação**: Testes unitários, otimização de desempenho, documentação

8. Rastreamento de progresso e tratamento de erros:

   - Reporte progresso após cada tarefa concluída
   - Interrompa execução se qualquer tarefa não paralela falhar
   - Para tarefas paralelas [P], continue com tarefas bem-sucedidas, reporte as que falharam
   - Forneça mensagens de erro claras com contexto para debugging
   - Sugira próximos passos se implementação não puder prosseguir
   - **IMPORTANTE** Para tarefas concluídas, certifique-se de marcar a tarefa como [X] no arquivo de tarefas.

9. Validação de conclusão:
   - Verifique se todas as tarefas requeridas estão concluídas
   - Verifique se funcionalidades implementadas correspondem à especificação original
   - Valide que testes passam e cobertura atende aos requisitos
   - Confirme que a implementação segue o plano técnico
   - Reporte status final com resumo do trabalho concluído

Nota: Este comando assume que uma quebra completa de tarefas existe em tasks.md. Se tarefas estiverem incompletas ou ausentes, sugira executar `/tasks` primeiro para regenerar a lista de tarefas.
