# Skill: defuddle — Explicação em Português (BR)

> Explica o conteúdo de [`SKILL.md`](SKILL.md). Não edite o `SKILL.md` (texto em inglês
> é o que faz a skill disparar).

## O que esta skill faz

Ensina o agente a usar o **Defuddle CLI** para extrair o **conteúdo limpo** de páginas
web em formato Markdown, removendo menus de navegação, anúncios e outros entulhos.
Isso **reduz o consumo de tokens** e melhora a leitura.

Quando usar: prefira o Defuddle **no lugar do WebFetch** sempre que o usuário passar uma
URL para ler ou analisar (documentação, artigos, posts de blog, páginas comuns).
**Não use** para URLs que terminam em `.md` — essas já são markdown; nesses casos use o
WebFetch direto.

## Instalação (se necessário)

```bash
npm install -g defuddle
```

## Uso

Sempre use `--md` para saída em Markdown:

```bash
defuddle parse <url> --md
```

Salvar em arquivo:

```bash
defuddle parse <url> --md -o conteudo.md
```

Extrair um metadado específico:

```bash
defuddle parse <url> -p title          # título
defuddle parse <url> -p description    # descrição
defuddle parse <url> -p domain         # domínio
```

## Formatos de saída

| Flag | Formato |
|------|---------|
| `--md` | Markdown (escolha padrão) |
| `--json` | JSON com HTML e markdown juntos |
| (nenhuma) | HTML |
| `-p <nome>` | Uma propriedade de metadado específica |

## Referência

- [Defuddle CLI no GitHub](https://github.com/kepano/defuddle-cli)
