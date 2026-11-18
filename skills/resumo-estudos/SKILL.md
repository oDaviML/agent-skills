---
name: resumo-estudos
description: Gera e atualiza material de estudo em LaTeX (pt-BR) num padrão visual e editorial fixo — resumos para prova, listas de exercícios de treino com nível (fácil/médio/difícil) e gabarito por seção, e resoluções detalhadas de questões com execução passo a passo visual. Margens reduzidas, títulos azul-petróleo, caixas "Questão da Lista", caixas verdes "Execução Passo a Passo" com o estado ao lado de cada etapa, tabelas booktabs e figuras logo após cada conceito. Use quando o usuário pedir resumo de estudos/da matéria/para prova, material de revisão, transformar slides/PDF/lista em LaTeX, lista de questões para treinar, ou explicação passo a passo de uma questão que não entendeu.
triggers: resumo de estudos, resumo da matéria, resumo da disciplina, resumo para prova, material de revisão, criar resumo latex, gerar resumo, resumo em latex, transformar slides em resumo, lista de treino, lista de exercícios para treinar, questões para treinar, gabarito, resolver questão passo a passo, explicar questão, não entendi o algoritmo
---

# Material de Estudos (LaTeX)

Produz e mantém material de estudo em LaTeX no padrão consolidado do usuário. Este
arquivo traz o **fluxo**; o padrão completo (layout, formatação, caixas, tabelas,
figuras, notação, tom de escrita, listas de treino, resoluções e checklist) está em
[guia-de-estilo.md](./guia-de-estilo.md) — **leia-o inteiro antes de escrever qualquer `.tex`**.

Arquivos desta skill (no mesmo diretório deste `SKILL.md`):
- [guia-de-estilo.md](./guia-de-estilo.md) — especificação completa (fonte da verdade).
- [template/main.tex](./template/main.tex) — preâmbulo base exato + esqueleto mínimo; copie, não redigite.
- [template/bloco-passo-a-passo.tex](./template/bloco-passo-a-passo.tex) — TikZ, cores, caixa `passoapasso`, macros `\etapa`,
  `\etapaLarga`, `\novo`, `\fixo`, `\legendaPasso`. Cole no preâmbulo, antes de
  `\begin{document}`, sempre que houver algoritmo/procedimento ou desenho de grafo.
- [template/bloco-lista-treino.tex](./template/bloco-lista-treino.tex) — etiquetas `\facil`/`\medio`/`\dificil`, caixas
  `questao` e `resposta`, ambiente `alternativas`, comando `\gabarito`. Depende do bloco
  passo a passo (usa as cores dele); cole depois dele.

## 0. Qual documento fazer

| Pedido | Documento | Guia |
|---|---|---|
| "resumo", "material de revisão", slides → LaTeX | Resumo de estudos | seções 1–10 |
| "lista de questões para treinar", "com gabarito" | Lista de treino (projeto novo) | seção 11 |
| "resolva essa questão passo a passo", "não entendi" | Resolução detalhada (projeto novo) | seção 12 |

Listas e resoluções são **projetos novos**: nunca alteram o resumo de origem.

## 1. Onde o documento vive

Escolha o diretório conforme o ambiente em execução:

1. **Workspace / Repositório ativo (Antigravity, Claude Code, Codex, Cursor, Gemini CLI, etc.):**
   - Se o usuário indicar uma pasta ou houver um workspace aberto: `<workspace>/resumos/<slug-do-tema>/main.tex` (ou o caminho pedido), com imagens em `figures/`, um `references.bib` vazio (se necessário) e scripts de conferência em `verificacao/`.
   - Se não houver workspace ativo (ex.: Antigravity sem pasta aberta): use `~/Documentos/Latex/<slug-do-tema>/main.tex` ou `~/.gemini/antigravity/scratch/resumos/<slug-do-tema>/main.tex`.
   - Compile com `pdflatex -interaction=nonstopmode -halt-on-error main.tex` (duas passadas) ou `tectonic main.tex`.
2. **Papyrus / Kiro Crew (quando rodando no Kiro com Papyrus disponível):**
   - **Papyrus (preferencial no Kiro):** `~/.kiro/crew/apps/papyrus/data/projects/<projeto>/main.tex`,
     com imagens em `<projeto>/figures/`, um `references.bib` vazio e scripts de conferência
     em `<projeto>/verificacao/`. O app compila: no fim, peça ao usuário **Ctrl+S**. Para
     validar antes de entregar, compile **uma cópia** em `$KIROCREW_SCRATCH` (ou `/tmp`) e copie só o
     `main.pdf` de volta.
   - **Fora do Papyrus no Kiro:** `~/.kiro/crew/workspace/resumos/<slug-do-tema>/main.tex`
     (+ `figures/`), compilado com `pdflatex -interaction=nonstopmode -halt-on-error main.tex` (duas passadas).
