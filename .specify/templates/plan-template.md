# Plano de Implementação: [FUNCIONALIDADE]

**Branch**: `[###-nome-funcionalidade]` | **Data**: [DATA] | **Spec**: [link]
**Entrada**: Especificação de funcionalidade de `/specs/[###-nome-funcionalidade]/spec.md`

**Nota**: Este template é preenchido pelo comando `/speckit.plan`. Veja `.specify/templates/commands/plan.md` para o fluxo de execução.

## Resumo

[Extrair da spec da funcionalidade: requisito primário + abordagem técnica da pesquisa]

## Contexto Técnico

<!--
  AÇÃO REQUERIDA: Substitua o conteúdo nesta seção com os detalhes técnicos
  do projeto. A estrutura aqui é apresentada em capacidade consultiva para guiar
  o processo de iteração.
-->

**Linguagem/Versão**: [ex.: Python 3.11, Swift 5.9, Rust 1.75 ou NECESSITA ESCLARECIMENTO]  
**Dependências Primárias**: [ex.: FastAPI, UIKit, LLVM ou NECESSITA ESCLARECIMENTO]  
**Armazenamento**: [se aplicável, ex.: PostgreSQL, CoreData, arquivos ou N/A]  
**Testes**: [ex.: pytest, XCTest, cargo test ou NECESSITA ESCLARECIMENTO]  
**Plataforma Alvo**: [ex.: servidor Linux, iOS 15+, WASM ou NECESSITA ESCLARECIMENTO]
**Tipo de Projeto**: [único/web/mobile - determina estrutura de fonte]  
**Objetivos de Desempenho**: [específico do domínio, ex.: 1000 req/s, 10k linhas/seg, 60 fps ou NECESSITA ESCLARECIMENTO]  
**Restrições**: [específico do domínio, ex.: <200ms p95, <100MB memória, capacidade offline ou NECESSITA ESCLARECIMENTO]  
**Escala/Escopo**: [específico do domínio, ex.: 10k usuários, 1M LOC, 50 telas ou NECESSITA ESCLARECIMENTO]

## Verificação de Constituição

_CONTROLE: Deve passar antes da pesquisa da Fase 0. Reveja após design da Fase 1._

[Controles determinados baseados no arquivo de constituição]

## Estrutura do Projeto

### Documentação (esta funcionalidade)

```
specs/[###-funcionalidade]/
├── plan.md              # Este arquivo (saída do comando /speckit.plan)
├── research.md          # Saída da Fase 0 (comando /speckit.plan)
├── data-model.md        # Saída da Fase 1 (comando /speckit.plan)
├── quickstart.md        # Saída da Fase 1 (comando /speckit.plan)
├── contracts/           # Saída da Fase 1 (comando /speckit.plan)
└── tasks.md             # Saída da Fase 2 (comando /speckit.tasks - NÃO criado por /speckit.plan)
```

### Código Fonte (raiz do repositório)

<!--
  AÇÃO REQUERIDA: Substitua a árvore de placeholder abaixo com o layout concreto
  para esta funcionalidade. Exclua opções não usadas e expanda a estrutura escolhida com
  caminhos reais (ex.: apps/admin, packages/algo). O plano entregue não deve
  incluir labels de Opção.
-->

```
# [REMOVA SE NÃO USADO] Opção 1: Projeto único (PADRÃO)
src/
├── models/
├── services/
├── cli/
└── lib/

tests/
├── contract/
├── integration/
└── unit/

# [REMOVA SE NÃO USADO] Opção 2: Aplicação web (quando "frontend" + "backend" detectados)
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

# [REMOVA SE NÃO USADO] Opção 3: Mobile + API (quando "iOS/Android" detectado)
api/
└── [mesmo que backend acima]

ios/ ou android/
└── [estrutura específica de plataforma: módulos de funcionalidade, fluxos de UI, testes de plataforma]
```

**Decisão de Estrutura**: [Documente a estrutura selecionada e referencie os
diretórios reais capturados acima]

## Rastreamento de Complexidade

_Preencha APENAS se Verificação de Constituição tiver violações que devem ser justificadas_

| Violação                 | Por Que Necessária    | Alternativa Mais Simples Rejeitada Porque  |
| ------------------------ | --------------------- | ------------------------------------------ |
| [ex.: 4º projeto]        | [necessidade atual]   | [por que 3 projetos insuficientes]         |
| [ex.: padrão Repository] | [problema específico] | [por que acesso direto ao BD insuficiente] |
