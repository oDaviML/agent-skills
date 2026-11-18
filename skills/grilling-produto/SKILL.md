---
name: grilling-produto
description: Entreviste o usuário implacavelmente sobre uma ideia de produto/aplicação até que tudo esteja definido nos mínimos detalhes — problema, usuários, casos de uso, requisitos funcionais e não funcionais, stack, telas e o conteúdo de cada uma — e entregue a documentação completa, o projeto no Linear e os cards de tarefa bem descritos (via MCP do Linear). Use quando o usuário trouxer uma ideia de app/produto para definir, quiser montar um PRD/especificação ou um backlog no Linear, ou usar gatilhos como "grill", "me sabatine", "define meu produto".
triggers: grill produto, grill meu produto, definir produto, definição de produto, especificar produto, montar prd, especificação de produto, tenho uma ideia de app, me sabatine sobre o produto, backlog no linear do produto
---

Entreviste o usuário implacavelmente até chegarem a um entendimento compartilhado e completo do produto. Mapeie isso como uma **árvore de definição**: cada decisão se ramifica nas decisões que dependem dela. A ideia base que o usuário traz é a raiz. Os galhos obrigatórios e a ordem natural de dependência estão em [arvore-de-produto.md](./arvore-de-produto.md) (neste diretório) — **leia-o inteiro antes da primeira rodada**.

Trabalhe a árvore em **rodadas**. A **fronteira** é toda decisão cujos pré-requisitos já estão resolvidos: as perguntas que você pode fazer _agora_ sem chutar respostas que ainda não ouviu (não pergunte da stack antes de saber os requisitos não funcionais; não pergunte o conteúdo de uma tela antes de saber os casos de uso que ela atende). Faça a fronteira em rodadas de **até 4 perguntas** (o limite de um cartão de perguntas, ver abaixo): numere cada pergunta e dê sua resposta recomendada. Se a fronteira tiver mais que isso, faça as que destravam mais galhos e deixe o resto para a próxima rodada. Depois espere as respostas antes da rodada seguinte.

### Como perguntar: cartão de perguntas (`ask_question` / `AskUserQuestion`)

O usuário **não** deve precisar digitar respostas. Cada rodada tem duas partes:

1. **Contexto no chat** (curto): o cabeçalho da rodada e, para cada pergunta, o que o cartão não comporta — alternativas com prós e contras, o porquê da recomendação, galhos abertos ou podados pela rodada anterior.

   ```
   🧭 **Rodada N** — galhos em foco: <galhos> · resolvidos: X · em aberto: Y
   <uma linha sobre galhos abertos/podados, se houver>

   ❓ **P1 — <título>**: <contexto, alternativas (a)/(b)/(c) com prós e contras>
   ➡️ Recomendo <opção>, porque <uma linha>.

   ❓ **P2 — <título>**: …
   ```

2. **Cartão interativo** com `ask_question` (ou `AskUserQuestion` no Claude Code / Cursor), uma pergunta do cartão por P, na mesma ordem:
   - **No Google Antigravity (`ask_question`)**:
     - Passe `questions` com um item por P:
       - `question`: `P1 · <galho> — <pergunta em 1 ou 2 frases>`
       - `options`: de 2 a 5 alternativas concretas em strings (sem numeração e sem opção "Outro", pois a UI já adiciona). **A primeira é sempre a recomendada**, prefixada com `(Recommended) ` e contendo o resumo de uma linha (ex.: `(Recommended) <opção> — <consequência em 1 linha>`). Inclua, quando fizer sentido, `Decidir depois` ou `Não se aplica`.
       - `is_multi_select: true` só quando várias respostas puderem valer ao mesmo tempo (ex.: plataformas, papéis de usuário, integrações).
     - No Antigravity a ferramenta aguarda e retorna as respostas diretamente.
   - **No Kiro (`ask_question`)**:
     - `header`: `P1 · <galho>` (até 50 caracteres).
     - `question`: a pergunta em uma ou duas frases (até 500 caracteres).
     - `options`: de 2 a 5 alternativas concretas (o máximo é 6). **A primeira é sempre a recomendada**, com o rótulo terminando em ` (recomendado)`. Use `description` para o resumo de uma linha de cada alternativa (prós/contras ou consequência). Inclua, quando fizer sentido, uma opção `Decidir depois` ou `Não se aplica`.
     - `multiSelect: true` só quando várias respostas podem valer ao mesmo tempo.
     - **Resposta personalizada:** o cartão já tem um campo de texto livre; não crie opção "Outro". Depois de chamar `ask_question`, **termine o turno imediatamente**.
   - **Em outros agentes (Claude Code `AskUserQuestion`, Cursor, Codex)**: adapte aos campos do schema nativo da ferramenta mantendo até 4 perguntas por rodada e a opção recomendada em primeiro lugar.

