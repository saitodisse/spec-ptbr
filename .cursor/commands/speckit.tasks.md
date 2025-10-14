---
description: Gerar um tasks.md acionável e ordenado por dependências para a funcionalidade baseado em artefatos de design disponíveis.
---

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Resumo

1. **Configuração**: Execute `.specify/scripts/bash/check-prerequisites.sh --json` a partir da raiz do repositório e analise FEATURE_DIR e lista AVAILABLE_DOCS. Todos os caminhos devem ser absolutos. Para aspas simples em argumentos como "I'm Groot", use sintaxe de escape: por exemplo 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

2. **Carregar documentos de design**: Leia de FEATURE_DIR:

   - **Obrigatório**: plan.md (tech stack, bibliotecas, estrutura), spec.md (histórias de usuário com prioridades)
   - **Opcional**: data-model.md (entidades), contracts/ (endpoints de API), research.md (decisões), quickstart.md (cenários de teste)
   - Nota: Nem todos os projetos têm todos os documentos. Gere tarefas baseadas no que está disponível.

3. **Executar fluxo de geração de tarefas**:

   - Carregue plan.md e extraia tech stack, bibliotecas, estrutura de projeto
   - Carregue spec.md e extraia histórias de usuário com suas prioridades (P1, P2, P3, etc.)
   - Se data-model.md existir: Extraia entidades e mapeie para histórias de usuário
   - Se contracts/ existir: Mapeie endpoints para histórias de usuário
   - Se research.md existir: Extraia decisões para tarefas de setup
   - Gere tarefas organizadas por história de usuário (veja Regras de Geração de Tarefas abaixo)
   - Gere grafo de dependências mostrando ordem de conclusão de histórias de usuário
   - Crie exemplos de execução paralela por história de usuário
   - Valide completude de tarefas (cada história de usuário tem todas as tarefas necessárias, testável independentemente)

4. **Gerar tasks.md**: Use `.specify/templates/tasks-template.md` como estrutura, preencha com:

   - Nome correto da funcionalidade de plan.md
   - Fase 1: Tarefas de setup (inicialização do projeto)
   - Fase 2: Tarefas fundamentais (pré-requisitos bloqueantes para todas histórias de usuário)
   - Fase 3+: Uma fase por história de usuário (em ordem de prioridade de spec.md)
   - Cada fase inclui: objetivo da história, critérios de teste independentes, testes (se solicitado), tarefas de implementação
   - Fase Final: Polish e preocupações transversais
   - Todas as tarefas devem seguir o formato estrito de checklist (veja Regras de Geração de Tarefas abaixo)
   - Caminhos de arquivo claros para cada tarefa
   - Seção de dependências mostrando ordem de conclusão de histórias
   - Exemplos de execução paralela por história
   - Seção de estratégia de implementação (MVP primeiro, entrega incremental)

5. **Reportar**: Produza caminho para tasks.md gerado e resumo:
   - Contagem total de tarefas
   - Contagem de tarefas por história de usuário
   - Oportunidades paralelas identificadas
   - Critérios de teste independentes para cada história
   - Escopo MVP sugerido (tipicamente apenas História de Usuário 1)
   - Validação de formato: Confirme que TODAS as tarefas seguem o formato de checklist (checkbox, ID, labels, caminhos de arquivo)

Contexto para geração de tarefas: $ARGUMENTS

O tasks.md deve ser imediatamente executável - cada tarefa deve ser específica o suficiente para que uma LLM possa completá-la sem contexto adicional.

## Regras de Geração de Tarefas

**CRÍTICO**: Tarefas DEVEM ser organizadas por história de usuário para permitir implementação e testes independentes.

**Testes são OPCIONAIS**: Gere tarefas de teste apenas se explicitamente solicitado na especificação da funcionalidade ou se usuário solicitar abordagem TDD.

### Formato de Checklist (OBRIGATÓRIO)

Toda tarefa DEVE seguir estritamente este formato:

```text
- [ ] [TaskID] [P?] [Story?] Descrição com caminho de arquivo
```

**Componentes do Formato**:

1. **Checkbox**: SEMPRE comece com `- [ ]` (checkbox markdown)
2. **ID da Tarefa**: Número sequencial (T001, T002, T003...) em ordem de execução
3. **Marcador [P]**: Inclua APENAS se a tarefa é paralelizável (arquivos diferentes, sem dependências em tarefas incompletas)
4. **Label [Story]**: OBRIGATÓRIO para tarefas de fase de história de usuário apenas
   - Formato: [US1], [US2], [US3], etc. (mapeia para histórias de usuário de spec.md)
   - Fase de setup: SEM label de história
   - Fase fundamental: SEM label de história
   - Fases de História de Usuário: DEVE ter label de história
   - Fase de polish: SEM label de história
5. **Descrição**: Ação clara com caminho de arquivo exato

**Exemplos**:

- ✅ CORRETO: `- [ ] T001 Criar estrutura de projeto conforme plano de implementação`
- ✅ CORRETO: `- [ ] T005 [P] Implementar middleware de autenticação em src/middleware/auth.py`
- ✅ CORRETO: `- [ ] T012 [P] [US1] Criar modelo User em src/models/user.py`
- ✅ CORRETO: `- [ ] T014 [US1] Implementar UserService em src/services/user_service.py`
- ❌ ERRADO: `- [ ] Criar modelo User` (faltando ID e label Story)
- ❌ ERRADO: `T001 [US1] Criar modelo` (faltando checkbox)
- ❌ ERRADO: `- [ ] [US1] Criar modelo User` (faltando ID de Tarefa)
- ❌ ERRADO: `- [ ] T001 [US1] Criar modelo` (faltando caminho de arquivo)

### Organização de Tarefas

1. **De Histórias de Usuário (spec.md)** - ORGANIZAÇÃO PRIMÁRIA:
   - Cada história de usuário (P1, P2, P3...) recebe sua própria fase
   - Mapeie todos os componentes relacionados à sua história:
     - Modelos necessários para aquela história
     - Serviços necessários para aquela história
     - Endpoints/UI necessários para aquela história
     - Se testes solicitados: Testes específicos para aquela história
   - Marque dependências de histórias (a maioria das histórias deve ser independente)
2. **De Contratos**:
   - Mapeie cada contrato/endpoint → para a história de usuário que ele serve
   - Se testes solicitados: Cada contrato → tarefa de teste de contrato [P] antes da implementação na fase daquela história
3. **Do Modelo de Dados**:
   - Mapeie cada entidade para a(s) história(s) de usuário que a necessita(m)
   - Se entidade serve múltiplas histórias: Coloque na história mais cedo ou fase de Setup
   - Relacionamentos → tarefas de camada de serviço na fase de história apropriada
4. **De Setup/Infraestrutura**:
   - Infraestrutura compartilhada → Fase de Setup (Fase 1)
   - Tarefas fundamentais/bloqueantes → Fase Fundamental (Fase 2)
   - Setup específico de história → dentro da fase daquela história

### Estrutura de Fases

- **Fase 1**: Setup (inicialização do projeto)
- **Fase 2**: Fundamental (pré-requisitos bloqueantes - DEVE completar antes de histórias de usuário)
- **Fase 3+**: Histórias de Usuário em ordem de prioridade (P1, P2, P3...)
  - Dentro de cada história: Testes (se solicitado) → Modelos → Serviços → Endpoints → Integração
  - Cada fase deve ser um incremento completo, testável independentemente
- **Fase Final**: Polish e Preocupações Transversais
