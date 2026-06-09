# Skill: obsidian-bases — Explicação em Português (BR)

> Explica o conteúdo de [`SKILL.md`](SKILL.md). Não edite o `SKILL.md` (texto em inglês
> é o que faz a skill disparar).

## O que esta skill faz

Ensina o agente a criar e editar arquivos **`.base`** — as **Bases** do Obsidian.
Uma Base é uma "visão tipo banco de dados" das suas notas: você define **filtros**
(quais notas aparecem), **fórmulas** (valores calculados) e **visões** (tabela, cards,
lista ou mapa). É parecido com tabela dinâmica ou um banco relacional leve.

## Fluxo de trabalho

1. **Crie o arquivo** `.base` com YAML válido.
2. **Defina o escopo** com `filters` (por tag, pasta, propriedade ou data).
3. **Adicione fórmulas** (opcional) na seção `formulas`.
4. **Configure as visões** (`table`, `cards`, `list`, `map`) usando `order` para dizer
   quais propriedades exibir.
5. **Valide**: confira se o YAML é válido e se toda `formula.X` referenciada está
   realmente definida em `formulas`.
6. **Teste no Obsidian** abrindo o arquivo `.base`.

## Estrutura (schema) básica

```yaml
# Filtros globais: valem para TODAS as visões
filters:
  and: []
  or: []
  not: []

# Fórmulas reutilizáveis em todas as visões
formulas:
  nome_da_formula: 'expressão'

# Nomes de exibição e configurações das propriedades
properties:
  nome_da_propriedade:
    displayName: "Nome Exibido"

# Resumos (summaries) personalizados
summaries:
  meu_resumo: 'values.mean().round(3)'

# Uma ou mais visões
views:
  - type: table | cards | list | map
    name: "Nome da Visão"
    limit: 10                 # opcional: limita resultados
    groupBy:                  # opcional: agrupa
      property: nome_da_propriedade
      direction: ASC | DESC
    filters:                  # filtros só desta visão
      and: []
    order:                    # propriedades a exibir, em ordem
      - file.name
      - nome_da_propriedade
      - formula.nome_da_formula
```

## Filtros

```yaml
# Filtro único
filters: 'status == "done"'

# E (todas as condições verdadeiras)
filters:
  and:
    - 'status == "done"'
    - 'priority > 3'

# OU (qualquer condição verdadeira)
filters:
  or:
    - 'file.hasTag("livro")'
    - 'file.hasTag("artigo")'

# NÃO (exclui o que casa)
filters:
  not:
    - 'file.hasTag("arquivado")'
```

**Operadores:** `==` (igual), `!=` (diferente), `>`, `<`, `>=`, `<=`,
`&&` (e lógico), `||` (ou lógico), `!` (não lógico).

## Tipos de propriedade

1. **De nota** — vêm do frontmatter: `note.autor` ou só `autor`.
2. **De arquivo** — metadados: `file.name`, `file.mtime`, `file.folder`, `file.tags`,
   `file.backlinks` etc.
3. **De fórmula** — valores calculados: `formula.minha_formula`.

A palavra-chave **`this`** refere-se ao próprio arquivo `.base` na área principal, ou
ao arquivo que está incorporando a Base quando ela é embedada.

## Fórmulas

```yaml
formulas:
  total: "preco * quantidade"                          # aritmética
  icone_status: 'if(done, "✅", "⏳")'                 # condicional
  criado_em: 'file.ctime.format("YYYY-MM-DD")'         # formatar data
  dias_ate_vencer: 'if(due, (date(due) - today()).days, "")'  # dias até vencer
```

**Funções principais:** `date()`, `now()`, `today()`, `if()`, `duration()`, `file()`, `link()`.
Referência completa em [`references/FUNCTIONS_REFERENCE.md`](references/FUNCTIONS_REFERENCE.md).

### ⚠️ Cuidado com Duration (duração)

Subtrair duas datas devolve um tipo **Duration**, não um número. Duration **não** aceita
`.round()`, `.floor()` ou `.ceil()` diretamente — acesse primeiro um campo numérico
(`.days`, `.hours`, `.minutes`, `.seconds`, `.milliseconds`) e só então arredonde.

```yaml
# CERTO
"(date(due_date) - today()).days"
"(now() - file.ctime).days.round(0)"

# ERRADO (causa erro)
# "((date(due) - today()) / 86400000).round(0)"
```

## Tipos de visão

- **table** — tabela com colunas (`order`) e rodapés de resumo (`summaries`).
- **cards** — galeria de cards (boa para capa de livro, imagem etc.).
- **list** — lista simples.
- **map** — mapa (precisa de propriedades de latitude/longitude e do plugin Maps).

## Resumos padrão (summaries)

`Average` (média), `Min`, `Max`, `Sum` (soma), `Range` (amplitude), `Median` (mediana),
`Stddev` (desvio padrão), `Earliest`/`Latest` (data mais antiga/recente),
`Checked`/`Unchecked` (contagem de booleanos), `Empty`/`Filled` (vazios/preenchidos),
`Unique` (valores únicos).

## Incorporar uma Base num arquivo Markdown

```markdown
![[MinhaBase.base]]

<!-- visão específica -->
![[MinhaBase.base#Nome da Visão]]
```

## Regras de aspas no YAML (importante)

- Use **aspas simples** em fórmulas que contêm aspas duplas: `'if(done, "Sim", "Não")'`.
- Use **aspas duplas** em textos simples: `"Nome da Visão"`.
- Strings com caracteres especiais (`: { } [ ] , & * # ? | - < > = ! % @ \``) **precisam**
  estar entre aspas.

## Problemas comuns

- **Aspas trocadas em fórmulas** → envolva a fórmula inteira em aspas simples.
- **Matemática de Duration sem acessar campo** → use `.days` antes de arredondar.
- **Falta de checagem de nulo** → proteja com `if()` quando a propriedade pode não existir.
- **Fórmula não definida** → toda `formula.X` usada em `order`/`properties` precisa existir
  na seção `formulas`.

## Referências

- [Sintaxe de Bases](https://help.obsidian.md/bases/syntax)
- [Funções](https://help.obsidian.md/bases/functions)
- [Visões](https://help.obsidian.md/bases/views)
- [Referência completa de funções](references/FUNCTIONS_REFERENCE.md)
