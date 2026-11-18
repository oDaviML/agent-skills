# Guia de Estilo — Resumo de Estudo em LaTeX

Referência completa do padrão dos resumos de estudo em LaTeX. Todo documento gerado pela
skill `resumo-estudos` segue **exatamente** esta estrutura, layout, formatação e escrita,
independentemente do assunto. O resultado é um único `main.tex` completo (do
`\documentclass` ao `\end{document}`), sem trechos omitidos nem "continue aqui".

---

## 1. Natureza do documento

- É um **material de estudo/revisão**: reúne teoria, definições, fórmulas, comparações,
  procedimentos e exercícios resolvidos de um assunto.
- Deve funcionar para duas coisas ao mesmo tempo:
  - **consulta rápida**: definições objetivas, termos destacados, fórmulas e tabelas-resumo;
  - **prática**: exercícios da lista resolvidos passo a passo, junto da teoria que aplicam.
- Idioma: **português do Brasil**, com acentuação e pontuação corretas em todo o texto
  corrido.
- Cubra **todo** o conteúdo fornecido. Não resuma demais e não omita tópicos: se o
  material traz um conceito, ele aparece no documento.

---

## 2. Compatibilidade e regras técnicas gerais

- Precisa compilar em **pdfLaTeX e Tectonic**. Use só pacotes padrão do TeX Live; nada que
  exija `--shell-escape` (não use `minted`).
- Não use `algorithm`/`algpseudocode`. Código, pseudocódigo e procedimentos literais vão
  sempre no ambiente `Pseudo` (seção 6.4).
- **Sem página de título**: não use `\maketitle`, `\title`, `\author` nem `\date`. O
  documento começa direto na primeira `\section`.
- Sem sumário, cabeçalho/rodapé personalizados ou bibliografia, a menos que eu peça
  ou forneça referências.
- Tabelas e figuras **nunca flutuam**: sempre `[H]`, para ficarem exatamente onde aparecem
  no texto.

---

## 3. Preâmbulo obrigatório (use exatamente este)

```latex
\documentclass[11pt]{article}

% --- Codificacao e idioma ---
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage[brazil]{babel}

% --- Tipografia moderna ---
\usepackage{lmodern}
\usepackage{microtype}

% --- Margens reduzidas (texto ocupa mais a pagina) ---
\usepackage[a4paper,margin=1.8cm,top=2cm,bottom=2cm]{geometry}

% --- Matematica ---
\usepackage{amsmath,amssymb}

% --- Tabelas ---
\usepackage{booktabs}
\usepackage{array}
\usepackage{ragged2e}
\usepackage{colortbl}
\usepackage{float}

% --- Blocos de codigo / pseudocodigo ---
\usepackage{fancyvrb}
\usepackage{xcolor}

% --- Caixas estilizadas para exemplos ---
\usepackage[most]{tcolorbox}

% --- Graficos ---
\usepackage{graphicx}

% --- Titulos de secao estilizados ---
\usepackage{titlesec}

% --- Paleta ---
\definecolor{corPrincipal}{HTML}{2C5F8A}    % azul petroleo
\definecolor{corSecundaria}{HTML}{4A4A4A}   % cinza escuro
\definecolor{corCodigoBg}{HTML}{F4F6F8}     % cinza muito claro
\definecolor{corCodigoBorda}{HTML}{D6DCE3}  % borda suave
\definecolor{corTabelaHeader}{HTML}{E8EEF4} % fundo do cabecalho de tabela

\usepackage[hidelinks,colorlinks=true,linkcolor=corPrincipal,urlcolor=corPrincipal]{hyperref}

% Espacamento entre linhas um pouco mais arejado
\usepackage{setspace}
\setstretch{1.08}

% Coluna de paragrafo justificada a esquerda para tabelas
\newcolumntype{L}[1]{>{\RaggedRight\arraybackslash}p{#1}}

% Estilo dos titulos: barra colorida e fonte sem serifa
\titleformat{\section}
  {\normalfont\sffamily\Large\bfseries\color{corPrincipal}}
  {\thesection}{0.6em}{}[{\color{corPrincipal!30}\titlerule[1pt]}]
\titleformat{\subsection}
  {\normalfont\sffamily\large\bfseries\color{corSecundaria}}
  {\thesubsection}{0.6em}{}
\titlespacing*{\section}{0pt}{1.4ex plus 1ex minus .2ex}{0.9ex}

% Blocos de codigo/pseudocodigo: borda suave cinza-clara
\fvset{fontsize=\small,xleftmargin=6pt,xrightmargin=6pt}
\DefineVerbatimEnvironment{Pseudo}{Verbatim}%
  {frame=single,framesep=8pt,rulecolor=\color{corCodigoBorda},%
   framerule=0.6pt,fontsize=\small,formatcom=\color{corSecundaria}}

% Cores das caixas de exemplo
\definecolor{corExemploBorda}{HTML}{2C5F8A}
\definecolor{corExemploBg}{HTML}{F0F5FA}
\definecolor{corExemploTitulo}{HTML}{1F4666}

% Caixa estilizada para "Exemplo / Questao da Lista"
\newtcolorbox{exemplo}[1][Exemplo Prático]{%
  breakable,
  enhanced,
  colback=corExemploBg,
  colframe=corExemploBorda,
  boxrule=0.6pt,
  arc=3pt,
  left=8pt, right=8pt, top=6pt, bottom=6pt,
  fonttitle=\sffamily\bfseries\color{white},
  coltitle=white,
  colbacktitle=corExemploTitulo,
  attach boxed title to top left={xshift=8pt, yshift=-2pt},
  boxed title style={arc=2pt, boxrule=0pt},
  title={#1}
}
```