Ao receber as respostas, associe cada uma à sua P pela ordem e pelo título/header. Uma resposta em texto livre é tratada como qualquer outra: se for vaga ou contradisser algo decidido, volta como pergunta na rodada seguinte. O usuário também pode responder digitando no chat ("P1 sim, P2 b…"); aceite os dois jeitos.

**Fallback:** se nenhuma ferramenta de perguntas interativas existir, recusar, ou devolver aviso de que a sessão não suporta cartões interativos, faça a rodada só em texto, com o mesmo formato do bloco de contexto, redigindo cada pergunta de modo que "sim" aceite a recomendação, e termine com uma linha `[OPTIONS: Aceitar todas as recomendações | …]`.

Recomende com base no que já foi decidido, não em defaults genéricos; quando não houver base para recomendar, diga isso no contexto e não marque nenhuma opção como recomendada.

Cada rodada respondida remodela a árvore: decisões resolvidas empurram a fronteira para fora e destravam perguntas que dependiam delas. Uma resposta pode também **abrir galhos novos** (ex.: "precisa funcionar offline" abre sincronização e resolução de conflitos) ou **podar galhos** (ex.: "sem login" poda permissões e recuperação de senha) — anuncie isso em uma linha no topo da rodada. Recalcule a fronteira e faça a próxima rodada. Uma pergunta cuja resposta depende de outra ainda aberta nesta rodada pertence a uma rodada _posterior_.

Descobrir _fatos_ é trabalho seu, nunca do usuário. Quando uma pergunta da fronteira precisar de um fato do ambiente ou do mundo (código existente no projeto, limites de uma API, preço de um serviço, o que concorrentes fazem, se uma lib suporta X), pesquise ou despache um sub-agente para buscá-lo; não pergunte ao usuário nada que você mesmo pode verificar. Não bloqueie por isso: uma exploração em andamento é um pré-requisito não resolvido, então só as perguntas que dependem dela esperam; faça o resto da fronteira agora. As _decisões_ são do usuário: coloque cada uma diante dele e espere.

**Sugira funcionalidades, não só pergunte.** Assim que o galho de concorrentes (4) e os casos de uso (5) tiverem as primeiras respostas, passe a trazer **sugestões de novas funcionalidades** de duas origens:

- **Derivadas do que já foi decidido:** extensões naturais e complementos das funcionalidades existentes (ex.: decidiu "agendar consulta" → sugerir lembrete automático, remarcação, lista de espera; decidiu "exportar relatório" → sugerir agendamento do envio por e-mail). Procure lacunas na jornada: o que o usuário precisa fazer antes, depois ou quando algo dá errado e que ainda não tem funcionalidade.
- **Do mercado:** pesquise ou despache um sub-agente para levantar o que produtos semelhantes oferecem (funcionalidades de destaque, o que usuários elogiam ou reclamam em avaliações, tendências da categoria) e devolver cada achado com o produto de origem e o link da fonte.

Apresente as sugestões dentro da rodada: o detalhe vai no bloco de contexto do chat, depois das perguntas:

```
💡 **S1** - **<nome da funcionalidade>** _(origem: derivada de UC-03 | mercado: <produto>, <link>)_: <o que faz e qual dor resolve, em 1–3 frases; esforço estimado (baixo/médio/alto) e o que ela abriria na árvore>
➡️ <recomendação: incluir no MVP | incluir depois | não incluir — com o porquê em uma linha>
```

No cartão, todas as sugestões da rodada ocupam **uma única pergunta** (contando no limite de 4): título/header `💡 Sugestões`, múltipla seleção ativa (`is_multi_select: true` / `multiSelect: true`), uma opção por sugestão com o rótulo `S1 · <nome> — <MVP | depois>` (a prioridade recomendada) e o resumo de uma linha. A `question` deve dizer: "Marque as que quer incluir com a prioridade indicada. As não marcadas serão recusadas. Use o campo de texto para mudar uma prioridade ou dar o motivo de uma recusa." Sugestões que você recomenda **não** incluir não entram no cartão; só aparecem no contexto, para o usuário poder pedir por texto.

