---
description: Identificar áreas subespecificadas na especificação de funcionalidade atual fazendo até 5 perguntas de esclarecimento altamente direcionadas e codificando as respostas de volta na especificação.
---

A entrada do usuário pode ser fornecida diretamente pelo agente ou como argumento de comando - você **DEVE** considerá-la antes de prosseguir com o prompt (se não estiver vazia).

Entrada do usuário:

$ARGUMENTS

Objetivo: Detectar e reduzir ambiguidade ou pontos de decisão ausentes na especificação de funcionalidade ativa e registrar as clarificações diretamente no arquivo de especificação.

Nota: Este fluxo de esclarecimento é esperado para ser executado (e completado) ANTES de invocar `/plan`. Se o usuário explicitamente declarar que está pulando o esclarecimento (ex., spike exploratório), você pode prosseguir, mas deve avisar que o risco de retrabalho downstream aumenta.

Etapas de execução:

1. Execute `.specify/scripts/bash/check-prerequisites.sh --json --paths-only` da raiz do repositório **uma vez** (modo combinado `--json --paths-only` / `-Json -PathsOnly`). Analise campos mínimos do payload JSON:

   - `FEATURE_DIR`
   - `FEATURE_SPEC`
   - (Opcionalmente capture `IMPL_PLAN`, `TASKS` para fluxos encadeados futuros.)
   - Se a análise JSON falhar, aborte e instrua o usuário a re-executar `/specify` ou verificar o ambiente do branch de funcionalidade.

2. Carregue o arquivo de especificação atual. Execute uma varredura estruturada de ambiguidade e cobertura usando esta taxonomia. Para cada categoria, marque status: Claro / Parcial / Ausente. Produza um mapa de cobertura interno usado para priorização (não produza o mapa bruto a menos que nenhuma pergunta será feita).

   Escopo Funcional e Comportamento:

   - Objetivos principais do usuário e critérios de sucesso
   - Declarações explícitas fora do escopo
   - Diferenciação de papéis/personas do usuário

   Domínio e Modelo de Dados:

   - Entidades, atributos, relacionamentos
   - Regras de identidade e unicidade
   - Transições de ciclo de vida/estado
   - Suposições de volume/escala de dados

   Fluxo de Interação e UX:

   - Jornadas/sequências críticas do usuário
   - Estados de erro/vazio/carregamento
   - Notas de acessibilidade ou localização

   Atributos de Qualidade Não-Funcionais:

   - Performance (alvos de latência, throughput)
   - Escalabilidade (horizontal/vertical, limites)
   - Confiabilidade e disponibilidade (expectativas de uptime, recuperação)
   - Observabilidade (sinais de logging, métricas, tracing)
   - Segurança e privacidade (authN/Z, proteção de dados, suposições de ameaça)
   - Restrições de conformidade/regulatórias (se houver)

   Integração e Dependências Externas:

   - Serviços/APIs externos e modos de falha
   - Formatos de importação/exportação de dados
   - Suposições de protocolo/versionamento

   Casos Extremos e Tratamento de Falhas:

   - Cenários negativos
   - Limitação de taxa / throttling
   - Resolução de conflitos (ex., edições concorrentes)

   Restrições e Tradeoffs:

   - Restrições técnicas (linguagem, armazenamento, hospedagem)
   - Tradeoffs explícitos ou alternativas rejeitadas

   Terminologia e Consistência:

   - Termos de glossário canônico
   - Sinônimos evitados / termos depreciados

   Sinais de Conclusão:

   - Testabilidade de critérios de aceitação
   - Indicadores mensuráveis estilo Definition of Done

   Diversos / Placeholders:

   - Marcadores TODO / decisões não resolvidas
   - Adjetivos ambíguos ("robusto", "intuitivo") sem quantificação

   Para cada categoria com status Parcial ou Ausente, adicione uma oportunidade de pergunta candidata a menos que:

   - O esclarecimento não mudaria materialmente a estratégia de implementação ou validação
   - A informação é melhor adiada para a fase de planejamento (note internamente)

