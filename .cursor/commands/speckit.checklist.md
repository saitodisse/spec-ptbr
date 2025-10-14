---
description: Gerar um checklist personalizado para a funcionalidade atual com base nos requisitos do usuário.
---

## Propósito do Checklist: "Testes Unitários para Inglês"

**CONCEITO CRÍTICO**: Checklists são **TESTES UNITÁRIOS PARA REDAÇÃO DE REQUISITOS** - eles validam a qualidade, clareza e completude dos requisitos em um determinado domínio.

**NÃO para verificação/testes**:

- ❌ NÃO "Verificar se o botão clica corretamente"
- ❌ NÃO "Testar se tratamento de erros funciona"
- ❌ NÃO "Confirmar se a API retorna 200"
- ❌ NÃO verificar se código/implementação corresponde à spec

**PARA validação de qualidade de requisitos**:

- ✅ "Os requisitos de hierarquia visual estão definidos para todos os tipos de card?" (completude)
- ✅ "O termo 'exibição proeminente' está quantificado com dimensionamento/posicionamento específico?" (clareza)
- ✅ "Os requisitos de estado hover estão consistentes em todos os elementos interativos?" (consistência)
- ✅ "Os requisitos de acessibilidade estão definidos para navegação por teclado?" (cobertura)
- ✅ "A spec define o que acontece quando o carregamento da imagem do logo falha?" (casos extremos)

**Metáfora**: Se sua spec é código escrito em inglês, o checklist é sua suíte de testes unitários. Você está testando se os requisitos estão bem escritos, completos, inequívocos e prontos para implementação - NÃO se a implementação funciona.

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Etapas de Execução

1. **Configuração**: Execute `.specify/scripts/bash/check-prerequisites.sh --json` a partir da raiz do repositório e analise o JSON para FEATURE_DIR e lista AVAILABLE_DOCS.

   - Todos os caminhos de arquivo devem ser absolutos.
   - Para aspas simples em argumentos como "I'm Groot", use sintaxe de escape: por exemplo 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

2. **Esclarecer intenção (dinâmico)**: Derive até TRÊS perguntas de esclarecimento contextuais iniciais (sem catálogo pré-fabricado). Elas DEVEM:

   - Ser geradas a partir do fraseado do usuário + sinais extraídos de spec/plan/tasks
   - Perguntar apenas sobre informações que mudam materialmente o conteúdo do checklist
   - Ser ignoradas individualmente se já estiverem inequívocas em `$ARGUMENTS`
   - Preferir precisão à amplitude

   Algoritmo de geração:

   1. Extrair sinais: palavras-chave do domínio da funcionalidade (por exemplo, auth, latency, UX, API), indicadores de risco ("crítico", "deve", "compliance"), sugestões de stakeholder ("QA", "revisão", "equipe de segurança"), e entregas explícitas ("a11y", "rollback", "contratos").
   2. Agrupar sinais em áreas de foco candidatas (máx 4) classificadas por relevância.
   3. Identificar audiência e timing prováveis (autor, revisor, QA, release) se não explícito.
   4. Detectar dimensões ausentes: amplitude de escopo, profundidade/rigor, ênfase em risco, limites de exclusão, critérios de aceitação mensuráveis.
   5. Formular perguntas escolhidas destes arquétipos:
      - Refinamento de escopo (por exemplo, "Isso deve incluir pontos de integração com X e Y ou permanecer limitado à correção do módulo local?")
      - Priorização de risco (por exemplo, "Quais dessas áreas de risco potencial devem receber verificações obrigatórias?")
      - Calibração de profundidade (por exemplo, "Esta é uma lista de sanidade leve pré-commit ou um controle formal de release?")
      - Enquadramento de audiência (por exemplo, "Isso será usado apenas pelo autor ou por pares durante a revisão de PR?")
      - Exclusão de limites (por exemplo, "Devemos explicitamente excluir itens de ajuste de desempenho desta rodada?")
      - Lacuna de classe de cenário (por exemplo, "Nenhum fluxo de recuperação detectado—caminhos de rollback / falha parcial estão no escopo?")

   Regras de formatação de perguntas:

   - Se apresentar opções, gere uma tabela compacta com colunas: Opção | Candidato | Por Que Importa
   - Limite a opções A–E no máximo; omita tabela se uma resposta livre for mais clara
   - Nunca peça ao usuário para reafirmar o que já disseram
   - Evite categorias especulativas (sem alucinação). Se incerto, pergunte explicitamente: "Confirme se X pertence ao escopo."

   Padrões quando interação impossível:

   - Profundidade: Padrão
   - Audiência: Revisor (PR) se relacionado a código; Autor caso contrário
   - Foco: 2 principais clusters de relevância

   Produza as perguntas (rotule Q1/Q2/Q3). Após respostas: se ≥2 classes de cenário (Alternativo / Exceção / Recuperação / domínio Não Funcional) permanecerem incertas, você PODE fazer até DOIS acompanhamentos direcionados (Q4/Q5) com uma justificativa de uma linha cada (por exemplo, "Risco de caminho de recuperação não resolvido"). Não exceda cinco perguntas totais. Pule escalação se o usuário declinar explicitamente mais.

