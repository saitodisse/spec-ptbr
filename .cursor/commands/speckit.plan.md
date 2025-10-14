---
description: Executar o fluxo de planejamento de implementação usando o template de plano para gerar artefatos de design.
---

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Resumo

1. **Configuração**: Execute `.specify/scripts/bash/setup-plan.sh --json` a partir da raiz do repositório e analise JSON para FEATURE_SPEC, IMPL_PLAN, SPECS_DIR, BRANCH. Para aspas simples em argumentos como "I'm Groot", use sintaxe de escape: por exemplo 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

2. **Carregar contexto**: Leia FEATURE_SPEC e `.specify/memory/constitution.md`. Carregue template IMPL_PLAN (já copiado).

3. **Executar fluxo de plano**: Siga a estrutura no template IMPL_PLAN para:

   - Preencher Contexto Técnico (marque desconhecidos como "NECESSITA ESCLARECIMENTO")
   - Preencher seção de Verificação de Constituição a partir da constituição
   - Avaliar controles (ERRO se violações injustificadas)
   - Fase 0: Gerar research.md (resolver todos NECESSITA ESCLARECIMENTO)
   - Fase 1: Gerar data-model.md, contracts/, quickstart.md
   - Fase 1: Atualizar contexto do agente executando o script do agente
   - Reavaliar Verificação de Constituição pós-design

4. **Parar e reportar**: Comando termina após planejamento da Fase 2. Reporte branch, caminho IMPL_PLAN e artefatos gerados.

## Fases

### Fase 0: Esboço e Pesquisa

1. **Extrair desconhecidos do Contexto Técnico** acima:

   - Para cada NECESSITA ESCLARECIMENTO → tarefa de pesquisa
   - Para cada dependência → tarefa de melhores práticas
   - Para cada integração → tarefa de padrões

2. **Gerar e despachar agentes de pesquisa**:

   ```
   Para cada desconhecido no Contexto Técnico:
     Tarefa: "Pesquisar {desconhecido} para {contexto da funcionalidade}"
   Para cada escolha de tecnologia:
     Tarefa: "Encontrar melhores práticas para {tech} em {domínio}"
   ```

3. **Consolidar descobertas** em `research.md` usando formato:
   - Decisão: [o que foi escolhido]
   - Justificativa: [por que escolhido]
   - Alternativas consideradas: [o que mais foi avaliado]

**Saída**: research.md com todos NECESSITA ESCLARECIMENTO resolvidos

### Fase 1: Design e Contratos

**Pré-requisitos:** `research.md` completo

1. **Extrair entidades da spec da funcionalidade** → `data-model.md`:

   - Nome da entidade, campos, relacionamentos
   - Regras de validação de requisitos
   - Transições de estado se aplicável

2. **Gerar contratos de API** de requisitos funcionais:

   - Para cada ação do usuário → endpoint
   - Use padrões REST/GraphQL padrão
   - Produza esquema OpenAPI/GraphQL para `/contracts/`

3. **Atualização de contexto do agente**:
   - Execute `.specify/scripts/bash/update-agent-context.sh cursor-agent`
   - Estes scripts detectam qual agente de IA está em uso
   - Atualize o arquivo de contexto específico do agente apropriado
   - Adicione apenas nova tecnologia do plano atual
   - Preserve adições manuais entre marcadores

**Saída**: data-model.md, /contracts/\*, quickstart.md, arquivo específico do agente

## Regras principais

- Use caminhos absolutos
- ERRO em falhas de controle ou esclarecimentos não resolvidos
