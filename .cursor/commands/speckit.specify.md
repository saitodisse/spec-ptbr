---
description: Criar ou atualizar a especificação de funcionalidade a partir de uma descrição de funcionalidade em linguagem natural.
---

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Resumo

O texto que o usuário digitou após `/speckit.specify` na mensagem de acionamento **é** a descrição da funcionalidade. Assuma que você sempre o tem disponível nesta conversa mesmo que `$ARGUMENTS` apareça literalmente abaixo. Não peça ao usuário para repeti-lo a menos que tenham fornecido um comando vazio.

Dada aquela descrição da funcionalidade, faça isto:

1. Execute o script `.specify/scripts/bash/create-new-feature.sh --json "$ARGUMENTS"` a partir da raiz do repositório e analise sua saída JSON para BRANCH_NAME e SPEC_FILE. Todos os caminhos de arquivo devem ser absolutos.
   **IMPORTANTE** Você deve executar este script apenas uma vez. O JSON é fornecido no terminal como saída - sempre se refira a ele para obter o conteúdo real que você está procurando. Para aspas simples em argumentos como "I'm Groot", use sintaxe de escape: por exemplo 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").
2. Carregue `.specify/templates/spec-template.md` para entender seções requeridas.

3. Siga este fluxo de execução:

   1. Analise descrição do usuário da Entrada
      Se vazia: ERRO "Nenhuma descrição de funcionalidade fornecida"
   2. Extraia conceitos-chave da descrição
      Identifique: atores, ações, dados, restrições
   3. Para aspectos pouco claros:
      - Faça suposições informadas baseadas em contexto e padrões da indústria
      - Marque apenas com [NECESSITA ESCLARECIMENTO: pergunta específica] se:
        - A escolha impacta significativamente escopo da funcionalidade ou experiência do usuário
        - Múltiplas interpretações razoáveis existem com implicações diferentes
        - Nenhum padrão razoável existe
      - **LIMITE: Máximo 3 marcadores [NECESSITA ESCLARECIMENTO] no total**
      - Priorize esclarecimentos por impacto: escopo > segurança/privacidade > experiência do usuário > detalhes técnicos
   4. Preencha seção Cenários de Usuário e Testes
      Se nenhum fluxo de usuário claro: ERRO "Não é possível determinar cenários de usuário"
   5. Gere Requisitos Funcionais
      Cada requisito deve ser testável
      Use padrões razoáveis para detalhes não especificados (documente suposições na seção Suposições)
   6. Defina Critérios de Sucesso
      Crie resultados mensuráveis e independentes de tecnologia
      Inclua tanto métricas quantitativas (tempo, desempenho, volume) quanto medidas qualitativas (satisfação do usuário, conclusão de tarefa)
      Cada critério deve ser verificável sem detalhes de implementação
   7. Identifique Entidades Principais (se dados envolvidos)
   8. Retorne: SUCESSO (spec pronta para planejamento)

4. Grave a especificação para SPEC_FILE usando a estrutura do template, substituindo placeholders por detalhes concretos derivados da descrição da funcionalidade (argumentos) enquanto preserva ordem de seções e cabeçalhos.