- Os comentários do preâmbulo ficam **sem acento** (segurança de codificação); o texto do
  documento fica **com acento**.
- Se o tema não usar matemática, código ou figuras, mantenha o preâmbulo inteiro mesmo
  assim (ele é a identidade visual do documento).

---

## 4. Identidade visual (resumo do layout)

- **Página:** A4, fonte 11pt, margens laterais de 1,8 cm e topo/base de 2 cm. O texto
  ocupa quase toda a largura da página, sem bordas largas.
- **Tipografia:** Latin Modern no corpo (com serifa), `microtype` ligado, entrelinha 1,08.
  Títulos **sem serifa e em negrito**.
- **Seções:** azul-petróleo (`corPrincipal`), tamanho `\Large`, com uma **régua colorida
  clara (30% da cor principal) abaixo do título**.
- **Subseções:** cinza escuro (`corSecundaria`), tamanho `\large`, sem régua.
- **Cores:** só a paleta definida (azul-petróleo, azul-escuro do título das caixas,
  azul-claro do fundo, cinzas), mais as cores dos blocos opcionais
  (`bloco-passo-a-passo.tex`: verde-petróleo, laranja, azul, vermelho; `bloco-lista-treino.tex`:
  verde, âmbar e vermelho dos níveis). Não introduza outras cores sem motivo.
- **Links/referências cruzadas:** coloridos na cor principal, sem caixas.
- **Caixas de exemplo:** fundo azul bem claro, borda azul fina, cantos arredondados e
  título em "etiqueta" azul-escuro com texto branco, encaixada no canto superior esquerdo.
  Podem quebrar entre páginas.
- **Blocos de código:** moldura cinza-clara fina, texto em cinza escuro, fonte pequena.
- **Tabelas:** estilo `booktabs`, sem linhas verticais, cabeçalho com fundo azul-claro.

---

## 5. Estrutura do corpo do documento

### 5.1 Hierarquia de títulos
- `\section{...}` — grandes temas, numeradas automaticamente. O título **lista os
  conceitos principais da seção**, separados por vírgula e "e"
  (padrão: `\section{Conceito A, Conceito B e Conceito C}`).
- `\subsection{...}` (numerada) — blocos de um tema grande que tem várias partes.
- `\subsection*{...}` (sem número) — tópicos curtos, muitas vezes em forma de pergunta
  (padrão: `\subsection*{Como fazer X usando Y?}`).
- `\subsubsection*{...}` (sem número, com numeração manual no próprio título) — variantes
  de um mesmo conceito ou procedimento (padrão: `\subsubsection*{1. X no caso A}` e
  `\subsubsection*{2. X no caso B}`).
