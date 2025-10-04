# Plano de Implementação: [FEATURE]

**Branch**: `[###-feature-name]` | **Data**: [DATE] | **Spec**: [link]
**Entrada**: Especificação de funcionalidade de `/specs/[###-feature-name]/spec.md`

## Fluxo de Execução (escopo do comando /plan)

```
1. Carregue especificação de funcionalidade do caminho de Entrada
   → Se não encontrada: ERRO "Nenhuma especificação de funcionalidade em {path}"
2. Preencha Contexto Técnico (escaneie por PRECISA CLARIFICAÇÃO)
   → Detecte Tipo de Projeto da estrutura do sistema de arquivos ou contexto (web=frontend+backend, mobile=app+api)
   → Defina Decisão de Estrutura baseada no tipo de projeto
3. Preencha a seção Verificação Constitucional baseada no conteúdo do documento constitucional.
4. Avalie seção Verificação Constitucional abaixo
   → Se violações existirem: Documente em Rastreamento de Complexidade
   → Se nenhuma justificativa possível: ERRO "Simplifique abordagem primeiro"
   → Atualize Rastreamento de Progresso: Verificação Constitucional Inicial
5. Execute Fase 0 → research.md
   → Se PRECISA CLARIFICAÇÃO permanecer: ERRO "Resolva incógnitas"
6. Execute Fase 1 → contracts, data-model.md, quickstart.md, arquivo de template específico do agente (ex., `CLAUDE.md` para Claude Code, `.github/copilot-instructions.md` para GitHub Copilot, `GEMINI.md` para Gemini CLI, `QWEN.md` para Qwen Code, ou `AGENTS.md` para todos os outros agentes).
7. Re-avalie seção Verificação Constitucional
   → Se novas violações: Refatore design, retorne à Fase 1
   → Atualize Rastreamento de Progresso: Verificação Constitucional Pós-Design
8. Planeje Fase 2 → Descreva abordagem de geração de tarefas (NÃO crie tasks.md)
9. PARE - Pronto para comando /tasks
```

**IMPORTANTE**: O comando /plan PARA na etapa 7. Fases 2-4 são executadas por outros comandos:

- Fase 2: comando /tasks cria tasks.md
- Fase 3-4: Execução de implementação (manual ou via ferramentas)

## Resumo

[Extraia da especificação de funcionalidade: requisito principal + abordagem técnica da pesquisa]

## Contexto Técnico

**Linguagem/Versão**: [ex., Python 3.11, Swift 5.9, Rust 1.75 ou PRECISA CLARIFICAÇÃO]  
**Dependências Principais**: [ex., FastAPI, UIKit, LLVM ou PRECISA CLARIFICAÇÃO]  
**Armazenamento**: [se aplicável, ex., PostgreSQL, CoreData, arquivos ou N/A]  
**Teste**: [ex., pytest, XCTest, cargo test ou PRECISA CLARIFICAÇÃO]  
**Plataforma Alvo**: [ex., servidor Linux, iOS 15+, WASM ou PRECISA CLARIFICAÇÃO]
**Tipo de Projeto**: [single/web/mobile - determina estrutura de código-fonte]  
**Objetivos de Performance**: [específico do domínio, ex., 1000 req/s, 10k linhas/seg, 60 fps ou PRECISA CLARIFICAÇÃO]  
**Restrições**: [específico do domínio, ex., <200ms p95, <100MB memória, offline-capable ou PRECISA CLARIFICAÇÃO]  
**Escala/Escopo**: [específico do domínio, ex., 10k usuários, 1M LOC, 50 telas ou PRECISA CLARIFICAÇÃO]

## Verificação Constitucional

_PORTÃO: Deve passar antes da pesquisa da Fase 0. Re-verifique após design da Fase 1._

[Portões determinados baseados no arquivo constitucional]

## Estrutura do Projeto

### Documentação (esta funcionalidade)

```
specs/[###-feature]/
├── plan.md              # Este arquivo (saída do comando /plan)
├── research.md          # Saída da Fase 0 (comando /plan)
├── data-model.md        # Saída da Fase 1 (comando /plan)
├── quickstart.md        # Saída da Fase 1 (comando /plan)
├── contracts/           # Saída da Fase 1 (comando /plan)
└── tasks.md             # Saída da Fase 2 (comando /tasks - NÃO criado por /plan)
```

### Código-Fonte (raiz do repositório)

<!--
  AÇÃO REQUERIDA: Substitua a árvore placeholder abaixo com o layout concreto
  para esta funcionalidade. Delete opções não usadas e expanda a estrutura escolhida com
  caminhos reais (ex., apps/admin, packages/something). O plano entregue não deve
  incluir rótulos de Opção.
-->

```
# [REMOVER SE NÃO USADO] Opção 1: Projeto único (PADRÃO)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [REMOVER SE NÃO USADO] Opção 2: Aplicação web (quando "frontend" + "backend" detectado)
backend/
├── src/
│   ├── models/
│   ├── services/
│   └── api/
└── tests/

frontend/
├── src/
│   ├── components/
│   ├── pages/
│   └── services/
└── tests/

# [REMOVER SE NÃO USADO] Opção 3: Mobile + API (quando "iOS/Android" detectado)
api/
└── [mesmo que backend acima]

ios/ ou android/
└── [estrutura específica da plataforma: módulos de funcionalidade, fluxos de UI, testes de plataforma]
```

