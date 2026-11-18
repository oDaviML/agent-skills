# 🧠 Agent Skills — LaTeX Study Summaries & Product Grill (Linear)

Repositório público de **Agent Skills** compatível com o ecossistema [skills.sh](https://skills.sh) (`npx skills`) e com as principais CLIs e IDEs de agentes de IA (**Google Antigravity**, **Kiro CLI**, **Claude Code**, **OpenAI Codex**, **Gemini CLI**, **Cursor**, **GitHub Copilot**, **Windsurf**, **OpenCode**, entre outros).

---

## 📦 Skills Incluídas

| Skill | Descrição | Principais Gatilhos |
| :--- | :--- | :--- |
| **[`resumo-estudos`](./skills/resumo-estudos/)** | Gera e atualiza materiais de estudo em **LaTeX (pt-BR)** seguindo um padrão editorial e visual rigoroso: resumos completos para prova, listas de exercícios de treino com etiqueta de dificuldade (`\facil`, `\medio`, `\dificil`) e gabarito por seção, e resoluções detalhadas passo a passo com diagramas TikZ e conferência de cálculos via script Python. | `"resumo de estudos"`, `"resumo em latex"`, `"lista de treino"`, `"resolver questão passo a passo"` |
| **[`grilling-produto`](./skills/grilling-produto/)** | Conduz uma entrevista estruturada em árvore (rodadas de até 4 perguntas de múltipla escolha com recomendações e sugestões proativas de funcionalidades 💡) até definir o produto nos mínimos detalhes (visão, personas, casos de uso, RF/RNF, modelo de dados, stack, telas). Ao final, gera a especificação local + ADRs e publica **projeto, documentos, milestones, épicos e cards com dependências no Linear** via MCP. | `"grill produto"`, `"definir produto"`, `"montar prd"`, `"me sabatine sobre o produto"`, `"backlog no linear"` |
| **[`grill-produto`](./skills/grill-produto/)** | Atalho/alias direto de invocação manual (`/grill-produto`) que carrega e executa a skill `grilling-produto`. | `/grill-produto` |

---

## 🚀 Instalação Rápida via `skills.sh` (`npx skills`)

A forma mais rápida de instalar em qualquer agente é utilizando a CLI oficial do [skills.sh](https://skills.sh):

### 1. Instalação interativa (detecta suas CLIs instaladas)

```bash
npx skills add oDaviML/agent-skills
```

### 2. Instalar globalmente em todas as CLIs detectadas de uma só vez

```bash
npx skills add oDaviML/agent-skills -g --all
```

### 3. Instalar apenas uma skill específica

```bash
# Apenas o gerador de resumos/listas em LaTeX
npx skills add oDaviML/agent-skills --skill resumo-estudos -g

# Apenas o entrevistador de produto + Linear
npx skills add oDaviML/agent-skills --skill grilling-produto --skill grill-produto -g
```

---

## 🖥️ Instalação por CLI / Agente (`npx skills`)

Use a flag `-a` (`--agent`) com `-g` (global) ou sem `-g` (apenas no projeto atual):

| Agente / CLI | Comando `npx skills` (Global) | Diretório Global | Diretório no Projeto |
| :--- | :--- | :--- | :--- |
| **Google Antigravity** | `npx skills add oDaviML/agent-skills -g -a antigravity -y` | `~/.gemini/config/skills/` | `.agents/skills/` |
| **Kiro CLI** | `npx skills add oDaviML/agent-skills -g -a kiro-cli -y` | `~/.kiro/skills/` | `.kiro/skills/` |
| **Claude Code** | `npx skills add oDaviML/agent-skills -g -a claude-code -y` | `~/.claude/skills/` | `.claude/skills/` |
| **OpenAI Codex** | `npx skills add oDaviML/agent-skills -g -a codex -y` | `~/.agents/skills/` | `.agents/skills/` |
| **Gemini CLI** | `npx skills add oDaviML/agent-skills -g -a gemini-cli -y` | `~/.gemini/skills/` | `.agents/skills/` |
| **Cursor** | `npx skills add oDaviML/agent-skills -g -a cursor -y` | `~/.cursor/skills/` | `.agents/skills/` |
| **GitHub Copilot** | `npx skills add oDaviML/agent-skills -g -a github-copilot -y` | `~/.copilot/skills/` | `.agents/skills/` |
| **Windsurf** | `npx skills add oDaviML/agent-skills -g -a windsurf -y` | `~/.codeium/windsurf/skills/` | `.windsurf/skills/` |
| **OpenCode** | `npx skills add oDaviML/agent-skills -g -a opencode -y` | `~/.config/opencode/skills/` | `.agents/skills/` |
| **Cline / Roo Code** | `npx skills add oDaviML/agent-skills -g -a cline -a roo -y` | `~/.agents/skills/` / `~/.roo/skills/` | `.agents/skills/` / `.roo/skills/` |
| **Trae** | `npx skills add oDaviML/agent-skills -g -a trae -y` | `~/.trae/skills/` | `.trae/skills/` |

> 💡 **Dica:** Se preferir copiar os arquivos em vez de criar symlinks, adicione `--copy` ao comando `npx skills add`.

### Comandos úteis do `npx skills`

```bash
# Listar todas as skills disponíveis neste repositório sem instalar
npx skills add oDaviML/agent-skills --list

# Verificar skills instaladas
npx skills list -g

# Atualizar todas as skills instaladas para a versão mais recente
npx skills update
```

---

## 🔌 Instalação como Plugin Nativo ou Manual

Além do `npx skills`, este repositório inclui manifestos nativos (`plugin.json` e `.claude-plugin/marketplace.json`) para funcionar como **Plugin** no Google Antigravity e no Claude Code:

### Google Antigravity (como Plugin ou Skills Globais)

**Opção A — Via `npx skills` (Skills Globais):**
```bash
npx skills add oDaviML/agent-skills -g -a antigravity -y
```

**Opção B — Como Plugin Completo (`~/.gemini/config/plugins/`):**
```bash
git clone https://github.com/oDaviML/agent-skills.git ~/.gemini/config/plugins/agent-skills
```
O Antigravity descobre automaticamente o `plugin.json` e ativa as skills na próxima mensagem (podendo ser gerenciado na aba **Customizations → Plugins**).

### Claude Code (via Plugin Marketplace ou Skills Globais)

```bash
# Via npx skills
npx skills add oDaviML/agent-skills -g -a claude-code -y

# Ou clonando diretamente em ~/.claude/skills
git clone https://github.com/oDaviML/agent-skills.git /tmp/agent-skills && \
  cp -r /tmp/agent-skills/skills/* ~/.claude/skills/
```

### Kiro CLI / Kiro Crew

```bash
# Para Kiro CLI (~/.kiro/skills/)
npx skills add oDaviML/agent-skills -g -a kiro-cli -y

# Para Kiro Crew (~/.kiro/crew/skills/)
git clone https://github.com/oDaviML/agent-skills.git /tmp/agent-skills && \
  cp -r /tmp/agent-skills/skills/* ~/.kiro/crew/skills/
```

---

## 🛠️ Pré-requisitos Recomendados

### Para `resumo-estudos` (LaTeX)
- **Compilador LaTeX:** `pdflatex` (TeX Live com pacotes padrão: `tcolorbox`, `tikz`, `booktabs`, `babel-portuges`, `fancyvrb`, `titlesec`) ou [`tectonic`](https://tectonic-typesetting.github.io/).
- **Poppler Utils (`pdftoppm`):** para renderizar páginas do PDF gerado ou extrair figuras de slides para inspeção visual.
- **Python 3:** usado pelo agente para rodar scripts de verificação matemática/algorítmica antes de preencher tabelas e gabaritos no `.tex`.

### Para `grilling-produto` / `grill-produto` (Produto & Linear)
- **Servidor MCP do Linear (opcional para a etapa C):** para publicar automaticamente o projeto, documentos, milestones, épicos e sub-issues no Linear após a aprovação da especificação.
  - No **Google Antigravity**, você pode instalar o plugin oficial do Linear pela aba **Plugins** (`antigravity-plugins-partners/plugin/linear`).
  - No **Claude Code / Cursor / Kiro / Codex**, configure o MCP oficial do Linear (`https://mcp.linear.app/mcp` via `mcp-remote` ou HTTP).
  - Caso o MCP do Linear não esteja conectado, a skill ainda realiza toda a entrevista, gera a especificação completa (`especificacao.md`), os ADRs (`adr/`) e o backlog estruturado local (`backlog.md`).

---

## 📂 Estrutura do Repositório

```text
agent-skills/
├── README.md                          # Documentação e guia de instalação multi-CLI
├── LICENSE                            # Licença MIT
├── plugin.json                        # Manifesto de Plugin para Google Antigravity
├── .claude-plugin/
│   └── marketplace.json               # Manifesto de Plugin para Claude Code & skills.sh
└── skills/
    ├── resumo-estudos/                # Skill de materiais de estudo em LaTeX
    │   ├── SKILL.md                   # Fluxo principal da skill
    │   ├── guia-de-estilo.md          # Especificação editorial e visual completa
    │   └── template/
    │       ├── main.tex               # Preâmbulo base obrigatório
    │       ├── bloco-passo-a-passo.tex# Macros TikZ e caixas de execução passo a passo
    │       └── bloco-lista-treino.tex # Caixas de questão/resposta, níveis e gabarito
    ├── grilling-produto/              # Skill de entrevista de produto + publicação Linear
    │   ├── SKILL.md                   # Fluxo de entrevista em árvore e sugestões 💡
    │   ├── arvore-de-produto.md       # 17 galhos obrigatórios em 7 níveis de dependência
    │   ├── entrega-linear.md          # Fluxo de especificação, ADRs, backlog e Linear MCP
    │   ├── agents/
    │   │   └── openai.yaml            # Metadados de interface
    │   └── template/
    │       ├── especificacao.md       # Template mestre de PRD/Especificação (17 seções)
    │       └── card-linear.md         # Templates de Épico e Tarefa (Dado/Quando/Então)
    └── grill-produto/                 # Atalho para acionar grilling-produto
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

---

## 📄 Licença

Distribuído sob a licença [MIT](./LICENSE). Criado por [Davi Martins](https://github.com/oDaviML).