- Siglas ou nomes estrangeiros no título vão entre parênteses, com en-dash para expandir
  a sigla: `\subsection{Nome em Português (SIGLA -- Nome Original)}`.

### 5.2 Ordem e organização do conteúdo
- Progressão **do geral ao específico e do básico ao avançado**:
  1. introdução/motivação (por que estudar, aplicações, contexto histórico, se houver);
  2. definições fundamentais e classificações/tipos;
  3. propriedades, relações e fórmulas;
  4. comparações (tabelas);
  5. métodos, procedimentos ou algoritmos;
  6. tópicos avançados, métricas ou aplicações.
- Inclua uma seção de **"Fórmulas Fundamentais"** (ou "Regras Fundamentais", se o tema não
  for quantitativo) que reúna em lista todas as fórmulas/regras importantes já vistas, para
  consulta rápida.
- **Conceitos relacionados ficam agrupados** na mesma seção, com subseções, em vez de
  espalhados por partes diferentes do documento.
- Se uma seção começa com tabela ou figura, escreva antes **uma frase introdutória**
  (ex.: "A Tabela~\ref{tab:x} resume ..."), para o título nunca ficar sozinho.
- Um parágrafo introdutório curto no começo de uma seção é opcional e só entra quando
  contextualiza (ex.: "Nesta seção, explicamos ...").

### 5.3 Formato padrão das definições (listas)
O corpo das seções é feito principalmente de **listas `itemize` de definições**:

```latex
\begin{itemize}
  \item \textbf{Nome do Conceito ($notação$, se houver):} Definição objetiva em uma
        ou duas frases. Consequência, propriedade ou ressalva importante.
    \begin{itemize}
      \item \textbf{Subconceito:} Definição do subconceito.
      \item \emph{Exemplo Prático (Nome do Conceito):} Analogia ou caso do mundo real.
    \end{itemize}
  \item \textbf{Próximo Conceito:} ...
\end{itemize}
```

Regras:
- O **nome do termo vai em negrito e termina com dois-pontos**.
- Notação, sigla ou símbolo do conceito entram **no próprio rótulo**, entre parênteses.
- Palavras decisivas dentro da definição vão em `\textbf{}` (ex.: \textbf{não},
  \textbf{todo}, \textbf{obrigatoriamente}), com moderação.
- Termos estrangeiros e expressões técnicas citadas vão em `\emph{}`.
- Até **3 níveis** de lista aninhada.
- Critérios, condições, etapas e passos numerados usam `enumerate` dentro do item.
- No código-fonte, as linhas de continuação de um `\item` ficam alinhadas com o início do
  texto do item.
- Prosa corrida (parágrafos) só para introduções, transições, observações e explicações
  que não cabem bem em lista.

---

## 6. Elementos especiais

### 6.1 Exemplos práticos curtos (dentro da lista)
Analogias do cotidiano aparecem como subitem em itálico, logo abaixo do conceito:

```latex
\item \emph{Exemplo Prático (Nome do Conceito):} Situação concreta e familiar que
      mostra o conceito funcionando, com os elementos destacados em \textbf{negrito}.
```

- Use contextos **concretos e conhecidos** (tecnologia, redes sociais, bancos, transporte,
  empresas, cotidiano acadêmico), adequados ao tema.
- No máximo um exemplo curto por conceito; exemplos longos vão para a caixa `exemplo`.

### 6.2 Caixas `exemplo` (questões e exemplos resolvidos)
Use a caixa para **questões da lista de exercícios** e para **exemplos resolvidos passo a
passo**. Ela fica **logo depois** do conceito que aplica; se estiver no meio de uma lista,
feche o `itemize`, coloque a caixa e reabra a lista depois.

**Títulos (siga à risca):**
- Uma questão da lista: `[Questão da Lista --- Assunto]`
- Várias questões: `[Questões da Lista --- Assunto]`
- Exemplo didático que não veio da lista: `[Exemplo --- Assunto]`
- **NUNCA** escreva "Questão de Prova". O separador é sempre `---` (travessão).