3. Projeto novo: monte o preâmbulo com `template/main.tex` + os blocos necessários.
   Projeto existente: **leia o arquivo atual primeiro**.

## 2. Coleta do material

- **Texto colado:** é o conteúdo-base; tudo que vier nele entra no documento.
- **PDF/slides** (pasta indicada pelo usuário, anexos da conversa ou `~/.kiro/crew/uploads/`): leia tudo. Para resumos,
  cubra a **teoria completa** dos slides de cada tópico, não só o que a lista pede. Antes
  de criar caixas passo a passo, **abra os exemplos dos slides** e copie o formato.
- **Extrair imagens dos PDFs:** quando o usuário pedir para não colar imagens à mão,
  renderize as páginas (`pdftoppm`), recorte só o desenho (sem título/logo do slide, sem
  cortar vértices ou rótulos) e confira cada recorte visualmente.
- **Imagens coladas:** procure-as nos anexos da conversa, na pasta indicada pelo usuário ou em `~/.kiro/crew/uploads/` (as mais recentes, em ordem),
  copie para `figures/` com o nome `<prefixo-da-secao>-NN-<descricao>.png` e insira cada
  uma **logo após o conceito** que o usuário associou a ela.
- **Lista de exercícios:** cada questão vira uma caixa `exemplo` junto do conceito, com o
  título **"Questão da Lista --- Assunto"** (nunca "Questão de Prova"). Questões de
  implementar código ficam de fora, a menos que o usuário peça. Se o enunciado for
  ambíguo, escolha uma interpretação, diga qual na resposta e liste-a na entrega.

## 3. Escrita (resumo das regras; detalhes no guia)

- pt-BR, didático, impessoal ou 1ª pessoa do plural; definição formal → "ou seja" →
  intuição; frases curtas; explicar o porquê.
- Listas `itemize` com `\textbf{Termo (notação):}`; exemplos curtos como
  `\emph{Exemplo Prático (Termo):}`.
- Tabelas e figuras **sempre `[H]`**, com `\caption` e `\label`.
- **Conceito → imagem**: cada figura imediatamente depois do seu conceito.
- **Passo a passo** (guia 6.8): estado ao lado de cada etapa, formato dos slides, pouco
  texto, contas explícitas sem `\min`, elementos escritos direto ($(a,b)$, nunca $e_1$).
- Sem `\maketitle`; sem marcadores de rascunho (`[cite: …]`, `(Adicionado)`). Ao
  remover marcadores, **mencione e ofereça recolocá-los**.

## 4. Conferência dos números

- Todo valor calculado (respostas, tabelas de execução, alternativas corretas) vem de um
  **script Python** que executa o método. Escreva primeiro os resultados esperados, rode o
  script e só então escreva o LaTeX.
- Em resoluções detalhadas, o script pode **gerar** os desenhos TikZ e as tabelas das
  iterações a partir de um `template.tex` com marcadores; ele precisa conferir cada passo
  (caminho, valor, invariantes como conservação ou corte = fluxo) com `assert`.
- Guarde os scripts em `<projeto>/verificacao/`.

## 5. Trabalho grande: sub-agentes

Para muitas seções independentes (várias caixas passo a passo, uma lista com 10 seções),
despache um sub-agente por seção com um **BRIEF comum**: preâmbulo/macros a usar, regras
de formato desta skill, exigência de dados novos e conferência por script, arquivo de
saída num diretório temporário de scratch (`scratch/` da conversa, `$KIROCREW_SCRATCH` ou `/tmp`). Depois revise cada parte renderizada, consolide no
`main.tex` e compile o documento inteiro.

## 6. Atualizações em documento existente

- **Edições cirúrgicas**: insira só o novo conteúdo na seção certa. **Nunca** reescreva o
  arquivo do zero nem mexa no preâmbulo sem pedido. Antes de trocar um bloco grande
  (ex.: refazer caixas), guarde a versão anterior no diretório de scratch.
- Conteúdo colado mais curto **soma** itens novos e **não remove** o que foi omitido.
- Depois de quebrar e reabrir listas, releia a região para não deixar `\item` órfão.
- Se o usuário disser que algo "sumiu", verifique primeiro se ainda está no arquivo.
- Correção de figura (ex.: recorte cortado): refaça o arquivo com o **mesmo nome**, assim
  o `.tex` não muda.

## 7. Entrega

1. Rode o checklist da seção 10 do guia (e o da 11 ou 12, se for o caso).
2. Compile (cópia em scratch se estiver no Papyrus), corrija erros e `Overfull`, renderize as páginas
   (`pdftoppm -r 70 -png`) e confira visualmente as páginas com desenhos.
3. Responda em pt-BR com: caminho absoluto do `.tex` e do PDF, o que tem em cada seção
   (tabela quando ajudar), número de páginas, como os números foram conferidos, as
   suposições feitas (interpretações de enunciado, conteúdo fora dos slides, imagens) e
   opções de próximos passos. No Papyrus, peça **Ctrl+S**.
