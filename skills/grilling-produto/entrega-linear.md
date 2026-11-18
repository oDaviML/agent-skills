# Entrega: documentação, projeto e cards no Linear

Esta fase começa **só depois** que o usuário aprovou com um "sim" explícito o **resumo completo de todas as decisões** apresentado no fim da entrevista (ver o final de `SKILL.md`). Se esse resumo ainda não foi mostrado e aprovado nesta sessão, pare e faça isso primeiro — nem arquivos de documentação nem o Linear são tocados antes. Ela tem três etapas, cada uma com um ponto de confirmação: (A) documentação local, (B) backlog em rascunho, (C) publicação no Linear. Nada é escrito no Linear antes de o usuário aprovar o rascunho da etapa B, porque o Linear é um sistema compartilhado com o time.

Diretório de saída: o mesmo de `decisoes.md` (`<workspace>/docs/produto/<slug>/` se houver projeto aberto, `~/.gemini/antigravity/scratch/produtos/<slug>/` no Antigravity sem workspace, ou `~/.kiro/crew/workspace/produtos/<slug>/` no Kiro). Abaixo ele é chamado de `<dir>`.

> **Nota sobre ferramentas MCP do Linear:** Nos exemplos abaixo os nomes das ferramentas são referenciados sem prefixo (`get_workspace`, `list_teams`, `save_project`, `save_issue`, etc.). No Google Antigravity, acione-as via `call_mcp_tool` com `ServerName: "linear"` (lendo o schema em `<appDataDir>/mcp/linear/<toolName>.json` quando necessário) ou via ferramentas nativas `mcp_linear_<toolName>`; no Claude Code / Kiro / Cursor / Codex, use o nome registrado pelo servidor MCP do Linear ativo.

## A. Documentação local

Gere, preenchendo só com o que foi decidido (o que ficou como "decidir depois" vai para riscos/questões em aberto, nunca é inventado):

1. `<dir>/especificacao.md`, a partir de [template/especificacao.md](./template/especificacao.md). É o documento mestre: todas as seções, com IDs estáveis (UC-, RF-, RNF-, RN-, T-).
2. `<dir>/adr/ADR-NN-<slug>.md`: um ADR para cada decisão de arquitetura/stack que teve alternativas discutidas na entrevista. Formato curto:
   ```
   # ADR-NN — <decisão>
   - Status: aceito · Data: <data> · Rodada: <N>
   ## Contexto
   <requisitos e restrições que motivaram a decisão, com IDs RNF/RF>
   ## Decisão
   <o que foi escolhido>
   ## Alternativas consideradas
   - <alternativa>: <por que foi descartada>
   ## Consequências
   <o que fica mais fácil, o que fica mais difícil, riscos>
   ```
3. `<dir>/README.md`: índice de uma tela com a visão em uma linha, links para os arquivos acima e para o projeto no Linear (preenchido na etapa C).

Mostre os caminhos absolutos e siga para B sem nova confirmação: o resumo já foi aprovado e os arquivos são locais e reversíveis.

## B. Backlog em rascunho

### B1. Perguntas de configuração (uma rodada no formato normal da skill: contexto no chat + cartão `ask_question`, até 4 perguntas por cartão; se houver mais, faça duas rodadas)

Antes da rodada, levante os fatos você mesmo pelo MCP do Linear (somente leitura): `get_workspace`, `list_teams`, `list_projects` (com `query` = nome do produto, para detectar um projeto que já exista), `list_issue_labels` (no time), `list_issue_statuses` (no time), `list_cycles` (no time, se houver ciclos) e `list_templates` (tipo `issue`, para ver se o time já tem um template de card). Use esses fatos como as **opções** do cartão (ex.: um time por opção, os projetos parecidos encontrados como opções de reaproveitamento). Depois pergunte só o que esses fatos não respondem, com recomendação:

- Qual time recebe o projeto (se houver mais de um).
- Projeto novo ou reaproveitar um existente com nome parecido.
- Granularidade: épicos por **módulo/área funcional** com sub-issues por **fatia vertical** (recomendação padrão), ou épicos por fase do plano de entrega.
- Estimativas: usar ou não; escala (pontos 1/2/3/5/8 ou T-shirt) se o time usa.
- Labels: reaproveitar as existentes ou criar o conjunto proposto em C1.
- Milestones: um por fase da seção 17 da especificação (recomendado).
- Responsável e ciclo: deixar sem responsável e fora de ciclo (recomendado) ou atribuir.
- Template do time: se `list_templates` achou um template de issue, perguntar se ele substitui `template/card-linear.md`.

Se o MCP do Linear não estiver disponível (ferramentas ausentes ou com erro depois de duas tentativas), diga isso em uma linha, gere o backlog local e pare antes da etapa C.

### B2. Quebra em cards

Derive o backlog da especificação, nesta ordem:

1. **Fundação** (épico próprio): repositório, ambientes, CI/CD, infra, autenticação base, observabilidade, design system — tudo que a seção 11 e os RNFs exigem antes das funcionalidades.
2. **Funcionalidades**: um épico por módulo/área (ou por fase, conforme B1). Cada sub-issue é uma **fatia vertical** entregável e testável sozinha (UI + API + dados de um comportamento), dimensionada para no máximo ~2 dias de trabalho. Se um card não cabe nisso, quebre-o.
3. **Requisitos transversais**: RNFs que não cabem dentro de uma funcionalidade (ex.: teste de carga para RNF-03, auditoria LGPD) viram cards próprios.
4. **Lançamento**: analytics, onboarding, documentação de usuário, checklist de go-live.