**Modelo A — questão única dissertativa/conceitual:**
```latex
\begin{exemplo}[Questão da Lista --- Assunto]
\emph{Enunciado da questão, em itálico, reproduzido fielmente.}

\textbf{Resposta:} Explicação conceitual em prosa, com a justificativa teórica.

\textbf{Exemplo:} Caso concreto que ilustra a resposta, com dados/notação específicos
e o porquê de ele satisfazer o que foi pedido.
\end{exemplo}
```

**Modelo B — várias questões curtas/numéricas:**
```latex
\begin{exemplo}[Questões da Lista --- Assunto]
\textbf{Q1.} \emph{Enunciado em itálico?}\\
\textbf{Resposta:} Regra/fórmula usada, desenvolvimento passo a passo encadeado
($\Rightarrow$) e resultado final com unidade ou conclusão.

\medskip
\textbf{Q2.} \emph{Próximo enunciado.}\\
\textbf{Resposta:} ...
\end{exemplo}
```

**Modelo C — questão com vários itens (a, b, c...):**
```latex
\begin{exemplo}[Questão da Lista --- Assunto]
\emph{Contexto/dado comum a todos os itens:}
\begin{center}
\texttt{dados, expressão ou estrutura do enunciado}
\end{center}
\emph{Determine o que se pede:}

\medskip
\textbf{a) Texto do item:}\\
Lembrete curto da regra aplicada. Raciocínio. Resposta final em \textbf{negrito}.

\medskip
\textbf{b) Próximo item:}\\
...
\end{exemplo}
```

Regras das caixas:
- Enunciado **sempre em `\emph{}`**; rótulos **`\textbf{Resposta:}`**,
  **`\textbf{Exemplo:}`** e **`\textbf{Q1.}`** em negrito.
- Itens/questões separados por `\medskip`; quebra de linha após o rótulo com `\\`.
- Toda resposta **relembra a regra ou definição** antes de aplicar e **termina com o
  resultado em negrito**.
- Dados longos, expressões e listas de valores ficam centralizados em
  `\begin{center}...\end{center}`.
- Se o enunciado tiver erro, ambiguidade ou mais de uma resposta possível, diga isso na
  resposta com naturalidade (ex.: "Existem outras soluções possíveis, dependendo de ...").
- Não deixe correções rascunhadas, tentativas descartadas ou "(adaptação)" na resposta:
  apresente só o raciocínio final e limpo.

### 6.3 Avisos e observações
- Alerta importante dentro de uma definição: `\textbf{Atenção:} ...`.
- Nota depois de um bloco (código, tabela, procedimento): `\noindent Observação: ...`.

### 6.4 Código, pseudocódigo e procedimentos literais
- Sempre precedidos de um rótulo em negrito sem recuo:
  `\noindent\textbf{Pseudocódigo (Variante):}` ou `\noindent\textbf{Algoritmo:}`.
- Ficam dentro de `\begin{Pseudo} ... \end{Pseudo}`.
- **Dentro do bloco, sem acentos** (ambiente literal); fora dele, acentuação normal.
- Pseudocódigo em **português**, palavras-chave em minúsculo (`para todo ... faca`,
  `se ... entao`, `senao`, `enquanto ... efetuar`, `retornar`), atribuição com `<-`.
- Passos numerados hierarquicamente (`1.`, `2.`, `a.`, `b.`, `i.`, `ii.`), indentação de
  4 espaços e comentários `//` explicando as linhas importantes.
- Código real de linguagem de programação também vai em `Pseudo`, mantendo a sintaxe
  original.

### 6.5 Tabelas
Use tabelas para **comparações** (conceitos lado a lado, vantagens × desvantagens,
características × categorias):

```latex
A Tabela~\ref{tab:rotulo} resume as diferenças entre ...

\begin{table}[H]
  \centering
  \caption{Comparação entre ...}
  \label{tab:rotulo}
  \small
  \begin{tabular}{L{3cm} L{3cm} L{9cm}}
    \toprule
    \rowcolor{corTabelaHeader}
    \textbf{Coluna 1} & \textbf{Coluna 2} & \textbf{Coluna 3} \\
    \midrule
    \textbf{Item 1} & ... & ... \\
    \addlinespace
    \textbf{Item 2} & ... & ... \\
    \bottomrule
  \end{tabular}
\end{table}
```

