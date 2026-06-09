# Skill: obsidian-cli — Explicação em Português (BR)

> Explica o conteúdo de [`SKILL.md`](SKILL.md). Não edite o `SKILL.md` (texto em inglês
> é o que faz a skill disparar).

## O que esta skill faz

Ensina o agente a usar o **`obsidian` CLI** — a linha de comando do Obsidian — para
interagir com uma instância do Obsidian **que esteja aberta**. Dá para ler, criar, buscar
e gerenciar notas, tarefas e propriedades, além de **desenvolver e depurar plugins e temas**
(recarregar, capturar erros, tirar screenshots, inspecionar o DOM).

> ⚠️ Requer o Obsidian **aberto** no momento da execução.

## Referência de comandos

Rode `obsidian help` para ver todos os comandos disponíveis (sempre atualizado).
Documentação oficial: <https://help.obsidian.md/cli>.

## Sintaxe

- **Parâmetros** recebem valor com `=`. Use aspas quando o valor tem espaços:

```bash
obsidian create name="Minha Nota" content="Olá mundo"
```

- **Flags** são chaves booleanas, sem valor:

```bash
obsidian create name="Minha Nota" silent overwrite
```

- Para conteúdo com várias linhas, use `\n` (nova linha) e `\t` (tabulação).

## Como mirar o arquivo

Muitos comandos aceitam `file` ou `path`. Sem nenhum dos dois, usam o **arquivo ativo**.

- `file=<nome>` — resolve como um wikilink (só o nome, sem caminho nem extensão).
- `path=<caminho>` — caminho exato a partir da raiz do vault, ex.: `pasta/nota.md`.

## Como mirar o vault

Por padrão, os comandos atuam no vault focado por último. Use `vault=<nome>` como
**primeiro** parâmetro para mirar um vault específico:

```bash
obsidian vault="Meu Vault" search query="teste"
```

## Padrões comuns

```bash
obsidian read file="Minha Nota"
obsidian create name="Nova Nota" content="# Olá" template="Template" silent
obsidian append file="Minha Nota" content="Nova linha"
obsidian search query="termo de busca" limit=10
obsidian daily:read
obsidian daily:append content="- [ ] Nova tarefa"
obsidian property:set name="status" value="done" file="Minha Nota"
obsidian tasks daily todo
obsidian tags sort=count counts
obsidian backlinks file="Minha Nota"
```

Dicas: `--copy` em qualquer comando copia a saída para a área de transferência;
`silent` evita que arquivos sejam abertos; `total` em comandos de listagem retorna a contagem.

## Desenvolvimento de plugins/temas

### Ciclo desenvolver/testar

Depois de alterar o código de um plugin ou tema:

1. **Recarregar** o plugin para pegar as mudanças:
   ```bash
   obsidian plugin:reload id=meu-plugin
   ```
2. **Verificar erros** (se houver, corrija e volte ao passo 1):
   ```bash
   obsidian dev:errors
   ```
3. **Conferir visualmente** com screenshot ou inspeção do DOM:
   ```bash
   obsidian dev:screenshot path=screenshot.png
   obsidian dev:dom selector=".workspace-leaf" text
   ```
4. **Ler o console** em busca de avisos ou logs inesperados:
   ```bash
   obsidian dev:console level=error
   ```

### Comandos extras de desenvolvedor

Executar JavaScript no contexto do app:

```bash
obsidian eval code="app.vault.getFiles().length"
```

Inspecionar valores de CSS:

```bash
obsidian dev:css selector=".workspace-leaf" prop=background-color
```

Alternar emulação mobile:

```bash
obsidian dev:mobile on
```

Rode `obsidian help` para ver mais comandos de desenvolvedor (controles de CDP e debugger).