Regras: no máximo ~3–5 sugestões por rodada (até 6 cabem no cartão), priorizando as de maior valor para as personas e menor esforço; nada de sugestões genéricas que qualquer app teria (ex.: "modo escuro") a menos que algo decidido as justifique; uma sugestão aceita vira decisão como qualquer outra (entra no escopo com prioridade MoSCoW e abre os galhos que dependem dela — requisitos, telas); uma recusada vai para `decisoes.md` como recusada, com o motivo se houver, e **nunca é sugerida de novo**. Continue sugerindo nas rodadas seguintes sempre que novas decisões abrirem oportunidades, até a fronteira esvaziar.

Não aceite respostas vagas como resolvidas. "Tem que ser rápido", "interface simples", "vários usuários" voltam na rodada seguinte como pergunta mensurável (quantos ms, em qual operação; quantos usuários simultâneos; o que "simples" exclui). Se duas decisões se contradizem, aponte o conflito como pergunta.

Mantenha um **registro de decisões** em `decisoes.md` no diretório do produto (`<workspace>/docs/produto/<slug-do-produto>/decisoes.md` se houver projeto aberto; `~/.gemini/antigravity/scratch/produtos/<slug-do-produto>/decisoes.md` no Antigravity sem workspace; ou `~/.kiro/crew/workspace/produtos/<slug-do-produto>/decisoes.md` no Kiro), atualizado ao fim de cada rodada: decisão, galho, rodada, e o que ficou em aberto. Isso permite retomar a sessão depois de uma compactação ou em outro dia — ao retomar, leia o registro antes de perguntar qualquer coisa.

A sessão termina quando a fronteira está vazia: todo galho de `arvore-de-produto.md` visitado (resolvido, ou explicitamente marcado como "fora de escopo"/"decidir depois" pelo usuário), nada assumido em silêncio. Então **não documente nada ainda** — nem arquivos de especificação, nem Linear. Primeiro apresente no chat um **resumo completo de todas as decisões**, organizado pelos galhos de `arvore-de-produto.md`, nesta forma:

```
📋 **Resumo das decisões** — <nome do produto> · <N> decisões em <R> rodadas

### 1. Visão e problema
- <decisão> _(rodada N)_
…
### 8. Requisitos funcionais
- RF-01 — <requisito> · UC-NN · Must
…
### 14. Telas
- T-01 — <tela>: <objetivo> · componentes: <…> · ações: <…> · estados: <…>
…
### ⏸️ Decidir depois / fora de escopo
- <item> — <galho>
### 💡 Sugestões de funcionalidades
- Aceitas: S<N> <nome> — <MVP | depois> · <origem>
- Recusadas: S<N> <nome> — <motivo>
### ⚠️ Premissas e riscos em aberto
- <item>
```

Inclua **toda** decisão registrada em `decisoes.md`, com os IDs (UC-, RF-, RNF-, RN-, T-) que irão para a especificação, e nada que não tenha sido decidido pelo usuário. Se o resumo for longo, não o encurte: ele é a última chance de o usuário corrigir algo antes de virar documentação. Ao fim do resumo, peça a aprovação com `ask_question`: `header` `Aprovação`, `question` "Está tudo correto? Posso gerar a documentação e publicar no Linear?", opções `Sim, pode documentar` · `Quero corrigir algumas decisões` · `Ainda não, vamos pausar aqui`, lembrando na `question` que o campo de texto aceita a correção direto. Aceite correções, atualizando `decisoes.md` e reapresentando só as seções alteradas, com um novo cartão de aprovação.

**Não gere documentação nem toque no Linear antes de um "sim" explícito a esse resumo.** Aprovado, passe para a **fase de entrega**: leia [entrega-linear.md](./entrega-linear.md) (neste diretório) inteiro e siga-o. Em resumo, ela:

1. gera a documentação local no mesmo diretório — `especificacao.md` (a partir de [template/especificacao.md](./template/especificacao.md)), ADRs das decisões de stack e um `README.md` índice;
2. faz uma última rodada curta de configuração do Linear (time, projeto novo ou existente, granularidade, estimativas, labels, milestones), quebra a especificação em épicos e cards com [template/card-linear.md](./template/card-linear.md) e salva o rascunho em `backlog.md`;
3. **só após um "sim" explícito ao rascunho**, cria pelo MCP do Linear o projeto, os documentos, os milestones, os épicos e os cards com dependências, registrando cada criação em `linear.md` para nunca duplicar ao retomar; depois verifica o que foi criado e informa links e caminhos absolutos.

Não comece a implementar o produto nem a criar o repositório de código a menos que o usuário peça.
