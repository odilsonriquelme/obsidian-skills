# Skill: json-canvas — Explicação em Português (BR)

> Explica o conteúdo de [`SKILL.md`](SKILL.md). Não edite o `SKILL.md` (texto em inglês
> é o que faz a skill disparar).

## O que esta skill faz

Ensina o agente a criar e editar arquivos **`.canvas`** — os **Canvas** do Obsidian.
Um Canvas é um quadro infinito onde você organiza ideias visualmente: notas, imagens,
links e grupos, conectados por setas. Ótimo para **mapas mentais, fluxogramas e quadros
de projeto**. O formato segue a especificação aberta [JSON Canvas 1.0](https://jsoncanvas.org/spec/1.0/).

## Estrutura do arquivo

Um `.canvas` é um JSON com dois arrays no topo:

```json
{
  "nodes": [],
  "edges": []
}
```

- `nodes` — os "nós" (caixas) colocados no quadro.
- `edges` — as ligações (setas) entre os nós.

## Fluxos comuns

- **Criar um canvas novo**: comece com `{"nodes": [], "edges": []}`, gere IDs únicos
  de 16 caracteres hexadecimais para cada nó, adicione os nós com os campos obrigatórios,
  e ligue-os com edges referenciando IDs válidos.
- **Adicionar um nó**: gere um ID que não colida, escolha posição (`x`,`y`) sem sobrepor
  outros (deixe 50–100px de espaço) e acrescente ao array `nodes`.
- **Conectar dois nós**: crie um edge com `fromNode`/`toNode` apontando para IDs existentes.
- **Editar**: leia o JSON, localize o nó/edge pelo `id`, altere e grave de volta.
- **Sempre validar**: IDs únicos e referências de edges resolvendo para nós existentes.

## Nós (nodes)

A ordem no array define a profundidade (z-index): o primeiro é o fundo, o último fica por cima.

Atributos genéricos: `id` (hex de 16 caracteres), `type` (`text`, `file`, `link` ou `group`),
`x`, `y`, `width`, `height` (em pixels) e `color` (opcional: preset `"1"`–`"6"` ou hex).

### Nó de texto

```json
{
  "id": "6f0ad84f44ce9c17",
  "type": "text",
  "x": 0, "y": 0, "width": 400, "height": 200,
  "text": "# Olá Mundo\n\nIsto é conteúdo em **Markdown**."
}
```

⚠️ **Quebra de linha**: use `\n` nas strings JSON. **Não** use `\\n` literal — o Obsidian
renderizaria os caracteres `\` e `n`.

### Nó de arquivo

```json
{
  "id": "a1b2c3d4e5f67890",
  "type": "file",
  "x": 500, "y": 0, "width": 400, "height": 300,
  "file": "Anexos/diagrama.png"
}
```

Campo opcional `subpath` para apontar a um título ou bloco (começa com `#`).

### Nó de link

```json
{
  "id": "c3d4e5f678901234",
  "type": "link",
  "x": 1000, "y": 0, "width": 400, "height": 200,
  "url": "https://obsidian.md"
}
```

### Nó de grupo

Container visual para organizar outros nós. Posicione os nós-filhos dentro dos limites do grupo.
Atributos opcionais: `label` (rótulo), `background` (imagem de fundo), `backgroundStyle`
(`cover`, `ratio` ou `repeat`).

## Ligações (edges)

Conectam nós via `fromNode` e `toNode`:

```json
{
  "id": "0123456789abcdef",
  "fromNode": "6f0ad84f44ce9c17",
  "fromSide": "right",
  "toNode": "a1b2c3d4e5f67890",
  "toSide": "left",
  "toEnd": "arrow",
  "label": "leva a"
}
```

- `fromSide`/`toSide`: `top`, `right`, `bottom`, `left` (ponto de ancoragem).
- `fromEnd`/`toEnd`: `none` ou `arrow` (seta). Padrão: `fromEnd=none`, `toEnd=arrow`.
- `label`: texto sobre a seta.

## Cores

O tipo `canvasColor` aceita um hex (`"#FF0000"`) ou um preset numérico:

| Preset | Cor |
|--------|-----|
| `"1"` | Vermelho |
| `"2"` | Laranja |
| `"3"` | Amarelo |
| `"4"` | Verde |
| `"5"` | Ciano |
| `"6"` | Roxo |

Os valores exatos dos presets são propositalmente indefinidos — cada app usa suas cores.

## Geração de IDs

Use strings hexadecimais minúsculas de 16 caracteres (valor aleatório de 64 bits), ex.:
`"6f0ad84f44ce9c17"`, `"a3b2c1d0e9f8a7b6"`.

## Dicas de layout

- Coordenadas podem ser negativas (o canvas é infinito).
- `x` cresce para a direita, `y` cresce para baixo; a posição é o **canto superior esquerdo**.
- Espace os nós 50–100px; deixe 20–50px de margem dentro de grupos.
- Alinhe a uma grade (múltiplos de 10 ou 20) para um visual mais limpo.

## Checklist de validação

1. Todos os `id` (nós e edges) são únicos.
2. Todo `fromNode`/`toNode` aponta para um nó existente.
3. Campos obrigatórios presentes por tipo (`text` para texto, `file` para arquivo, `url` para link).
4. `type` é um de: `text`, `file`, `link`, `group`.
5. `fromSide`/`toSide` ∈ {`top`, `right`, `bottom`, `left`}.
6. `fromEnd`/`toEnd` ∈ {`none`, `arrow`}.
7. Cores são preset `"1"`–`"6"` ou hex válido.
8. O JSON é válido e parseável.

## Exemplos completos

Veja [`references/EXAMPLES.md`](references/EXAMPLES.md) — inclui mapas mentais, quadros
de projeto, canvas de pesquisa e fluxogramas completos.

## Referências

- [Especificação JSON Canvas 1.0](https://jsoncanvas.org/spec/1.0/)
- [JSON Canvas no GitHub](https://github.com/obsidianmd/jsoncanvas)
