---
description: Escreve a proxima nota da lista (repita ate a fila acabar)
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(ls:*)
---

Escreva a proxima nota de referencia da carga inicial.

Se `docs/_inventario.md` nao existir, diga que e preciso rodar
`/docs-kit:lista` primeiro e pare.

Pegue o **primeiro item ainda marcado `[ ]`** no `docs/_inventario.md`.
Se nao houver nenhum, diga que a carga inicial terminou e pare.

Para esse item:

1. Leia os arquivos do caminho indicado **e os testes relacionados** — os
   testes revelam o comportamento esperado e reduzem chute.
2. Crie `docs/referencia/<slug>.md` usando exatamente esta estrutura:

```markdown
---
tipo: referencia
modulo: <slug>
atualizado: <YYYY-MM-DD>
---

# <Nome do modulo ou comportamento>

<Uma frase dizendo o que isto e e para que serve.>

## Como funciona

<Fluxo principal. Estado atual, presente do indicativo. Sem historico.>

## Onde fica no codigo

- `caminho/do/arquivo.ts` — o que ele faz

## Depende de

<Outros modulos, servicos externos, tabelas, variaveis de ambiente.>

## Cuidados

<Armadilhas conhecidas. Remova a secao se nao houver.>

## Relacionadas

- [[outra-nota]]
```

3. Descreva o que o codigo FAZ hoje, citando os arquivos-fonte reais.
4. Onde nao tiver certeza, escreva `⚠️ confirmar` explicitando a duvida.
   Nunca preencha com suposicao.
5. **Marque o item como feito** no `docs/_inventario.md`, trocando
   `- [ ]` por `- [x]` naquela linha.

Nao commite.

## Ao terminar

- Mostre o caminho da nota criada.
- Liste os pontos marcados com `⚠️ confirmar`.
- Diga quantos itens ainda faltam na fila.
