---
description: Executar o fluxo de trabalho de planejamento de implementação usando o template de plano para gerar artefatos de design.
---

A entrada do usuário pode ser fornecida diretamente pelo agente ou como argumento de comando - você **DEVE** considerá-la antes de prosseguir com o prompt (se não estiver vazia).

Entrada do usuário:

$ARGUMENTS

Dados os detalhes de implementação fornecidos como argumento, faça isso:

1. Execute `.specify/scripts/bash/setup-plan.sh --json` da raiz do repositório e analise JSON para FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH. Todos os caminhos de arquivo futuros devem ser absolutos.
   - ANTES de prosseguir, inspecione FEATURE_SPEC para uma seção `## Esclarecimentos` com pelo menos um subtítulo `Sessão`. Se ausente ou áreas claramente ambíguas permanecerem (adjetivos vagos, escolhas críticas não resolvidas), PAUSE e instrua o usuário a executar `/clarify` primeiro para reduzir retrabalho. Continue apenas se: (a) Esclarecimentos existirem OU (b) uma substituição explícita do usuário for fornecida (ex., "prossiga sem esclarecimento"). Não tente fabricar esclarecimentos você mesmo.
2. Leia e analise a especificação de funcionalidade para entender:

   - Os requisitos de funcionalidade e histórias de usuário
   - Requisitos funcionais e não-funcionais
   - Critérios de sucesso e critérios de aceitação
   - Quaisquer restrições técnicas ou dependências mencionadas

3. Leia a constituição em `.specify/memory/constitution.md` para entender requisitos constitucionais.

4. Execute o template de plano de implementação:

   - Carregue `.specify/templates/plan-template.md` (já copiado para caminho IMPL_PLAN)
   - Defina caminho de entrada para FEATURE_SPEC
   - Execute as etapas 1-9 da função Execution Flow (principal)
   - O template é autocontido e executável
   - Siga tratamento de erro e verificações de portão conforme especificado
   - Deixe o template guiar geração de artefatos em $SPECS_DIR:
     - Fase 0 gera research.md
     - Fase 1 gera data-model.md, contracts/, quickstart.md
     - Fase 2 gera tasks.md
   - Incorpore detalhes fornecidos pelo usuário dos argumentos no Contexto Técnico: $ARGUMENTS
   - Atualize Rastreamento de Progresso conforme você completa cada fase

5. Verifique que execução foi completada:

   - Verifique que Rastreamento de Progresso mostra todas as fases completas
   - Garanta que todos os artefatos obrigatórios foram gerados
   - Confirme que nenhum estado ERROR na execução

6. Reporte resultados com nome do branch, caminhos de arquivo e artefatos gerados.

Use caminhos absolutos com a raiz do repositório para todas as operações de arquivo para evitar problemas de caminho.
