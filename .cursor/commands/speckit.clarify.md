---
description: Identificar áreas subespecificadas na spec da funcionalidade atual fazendo até 5 perguntas de esclarecimento altamente direcionadas e codificando respostas de volta na spec.
---

## Entrada do Usuário

```text
$ARGUMENTS
```

Você **DEVE** considerar a entrada do usuário antes de prosseguir (se não estiver vazia).

## Resumo

Objetivo: Detectar e reduzir ambiguidade ou pontos de decisão ausentes na especificação de funcionalidade ativa e registrar os esclarecimentos diretamente no arquivo da spec.

Nota: Este fluxo de esclarecimento é esperado para executar (e ser concluído) ANTES de invocar `/speckit.plan`. Se o usuário declarar explicitamente que está pulando o esclarecimento (por exemplo, spike exploratório), você pode prosseguir, mas deve avisar que o risco de retrabalho downstream aumenta.

Etapas de execução:

1. Execute `.specify/scripts/bash/check-prerequisites.sh --json --paths-only` a partir da raiz do repositório **uma vez** (modo combinado `--json --paths-only` / `-Json -PathsOnly`). Analise campos de payload JSON mínimos:

   - `FEATURE_DIR`
   - `FEATURE_SPEC`
   - (Opcionalmente capture `IMPL_PLAN`, `TASKS` para fluxos encadeados futuros.)
   - Se análise JSON falhar, aborte e instrua usuário a reexecutar `/speckit.specify` ou verificar ambiente de branch da funcionalidade.
   - Para aspas simples em argumentos como "I'm Groot", use sintaxe de escape: por exemplo 'I'\''m Groot' (ou aspas duplas se possível: "I'm Groot").

2. Carregue o arquivo da spec atual. Execute uma varredura estruturada de ambiguidade e cobertura usando esta taxonomia. Para cada categoria, marque status: Claro / Parcial / Ausente. Produza um mapa de cobertura interno usado para priorização (não produza mapa bruto a menos que nenhuma pergunta seja feita).

   Escopo Funcional e Comportamento:

   - Objetivos principais do usuário e critérios de sucesso
   - Declarações explícitas de fora de escopo
   - Diferenciação de papéis de usuário / personas

   Domínio e Modelo de Dados:

   - Entidades, atributos, relacionamentos
   - Regras de identidade e unicidade
   - Transições de ciclo de vida/estado
   - Suposições de volume / escala de dados

   Interação e Fluxo UX:

   - Jornadas / sequências críticas do usuário
   - Estados de erro/vazio/carregamento
   - Notas de acessibilidade ou localização

   Atributos de Qualidade Não Funcionais:

   - Desempenho (latência, metas de throughput)
   - Escalabilidade (horizontal/vertical, limites)
   - Confiabilidade e disponibilidade (uptime, expectativas de recuperação)
   - Observabilidade (logging, métricas, sinais de tracing)
   - Segurança e privacidade (authN/Z, proteção de dados, suposições de ameaça)
   - Restrições de compliance / regulatórias (se houver)

   Integração e Dependências Externas:

   - Serviços/APIs externos e modos de falha
   - Formatos de importação/exportação de dados
   - Suposições de protocolo/versionamento

   Casos Extremos e Tratamento de Falhas:

   - Cenários negativos
   - Limitação de taxa / throttling
   - Resolução de conflito (por exemplo, edições concorrentes)

   Restrições e Trade-offs:

   - Restrições técnicas (linguagem, armazenamento, hospedagem)
   - Trade-offs explícitos ou alternativas rejeitadas

   Terminologia e Consistência:

   - Termos de glossário canônico
   - Sinônimos evitados / termos obsoletos

   Sinais de Completude:

   - Testabilidade de critérios de aceitação
   - Indicadores mensuráveis de Definition of Done

   Diversos / Placeholders:

   - Marcadores TODO / decisões não resolvidas
   - Adjetivos ambíguos ("robusto", "intuitivo") sem quantificação

   Para cada categoria com status Parcial ou Ausente, adicione uma oportunidade de pergunta candidata a menos que:

   - Esclarecimento não mudaria materialmente a estratégia de implementação ou validação
   - Informação é melhor adiada para fase de planejamento (note internamente)

