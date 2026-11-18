# Árvore de definição de produto

Galhos que **toda** sessão precisa visitar. A ordem dos níveis é a ordem de dependência: um galho do nível N só entra na fronteira quando os galhos do nível anterior de que ele depende estão resolvidos. Dentro de um nível, os galhos podem ser perguntados na mesma rodada. As perguntas listadas são pontos de partida, não um roteiro — pule o que a ideia base já responde, aprofunde o que for ambíguo e pode o que não se aplicar (com confirmação do usuário).

## Nível 0 — Raiz

1. **Visão e problema**
   - Qual problema concreto o produto resolve? Como as pessoas resolvem hoje e por que isso é ruim?
   - Frase de visão em uma linha ("Para <quem>, que <dor>, o <produto> é um <categoria> que <benefício>").
   - Qual é o objetivo do projeto: produto comercial, ferramenta interna, projeto pessoal/estudo, MVP para validar hipótese?

## Nível 1 — Quem e por quê

2. **Usuários e personas**
   - Tipos de usuário (personas/papéis), o que cada um quer e com que frequência usa.
   - Quem paga × quem usa × quem administra (se diferentes).
   - Nível técnico, dispositivo predominante, contexto de uso (no celular na rua, no desktop no trabalho…).
3. **Objetivos e métricas de sucesso**
   - Como saber que deu certo? Métricas (ativação, retenção, conversão, tempo economizado) e metas.
4. **Concorrentes e alternativas** (pesquise você; pergunte só o diferencial)
   - O que existe, o que copiar, o que evitar, qual é o diferencial.
   - Guarde a lista de funcionalidades de destaque de cada concorrente: ela alimenta as sugestões 💡 do mercado (ver `SKILL.md`).

## Nível 2 — O que o produto faz

5. **Casos de uso / histórias de usuário**
   - Para cada persona: "Como <persona>, quero <ação> para <benefício>".
   - Fluxo principal e fluxos alternativos/de exceção de cada caso de uso relevante.
   - Jornada ponta a ponta do primeiro uso (descoberta → cadastro → primeiro valor).
6. **Escopo e priorização**
   - MVP × versões futuras (MoSCoW: must / should / could / won't).
   - Sugestões 💡 aceitas entram aqui com sua prioridade; recusadas vão para "fora do escopo" com o motivo.
   - O que está explicitamente **fora** do escopo.
7. **Regras de negócio**
   - Restrições, cálculos, estados e transições (ex.: ciclo de vida de um pedido), limites, validações.

## Nível 3 — Requisitos

8. **Requisitos funcionais** (derivados dos casos de uso)
   - Lista numerada RF-01…; cada um verificável e ligado a um caso de uso.
   - Autenticação/contas, papéis e permissões, notificações, busca/filtros, importação/exportação, pagamentos, integrações externas — confirme cada um que se aplicar.
9. **Requisitos não funcionais** (sempre mensuráveis)
   - Desempenho (latência, throughput), escala (usuários, volume de dados, crescimento).
   - Disponibilidade/SLA, backup e recuperação.
   - Segurança e privacidade (LGPD, dados sensíveis, criptografia, auditoria).
   - Usabilidade e acessibilidade (WCAG), idiomas/i18n, fuso/moeda.
   - Plataformas e compatibilidade (web, iOS, Android, desktop; navegadores; offline).
   - Manutenibilidade, observabilidade (logs, métricas, alertas), custo operacional máximo.
10. **Dados**
    - Entidades principais, atributos e relacionamentos (modelo de domínio).
    - Origem dos dados, retenção, exclusão, dados de terceiros.

## Nível 4 — Como

11. **Arquitetura e stack tecnológica** (depende de 8, 9 e 10)
    - Tipo de cliente (web SPA/SSR, mobile nativo/híbrido, desktop, CLI).
    - Frontend, backend, banco de dados, autenticação, armazenamento de arquivos, filas/jobs.
    - Hospedagem/infra, CI/CD, ambientes (dev/stage/prod).
    - Integrações e APIs de terceiros (pesquise limites e custos você mesmo).
    - Restrições do usuário: linguagens que domina, orçamento, time disponível.
12. **Design e identidade visual** (pule se o produto não tiver interface)
    - Tom e estilo, design system/biblioteca de componentes, tema claro/escuro, responsividade, referências visuais.

## Nível 5 — Telas (só se houver interface)

13. **Mapa de telas e navegação** (depende de 5 e 8)
    - Inventário de telas; estrutura de navegação (abas, menu lateral, rotas); quais telas exigem login/papel.
14. **Conteúdo de cada tela** — uma sub-árvore por tela, perguntada em rodadas próprias:
    - Objetivo da tela e casos de uso que atende.
    - Componentes e dados exibidos (de onde vêm), ações disponíveis e para onde levam.
    - Estados: vazio, carregando, erro, sucesso, sem permissão.
    - Validações de formulário e mensagens.
    - Diferenças em mobile × desktop.
    - Opcional: wireframe em texto/ASCII ou widget para validar com o usuário.

## Nível 6 — Fechamento

15. **Operação e negócio**
    - Modelo de monetização/preço (se houver), onboarding, suporte, analytics de produto.
16. **Riscos, premissas e questões em aberto**
    - Maiores riscos (técnicos, de mercado, legais) e como mitigar; premissas não validadas.
17. **Plano de entrega**
    - Marcos/fases, ordem de implementação, critérios de aceite do MVP.
    - Datas-alvo por fase (viram milestones no Linear), tamanho do time e ritmo (ciclos/sprints).
    - As perguntas de configuração do Linear (time, labels, estimativas) não entram aqui: ficam na etapa B1 de `entrega-linear.md`.
