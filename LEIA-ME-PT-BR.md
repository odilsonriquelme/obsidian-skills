# Obsidian Skills — Explicação em Português (BR)

> **O que é este repositório?**
> Um conjunto de **Agent Skills** (habilidades de agente) para trabalhar com o **Obsidian**.
> Skills são "manuais de instrução" que um agente de IA (como o Claude Code) lê
> automaticamente quando percebe que a tarefa combina com a descrição da skill.
> Assim o agente passa a saber a sintaxe certa do Obsidian sem você precisar explicar.
>
> Repositório original: <https://github.com/kepano/obsidian-skills> (autor: Steph Ango / kepano).
> Estas skills seguem a [especificação Agent Skills](https://agentskills.io/specification),
> então funcionam em qualquer agente compatível — Claude Code, Codex CLI, OpenCode, etc.

> **Observação importante sobre os arquivos `SKILL.md`**
> Os arquivos `SKILL.md` e os `references/*.md` foram **mantidos em inglês de propósito**.
> O agente decide quando ativar uma skill lendo o campo `description` (em inglês) no topo
> do arquivo. Traduzir esse texto pode fazer a skill **deixar de disparar** corretamente.
> Por isso, as explicações em português ficam em arquivos separados chamados
> `EXPLICACAO-PT-BR.md` dentro de cada pasta de skill, sem tocar nos originais.

---

## Como instalar

### Opção 1 — via Marketplace do Claude Code (mais fácil)

```
/plugin marketplace add kepano/obsidian-skills
/plugin install obsidian@obsidian-skills
```

### Opção 2 — via `npx skills`

```
npx skills add git@github.com:kepano/obsidian-skills.git
```

### Opção 3 — manualmente (Claude Code)

Copie o conteúdo deste repositório para uma pasta `.claude/` na raiz do seu vault
do Obsidian (ou na pasta que você usa com o Claude Code).

### Opção 4 — manualmente (Codex CLI)

Copie a pasta `skills/` para o caminho de skills do Codex (normalmente `~/.codex/skills`).

### Opção 5 — manualmente (OpenCode)

Clone o **repositório inteiro** (não só a pasta `skills/`) para `~/.opencode/skills/`:

```sh
git clone https://github.com/kepano/obsidian-skills.git ~/.opencode/skills/obsidian-skills
```

O OpenCode descobre automaticamente todos os arquivos `SKILL.md`. Reinicie o OpenCode
para as skills aparecerem.

---

## As 5 skills, em resumo

| Skill | Para que serve | Quando o agente ativa |
|-------|----------------|------------------------|
| **obsidian-markdown** | Criar/editar notas `.md` com a sintaxe extra do Obsidian (wikilinks, embeds, callouts, propriedades) | Quando você mexe em arquivos `.md` ou cita wikilinks, callouts, frontmatter, tags, embeds |
| **obsidian-bases** | Criar/editar arquivos `.base` — visões tipo banco de dados das suas notas (tabelas, cards, filtros, fórmulas) | Quando você trabalha com arquivos `.base` ou pede tabelas/visões filtradas das notas |
| **json-canvas** | Criar/editar arquivos `.canvas` — quadros visuais (mapas mentais, fluxogramas, conexões entre nós) | Quando você mexe em arquivos `.canvas` ou pede mapa mental/fluxograma |
| **obsidian-cli** | Operar o Obsidian pela linha de comando (ler, criar, buscar notas; desenvolver/depurar plugins e temas) | Quando você pede operações no vault pelo terminal ou desenvolvimento de plugin/tema |
| **defuddle** | Extrair o conteúdo limpo (em markdown) de páginas web, removendo menus/anúncios e economizando tokens | Quando você passa uma URL para ler/analisar (em vez do WebFetch) |

Cada pasta de skill tem um `EXPLICACAO-PT-BR.md` com os detalhes traduzidos e comentados.

---

## Estrutura de pastas

```
obsidian-skills/
├── README.md                       → leia-me original (inglês)
├── LEIA-ME-PT-BR.md                → este arquivo (visão geral em PT-BR)
├── LICENSE                         → licença MIT
├── .claude-plugin/
│   ├── plugin.json                 → metadados do plugin (nome, versão, autor)
│   └── marketplace.json            → registro p/ o marketplace do Claude Code
└── skills/
    ├── obsidian-markdown/
    │   ├── SKILL.md                → instruções (inglês) — NÃO editar
    │   ├── EXPLICACAO-PT-BR.md     → explicação em português
    │   └── references/             → referências detalhadas (callouts, embeds, propriedades)
    ├── obsidian-bases/
    │   ├── SKILL.md
    │   ├── EXPLICACAO-PT-BR.md
    │   └── references/             → referência completa de funções
    ├── json-canvas/
    │   ├── SKILL.md
    │   ├── EXPLICACAO-PT-BR.md
    │   └── references/             → exemplos completos de canvas
    ├── obsidian-cli/
    │   ├── SKILL.md
    │   └── EXPLICACAO-PT-BR.md
    └── defuddle/
        ├── SKILL.md
        └── EXPLICACAO-PT-BR.md
```

---

## Glossário rápido (termos do Obsidian)

- **Vault** — o "cofre", ou seja, a pasta onde ficam todas as suas notas do Obsidian.
- **Wikilink** — link interno entre notas no formato `[[Nome da Nota]]`. O Obsidian
  atualiza esses links sozinho quando você renomeia a nota.
- **Embed** — incorporar o conteúdo de outra nota/imagem/PDF dentro da nota atual, com `![[...]]`.
- **Callout** — caixa de destaque colorida (`> [!nota]`, `> [!aviso]` etc.).
- **Frontmatter / Properties** — bloco YAML no topo da nota com metadados (tags, data, status…).
- **Base** (`.base`) — uma "tabela dinâmica" que monta visões das suas notas com filtros e fórmulas.
- **Canvas** (`.canvas`) — quadro infinito para organizar notas e ideias visualmente.

---

*Licença: MIT (mesma do projeto original). As explicações em português foram adicionadas
para facilitar o uso por falantes de português; o conteúdo técnico das skills segue o original.*