3. Gere (internamente) uma fila priorizada de perguntas de esclarecimento candidatas (máximo 5). NÃO as produza todas de uma vez. Aplique estas restrições:

   - Máximo de 10 perguntas totais em toda a sessão.
   - Cada pergunta deve ser respondível com QUALQUER:
     - Uma seleção de múltipla escolha curta (2–5 opções distintas, mutuamente exclusivas), OU
     - Uma resposta de uma palavra / frase curta (restrinja explicitamente: "Responda em <=5 palavras").
   - Inclua apenas perguntas cujas respostas impactam materialmente arquitetura, modelagem de dados, decomposição de tarefas, design de teste, comportamento UX, prontidão operacional ou validação de compliance.
   - Garanta equilíbrio de cobertura de categoria: tente cobrir as categorias não resolvidas de maior impacto primeiro; evite fazer duas perguntas de baixo impacto quando uma única área de alto impacto (por exemplo, postura de segurança) está não resolvida.
   - Exclua perguntas já respondidas, preferências estilísticas triviais ou detalhes de execução em nível de plano (a menos que bloqueie correção).
   - Favoreça esclarecimentos que reduzem risco de retrabalho downstream ou previnem testes de aceitação desalinhados.
   - Se mais de 5 categorias permanecerem não resolvidas, selecione as 5 principais por heurística de (Impacto \* Incerteza).

4. Loop de questionamento sequencial (interativo):

   - Apresente EXATAMENTE UMA pergunta por vez.
   - Para perguntas de múltipla escolha:

     - **Analise todas as opções** e determine a **opção mais adequada** baseada em:
       - Melhores práticas para o tipo de projeto
       - Padrões comuns em implementações similares
       - Redução de risco (segurança, desempenho, manutenibilidade)
       - Alinhamento com quaisquer objetivos ou restrições explícitas do projeto visíveis na spec
     - Apresente sua **opção recomendada proeminentemente** no topo com raciocínio claro (1-2 frases explicando por que esta é a melhor escolha).
     - Formate como: `**Recomendado:** Opção [X] - <raciocínio>`
     - Então renderize todas as opções como uma tabela Markdown:

     | Opção | Descrição                                           |
     | ----- | --------------------------------------------------- | ------------------------------------------------------------ |
     | A     | <Descrição da Opção A>                              |
     | B     | <Descrição da Opção B>                              |
     | C     | <Descrição da Opção C>                              | (adicione D/E conforme necessário até 5)                     |
     | Curta | Forneça uma resposta curta diferente (<=5 palavras) | (Inclua apenas se alternativa de forma livre for apropriada) |

     - Após a tabela, adicione: `Você pode responder com a letra da opção (por exemplo, "A"), aceitar a recomendação dizendo "sim" ou "recomendado", ou fornecer sua própria resposta curta.`

   - Para estilo de resposta curta (sem opções discretas significativas):
     - Forneça sua **resposta sugerida** baseada em melhores práticas e contexto.
     - Formate como: `**Sugerido:** <sua resposta proposta> - <raciocínio breve>`
     - Então produza: `Formato: Resposta curta (<=5 palavras). Você pode aceitar a sugestão dizendo "sim" ou "sugerido", ou fornecer sua própria resposta.`
   - Após o usuário responder:
     - Se o usuário responder com "sim", "recomendado" ou "sugerido", use sua recomendação/sugestão previamente declarada como a resposta.
     - Caso contrário, valide que a resposta mapeia para uma opção ou se encaixa na restrição de <=5 palavras.
     - Se ambíguo, peça uma desambiguação rápida (contagem ainda pertence à mesma pergunta; não avance).
     - Uma vez satisfatório, registre-a na memória de trabalho (não grave em disco ainda) e mova para a próxima pergunta enfileirada.
   - Pare de fazer mais perguntas quando:
     - Todas as ambiguidades críticas resolvidas cedo (itens enfileirados restantes se tornam desnecessários), OU
     - Usuário sinaliza conclusão ("pronto", "bom", "chega"), OU
     - Você alcançar 5 perguntas feitas.
   - Nunca revele perguntas enfileiradas futuras antecipadamente.
   - Se nenhuma pergunta válida existir no início, reporte imediatamente nenhuma ambiguidade crítica.

