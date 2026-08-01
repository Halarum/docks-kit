---
description: Prepara o projeto para a documentacao viva (roda uma vez por repo)
allowed-tools: Read, Write, Glob, Bash(git rev-parse:*)
---

Prepare este repositorio para a documentacao viva. Rode isto UMA vez por projeto.

## 1. Crie a estrutura

Se ainda nao existirem, crie:

- `docs/referencia/` (vazia, com um `.gitkeep`)
- `docs/.base` — uma unica linha com o nome da branch base deste
  repositorio (aquela contra a qual o trabalho e comparado, ex `homolog`
  ou `dev`).

Para descobrir a base, olhe as branches remotas e pergunte ao usuario
qual delas e a base, sugerindo a mais provavel. Nao adivinhe em silencio.

## 2. Detecte o projeto

Leia `package.json`, `README.md` e a estrutura de pastas para entender:
nome do projeto, stack, e quais sao os modulos/apps principais.

## 3. Escreva ou atualize o CLAUDE.md

Na raiz. Se ja existir, NAO sobrescreva — apenas adicione a secao
`## Documentacao` (se ela ainda nao estiver la):

```markdown
## Documentacao

Notas de referencia (como cada parte funciona hoje): `docs/referencia/`

Antes de mexer em algo, procure a nota correspondente em `docs/referencia/`.
Depois de mexer, rode `/docs-kit:doc` para manter tudo sincronizado.
```

Se o `CLAUDE.md` nao existir, crie um com essa secao mais um resumo curto do
projeto (nome, o que e, stack) que voce detectou no passo 2.

## 4. Feche

Mostre em duas linhas o que foi criado e diga que o proximo passo e
`/docs-kit:lista` para comecar a documentar o que ja existe.

Nao crie nenhuma nota de referencia agora. Nao commite nada.
