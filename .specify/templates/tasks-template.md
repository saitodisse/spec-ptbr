---
description: "Template de lista de tarefas para implementação de funcionalidade"
---

# Tarefas: [NOME DA FUNCIONALIDADE]

**Entrada**: Documentos de design de `/specs/[###-nome-funcionalidade]/`
**Pré-requisitos**: plan.md (obrigatório), spec.md (obrigatório para histórias de usuário), research.md, data-model.md, contracts/

**Testes**: Os exemplos abaixo incluem tarefas de teste. Testes são OPCIONAIS - inclua-os apenas se explicitamente solicitado na especificação da funcionalidade.

**Organização**: Tarefas são agrupadas por história de usuário para permitir implementação e testes independentes de cada história.

## Formato: `[ID] [P?] [Story] Descrição`

- **[P]**: Pode executar em paralelo (arquivos diferentes, sem dependências)
- **[Story]**: A qual história de usuário esta tarefa pertence (ex: US1, US2, US3)
- Inclua caminhos de arquivo exatos nas descrições

## Convenções de Caminho

- **Projeto único**: `src/`, `tests/` na raiz do repositório
- **Aplicativo web**: `backend/src/`, `frontend/src/`
- **Mobile**: `api/src/`, `ios/src/` ou `android/src/`
- Caminhos mostrados abaixo assumem projeto único - ajuste baseado na estrutura de plan.md

<!--
  ============================================================================
  IMPORTANTE: As tarefas abaixo são TAREFAS DE AMOSTRA apenas para propósitos de ilustração.

  O comando /speckit.tasks DEVE substituir estas com tarefas reais baseadas em:
  - Histórias de usuário de spec.md (com suas prioridades P1, P2, P3...)
  - Requisitos de funcionalidade de plan.md
  - Entidades de data-model.md
  - Endpoints de contracts/

  As tarefas DEVEM ser organizadas por história de usuário para que cada história possa ser:
  - Implementada independentemente
  - Testada independentemente
  - Entregue como um incremento MVP

  NÃO mantenha estas tarefas de amostra no arquivo tasks.md gerado.
  ============================================================================
-->

## Fase 1: Setup (Infraestrutura Compartilhada)

**Propósito**: Inicialização de projeto e estrutura básica

- [ ] T001 Criar estrutura de projeto conforme plano de implementação
- [ ] T002 Inicializar projeto [linguagem] com dependências [framework]
- [ ] T003 [P] Configurar ferramentas de linting e formatação

---

## Fase 2: Fundamental (Pré-requisitos Bloqueantes)

**Propósito**: Infraestrutura central que DEVE estar completa antes de QUALQUER história de usuário poder ser implementada

**⚠️ CRÍTICO**: Nenhum trabalho de história de usuário pode começar até esta fase estar completa

Exemplos de tarefas fundamentais (ajuste baseado em seu projeto):

- [ ] T004 Configurar schema de banco de dados e framework de migrações
- [ ] T005 [P] Implementar framework de autenticação/autorização
- [ ] T006 [P] Configurar estrutura de roteamento de API e middleware
- [ ] T007 Criar modelos/entidades base de que todas as histórias dependem
- [ ] T008 Configurar infraestrutura de tratamento de erros e logging
- [ ] T009 Configurar gerenciamento de configuração de ambiente

**Checkpoint**: Fundação pronta - implementação de histórias de usuário pode agora começar em paralelo

---

## Fase 3: História de Usuário 1 - [Título] (Prioridade: P1) 🎯 MVP

**Objetivo**: [Breve descrição do que esta história entrega]

**Teste Independente**: [Como verificar que esta história funciona por conta própria]

### Testes para História de Usuário 1 (OPCIONAL - apenas se testes solicitados) ⚠️

**NOTA: Escreva estes testes PRIMEIRO, garanta que FALHEM antes da implementação**