3. **Entender solicitação do usuário**: Combine `$ARGUMENTS` + respostas de esclarecimento:

   - Derive tema do checklist (por exemplo, segurança, revisão, deploy, ux)
   - Consolide itens obrigatórios explícitos mencionados pelo usuário
   - Mapeie seleções de foco para estrutura de categoria
   - Infira qualquer contexto ausente de spec/plan/tasks (NÃO alucine)

4. **Carregar contexto da funcionalidade**: Leia de FEATURE_DIR:

   - spec.md: Requisitos e escopo da funcionalidade
   - plan.md (se existir): Detalhes técnicos, dependências
   - tasks.md (se existir): Tarefas de implementação

   **Estratégia de Carregamento de Contexto**:

   - Carregue apenas as porções necessárias relevantes para áreas de foco ativas (evite despejo de arquivo completo)
   - Prefira resumir seções longas em bullets concisos de cenário/requisito
   - Use divulgação progressiva: adicione recuperação de acompanhamento apenas se lacunas detectadas
   - Se documentos fonte forem grandes, gere itens de resumo intermediários em vez de incorporar texto bruto

5. **Gerar checklist** - Criar "Testes Unitários para Requisitos":

   - Crie diretório `FEATURE_DIR/checklists/` se não existir
   - Gere nome de arquivo de checklist único:
     - Use nome curto e descritivo baseado no domínio (por exemplo, `ux.md`, `api.md`, `seguranca.md`)
     - Formato: `[dominio].md`
     - Se arquivo existir, anexe ao arquivo existente
   - Numere itens sequencialmente começando de CHK001
   - Cada execução de `/speckit.checklist` cria um NOVO arquivo (nunca sobrescreve checklists existentes)

   **PRINCÍPIO CENTRAL - Teste os Requisitos, Não a Implementação**:
   Cada item do checklist DEVE avaliar os REQUISITOS EM SI para:

   - **Completude**: Todos os requisitos necessários estão presentes?
   - **Clareza**: Os requisitos são inequívocos e específicos?
   - **Consistência**: Os requisitos se alinham entre si?
   - **Mensurabilidade**: Os requisitos podem ser verificados objetivamente?
   - **Cobertura**: Todos os cenários/casos extremos estão endereçados?

   **Estrutura de Categoria** - Agrupe itens por dimensões de qualidade de requisitos:

   - **Completude de Requisitos** (Todos os requisitos necessários estão documentados?)
   - **Clareza de Requisitos** (Os requisitos são específicos e inequívocos?)
   - **Consistência de Requisitos** (Os requisitos se alinham sem conflitos?)
   - **Qualidade de Critérios de Aceitação** (Os critérios de sucesso são mensuráveis?)
   - **Cobertura de Cenários** (Todos os fluxos/casos estão endereçados?)
   - **Cobertura de Casos Extremos** (As condições de contorno estão definidas?)
   - **Requisitos Não Funcionais** (Desempenho, Segurança, Acessibilidade, etc. - estão especificados?)
   - **Dependências e Suposições** (Estão documentadas e validadas?)
   - **Ambiguidades e Conflitos** (O que precisa de esclarecimento?)

   **COMO ESCREVER ITENS DE CHECKLIST - "Testes Unitários para Inglês"**:

   ❌ **ERRADO** (Testando implementação):

   - "Verificar se a landing page exibe 3 cards de episódio"
   - "Testar se estados hover funcionam no desktop"
   - "Confirmar se clique no logo navega para home"

   ✅ **CORRETO** (Testando qualidade de requisitos):

   - "O número exato e layout de episódios em destaque estão especificados?" [Completude]
   - "O termo 'exibição proeminente' está quantificado com dimensionamento/posicionamento específico?" [Clareza]
   - "Os requisitos de estado hover estão consistentes em todos os elementos interativos?" [Consistência]
   - "Os requisitos de navegação por teclado estão definidos para toda UI interativa?" [Cobertura]
   - "O comportamento de fallback está especificado quando a imagem do logo falha ao carregar?" [Casos Extremos]
   - "Os estados de carregamento estão definidos para dados de episódios assíncronos?" [Completude]
   - "A spec define hierarquia visual para elementos de UI concorrentes?" [Clareza]

   **ESTRUTURA DO ITEM**:
   Cada item deve seguir este padrão:

   - Formato de pergunta questionando sobre qualidade de requisito
   - Foque no que está ESCRITO (ou não escrito) na spec/plan
   - Inclua dimensão de qualidade entre colchetes [Completude/Clareza/Consistência/etc.]
   - Referencie seção da spec `[Spec §X.Y]` ao verificar requisitos existentes
   - Use marcador `[Lacuna]` ao verificar requisitos ausentes

   **EXEMPLOS POR DIMENSÃO DE QUALIDADE**:

   Completude:

   - "Os requisitos de tratamento de erros estão definidos para todos os modos de falha da API? [Lacuna]"
   - "Os requisitos de acessibilidade estão especificados para todos os elementos interativos? [Completude]"
   - "Os requisitos de breakpoint mobile estão definidos para layouts responsivos? [Lacuna]"

   Clareza:

   - "O termo 'carregamento rápido' está quantificado com limites de tempo específicos? [Clareza, Spec §NFR-2]"
   - "Os critérios de seleção de 'episódios relacionados' estão explicitamente definidos? [Clareza, Spec §FR-5]"
   - "O termo 'proeminente' está definido com propriedades visuais mensuráveis? [Ambiguidade, Spec §FR-4]"

   Consistência:

   - "Os requisitos de navegação se alinham em todas as páginas? [Consistência, Spec §FR-10]"
   - "Os requisitos de componente card estão consistentes entre landing e páginas de detalhe? [Consistência]"

   Cobertura:

   - "Os requisitos estão definidos para cenários de estado zero (sem episódios)? [Cobertura, Caso Extremo]"
   - "Os cenários de interação simultânea de usuários estão endereçados? [Cobertura, Lacuna]"
   - "Os requisitos estão especificados para falhas parciais de carregamento de dados? [Cobertura, Fluxo de Exceção]"

   Mensurabilidade:

   - "Os requisitos de hierarquia visual são mensuráveis/testáveis? [Critérios de Aceitação, Spec §FR-1]"
   - "O termo 'peso visual balanceado' pode ser verificado objetivamente? [Mensurabilidade, Spec §FR-2]"

   **Classificação e Cobertura de Cenários** (Foco em Qualidade de Requisitos):

   - Verifique se requisitos existem para: cenários Primário, Alternativo, Exceção/Erro, Recuperação, Não Funcional
   - Para cada classe de cenário, pergunte: "Os requisitos de [tipo de cenário] estão completos, claros e consistentes?"
   - Se classe de cenário ausente: "Os requisitos de [tipo de cenário] estão intencionalmente excluídos ou ausentes? [Lacuna]"
   - Inclua resiliência/rollback quando ocorrer mutação de estado: "Os requisitos de rollback estão definidos para falhas de migração? [Lacuna]"

   **Requisitos de Rastreabilidade**:

   - MÍNIMO: ≥80% dos itens DEVEM incluir pelo menos uma referência de rastreabilidade
   - Cada item deve referenciar: seção da spec `[Spec §X.Y]`, ou usar marcadores: `[Lacuna]`, `[Ambiguidade]`, `[Conflito]`, `[Suposição]`
   - Se nenhum sistema de ID existir: "Um esquema de ID de requisitos e critérios de aceitação está estabelecido? [Rastreabilidade]"

   **Superfície e Resolva Problemas** (Problemas de Qualidade de Requisitos):
   Faça perguntas sobre os requisitos em si:

   - Ambiguidades: "O termo 'rápido' está quantificado com métricas específicas? [Ambiguidade, Spec §NFR-1]"
   - Conflitos: "Os requisitos de navegação conflitam entre §FR-10 e §FR-10a? [Conflito]"
   - Suposições: "A suposição de 'API de podcast sempre disponível' está validada? [Suposição]"
   - Dependências: "Os requisitos de API de podcast externa estão documentados? [Dependência, Lacuna]"
   - Definições ausentes: "O termo 'hierarquia visual' está definido com critérios mensuráveis? [Lacuna]"

   **Consolidação de Conteúdo**:

   - Limite suave: Se itens candidatos brutos > 40, priorize por risco/impacto
   - Mescle quase-duplicatas verificando o mesmo aspecto de requisito
   - Se >5 casos extremos de baixo impacto, crie um item: "Os casos extremos X, Y, Z estão endereçados nos requisitos? [Cobertura]"

   **🚫 ABSOLUTAMENTE PROIBIDO** - Isso os torna um teste de implementação, não um teste de requisitos:

   - ❌ Qualquer item começando com "Verificar", "Testar", "Confirmar", "Checar" + comportamento de implementação
   - ❌ Referências à execução de código, ações do usuário, comportamento do sistema
   - ❌ "Exibe corretamente", "funciona adequadamente", "funciona como esperado"
   - ❌ "Clicar", "navegar", "renderizar", "carregar", "executar"
   - ❌ Casos de teste, planos de teste, procedimentos de QA
   - ❌ Detalhes de implementação (frameworks, APIs, algoritmos)

   **✅ PADRÕES OBRIGATÓRIOS** - Isso testa qualidade de requisitos:

   - ✅ "Os [tipo de requisito] estão definidos/especificados/documentados para [cenário]?"
   - ✅ "O [termo vago] está quantificado/esclarecido com critérios específicos?"
   - ✅ "Os requisitos estão consistentes entre [seção A] e [seção B]?"
   - ✅ "O [requisito] pode ser objetivamente medido/verificado?"
   - ✅ "Os [casos extremos/cenários] estão endereçados nos requisitos?"
   - ✅ "A spec define [aspecto ausente]?"

