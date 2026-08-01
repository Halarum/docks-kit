# docs-kit

Documentação viva para os projetos de desenvolvimento. Tudo em markdown puro dentro do repo, versionado no git.

O que fica em cada projeto:

- `docs/referencia/` — como cada parte funciona **hoje** (atualizado no lugar)
- `docs/diagramas/` — o `.json` (fonte revisável) e o `.html` de cada diagrama, mais o [`INDICE.md`](http://INDICE.md) com o que cada um mostra
- `docs/.base` — o nome da branch base do repositório
- `docs/_[inventario.md](http://inventario.md)` — a fila da carga inicial

Instruções completas, fluxo detalhado e perguntas frequentes: [**MANUAL.md**](http://MANUAL.md).

---

## Instalação (uma vez por máquina)

**1. O plugin**

```
/plugin marketplace add Halarum/docks-kit
/plugin install docs-kit@halarum
/reload-plugins

```

**2. Archify** (opcional — só para o `/docs-kit:mapa`)

```
npx skills add tt-a1i/archify -g
node ~/.claude/skills/archify/bin/archify.mjs doctor

```

O `doctor` confirma que o ambiente está ok. Se o caminho não bater, procure onde a skill foi instalada dentro de `~/.claude/skills/`. Sem o Archify, os outros quatro comandos funcionam normalmente.

---

## Ligar num projeto (uma vez por repo)

```
## Primeira vez num projeto — a ordem completa

1. `/docs-kit:init` — responda qual é a branch base
2. `/docs-kit:lista` — monta a fila em `docs/_inventario.md`
3. `/docs-kit:nota` — escreve uma nota; leia e resolva os `⚠️ confirmar`
4. Repita o passo 3 até a fila acabar
5. `git add . && git commit -m "documentação inicial" && git push`

Terminou a carga inicial. Do dia seguinte em diante, apenas `/docs-kit:doc`
antes de commitar.

OBS:
/docs-kit:lista → high. É varredura e organização — não precisa do raciocínio máximo, precisa de cobertura.
/docs-kit:nota → xhigh. É aqui que mora o risco de chute: ler módulo, cruzar com teste, decidir o que é comportamento real. É o passo que vale pagar mais caro.
max eu reservaria só se alguma nota sair rasa ou com muito ⚠️ 
```



&nbsp;

Cria `docs/referencia/`, o arquivo `docs/.base` e a seção de documentação no [`CLAUDE.md`](http://CLAUDE.md). O `docs/.base` guarda o nome da branch base deste repositório (`homolog`, `dev`, o que for) — é contra ela que o `/docs-kit:doc` compara. Cada repo tem a sua.

---

## Uso

**Todo dia — um comando só, antes de commitar:**

```
/docs-kit:doc

```

Lê o `git diff`, atualiza as notas de referência do que mudou e cria as que faltarem. Não commita nada.

**Carga inicial, para documentar o que já existe:**

```
/docs-kit:lista    # monta a lista dos módulos a documentar (uma vez)
/docs-kit:nota     # escreve a próxima nota da lista (repita até acabar)

```

**Quando o fluxo ou a arquitetura mudam de verdade:**

```
/docs-kit:mapa fluxo de pagamento da locação

```

---

## Fora de escopo por enquanto

CHANGELOG, fechamento de versão, hook de pre-push e gate de CI. Entram depois sem quebrar nada — o `/docs-kit:doc` continua o mesmo, só ganha passos.