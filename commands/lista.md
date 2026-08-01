---
description: Monta a lista dos modulos a documentar (roda uma vez por projeto)
allowed-tools: Read, Write, Grep, Glob, Bash(ls:*), Bash(find:*)
---

Monte a fila da carga inicial da documentacao. **Nao crie nenhuma nota
nesta execucao** — aqui voce so lista.

Se `docs/_inventario.md` ja existir, avise que a lista ja foi montada e
pare (nao sobrescreva).

1. Explore o repositorio e identifique os modulos / areas funcionais.
   Se o repo for grande, use subagents em paralelo (um por diretorio de
   topo) para nao estourar o contexto.
2. Ignore o que nao merece nota: build, config, scripts triviais,
   dependencias, arquivos gerados.
3. Escreva `docs/_inventario.md` neste formato exato:

```markdown
# Inventario

Fila da carga inicial. Rode `/docs-kit:nota` para documentar o proximo
item nao marcado.

- [ ] `<slug>` — `<caminho>` — <uma frase do que faz>
- [ ] `<slug>` — `<caminho>` — <uma frase do que faz>
```

4. Ao terminar, mostre quantos itens entraram na fila e diga que o
   proximo passo e rodar `/docs-kit:nota`.