5. Integração após CADA resposta aceita (abordagem de atualização incremental):

   - Mantenha representação em memória da spec (carregada uma vez no início) mais o conteúdo bruto do arquivo.
   - Para a primeira resposta integrada nesta sessão:
     - Garanta que uma seção `## Esclarecimentos` exista (crie-a logo após a seção contextual/visão geral de maior nível conforme o template da spec se ausente).
     - Sob ela, crie (se não presente) um subcabeçalho `### Sessão AAAA-MM-DD` para hoje.
   - Anexe uma linha bullet imediatamente após aceitação: `- P: <pergunta> → R: <resposta final>`.
   - Então aplique imediatamente o esclarecimento às seções mais apropriadas:
     - Ambiguidade funcional → Atualize ou adicione um bullet em Requisitos Funcionais.
     - Interação do usuário / distinção de ator → Atualize Histórias de Usuário ou subseção Atores (se presente) com papel esclarecido, restrição ou cenário.
     - Forma de dados / entidades → Atualize Modelo de Dados (adicione campos, tipos, relacionamentos) preservando ordenação; note restrições adicionadas sucintamente.
     - Restrição não funcional → Adicione/modifique critérios mensuráveis na seção Não Funcional / Atributos de Qualidade (converta adjetivo vago para métrica ou alvo explícito).
     - Caso extremo / fluxo negativo → Adicione um novo bullet sob Casos Extremos / Tratamento de Erros (ou crie tal subseção se template fornecer placeholder para ela).
     - Conflito terminológico → Normalize termo em toda spec; retenha original apenas se necessário adicionando `(anteriormente referido como "X")` uma vez.
   - Se o esclarecimento invalidar uma declaração ambígua anterior, substitua aquela declaração ao invés de duplicar; não deixe texto contraditório obsoleto.
   - Salve o arquivo da spec APÓS cada integração para minimizar risco de perda de contexto (sobrescrita atômica).
   - Preserve formatação: não reordene seções não relacionadas; mantenha hierarquia de cabeçalhos intacta.
   - Mantenha cada esclarecimento inserido mínimo e testável (evite desvio narrativo).

6. Validação (executada APÓS CADA gravação mais passagem final):

   - Sessão de esclarecimentos contém exatamente um bullet por resposta aceita (sem duplicatas).
   - Total de perguntas feitas (aceitas) ≤ 5.
   - Seções atualizadas não contêm placeholders vagos persistentes que a nova resposta deveria resolver.
   - Nenhuma declaração contraditória anterior permanece (escaneie por escolhas alternativas agora inválidas removidas).
   - Estrutura Markdown válida; apenas novos cabeçalhos permitidos: `## Esclarecimentos`, `### Sessão AAAA-MM-DD`.
   - Consistência terminológica: mesmo termo canônico usado em todas as seções atualizadas.

7. Grave a spec atualizada de volta para `FEATURE_SPEC`.

8. Reporte conclusão (após loop de questionamento terminar ou término antecipado):
   - Número de perguntas feitas e respondidas.
   - Caminho para spec atualizada.
   - Seções tocadas (liste nomes).
   - Tabela de resumo de cobertura listando cada categoria de taxonomia com Status: Resolvido (era Parcial/Ausente e endereçado), Adiado (excede quota de perguntas ou mais adequado para planejamento), Claro (já suficiente), Pendente (ainda Parcial/Ausente mas baixo impacto).
   - Se algum Pendente ou Adiado permanecer, recomende se prosseguir para `/speckit.plan` ou executar `/speckit.clarify` novamente mais tarde pós-plano.
   - Comando seguinte sugerido.

Regras de comportamento:

- Se nenhuma ambiguidade significativa encontrada (ou todas perguntas potenciais seriam de baixo impacto), responda: "Nenhuma ambiguidade crítica detectada que valha esclarecimento formal." e sugira prosseguir.
- Se arquivo da spec ausente, instrua usuário a executar `/speckit.specify` primeiro (não crie uma nova spec aqui).
- Nunca exceda 5 perguntas totais feitas (tentativas de esclarecimento para uma única pergunta não contam como novas perguntas).
- Evite perguntas especulativas de tech stack a menos que a ausência bloqueie clareza funcional.
- Respeite sinais de término antecipado do usuário ("pare", "pronto", "prossiga").
- Se nenhuma pergunta feita devido a cobertura completa, produza um resumo de cobertura compacto (todas categorias Claro) então sugira avançar.
- Se quota alcançada com categorias não resolvidas de alto impacto restantes, sinalize-as explicitamente sob Adiado com justificativa.

Contexto para priorização: $ARGUMENTS