6. **Referência de Estrutura**: Gere o checklist seguindo o template canônico em `.specify/templates/checklist-template.md` para título, seção meta, cabeçalhos de categoria e formatação de ID. Se template não disponível, use: título H1, linhas meta purpose/created, seções de categoria `##` contendo linhas `- [ ] CHK### <item de requisito>` com IDs incrementando globalmente começando de CHK001.

7. **Reportar**: Produza caminho completo para checklist criado, contagem de itens e lembre o usuário que cada execução cria um novo arquivo. Resuma:
   - Áreas de foco selecionadas
   - Nível de profundidade
   - Ator/timing
   - Quaisquer itens obrigatórios especificados explicitamente pelo usuário incorporados

**Importante**: Cada invocação do comando `/speckit.checklist` cria um arquivo de checklist usando nomes curtos e descritivos, a menos que o arquivo já exista. Isso permite:

- Múltiplos checklists de diferentes tipos (por exemplo, `ux.md`, `teste.md`, `seguranca.md`)
- Nomes de arquivos simples e memoráveis que indicam o propósito do checklist
- Identificação e navegação fáceis na pasta `checklists/`

Para evitar desordem, use tipos descritivos e limpe checklists obsoletos quando terminar.

## Tipos de Checklist Exemplo e Itens de Amostra

