# Especificação de Funcionalidade: [FEATURE NAME]

**Branch de Funcionalidade**: `[###-feature-name]`  
**Criado**: [DATE]  
**Status**: Rascunho  
**Entrada**: Descrição do usuário: "$ARGUMENTS"

## Fluxo de Execução (principal)

```
1. Analise descrição do usuário da Entrada
   → Se vazia: ERRO "Nenhuma descrição de funcionalidade fornecida"
2. Extraia conceitos-chave da descrição
   → Identifique: atores, ações, dados, restrições
3. Para cada aspecto não claro:
   → Marque com [PRECISA CLARIFICAÇÃO: pergunta específica]
4. Preencha seção Cenários de Usuário e Teste
   → Se nenhum fluxo de usuário claro: ERRO "Não é possível determinar cenários de usuário"
5. Gere Requisitos Funcionais
   → Cada requisito deve ser testável
   → Marque requisitos ambíguos
6. Identifique Entidades-Chave (se dados envolvidos)
7. Execute Lista de Verificação de Revisão
   → Se qualquer [PRECISA CLARIFICAÇÃO]: AVISO "Especificação tem incertezas"
   → Se detalhes de implementação encontrados: ERRO "Remova detalhes técnicos"
8. Retorne: SUCESSO (especificação pronta para planejamento)
```

---

## ⚡ Diretrizes Rápidas

- ✅ Foque no QUE os usuários precisam e POR QUÊ
- ❌ Evite COMO implementar (sem stack tecnológico, APIs, estrutura de código)
- 👥 Escrito para stakeholders de negócio, não desenvolvedores

### Requisitos de Seção

- **Seções obrigatórias**: Devem ser completadas para toda funcionalidade
- **Seções opcionais**: Inclua apenas quando relevante para a funcionalidade
- Quando uma seção não se aplica, remova-a completamente (não deixe como "N/A")

### Para Geração de IA

Ao criar esta especificação a partir de um prompt do usuário:

1. **Marque todas as ambiguidades**: Use [PRECISA CLARIFICAÇÃO: pergunta específica] para qualquer suposição que você precisaria fazer
2. **Não adivinhe**: Se o prompt não especificar algo (ex., "sistema de login" sem método de auth), marque-o
3. **Pense como um testador**: Todo requisito vago deve falhar no item de lista de verificação "testável e não ambíguo"
4. **Áreas comuns subespecificadas**:
   - Tipos de usuário e permissões
   - Políticas de retenção/exclusão de dados
   - Alvos de performance e escala
   - Comportamentos de tratamento de erro
   - Requisitos de integração
   - Necessidades de segurança/conformidade

---

## Cenários de Usuário e Teste _(obrigatório)_

### História Principal do Usuário

[Descreva a jornada principal do usuário em linguagem simples]

### Cenários de Aceitação

1. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]
2. **Dado** [estado inicial], **Quando** [ação], **Então** [resultado esperado]

### Casos Extremos

- O que acontece quando [condição de limite]?
- Como o sistema lida com [cenário de erro]?

## Requisitos _(obrigatório)_

### Requisitos Funcionais

- **RF-001**: Sistema DEVE [capacidade específica, ex., "permitir que usuários criem contas"]
- **RF-002**: Sistema DEVE [capacidade específica, ex., "validar endereços de email"]
- **RF-003**: Usuários DEVEM ser capazes de [interação-chave, ex., "redefinir sua senha"]
- **RF-004**: Sistema DEVE [requisito de dados, ex., "persistir preferências do usuário"]
- **RF-005**: Sistema DEVE [comportamento, ex., "registrar todos os eventos de segurança"]

_Exemplo de marcação de requisitos não claros:_

- **RF-006**: Sistema DEVE autenticar usuários via [PRECISA CLARIFICAÇÃO: método de auth não especificado - email/senha, SSO, OAuth?]
- **RF-007**: Sistema DEVE reter dados do usuário por [PRECISA CLARIFICAÇÃO: período de retenção não especificado]

### Entidades-Chave _(inclua se funcionalidade envolve dados)_

- **[Entidade 1]**: [O que representa, atributos-chave sem implementação]
- **[Entidade 2]**: [O que representa, relacionamentos com outras entidades]

---

## Lista de Verificação de Revisão e Aceitação

_PORTÃO: Verificações automatizadas executadas durante execução main()_

### Qualidade do Conteúdo

- [ ] Nenhum detalhe de implementação (linguagens, frameworks, APIs)
- [ ] Focado no valor do usuário e necessidades de negócio
- [ ] Escrito para stakeholders não-técnicos
- [ ] Todas as seções obrigatórias completadas

### Completude dos Requisitos

- [ ] Nenhum marcador [PRECISA CLARIFICAÇÃO] permanece
- [ ] Requisitos são testáveis e não ambíguos
- [ ] Critérios de sucesso são mensuráveis
- [ ] Escopo é claramente delimitado
- [ ] Dependências e suposições identificadas

---

## Status de Execução

_Atualizado por main() durante processamento_

- [ ] Descrição do usuário analisada
- [ ] Conceitos-chave extraídos
- [ ] Ambiguidades marcadas
- [ ] Cenários de usuário definidos
- [ ] Requisitos gerados
- [ ] Entidades identificadas
- [ ] Lista de verificação de revisão passou

---