- **Sempre `[H]`**; `\caption` **acima** da tabela; `\label` com prefixo `tab:`.
- Sem linhas verticais; só `\toprule`, `\midrule`, `\bottomrule` e `\addlinespace` entre
  as linhas.
- Cabeçalho com `\rowcolor{corTabelaHeader}` e texto em negrito; **primeira coluna em
  negrito**.
- Colunas `L{...}` (texto quebrando, alinhado à esquerda) somando **cerca de 15–16 cm**,
  para usar a largura toda; fonte `\small`.
- Toda tabela é citada no texto com `Tabela~\ref{...}`.

### 6.6 Figuras
```latex
\begin{figure}[H]
  \centering
  \includegraphics[width=0.9\textwidth]{figures/<prefixo>-NN-<descricao>.png}
  \caption{Frase completa explicando o que a figura demonstra e qual conclusão
           o leitor deve tirar dela.}
  \label{fig:<descricao>}
\end{figure}
```

- **Sempre `[H]`**, `\centering`, largura entre `0.9\textwidth` e `0.95\textwidth`
  (menor só se a imagem ficar desproporcional).
- Imagens na pasta `figures/`, nomeadas `prefixo-da-secao-NN-descricao.png`
  (prefixo curto do tema da seção + número de 2 dígitos + descrição em kebab-case, sem
  acentos).
- `\caption` **abaixo**, como **frase descritiva completa** (não só um título), usando a
  mesma notação do texto.
- `\label` semântico com prefixo `fig:`.
- **Não invente `\includegraphics`** para imagens que não foram fornecidas: nesse caso,
  descreva o exemplo em texto, tabela ou notação.

### 6.7 REGRA DE OURO — conceito → imagem
**Cada figura vem imediatamente depois do conceito que ilustra.** Nunca explique vários
conceitos em sequência e deixe as figuras todas para o fim. Dentro de listas:

```latex
\begin{itemize}
  \item \textbf{Conceito A:} ...
\end{itemize}

\begin{figure}[H] ... figura de A ... \end{figure}

\begin{itemize}
  \item \textbf{Conceito B:} ...
\end{itemize}

\begin{figure}[H] ... figura de B ... \end{figure}
```

Exceção: duas ou mais figuras do **mesmo** conceito (ex.: "antes" e "depois") podem ficar
seguidas. A mesma lógica vale para caixas `exemplo`: a questão fica junto do conceito, não
acumulada no fim da seção.

### 6.8 Caixas "Execução Passo a Passo" (algoritmos, procedimentos, cálculos)
Quando o tema tem algoritmos ou procedimentos com estado que muda (tabelas, matrizes,
conjuntos, valores acumulados), cada um ganha uma ou mais caixas `passoapasso`, **logo
depois** do pseudocódigo ou da figura do método. O código do ambiente, das cores, dos
estilos TikZ e das macros está em `template/bloco-passo-a-passo.tex`: cole-o no preâmbulo,
depois da caixa `exemplo`.

**Visual (distinto das caixas de questão):** faixa de título verde-petróleo com
"▶ Execução Passo a Passo --- Nome", barra lateral grossa e cantos retos. As caixas
`exemplo` continuam azuis e arredondadas.

**Layout de cada etapa — o estado fica AO LADO do desenho:**
```latex
\begin{passoapasso}[Nome do Algoritmo]
Uma frase de introdução (o que o caso mostra).

\etapa{Etapa 1 --- o que acontece}{<desenho TikZ>}{<tabela/estado desta etapa + 1-2 linhas>}
\etapa{Etapa 2}{...}{...}
% \etapaLarga{...}{...}{...} quando o desenho precisa de mais largura (55% / 43%)

\textbf{Resultado: ...}
\end{passoapasso}
```
- Cada etapa é uma linha: **desenho à esquerda, tabela ou estado daquela etapa à
  direita**. Nunca desenhe todos os passos e deixe uma tabela grande só no fim: o leitor
  não deve precisar rolar para conferir os valores.