**Qualidade de Requisitos UX:** `ux.md`

Itens de amostra (testando os requisitos, NÃO a implementação):

- "Os requisitos de hierarquia visual estão definidos com critérios mensuráveis? [Clareza, Spec §FR-1]"
- "O número e posicionamento de elementos de UI estão explicitamente especificados? [Completude, Spec §FR-1]"
- "Os requisitos de estado de interação (hover, focus, active) estão consistentemente definidos? [Consistência]"
- "Os requisitos de acessibilidade estão especificados para todos os elementos interativos? [Cobertura, Lacuna]"
- "O comportamento de fallback está definido quando imagens falham ao carregar? [Caso Extremo, Lacuna]"
- "O termo 'exibição proeminente' pode ser objetivamente medido? [Mensurabilidade, Spec §FR-4]"

**Qualidade de Requisitos de API:** `api.md`

Itens de amostra:

- "Os formatos de resposta de erro estão especificados para todos os cenários de falha? [Completude]"
- "Os requisitos de limitação de taxa estão quantificados com limites específicos? [Clareza]"
- "Os requisitos de autenticação estão consistentes em todos os endpoints? [Consistência]"
- "Os requisitos de retry/timeout estão definidos para dependências externas? [Cobertura, Lacuna]"
- "A estratégia de versionamento está documentada nos requisitos? [Lacuna]"