- [ ] T010 [P] [US1] Teste de contrato para [endpoint] em tests/contract/test\_[nome].py
- [ ] T011 [P] [US1] Teste de integração para [jornada do usuário] em tests/integration/test\_[nome].py

### Implementação para História de Usuário 1

- [ ] T012 [P] [US1] Criar modelo [Entidade1] em src/models/[entidade1].py
- [ ] T013 [P] [US1] Criar modelo [Entidade2] em src/models/[entidade2].py
- [ ] T014 [US1] Implementar [Serviço] em src/services/[servico].py (depende de T012, T013)
- [ ] T015 [US1] Implementar [endpoint/funcionalidade] em src/[local]/[arquivo].py
- [ ] T016 [US1] Adicionar validação e tratamento de erros
- [ ] T017 [US1] Adicionar logging para operações da história de usuário 1

**Checkpoint**: Neste ponto, História de Usuário 1 deve estar totalmente funcional e testável independentemente

---

## Fase 4: História de Usuário 2 - [Título] (Prioridade: P2)

**Objetivo**: [Breve descrição do que esta história entrega]

**Teste Independente**: [Como verificar que esta história funciona por conta própria]

### Testes para História de Usuário 2 (OPCIONAL - apenas se testes solicitados) ⚠️

- [ ] T018 [P] [US2] Teste de contrato para [endpoint] em tests/contract/test\_[nome].py
- [ ] T019 [P] [US2] Teste de integração para [jornada do usuário] em tests/integration/test\_[nome].py

### Implementação para História de Usuário 2

- [ ] T020 [P] [US2] Criar modelo [Entidade] em src/models/[entidade].py
- [ ] T021 [US2] Implementar [Serviço] em src/services/[servico].py
- [ ] T022 [US2] Implementar [endpoint/funcionalidade] em src/[local]/[arquivo].py
- [ ] T023 [US2] Integrar com componentes da História de Usuário 1 (se necessário)

**Checkpoint**: Neste ponto, Histórias de Usuário 1 E 2 devem ambas funcionar independentemente

---

## Fase 5: História de Usuário 3 - [Título] (Prioridade: P3)

**Objetivo**: [Breve descrição do que esta história entrega]

**Teste Independente**: [Como verificar que esta história funciona por conta própria]

### Testes para História de Usuário 3 (OPCIONAL - apenas se testes solicitados) ⚠️

- [ ] T024 [P] [US3] Teste de contrato para [endpoint] em tests/contract/test\_[nome].py
- [ ] T025 [P] [US3] Teste de integração para [jornada do usuário] em tests/integration/test\_[nome].py

### Implementação para História de Usuário 3

- [ ] T026 [P] [US3] Criar modelo [Entidade] em src/models/[entidade].py
- [ ] T027 [US3] Implementar [Serviço] em src/services/[servico].py
- [ ] T028 [US3] Implementar [endpoint/funcionalidade] em src/[local]/[arquivo].py

**Checkpoint**: Todas as histórias de usuário devem agora estar independentemente funcionais

---

[Adicione mais fases de histórias de usuário conforme necessário, seguindo o mesmo padrão]

---

## Fase N: Polish e Preocupações Transversais

**Propósito**: Melhorias que afetam múltiplas histórias de usuário

- [ ] TXXX [P] Atualizações de documentação em docs/
- [ ] TXXX Limpeza de código e refatoração
- [ ] TXXX Otimização de desempenho em todas as histórias
- [ ] TXXX [P] Testes unitários adicionais (se solicitado) em tests/unit/
- [ ] TXXX Endurecimento de segurança
- [ ] TXXX Executar validação de quickstart.md

---

## Dependências e Ordem de Execução

### Dependências de Fase

- **Setup (Fase 1)**: Sem dependências - pode começar imediatamente
- **Fundamental (Fase 2)**: Depende de conclusão de Setup - BLOQUEIA todas histórias de usuário
- **Histórias de Usuário (Fase 3+)**: Todas dependem de conclusão da fase Fundamental
  - Histórias de usuário podem então prosseguir em paralelo (se pessoal disponível)
  - Ou sequencialmente em ordem de prioridade (P1 → P2 → P3)
