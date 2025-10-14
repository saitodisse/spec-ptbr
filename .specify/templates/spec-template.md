# Especificação de Funcionalidade: [NOME DA FUNCIONALIDADE]

**Branch da Funcionalidade**: `[###-nome-funcionalidade]`  
**Criado**: [DATA]  
**Status**: Rascunho  
**Entrada**: Descrição do usuário: "$ARGUMENTS"

## Cenários de Usuário e Testes _(obrigatório)_

<!--
  IMPORTANTE: Histórias de usuário devem ser PRIORIZADAS como jornadas de usuário ordenadas por importância.
  Cada história de usuário/jornada deve ser TESTÁVEL INDEPENDENTEMENTE - significando que se você implementar apenas UMA delas,
  você ainda deve ter um MVP (Produto Mínimo Viável) viável que entrega valor.

  Atribua prioridades (P1, P2, P3, etc.) a cada história, onde P1 é a mais crítica.
  Pense em cada história como uma fatia autônoma de funcionalidade que pode ser:
  - Desenvolvida independentemente
  - Testada independentemente
  - Implantada independentemente
  - Demonstrada aos usuários independentemente
-->

### História de Usuário 1 - [Título Breve] (Prioridade: P1)

[Descreva esta jornada de usuário em linguagem simples]

**Por que esta prioridade**: [Explique o valor e por que tem este nível de prioridade]

**Teste Independente**: [Descreva como isso pode ser testado independentemente - ex.: "Pode ser totalmente testado por [ação específica] e entrega [valor específico]"]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]
2. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

---

### História de Usuário 2 - [Título Breve] (Prioridade: P2)

[Descreva esta jornada de usuário em linguagem simples]

**Por que esta prioridade**: [Explique o valor e por que tem este nível de prioridade]

**Teste Independente**: [Descreva como isso pode ser testado independentemente]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

---

### História de Usuário 3 - [Título Breve] (Prioridade: P3)

[Descreva esta jornada de usuário em linguagem simples]

**Por que esta prioridade**: [Explique o valor e por que tem este nível de prioridade]

**Teste Independente**: [Descreva como isso pode ser testado independentemente]

**Cenários de Aceitação**:

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

---

[Adicione mais histórias de usuário conforme necessário, cada uma com uma prioridade atribuída]

### Casos Extremos

<!--
  AÇÃO REQUERIDA: O conteúdo nesta seção representa placeholders.
  Preencha-os com os casos extremos corretos.
-->

- O que acontece quando [condição de contorno]?
- Como o sistema lida com [cenário de erro]?

## Requisitos _(obrigatório)_

<!--
  AÇÃO REQUERIDA: O conteúdo nesta seção representa placeholders.
  Preencha-os com os requisitos funcionais corretos.
-->

### Requisitos Funcionais

- **FR-001**: Sistema DEVE [capacidade específica, ex.: "permitir que usuários criem contas"]
- **FR-002**: Sistema DEVE [capacidade específica, ex.: "validar endereços de email"]
- **FR-003**: Usuários DEVEM ser capazes de [interação chave, ex.: "redefinir sua senha"]
- **FR-004**: Sistema DEVE [requisito de dados, ex.: "persistir preferências do usuário"]
- **FR-005**: Sistema DEVE [comportamento, ex.: "registrar todos os eventos de segurança"]

_Exemplo de marcação de requisitos pouco claros:_

- **FR-006**: Sistema DEVE autenticar usuários via [NECESSITA ESCLARECIMENTO: método de autenticação não especificado - email/senha, SSO, OAuth?]
- **FR-007**: Sistema DEVE reter dados do usuário por [NECESSITA ESCLARECIMENTO: período de retenção não especificado]

### Entidades Principais _(inclua se funcionalidade envolve dados)_

- **[Entidade 1]**: [O que representa, atributos chave sem implementação]
- **[Entidade 2]**: [O que representa, relacionamentos com outras entidades]

## Critérios de Sucesso _(obrigatório)_

<!--
  AÇÃO REQUERIDA: Defina critérios de sucesso mensuráveis.
  Estes devem ser independentes de tecnologia e mensuráveis.
-->

### Resultados Mensuráveis

- **SC-001**: [Métrica mensurável, ex.: "Usuários podem completar criação de conta em menos de 2 minutos"]
- **SC-002**: [Métrica mensurável, ex.: "Sistema lida com 1000 usuários simultâneos sem degradação"]
- **SC-003**: [Métrica de satisfação do usuário, ex.: "90% dos usuários completam tarefa primária na primeira tentativa"]
- **SC-004**: [Métrica de negócio, ex.: "Reduzir tickets de suporte relacionados a [X] em 50%"]