3. Gere (internamente) uma fila priorizada de perguntas de esclarecimento candidatas (máximo 5). NÃO as produza todas de uma vez. Aplique estas restrições:

   - Máximo de 5 perguntas totais em toda a sessão.
   - Cada pergunta deve ser respondível com OU:
     - Uma seleção de múltipla escolha curta (2–5 opções distintas e mutuamente exclusivas), OU
     - Uma resposta de uma palavra / frase curta (restrinja explicitamente: "Responda em <=5 palavras").
   - Inclua apenas perguntas cujas respostas impactam materialmente arquitetura, modelagem de dados, decomposição de tarefas, design de testes, comportamento UX, prontidão operacional, ou validação de conformidade.
   - Garanta equilíbrio de cobertura de categoria: tente cobrir as categorias não resolvidas de maior impacto primeiro; evite fazer duas perguntas de baixo impacto quando uma única área de alto impacto (ex., postura de segurança) está não resolvida.
   - Exclua perguntas já respondidas, preferências estilísticas triviais, ou detalhes de execução de nível de plano (a menos que bloqueiem correção).
   - Favoreça esclarecimentos que reduzam risco de retrabalho downstream ou previnam testes de aceitação desalinhados.
   - Se mais de 5 categorias permanecerem não resolvidas, selecione as top 5 pela heurística (Impacto \* Incerteza).

4. Loop de questionamento sequencial (interativo):

   - Apresente EXATAMENTE UMA pergunta por vez.
   - Para perguntas de múltipla escolha, renderize opções como uma tabela Markdown:

     | Opção | Descrição                                           |
     | ----- | --------------------------------------------------- | ------------------------------------------------------------ |
     | A     | <Descrição da Opção A>                              |
     | B     | <Descrição da Opção B>                              |
     | C     | <Descrição da Opção C>                              | (adicione D/E conforme necessário até 5)                     |
     | Curto | Forneça uma resposta curta diferente (<=5 palavras) | (Inclua apenas se alternativa de forma livre for apropriada) |

   - Para estilo de resposta curta (sem opções discretas significativas), produza uma única linha após a pergunta: `Formato: Resposta curta (<=5 palavras)`.
   - Após o usuário responder:
     - Valide que a resposta mapeia para uma opção ou se encaixa na restrição <=5 palavras.
     - Se ambíguo, peça uma desambiguação rápida (contagem ainda pertence à mesma pergunta; não avance).
     - Uma vez satisfatório, registre na memória de trabalho (ainda não escreva no disco) e mova para a próxima pergunta na fila.
   - Pare de fazer mais perguntas quando:
     - Todas as ambiguidades críticas resolvidas cedo (itens restantes na fila se tornam desnecessários), OU
     - Usuário sinaliza conclusão ("pronto", "bom", "não mais"), OU
     - Você alcança 5 perguntas feitas.
   - Nunca revele perguntas futuras na fila antecipadamente.
   - Se nenhuma pergunta válida existir no início, reporte imediatamente nenhuma ambiguidade crítica.