- **Pouco texto:** uma frase de introdução, 1 a 2 linhas por etapa, resultado em negrito.
- **Cores com significado fixo:** laranja (`corAtual`, `\novo{}`) = examinado ou alterado
  no passo; azul (`corArvore`, `\fixo{}`) = escolhido ou definitivo; vermelho tracejado
  (`arRej`) = rejeitado ou problema; cinza (`ar`) = ainda não usado. Coloque a legenda
  (`\legendaPasso` ou equivalente) uma vez, antes da primeira caixa ou na primeira caixa.
- **Siga o formato dos slides/materiais da disciplina.** Antes de montar as caixas, abra os
  exemplos dos slides e copie a forma de mostrar o método (quais conjuntos, quais linhas
  da tabela, como as contas são escritas). Não invente um formato próprio.
- **Contas explícitas, como no material:** escreva a soma e a comparação
  (`dist-est[b] = dist[c] + d_{cb} = 2 + 1 = 3`, `3 + 2 = 5 < 6`). **Não use
  `\min(\ldots)`** para exemplificar, a menos que a própria definição do método seja uma
  fórmula com mínimo (aí ela aparece só na fórmula).
- **Elementos escritos direto:** arestas como $(a,b)$ ou $\{a,b\}$, itens pelo próprio nome.
  Nunca crie rótulos intermediários do tipo $e_1, e_2$ que obrigam o leitor a consultar uma
  legenda.
- **Vários casos por método:** o caso normal e, quando existir, o caso-limite que o
  material destaca (ex.: detecção de ciclo negativo, empates, falha do método). Não inclua
  um caso que o usuário mandou tirar ou que o material não cobre.
- **Exemplos novos:** os dados das caixas são diferentes dos slides e da lista, para servir
  de treino extra.
- **Números conferidos por script:** todo valor de tabela vem de um script (ex.: Python)
  que executa o método; nunca escreva estados calculados de cabeça sem conferir.
- Caixas longas podem quebrar entre páginas (`breakable`). Se uma caixa começar no fim de
  uma página, avise o usuário e ofereça forçar `\clearpage` antes dela.

---

## 7. Notação e convenções tipográficas

- Toda expressão matemática, variável ou símbolo em modo matemático, inclusive dentro do
  texto (`$n$`, `$x_i$`).
- Cardinalidade/módulo com `\lvert ... \rvert` (nunca `|...|` cru nem `\vert{}`).
- Conjunto vazio com `\varnothing`; conjuntos com `\{ ... \}`; pares ordenados com `( ... )`.
- Frações com `\frac{}{}` e espaço em volta dos operadores (`n - 1`, não `n-1`).
- Desenvolvimentos encadeados com `\Rightarrow`; desigualdades com `\le` / `\ge`.
- Fórmula curta fica inline na definição; fórmula de destaque vai em `equation*` ou
  centralizada.
- Complexidade/ordem de grandeza (se o tema tiver): `$O(\cdot)$`.
- Mantenha **uma notação única** para cada conceito no documento inteiro: a do rótulo da
  definição é a que aparece nos exemplos, tabelas e legendas.
- Aspas com ``assim'' (padrão LaTeX); travessão `---` para apostos; en-dash `--` para
  intervalos e sequências (`1 -- 2 -- 3`).
- Abreviações: `ex.:`, `vs.\ `, `i.e.` usados de forma consistente.

---

## 8. Tom e estilo de escrita

- **Didático, claro e direto**, como o resumo de um aluno que domina a matéria: rigoroso
  nas definições e sempre com uma explicação intuitiva logo depois.
- Voz **impessoal ou na 1ª pessoa do plural** ("Nesta seção, explicamos...",
  "verificamos...", "podemos concluir que..."). **Nunca** fale com o leitor por "você".
- Frases **curtas e objetivas**; cada definição em 1 a 3 frases.
- Padrão de explicação: **definição formal → reformulação intuitiva**, com conectores
  como "ou seja", "isto é", "ou ainda", "logo", "portanto", "dessa forma".
  Padrão: "X é dito Y quando ... --- ou seja, quando ...".
