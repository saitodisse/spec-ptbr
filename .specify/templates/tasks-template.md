# Tarefas: [FEATURE NAME]

**Entrada**: Documentos de design de `/specs/[###-feature-name]/`
**Pré-requisitos**: plan.md (obrigatório), research.md, data-model.md, contracts/

## Fluxo de Execução (principal)

```
1. Carregue plan.md do diretório de funcionalidade
   → Se não encontrado: ERRO "Nenhum plano de implementação encontrado"
   → Extraia: stack tecnológico, bibliotecas, estrutura
2. Carregue documentos de design opcionais:
   → data-model.md: Extraia entidades → tarefas de modelo
   → contracts/: Cada arquivo → tarefa de teste de contrato
   → research.md: Extraia decisões → tarefas de configuração
3. Gere tarefas por categoria:
   → Configuração: inicialização do projeto, dependências, linting
   → Testes: testes de contrato, testes de integração
   → Núcleo: modelos, serviços, comandos CLI
   → Integração: DB, middleware, logging
   → Polimento: testes unitários, performance, docs
4. Aplique regras de tarefa:
   → Arquivos diferentes = marque [P] para paralelo
   → Mesmo arquivo = sequencial (sem [P])
   → Testes antes da implementação (TDD)
5. Numere tarefas sequencialmente (T001, T002...)
6. Gere grafo de dependências
7. Crie exemplos de execução paralela
8. Valide completude de tarefas:
   → Todos os contratos têm testes?
   → Todas as entidades têm modelos?
   → Todos os endpoints implementados?
9. Retorne: SUCESSO (tarefas prontas para execução)
```

## Formato: `[ID] [P?] Descrição`

- **[P]**: Pode executar em paralelo (arquivos diferentes, sem dependências)
- Inclua caminhos de arquivo exatos nas descrições

## Convenções de Caminho

- **Projeto único**: `src/`, `tests/` na raiz do repositório
- **App web**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` ou `android/src/`
- Caminhos mostrados abaixo assumem projeto único - ajuste baseado na estrutura plan.md

## Fase 3.1: Configuração

- [ ] T001 Criar estrutura do projeto conforme plano de implementação
- [ ] T002 Inicializar projeto [linguagem] com dependências [framework]
- [ ] T003 [P] Configurar ferramentas de linting e formatação

## Fase 3.2: Testes Primeiro (TDD) ⚠️ DEVE COMPLETAR ANTES DE 3.3

**CRÍTICO: Estes testes DEVEM ser escritos e DEVEM FALHAR antes de QUALQUER implementação**

- [ ] T004 [P] Teste de contrato POST /api/users em tests/contract/test_users_post.py
- [ ] T005 [P] Teste de contrato GET /api/users/{id} em tests/contract/test_users_get.py
- [ ] T006 [P] Teste de integração registro de usuário em tests/integration/test_registration.py
- [ ] T007 [P] Teste de integração fluxo de auth em tests/integration/test_auth.py

## Fase 3.3: Implementação do Núcleo (APENAS após testes estarem falhando)

- [ ] T008 [P] Modelo User em src/models/user.py
- [ ] T009 [P] UserService CRUD em src/services/user_service.py
- [ ] T010 [P] CLI --create-user em src/cli/user_commands.py
- [ ] T011 Endpoint POST /api/users
- [ ] T012 Endpoint GET /api/users/{id}
- [ ] T013 Validação de entrada
- [ ] T014 Tratamento de erro e logging

## Fase 3.4: Integração

- [ ] T015 Conectar UserService ao DB
- [ ] T016 Middleware de auth
- [ ] T017 Logging de request/response
- [ ] T018 CORS e headers de segurança

## Fase 3.5: Polimento

- [ ] T019 [P] Testes unitários para validação em tests/unit/test_validation.py
- [ ] T020 Testes de performance (<200ms)
- [ ] T021 [P] Atualizar docs/api.md
- [ ] T022 Remover duplicação
- [ ] T023 Executar manual-testing.md

## Dependências

- Testes (T004-T007) antes da implementação (T008-T014)
- T008 bloqueia T009, T015
- T016 bloqueia T018
- Implementação antes do polimento (T019-T023)

## Exemplo Paralelo

```
# Lance T004-T007 juntos:
Tarefa: "Teste de contrato POST /api/users em tests/contract/test_users_post.py"
Tarefa: "Teste de contrato GET /api/users/{id} em tests/contract/test_users_get.py"
Tarefa: "Teste de integração registro em tests/integration/test_registration.py"
Tarefa: "Teste de integração auth em tests/integration/test_auth.py"
```

## Notas

- Tarefas [P] = arquivos diferentes, sem dependências
- Verifique que testes falham antes de implementar
- Commit após cada tarefa
- Evite: tarefas vagas, conflitos de mesmo arquivo

## Regras de Geração de Tarefas

_Aplicadas durante execução main()_

1. **Dos Contratos**:
   - Cada arquivo de contrato → tarefa de teste de contrato [P]
   - Cada endpoint → tarefa de implementação
2. **Do Modelo de Dados**:
   - Cada entidade → tarefa de criação de modelo [P]
   - Relacionamentos → tarefas de camada de serviço
3. **Das Histórias de Usuário**:

   - Cada história → teste de integração [P]
   - Cenários quickstart → tarefas de validação

4. **Ordenação**:
   - Configuração → Testes → Modelos → Serviços → Endpoints → Polimento
   - Dependências bloqueiam execução paralela

## Lista de Verificação de Validação

_PORTÃO: Verificado por main() antes de retornar_

- [ ] Todos os contratos têm testes correspondentes
- [ ] Todas as entidades têm tarefas de modelo
- [ ] Todos os testes vêm antes da implementação
- [ ] Tarefas paralelas verdadeiramente independentes
- [ ] Cada tarefa especifica caminho de arquivo exato
- [ ] Nenhuma tarefa modifica mesmo arquivo que outra tarefa [P]