**Qualidade de Requisitos de Desempenho:** `desempenho.md`

Itens de amostra:

- "Os requisitos de desempenho estão quantificados com métricas específicas? [Clareza]"
- "As metas de desempenho estão definidas para todas as jornadas críticas do usuário? [Cobertura]"
- "Os requisitos de desempenho sob diferentes condições de carga estão especificados? [Completude]"
- "Os requisitos de desempenho podem ser objetivamente medidos? [Mensurabilidade]"
- "Os requisitos de degradação estão definidos para cenários de alta carga? [Caso Extremo, Lacuna]"

**Qualidade de Requisitos de Segurança:** `seguranca.md`

Itens de amostra:

- "Os requisitos de autenticação estão especificados para todos os recursos protegidos? [Cobertura]"
- "Os requisitos de proteção de dados estão definidos para informações sensíveis? [Completude]"
- "O modelo de ameaças está documentado e os requisitos alinhados a ele? [Rastreabilidade]"
- "Os requisitos de segurança estão consistentes com obrigações de compliance? [Consistência]"
- "Os requisitos de resposta a falha/violação de segurança estão definidos? [Lacuna, Fluxo de Exceção]"

## Anti-Exemplos: O Que NÃO Fazer

**❌ ERRADO - Isso testa implementação, não requisitos:**

```markdown
- [ ] CHK001 - Verificar se landing page exibe 3 cards de episódio [Spec §FR-001]
- [ ] CHK002 - Testar se estados hover funcionam corretamente no desktop [Spec §FR-003]
- [ ] CHK003 - Confirmar se clique no logo navega para página home [Spec §FR-010]
- [ ] CHK004 - Checar se seção de episódios relacionados mostra 3-5 itens [Spec §FR-005]
```

**✅ CORRETO - Isso testa qualidade de requisitos:**

```markdown
- [ ] CHK001 - O número e layout de episódios em destaque estão explicitamente especificados? [Completude, Spec §FR-001]
- [ ] CHK002 - Os requisitos de estado hover estão consistentemente definidos para todos os elementos interativos? [Consistência, Spec §FR-003]
- [ ] CHK003 - Os requisitos de navegação estão claros para todos os elementos de marca clicáveis? [Clareza, Spec §FR-010]
- [ ] CHK004 - Os critérios de seleção para episódios relacionados estão documentados? [Lacuna, Spec §FR-005]
- [ ] CHK005 - Os requisitos de estado de carregamento estão definidos para dados de episódios assíncronos? [Lacuna]
- [ ] CHK006 - Os requisitos de "hierarquia visual" podem ser objetivamente medidos? [Mensurabilidade, Spec §FR-001]
```

**Diferenças Principais:**

- Errado: Testa se o sistema funciona corretamente
- Correto: Testa se os requisitos estão escritos corretamente
- Errado: Verificação de comportamento
- Correto: Validação de qualidade de requisito
- Errado: "Ele faz X?"
- Correto: "X está claramente especificado?"