- Definições formais abertas com "Dado(a) ..., ...", "Diz-se que ...", "É dito ... quando ...".
- **Explique o porquê** das regras, não só o quê (a consequência lógica ou a motivação).
- Sempre que o conceito tiver variantes, deixe o contraste explícito: geral × particular,
  necessário × suficiente, mínimo × máximo, caso A × caso B.
- Para métodos e estruturas, cite **vantagens, desvantagens e custos** quando fizer sentido.
- Linguagem técnica correta, sem gírias, mas acessível; termos estrangeiros só quando são
  o nome consagrado, e em `\emph{}`.
- **Sem floreios**: sem frases de abertura/encerramento ("Neste resumo veremos...",
  "Espero que ajude"), sem emojis, sem metacomentários.
- **Sem marcadores de rascunho** no texto final: remova `[cite: ...]`, `(Adicionado)`,
  `(Adaptação)`, notas de revisão, TODOs e afins.
- **Explicação simples antes de completa:** quando o usuário diz que não entendeu um
  assunto, prefira menos texto e mais desenho. Uma ideia por frase, analogia concreta
  (ex.: canos de água para fluxo), e os termos técnicos apresentados um de cada vez, cada
  um com a sua pergunta intuitiva ("quanto ainda posso mexer?").
- **Fiel ao material:** a forma de apresentar métodos, a notação e os nomes seguem os
  slides da disciplina. Conteúdo que não está nos slides (complexidade, variantes) pode
  entrar, mas é dito como complemento e informado ao usuário na entrega.
- **Teoria completa, não só a lista:** um resumo cobre a teoria dos slides de cada tópico,
  com peso nos fundamentos e nas comparações; a lista de exercícios complementa, não
  define o conteúdo.

---

## 9. Regras para quando eu pedir alterações depois

- **Edições cirúrgicas**: acrescente ou mude só o necessário, na seção certa, preservando
  o preâmbulo, a estilização e a estrutura já existentes. Nunca reescreva o documento do
  zero.
- Conteúdo novo **soma** ao existente; não remova nada que eu não tenha mandado remover,
  mesmo que o material novo venha mais resumido.
- Conteúdo novo segue os mesmos padrões deste prompt (listas, caixas, figuras
  conceito → imagem, notação, tom).

---

## 10. Checklist final antes de entregar

1. Preâmbulo idêntico ao da seção 3; nenhum `\maketitle`/título/data.
2. Hierarquia de títulos conforme 5.1; seções em ordem lógica; conceitos relacionados
   agrupados.
3. Definições no formato `\textbf{Termo (notação):}` + explicação intuitiva.
4. Toda figura e toda caixa de exemplo logo depois do conceito correspondente.
5. Tabelas e figuras com `[H]`, `\caption` e `\label`; tabelas citadas no texto.
6. Caixas com títulos "Questão da Lista --- ..." / "Questões da Lista --- ..." /
   "Exemplo --- ...", nunca "Questão de Prova"; enunciado em itálico; resultado em negrito.
7. Blocos `Pseudo` sem acentos e com rótulo antes.
8. Notação consistente; nenhum `|` cru; nenhum marcador de rascunho.
9. Listas bem abertas e fechadas (nenhum `\item` solto depois de reabrir uma lista).
10. Compila sem erros em pdfLaTeX e em Tectonic.
11. Caixas `passoapasso` (se houver): estado ao lado de cada etapa, formato dos slides, sem
    `\min` nas contas, elementos escritos direto, números conferidos por script.
12. Páginas renderizadas conferidas visualmente (nada sobreposto, nada saindo das caixas).

---

## 11. Lista de exercícios de treino (documento separado)

Quando o usuário pede uma lista de questões para treinar a partir de um resumo:

### 11.1 Estrutura
- **Novo documento/projeto** (ex.: `lista-treino-<tema>`); o resumo **não é alterado**.
- Mesmo preâmbulo do resumo + `template/bloco-passo-a-passo.tex` (cores e TikZ) +
  `template/bloco-lista-treino.tex` (etiquetas, `questao`, `alternativas`, `\gabarito`,
  `resposta`).