Regras de cobertura (verifique antes de mostrar o rascunho):
- Todo RF de prioridade Must está em pelo menos um card; todo card cita ao menos um UC/RF/RNF/T.
- Toda tela T-NN está coberta por algum card.
- Todo RNF está em um card próprio ou nos critérios de aceite de outros cards.
- Nenhum card tem conteúdo que não esteja na especificação. Se faltar informação para um card, ela volta como pergunta, não como suposição.

Escreva cada card com [template/card-linear.md](./template/card-linear.md) (épicos com o template de épico, sub-issues com o de tarefa). Use IDs locais (`EP-01`, `CD-001`) para pai e dependências; eles viram identificadores do Linear na etapa C.

Salve tudo em `<dir>/backlog.md`: primeiro uma tabela-resumo (ID local · título · tipo · épico · milestone · prioridade · estimativa · bloqueado por · requisitos cobertos), depois a matriz de cobertura RF → cards, depois os cards completos.

### B3. Confirmação

Mostre ao usuário a tabela-resumo, a contagem (épicos, cards, labels e milestones a criar) e o caminho de `backlog.md`, e peça a aprovação com `ask_question`: `header` `Publicar no Linear`, opções `Sim, pode publicar` · `Quero ajustar o backlog` · `Não publicar agora`, com a `question` lembrando que o campo de texto aceita ajustes direto. Aceite ajustes ("junta CD-004 e CD-005", "tira o épico de lançamento") e reapresente com um novo cartão. **Não publique sem um "sim" explícito.**

## C. Publicação no Linear

### C0. Registro de idempotência

Mantenha `<dir>/linear.md`: uma tabela `ID local → tipo → identificador Linear → URL`, atualizada **logo após cada criação** (não no fim). Antes de criar qualquer coisa, leia esse arquivo e pule o que já tem identificador. Isso permite retomar uma publicação interrompida sem duplicar nada. Se um ID local já existir no Linear, atualize (`id` no `save_*`) em vez de criar outro.

### C1. Ordem de criação

1. **Labels** (`save_issue_label` com `teamId`), só as que não existem e foram aprovadas. Conjunto padrão sugerido: grupo `Tipo` (Feature, Fundação, Infra, Débito, Pesquisa) e grupo `Área` (um por módulo do produto). Não recrie labels que já existem com o mesmo nome.
2. **Projeto** (`save_project`): `name` = nome do produto; `addTeams` = time escolhido; `summary` = frase de visão (até 255 caracteres); `description` = visão, problema, personas, escopo do MVP, fora do escopo e links para os documentos; `startDate`/`targetDate` se o plano de entrega tiver datas; `priority` se o usuário definiu.
3. **Documentos** (`save_document` com `project`): um por grupo, copiado da especificação, para que o time leia no Linear sem abrir arquivos locais:
   - "Visão, personas e escopo" (seções 1–4 e 6)
   - "Casos de uso e regras de negócio" (5 e 7)
   - "Requisitos funcionais e não funcionais" (8 e 9)
   - "Dados, arquitetura e stack" (10 e 11, mais um resumo dos ADRs)
   - "Design e telas" (12–14), se houver interface
   - "Operação, riscos e plano de entrega" (15–17)
4. **Milestones** (`save_milestone` com `project`): um por fase da seção 17, com o critério de aceite da fase na `description` e `targetDate` se houver.
5. **Épicos** (`save_issue`): `team`, `project`, `title`, `description` (template de épico), `labels`, `milestone`, `priority`.
6. **Cards** (`save_issue`), em ordem topológica de dependências, para que todo bloqueador já exista: `team`, `project`, `parentId` = identificador do épico, `title`, `description` (template de tarefa), `labels`, `milestone`, `priority`, `estimate` (só se o time usa estimativas), `blockedBy` = identificadores Linear dos bloqueadores. Se houver ciclo de dependência, crie sem `blockedBy` e aplique as relações numa segunda passada com `save_issue` (`id` + `blockedBy`).
7. **Atualizar referências locais**: depois de criar os cards, substitua no `backlog.md` e no `README.md` os IDs locais pelos identificadores Linear (ex.: `CD-007 → STR-142`).

Regras do `save_issue`/`save_document`:
- `description`/`content` em Markdown com quebras de linha literais, sem escapar caracteres.
- Prioridade do Linear: 1 Urgente, 2 Alta, 3 Média, 4 Baixa. Mapeamento padrão: Fundação e Must que bloqueiam outros → 2; demais Must → 3; Should → 4; Could fica fora do MVP (no backlog local, não publicado, salvo pedido do usuário).
- Não atribua responsável, ciclo ou estado diferente do padrão se o usuário não pediu.
- Em erro de uma criação: registre em `linear.md` com status `erro`, continue com o que não depende dela e liste as falhas no fim. Não tente a mesma chamada mais de duas vezes.

### C2. Verificação

- `list_issues` com `project` (pagine até o fim) e compare a contagem e os títulos com `backlog.md`.
- `get_project` com `includeMilestones` e `includeResources` para confirmar documentos e milestones.
- Abra por amostragem 2 cards com `get_issue` (`includeRelations`) e confira pai, labels e bloqueios.

### C3. Relatório final

Informe: link do projeto, contagem do que foi criado (labels, documentos, milestones, épicos, cards), falhas se houver, e os caminhos absolutos de `especificacao.md`, `backlog.md`, `linear.md` e da pasta `adr/`. Não comece a implementar o produto nem a criar o repositório de código a menos que o usuário peça.