5. Integração após CADA resposta aceita (abordagem de atualização incremental):

   - Mantenha representação em memória da especificação (carregada uma vez no início) mais o conteúdo bruto do arquivo.
   - Para a primeira resposta integrada nesta sessão:
     - Garanta que uma seção `## Esclarecimentos` existe (crie-a logo após a seção contextual/visão geral de mais alto nível conforme o template de especificação se ausente).
     - Sob ela, crie (se não presente) um subtítulo `### Sessão YYYY-MM-DD` para hoje.
   - Anexe uma linha de bullet imediatamente após aceitação: `- P: <pergunta> → R: <resposta final>`.
   - Então aplique imediatamente o esclarecimento à(s) seção(ões) mais apropriada(s):
     - Ambiguidade funcional → Atualize ou adicione um bullet em Requisitos Funcionais.
     - Distinção de interação/ator do usuário → Atualize Histórias de Usuário ou subseção Atores (se presente) com papel, restrição ou cenário esclarecido.
     - Forma/entidades de dados → Atualize Modelo de Dados (adicione campos, tipos, relacionamentos) preservando ordenação; note restrições adicionadas sucintamente.
     - Restrição não-funcional → Adicione/modifique critérios mensuráveis na seção Não-Funcional / Atributos de Qualidade (converta adjetivo vago para métrica ou alvo explícito).
     - Caso extremo / fluxo negativo → Adicione um novo bullet sob Casos Extremos / Tratamento de Erro (ou crie tal subseção se o template fornecer placeholder para ela).
     - Conflito terminológico → Normalize termo através da especificação; retenha original apenas se necessário adicionando `(anteriormente referido como "X")` uma vez.
   - Se o esclarecimento invalida uma declaração ambígua anterior, substitua essa declaração ao invés de duplicar; não deixe texto contraditório obsoleto.
   - Salve o arquivo de especificação APÓS cada integração para minimizar risco de perda de contexto (sobrescrita atômica).
   - Preserve formatação: não reordene seções não relacionadas; mantenha hierarquia de cabeçalhos intacta.
   - Mantenha cada esclarecimento inserido mínimo e testável (evite deriva narrativa).

6. Validação (executada após CADA escrita mais passagem final):

   - Sessão de esclarecimentos contém exatamente um bullet por resposta aceita (sem duplicatas).
   - Total de perguntas feitas (aceitas) ≤ 5.
   - Seções atualizadas não contêm placeholders vagos persistentes que a nova resposta deveria resolver.
   - Nenhuma declaração anterior contraditória permanece (escaneie por escolhas alternativas agora inválidas removidas).
   - Estrutura Markdown válida; apenas cabeçalhos novos permitidos: `## Esclarecimentos`, `### Sessão YYYY-MM-DD`.
   - Consistência terminológica: mesmo termo canônico usado através de todas as seções atualizadas.

7. Escreva a especificação atualizada de volta para `FEATURE_SPEC`.

8. Reporte conclusão (após loop de questionamento terminar ou terminação antecipada):
   - Número de perguntas feitas e respondidas.
   - Caminho para especificação atualizada.
   - Seções tocadas (liste nomes).
   - Tabela de resumo de cobertura listando cada categoria de taxonomia com Status: Resolvido (era Parcial/Ausente e endereçado), Adiado (excede cota de perguntas ou melhor adequado para planejamento), Claro (já suficiente), Pendente (ainda Parcial/Ausente mas baixo impacto).
   - Se algum Pendente ou Adiado permanecer, recomende se prosseguir para `/plan` ou executar `/clarify` novamente mais tarde pós-plano.
   - Comando próximo sugerido.

Regras de comportamento:

- Se nenhuma ambiguidade significativa encontrada (ou todas as perguntas potenciais seriam de baixo impacto), responda: "Nenhuma ambiguidade crítica detectada que valha esclarecimento formal." e sugira prosseguir.
- Se arquivo de especificação ausente, instrua o usuário a executar `/specify` primeiro (não crie uma nova especificação aqui).
- Nunca exceda 5 perguntas totais feitas (tentativas de esclarecimento para uma única pergunta não contam como novas perguntas).
- Evite perguntas especulativas de stack tecnológico a menos que a ausência bloqueie clareza funcional.
- Respeite sinais de terminação antecipada do usuário ("pare", "pronto", "prossiga").
- Se nenhuma pergunta feita devido à cobertura completa, produza um resumo de cobertura compacto (todas as categorias Claras) então sugira avançar.
- Se cota alcançada com categorias de alto impacto não resolvidas restantes, sinalize-as explicitamente sob Adiado com justificativa.

Contexto para priorização: $ARGUMENTS