- **Polish (Fase Final)**: Depende de todas histórias de usuário desejadas estarem completas

### Dependências de História de Usuário

- **História de Usuário 1 (P1)**: Pode começar após Fundamental (Fase 2) - Sem dependências de outras histórias
- **História de Usuário 2 (P2)**: Pode começar após Fundamental (Fase 2) - Pode integrar com US1 mas deve ser testável independentemente
- **História de Usuário 3 (P3)**: Pode começar após Fundamental (Fase 2) - Pode integrar com US1/US2 mas deve ser testável independentemente

### Dentro de Cada História de Usuário

- Testes (se incluídos) DEVEM ser escritos e FALHAR antes da implementação
- Modelos antes de serviços
- Serviços antes de endpoints
- Implementação central antes de integração
- História completa antes de mover para próxima prioridade

### Oportunidades Paralelas

- Todas tarefas de Setup marcadas [P] podem executar em paralelo
- Todas tarefas Fundamentais marcadas [P] podem executar em paralelo (dentro da Fase 2)
- Uma vez fase Fundamental completa, todas histórias de usuário podem começar em paralelo (se capacidade de equipe permitir)
- Todos os testes para uma história de usuário marcados [P] podem executar em paralelo
- Modelos dentro de uma história marcados [P] podem executar em paralelo
- Diferentes histórias de usuário podem ser trabalhadas em paralelo por diferentes membros da equipe

---

## Exemplo Paralelo: História de Usuário 1

```bash
# Lançar todos os testes para História de Usuário 1 juntos (se testes solicitados):
Tarefa: "Teste de contrato para [endpoint] em tests/contract/test_[nome].py"
Tarefa: "Teste de integração para [jornada do usuário] em tests/integration/test_[nome].py"

# Lançar todos os modelos para História de Usuário 1 juntos:
Tarefa: "Criar modelo [Entidade1] em src/models/[entidade1].py"
Tarefa: "Criar modelo [Entidade2] em src/models/[entidade2].py"
```

---

## Estratégia de Implementação

### MVP Primeiro (Apenas História de Usuário 1)

1. Completar Fase 1: Setup
2. Completar Fase 2: Fundamental (CRÍTICO - bloqueia todas as histórias)
3. Completar Fase 3: História de Usuário 1
4. **PARAR e VALIDAR**: Testar História de Usuário 1 independentemente
5. Deploy/demo se pronto

### Entrega Incremental

1. Completar Setup + Fundamental → Fundação pronta
2. Adicionar História de Usuário 1 → Testar independentemente → Deploy/Demo (MVP!)
3. Adicionar História de Usuário 2 → Testar independentemente → Deploy/Demo
4. Adicionar História de Usuário 3 → Testar independentemente → Deploy/Demo
5. Cada história adiciona valor sem quebrar histórias anteriores

### Estratégia de Equipe Paralela

Com múltiplos desenvolvedores:

1. Equipe completa Setup + Fundamental juntos
2. Uma vez Fundamental pronto:
   - Desenvolvedor A: História de Usuário 1
   - Desenvolvedor B: História de Usuário 2
   - Desenvolvedor C: História de Usuário 3
3. Histórias completam e integram independentemente

---

## Notas

- Tarefas [P] = arquivos diferentes, sem dependências
- Label [Story] mapeia tarefa para história de usuário específica para rastreabilidade
- Cada história de usuário deve ser independentemente completável e testável
- Verifique que testes falham antes de implementar
- Commit após cada tarefa ou grupo lógico
- Pare em qualquer checkpoint para validar história independentemente
- Evite: tarefas vagas, conflitos de mesmo arquivo, dependências entre histórias que quebram independência