- Abre com um cabeçalho simples (sem `\maketitle`): título em `\sffamily\Large` azul,
  régua clara e um parágrafo explicando as etiquetas de nível e onde fica o gabarito.
- **As seções são as mesmas do resumo, na mesma ordem**, começando pela revisão/
  nivelamento quando o resumo tiver uma. Uma seção final pode misturar os tópicos.
- Cada seção: frase curta dizendo o que ela treina → questões → `\gabarito` (página nova)
  → respostas.

### 11.2 Questões
```latex
\begin{questao}{1.3}{\medio}   % \facil | \medio | \dificil
\emph{Enunciado em itálico.}
\begin{alternativas}
  \item ...
\end{alternativas}
\end{questao}
```
- **Etiqueta de nível em toda questão:** FÁCIL (verde), MÉDIO (âmbar), DIFÍCIL
  (vermelho), no canto do título. Dentro de cada seção, da mais fácil para a mais difícil.
- De 8 a 10 questões por seção, distribuídas mais ou menos 3 / 4 / 2–3.
- Tipos variados: múltipla escolha (só uma correta), verdadeiro ou falso com
  justificativa, resposta curta, execução de método em dados novos, prova/justificativa
  curta.
- **Dados novos:** grafos, tabelas e números diferentes dos do resumo, dos slides e da
  lista original; figuras desenhadas em TikZ no próprio enunciado.
- **Não peça para implementar código**, a menos que o usuário peça.
- Numeração `seção.questão` (1.1, 1.2, ...).

### 11.3 Gabarito
```latex
\gabarito
\begin{resposta}{1.3}
\textbf{Resposta:} (c) ...\\
\textbf{Explicação:} relembra a regra usada e mostra o raciocínio em poucas linhas.
\end{resposta}
```
- Fica **no fim de cada seção, em página nova**, para as respostas não ficarem à vista
  durante a resolução.
- Resultado em negrito + explicação curta que relembra a regra. Execuções de método seguem
  o mesmo formato das caixas passo a passo (seção 6.8): contas explícitas, sem `\min`,
  elementos escritos direto.
- **Toda resposta numérica ou de execução é conferida por script**; nas de múltipla
  escolha, o script confirma que só uma alternativa está certa. Guarde os scripts em
  `verificacao/` dentro do projeto.

---

## 12. Resolução detalhada de uma questão (documento de explicação)

Quando o usuário manda uma questão e diz que não entendeu o assunto:

- **Novo documento/projeto** (ex.: `<assunto>-q<numero>`), com o preâmbulo do resumo +
  `template/bloco-passo-a-passo.tex` + os estilos extras que o tema pedir (definidos logo
  depois, com o mesmo padrão de nomes e cores).
- **Seção 1 — a ideia:** só os conceitos de que a questão precisa, do mais simples ao mais
  técnico, cada um com uma analogia e, se possível, um mini-desenho de um caso mínimo
  (ex.: uma aresta só). Depois o pseudocódigo, uma tabela comparando métodos parecidos
  (quando a questão pede mais de um) e o teorema ou regra usado para conferir o resultado.
  Termine com "como ler os desenhos" e a legenda de cores.
- **Uma seção por item da questão:** caixa `exemplo` com o enunciado e a figura original,
  os dados limpos (ex.: só as capacidades, sem valores prontos da figura), uma caixa
  `passoapasso` por método com uma `\etapa`/`\etapaLarga` por iteração (desenho do estado
  à esquerda; contas, resultado parcial e tabela à direita, com o que mudou em laranja),
  e uma subseção de conferência do resultado.
- **Destaque o ponto que costuma confundir** numa nota curta na etapa em que ele aparece
  (ex.: "Seta para trás em uso: ...").
- Termine com uma **"Receita para a Prova"**: os passos numerados e o erro mais comum.
- Os desenhos e as tabelas das iterações são **gerados por um script** que executa o
  método, confere cada passo com o esperado e escreve o LaTeX; guarde-o em `verificacao/`.
  Se o usuário editar o `main.tex` à mão depois, não rode o gerador de novo sem avisar.

---