5. **Validação de Qualidade de Especificação**: Após gravar a spec inicial, valide-a contra critérios de qualidade:

   a. **Criar Checklist de Qualidade de Spec**: Gere um arquivo de checklist em `FEATURE_DIR/checklists/requirements.md` usando a estrutura de template de checklist com estes itens de validação:

   ```markdown
   # Checklist de Qualidade de Especificação: [NOME DA FUNCIONALIDADE]

   **Propósito**: Validar completude e qualidade da especificação antes de prosseguir para planejamento
   **Criado**: [DATA]
   **Funcionalidade**: [Link para spec.md]

   ## Qualidade de Conteúdo

   - [ ] Sem detalhes de implementação (linguagens, frameworks, APIs)
   - [ ] Focado em valor do usuário e necessidades de negócio
   - [ ] Escrito para stakeholders não técnicos
   - [ ] Todas as seções obrigatórias completas

   ## Completude de Requisitos

   - [ ] Nenhum marcador [NECESSITA ESCLARECIMENTO] permanece
   - [ ] Requisitos são testáveis e inequívocos
   - [ ] Critérios de sucesso são mensuráveis
   - [ ] Critérios de sucesso são independentes de tecnologia (sem detalhes de implementação)
   - [ ] Todos os cenários de aceitação estão definidos
   - [ ] Casos extremos estão identificados
   - [ ] Escopo está claramente delimitado
   - [ ] Dependências e suposições identificadas

   ## Prontidão da Funcionalidade

   - [ ] Todos os requisitos funcionais têm critérios de aceitação claros
   - [ ] Cenários de usuário cobrem fluxos primários
   - [ ] Funcionalidade atende resultados mensuráveis definidos em Critérios de Sucesso
   - [ ] Nenhum detalhe de implementação vaza para especificação

   ## Notas

   - Itens marcados incompletos requerem atualizações de spec antes de `/speckit.clarify` ou `/speckit.plan`
   ```

   b. **Executar Verificação de Validação**: Revise a spec contra cada item de checklist:

   - Para cada item, determine se passa ou falha
   - Documente problemas específicos encontrados (cite seções relevantes da spec)

   c. **Tratar Resultados de Validação**:

   - **Se todos os itens passarem**: Marque checklist completo e prossiga para etapa 6

   - **Se itens falharem (excluindo [NECESSITA ESCLARECIMENTO])**:

     1. Liste os itens que falharam e problemas específicos
     2. Atualize a spec para endereçar cada problema
     3. Reexecute validação até todos os itens passarem (máx 3 iterações)
     4. Se ainda falhar após 3 iterações, documente problemas restantes em notas do checklist e avise usuário

   - **Se marcadores [NECESSITA ESCLARECIMENTO] permanecerem**:

     1. Extraia todos os marcadores [NECESSITA ESCLARECIMENTO: ...] da spec
     2. **VERIFICAÇÃO DE LIMITE**: Se mais de 3 marcadores existirem, mantenha apenas os 3 mais críticos (por impacto de escopo/segurança/UX) e faça suposições informadas para o resto
     3. Para cada esclarecimento necessário (máx 3), apresente opções ao usuário neste formato:

        ```markdown
        ## Pergunta [N]: [Tópico]

        **Contexto**: [Cite seção relevante da spec]

        **O que precisamos saber**: [Pergunta específica do marcador NECESSITA ESCLARECIMENTO]

        **Respostas Sugeridas**:

        | Opção         | Resposta                     | Implicações                                    |
        | ------------- | ---------------------------- | ---------------------------------------------- |
        | A             | [Primeira resposta sugerida] | [O que isso significa para a funcionalidade]   |
        | B             | [Segunda resposta sugerida]  | [O que isso significa para a funcionalidade]   |
        | C             | [Terceira resposta sugerida] | [O que isso significa para a funcionalidade]   |
        | Personalizado | Forneça sua própria resposta | [Explique como fornecer entrada personalizada] |

        **Sua escolha**: _[Aguarde resposta do usuário]_
        ```

     4. **CRÍTICO - Formatação de Tabela**: Garanta que tabelas markdown estejam adequadamente formatadas:
        - Use espaçamento consistente com pipes alinhados
        - Cada célula deve ter espaços ao redor do conteúdo: `| Conteúdo |` não `|Conteúdo|`
        - Separador de cabeçalho deve ter pelo menos 3 traços: `|--------|`
        - Teste que a tabela renderiza corretamente em preview markdown
     5. Numere perguntas sequencialmente (P1, P2, P3 - máx 3 total)
     6. Apresente todas as perguntas juntas antes de aguardar respostas
     7. Aguarde o usuário responder com suas escolhas para todas as perguntas (por exemplo, "P1: A, P2: Personalizado - [detalhes], P3: B")
     8. Atualize a spec substituindo cada marcador [NECESSITA ESCLARECIMENTO] pela resposta selecionada ou fornecida pelo usuário
     9. Reexecute validação após todos os esclarecimentos serem resolvidos

   d. **Atualizar Checklist**: Após cada iteração de validação, atualize o arquivo de checklist com status atual de passar/falhar

