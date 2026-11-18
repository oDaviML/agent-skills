# Templates de card do Linear

Dois templates: **épico** (issue pai) e **tarefa** (sub-issue). O bloco "Metadados" de cada um vira campos do `save_issue` e **não** entra na `description`. Tudo abaixo de "Descrição" é a `description`, em Markdown.

Regras de escrita:
- Título no formato `[Área] Verbo no infinitivo + objeto`, até ~70 caracteres. Ex.: `[Agenda] Remarcar consulta pelo app`.
- Escreva para alguém do time que **não** participou da entrevista: o card tem que bastar sozinho, com links para os documentos do projeto quando for preciso aprofundar.
- Só use o que está na especificação. Sem "TBD": se falta algo, a pergunta volta para o usuário antes de publicar.
- Critérios de aceite verificáveis, no formato Dado/Quando/Então, cada um como checkbox. Inclua os estados de erro e os RNFs que se aplicam.
- Apague as seções que não se aplicam (ex.: "Tela" num card de infra). Não deixe seção vazia.

---

## Épico

### Metadados
- **ID local:** EP-NN
- **Labels:** Tipo/<…>, Área/<…>
- **Milestone:** <fase>
- **Prioridade:** <1–4>

### Descrição

```markdown
## Objetivo
<O que este conjunto de entregas permite ao usuário, em 1–2 frases, ligado à visão do produto.>

## Personas e casos de uso
- <Persona> — UC-NN <nome>, UC-NN <nome>

## Escopo
**Inclui:**
- <capacidade>

**Não inclui:**
- <o que fica para depois ou fora, com o motivo>

## Requisitos cobertos
RF-NN, RF-NN · RNF-NN · Telas T-NN, T-NN

## Critério de conclusão do épico
- [ ] Todas as sub-issues concluídas
- [ ] <resultado observável de ponta a ponta, ex.: "um paciente novo consegue agendar e receber a confirmação">

## Referências
- Documento: <título do documento do projeto>
```

---

## Tarefa

### Metadados
- **ID local:** CD-NNN
- **Épico (pai):** EP-NN
- **Labels:** Tipo/<…>, Área/<…>
- **Milestone:** <fase>
- **Prioridade:** <1–4>
- **Estimativa:** <pontos, se o time usa>
- **Bloqueado por:** CD-NNN, CD-NNN

### Descrição

```markdown
## Contexto
<Por que este card existe: a necessidade do usuário ou a restrição técnica por trás dele, em 2–4 frases. Cite a persona e o caso de uso.>

## Objetivo
<O resultado concreto ao fim do card, em uma frase: "Ao concluir, <persona> consegue <ação>.">

## Requisitos relacionados
- **Casos de uso:** UC-NN — <nome>
- **Funcionais:** RF-NN — <texto curto>
- **Não funcionais:** RNF-NN — <texto curto e meta mensurável>
- **Regras de negócio:** RN-NN — <texto curto>

## Comportamento esperado
1. <Passo do fluxo principal, do ponto de vista do usuário ou do sistema>
2. <…>

**Fluxos alternativos e erros:**
- <Situação> → <o que o sistema faz e a mensagem exibida>

## Tela (se houver interface)
- **Tela:** T-NN — <nome> · **Acesso:** <pública / logado / papel>
- **Componentes e dados:** <o que aparece e de onde vem>
- **Ações:** <ação> → <destino>
- **Estados:** vazio: <…> · carregando: <…> · erro: <…> · sucesso: <…>
- **Validações:** <campo>: <regra> → "<mensagem>"
- **Mobile × desktop:** <diferenças>

## Escopo
**Inclui:**
- <item>

**Não inclui:**
- <item que pode parecer parte do card mas não é, e onde ele está (CD-NNN ou fora do MVP)>

## Critérios de aceite
- [ ] **Dado** <contexto>, **quando** <ação>, **então** <resultado observável>
- [ ] **Dado** <contexto de erro>, **quando** <ação>, **então** <mensagem/comportamento>
- [ ] <RNF aplicável, mensurável: "a listagem responde em < 300 ms (p95) com 10 mil registros">

## Notas técnicas
- **Stack/camadas tocadas:** <frontend, backend, banco, integração>
- **Dados:** <entidades e campos criados/alterados>
- **API:** <endpoint ou contrato esperado, se já decidido>
- **Decisões de arquitetura:** ADR-NN — <título>
- **Segurança/privacidade:** <permissões, dados sensíveis, LGPD>

## Dependências
- Bloqueado por: <CD-NNN — motivo>
- Externas: <credencial, conta, aprovação, API de terceiro>

## Definição de pronto
- [ ] Critérios de aceite verificados
- [ ] Testes automatizados para o fluxo principal e os erros listados
- [ ] Code review aprovado e CI verde
- [ ] Acessível e responsivo conforme RNFs (se houver tela)
- [ ] Documentação/changelog atualizados, se aplicável

## Referências
- Documento do projeto: <título>
- Especificação: seção <N>
```
