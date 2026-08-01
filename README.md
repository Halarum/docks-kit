# docs-kit

Documentação viva para os projetos da Halarum (Traki e Visiun). Tudo em markdown
puro dentro do repo, versionado no git.

O sistema tem três camadas. Só a primeira é usada todo dia.

| Camada | Ferramenta | Quando |
|---|---|---|
| Escrever e manter a doc | `docs-kit` (este plugin) | todo dia, antes de commitar |
| Diagramas | Archify | quando a arquitetura muda |
| Ler / editar / buscar | OpenKnowledge | sempre aberto, sem comando |

O que fica no repo:

- `docs/referencia/` — como cada parte funciona **hoje** (atualizado no lugar)
- `docs/diagramas/` — o `.json` (fonte revisável) e o `.html` de cada diagrama

---

## Instalação (uma vez por máquina)

**1. O plugin**

```
/plugin marketplace add halarumdigital/docs-kit
/plugin install docs-kit@halarum
/reload-plugins
```

**2. Archify** (skill de diagramas)

```
npx skills add tt-a1i/archify -g
node ~/.claude/skills/archify/bin/archify.mjs doctor
```

O `doctor` confirma que o ambiente está ok. Se o caminho não bater, procure
onde a skill foi instalada dentro de `~/.claude/skills/`.

**3. OpenKnowledge** (editor + busca). Precisa de Node 24+ e git — você já tem.

```
npm install -g @inkeep/open-knowledge
```

---

## Ligar num projeto (uma vez por repo)

Dentro do Traki, e depois dentro do Visiun:

```
/docs-kit:init
```

Cria `docs/referencia/`, o arquivo `docs/.base` e a seção de documentação no
`CLAUDE.md`. O `docs/.base` guarda o nome da branch base deste repositório
(`homolog`, `dev`, o que for) — é contra ela que o `/docs-kit:doc` compara.
Cada repo tem a sua.

Depois, no terminal, na raiz do projeto:

```
ok init
```

Isso registra o servidor MCP do OpenKnowledge no Claude Code e instala uma skill
local em `.claude/skills/open-knowledge/`. A partir daí o Claude Code busca a
documentação pelo MCP, não só por grep.

Para abrir o editor no navegador:

```
ok start --open
```

---

## Uso

**Todo dia — um comando só, antes de commitar:**

```
/docs-kit:doc
```

Lê o `git diff`, atualiza as notas de referência do que mudou e cria as que
faltarem. Não commita nada.

**Carga inicial, para documentar o que já existe:**

```
/docs-kit:lista    # monta a lista dos módulos a documentar (uma vez)
/docs-kit:nota     # escreve a próxima nota da lista (repita até acabar)
```

**Quando a arquitetura muda:**

```
/docs-kit:mapa
/docs-kit:mapa sequence do fluxo de login
```

---

## Fora de escopo por enquanto

CHANGELOG, fechamento de versão e gate de CI para equipe. Entram depois sem
quebrar nada — o `/docs-kit:doc` continua o mesmo, só ganha passos.