6. Reporte conclusão com nome da branch, caminho do arquivo da spec, resultados do checklist e prontidão para a próxima fase (`/speckit.clarify` ou `/speckit.plan`).

**NOTA:** O script cria e faz checkout da nova branch e inicializa o arquivo da spec antes de gravar.

## Diretrizes Gerais

## Diretrizes Rápidas

- Foque em **O QUE** os usuários precisam e **POR QUÊ**.
- Evite COMO implementar (sem tech stack, APIs, estrutura de código).
- Escrito para stakeholders de negócio, não desenvolvedores.
- NÃO crie nenhum checklist que esteja incorporado na spec. Isso será um comando separado.

### Requisitos de Seção

- **Seções obrigatórias**: Devem ser completas para cada funcionalidade
- **Seções opcionais**: Inclua apenas quando relevante para a funcionalidade
- Quando uma seção não se aplicar, remova-a completamente (não deixe como "N/A")

### Para Geração por IA

Ao criar esta spec a partir de um prompt do usuário:

1. **Faça suposições informadas**: Use contexto, padrões da indústria e padrões comuns para preencher lacunas
2. **Documente suposições**: Registre padrões razoáveis na seção Suposições
3. **Limite esclarecimentos**: Máximo 3 marcadores [NECESSITA ESCLARECIMENTO] - use apenas para decisões críticas que:
   - Impactam significativamente escopo da funcionalidade ou experiência do usuário
   - Têm múltiplas interpretações razoáveis com implicações diferentes
   - Carecem de qualquer padrão razoável
4. **Priorize esclarecimentos**: escopo > segurança/privacidade > experiência do usuário > detalhes técnicos
5. **Pense como um testador**: Todo requisito vago deve falhar o item de checklist "testável e inequívoco"
6. **Áreas comuns necessitando esclarecimento** (apenas se nenhum padrão razoável existir):
   - Escopo e limites da funcionalidade (incluir/excluir casos de uso específicos)
   - Tipos de usuário e permissões (se múltiplas interpretações conflitantes possíveis)
   - Requisitos de segurança/compliance (quando legalmente/financeiramente significativo)

**Exemplos de padrões razoáveis** (não pergunte sobre estes):

- Retenção de dados: Práticas padrão da indústria para o domínio
- Alvos de desempenho: Expectativas padrão de aplicativo web/mobile a menos que especificado
- Tratamento de erros: Mensagens amigáveis ao usuário com fallbacks apropriados
- Método de autenticação: Baseado em sessão padrão ou OAuth2 para aplicativos web
- Padrões de integração: APIs RESTful a menos que especificado de outra forma

### Diretrizes de Critérios de Sucesso

Critérios de sucesso devem ser:

1. **Mensuráveis**: Incluir métricas específicas (tempo, porcentagem, contagem, taxa)
2. **Independentes de tecnologia**: Sem menção de frameworks, linguagens, bancos de dados ou ferramentas
3. **Focados no usuário**: Descrever resultados da perspectiva do usuário/negócio, não internos do sistema
4. **Verificáveis**: Podem ser testados/validados sem conhecer detalhes de implementação

**Bons exemplos**:

- "Usuários podem completar checkout em menos de 3 minutos"
- "Sistema suporta 10.000 usuários simultâneos"
- "95% das buscas retornam resultados em menos de 1 segundo"
- "Taxa de conclusão de tarefas melhora em 40%"

**Maus exemplos** (focados em implementação):

- "Tempo de resposta da API está abaixo de 200ms" (muito técnico, use "Usuários veem resultados instantaneamente")
- "Banco de dados pode lidar com 1000 TPS" (detalhe de implementação, use métrica voltada ao usuário)
- "Componentes React renderizam eficientemente" (específico de framework)
- "Taxa de acerto de cache Redis acima de 80%" (específico de tecnologia)
