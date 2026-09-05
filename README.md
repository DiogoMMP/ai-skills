# ai-skills

Plugin de [Claude Code](https://code.claude.com) com uma coleção pessoal de skills.

## Instalação

```bash
claude --plugin-dir ./ai-skills
```

Ou, depois de publicado numa marketplace:

```
/plugin install ai-skills
```

## Estrutura

```
ai-skills/
├── .claude-plugin/
│   └── plugin.json      # manifesto do plugin
└── skills/
    └── <nome-da-skill>/
        └── SKILL.md
```

## Skills

| Skill | Categoria | Resumo | Descrição |
| :--- | :--- | :--- | :--- |
| [commit-push](skills/commit-push/SKILL.md) | Git & GitHub | Cria commits (Conventional Commits) e faz push, seguindo GitFlow | Lê o estado do repositório, valida a branch contra o modelo GitFlow (`main`/`develop`/`feature`/`bugfix`/`release`/`hotfix`), agrupa o diff em commits coerentes e mostra a(s) mensagem(ns) propostas no chat, aguardando validação explícita antes de commitar — e pergunta separadamente, só depois, se deve fazer push. Uma issue do GitHub pode ser referenciada (`Refs: #12`) mas nunca é obrigatória. |
| [create-pr](skills/create-pr/SKILL.md) | Git & GitHub | Abre um pull request preenchendo o template do repositório | Confirma que a branch já está commitada e pushed (nunca commita nem faz push), resolve a branch base a partir do GitFlow, lê primeiro o template de PR do repositório e preenche-o com base no diff real — em português por defeito — linkando os registos em `docs/changes/` que a branch toca. Mostra o título e o corpo completos e só corre `gh pr create` depois de aprovação explícita. |
| [implement-change](skills/implement-change/SKILL.md) | Desenvolvimento | Implementa uma feature, bugfix ou hotfix ponta a ponta, com registo escrito | Aceita o pedido em qualquer formato (texto livre, issue, página de wiki, spec, screenshots), determina o tipo de mudança (feature/bugfix/hotfix), investiga o projeto, propõe um plano no chat e espera aprovação, escreve a spec em `docs/changes/<tipo>/<PREFIXO><NNNN>-<slug>/README.md` antes de tocar em código — separado de `docs/wiki/`/`docs/notes/` para não poluir uma knowledge base que viva no mesmo `docs/` — implementa, corre a verificação própria do projeto e fecha com um `RESULT.md` honesto sobre o que realmente aconteceu, desvios incluídos. Não commita nem abre PR — isso fica para `commit-push` e `create-pr`. |
| [create-readme](skills/create-readme/SKILL.md) | Desenvolvimento | Escreve ou reescreve o README.md raiz no formato da casa | Investiga o repositório (stack, arquitetura, módulos, portas, testes, CI) e preenche um template fixo — badges, índice, stack técnica, diagrama de arquitetura, layout anotado, getting started, testes, contribuição — só com factos que consegue provar em ficheiros reais. Nunca inventa comandos, versões ou URLs, remove secções sem evidência em vez de as inventar, e nunca escreve nada que pareça um segredo. Mostra o rascunho completo e espera aprovação antes de escrever o ficheiro. |
| [knowledge-base](skills/knowledge-base/SKILL.md) | Knowledge base pessoal | Cria a estrutura de pastas de uma knowledge base pessoal | Cria a estrutura de pastas de uma knowledge base pessoal, em dois modos: **research** (estilo Karpathy: `raw/` + `wiki/` + `outputs/` + `tools/`, para quando há muito material externo a ingerir) ou **code** (mais leve, `wiki/` + `notes/`, pensado para repos de código pessoais, alimentado por notas soltas e pelo git log em vez de um `raw/`). Pergunta o modo, a pasta de destino, opcionalmente se existe modelo de domínio a documentar (modo code), e se quer ligar a um vault geral partilhado (via junction `wiki/_geral`, visível no mesmo grafo do Obsidian). |
| [compile-wiki](skills/compile-wiki/SKILL.md) | Knowledge base pessoal | Compila/atualiza a `wiki/` a partir de notas e do git | Faz a compilação, seguindo o modo definido no `KB_GUIDE.md`: em modo research lê `raw/`, resume e organiza em artigos por conceito; em modo code lê `notes/` (ficheiros com frontmatter `status: pending`/`compiled`) e tudo o que houver de novo no git — commits, issues e pull requests (via `gh`, se disponível) — desde o último `last_compiled_at`, e regenera sempre por inteiro `wiki/Estrutura.md` (estrutura de pastas, stack técnica, pontos de entrada e o modelo de domínio, se estiver registado). Em ambos os modos cria backlinks e mantém `wiki/index.md` atualizado — os artigos de decisão de forma incremental, a `Estrutura.md` sempre do zero. |
| [capture-note](skills/capture-note/SKILL.md) | Knowledge base pessoal | Grava rapidamente uma nota solta em `notes/` | Grava rapidamente um pensamento em `notes/` com o frontmatter certo (`status: pending`), sem organizar nada — isso fica para o `compile-wiki`. |
| [adr-new](skills/adr-new/SKILL.md) | Knowledge base pessoal | Cria uma nota ADR de decisão arquitetural | Cria uma nota estilo ADR (Contexto/Decisão/Consequências) em `notes/`, útil para documentar uma decisão arquitetural no momento em que é tomada. |
| [resume](skills/resume/SKILL.md) | Knowledge base pessoal | Briefing para retomar um projeto pessoal parado | Briefing só de leitura: lê `wiki/Estrutura.md` (ou `index.md`), notas pendentes, e atividade git recente (incluindo alterações por commitar), e resume "onde ficaste" com sugestão de próximo passo. |
| [wiki-lint](skills/wiki-lint/SKILL.md) | Knowledge base pessoal | Health check da `wiki/` (links partidos, órfãos, etc.) | Health check da `wiki/`: links partidos, artigos órfãos (não referenciados no `index.md`), referências desatualizadas a ficheiros/decisões que já não existem, e entradas soltas em `wiki/sources.md`/`notes/`. Reporta primeiro, só corrige o que for aprovado. |

## Licença

MIT — ver [LICENSE](LICENSE).