**Decisão de Estrutura**: [Documente a estrutura selecionada e referencie os
diretórios reais capturados acima]

## Fase 0: Esboço e Pesquisa

1. **Extraia incógnitas do Contexto Técnico** acima:

   - Para cada PRECISA CLARIFICAÇÃO → tarefa de pesquisa
   - Para cada dependência → tarefa de melhores práticas
   - Para cada integração → tarefa de padrões

2. **Gere e despache agentes de pesquisa**:

   ```
   Para cada incógnita no Contexto Técnico:
     Tarefa: "Pesquise {incógnita} para {contexto de funcionalidade}"
   Para cada escolha de tecnologia:
     Tarefa: "Encontre melhores práticas para {tech} em {domínio}"
   ```

3. **Consolide descobertas** em `research.md` usando formato:
   - Decisão: [o que foi escolhido]
   - Justificativa: [por que escolhido]
   - Alternativas consideradas: [o que mais foi avaliado]

**Saída**: research.md com todas as PRECISA CLARIFICAÇÃO resolvidas

## Fase 1: Design e Contratos

_Pré-requisitos: research.md completo_

1. **Extraia entidades da especificação de funcionalidade** → `data-model.md`:

   - Nome da entidade, campos, relacionamentos
   - Regras de validação dos requisitos
   - Transições de estado se aplicável

2. **Gere contratos de API** dos requisitos funcionais:

   - Para cada ação do usuário → endpoint
   - Use padrões REST/GraphQL padrão
   - Produza esquema OpenAPI/GraphQL para `/contracts/`

3. **Gere testes de contrato** dos contratos:

   - Um arquivo de teste por endpoint
   - Afirme esquemas de request/response
   - Testes devem falhar (nenhuma implementação ainda)

4. **Extraia cenários de teste** das histórias de usuário:

   - Cada história → cenário de teste de integração
   - Teste quickstart = etapas de validação da história

5. **Atualize arquivo de agente incrementalmente** (operação O(1)):
   - Execute `.specify/scripts/bash/update-agent-context.sh cursor`
     **IMPORTANTE**: Execute exatamente como especificado acima. Não adicione ou remova argumentos.
   - Se existir: Adicione apenas NOVA tech do plano atual
   - Preserve adições manuais entre marcadores
   - Atualize mudanças recentes (mantenha últimas 3)
   - Mantenha sob 150 linhas para eficiência de token
   - Produza para raiz do repositório

**Saída**: data-model.md, /contracts/\*, testes falhando, quickstart.md, arquivo específico do agente

## Fase 2: Abordagem de Planejamento de Tarefas

_Esta seção descreve o que o comando /tasks fará - NÃO execute durante /plan_

**Estratégia de Geração de Tarefas**:

- Carregue `.specify/templates/tasks-template.md` como base
- Gere tarefas dos documentos de design da Fase 1 (contratos, modelo de dados, quickstart)
- Cada contrato → tarefa de teste de contrato [P]
- Cada entidade → tarefa de criação de modelo [P]
- Cada história de usuário → tarefa de teste de integração
- Tarefas de implementação para fazer testes passarem

**Estratégia de Ordenação**:

- Ordem TDD: Testes antes da implementação
- Ordem de dependência: Modelos antes de serviços antes de UI
- Marque [P] para execução paralela (arquivos independentes)

**Saída Estimada**: 25-30 tarefas numeradas e ordenadas em tasks.md

**IMPORTANTE**: Esta fase é executada pelo comando /tasks, NÃO por /plan

## Fase 3+: Implementação Futura

_Estas fases estão além do escopo do comando /plan_

**Fase 3**: Execução de tarefas (comando /tasks cria tasks.md)  
**Fase 4**: Implementação (execute tasks.md seguindo princípios constitucionais)  
**Fase 5**: Validação (execute testes, execute quickstart.md, validação de performance)

## Rastreamento de Complexidade

_Preencha APENAS se Verificação Constitucional tem violações que devem ser justificadas_

| Violação                 | Por Que Necessário    | Alternativa Mais Simples Rejeitada Porque  |
| ------------------------ | --------------------- | ------------------------------------------ |
| [ex., 4º projeto]        | [necessidade atual]   | [por que 3 projetos insuficiente]          |
| [ex., padrão Repository] | [problema específico] | [por que acesso direto ao DB insuficiente] |

## Rastreamento de Progresso

_Esta lista de verificação é atualizada durante o fluxo de execução_

**Status da Fase**:

- [ ] Fase 0: Pesquisa completa (comando /plan)
- [ ] Fase 1: Design completo (comando /plan)
- [ ] Fase 2: Planejamento de tarefas completo (comando /plan - descreva abordagem apenas)
- [ ] Fase 3: Tarefas geradas (comando /tasks)
- [ ] Fase 4: Implementação completa
- [ ] Fase 5: Validação passou

**Status do Portão**:

- [ ] Verificação Constitucional Inicial: PASSOU
- [ ] Verificação Constitucional Pós-Design: PASSOU
- [ ] Todas as PRECISA CLARIFICAÇÃO resolvidas
- [ ] Desvios de complexidade documentados

---

_Baseado na Constituição v2.1.1 - Veja `/memory/constitution.md`_
