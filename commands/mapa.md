---
description: Gera ou atualiza um diagrama do projeto (requer Archify)
argument-hint: [o que diagramar, ex "fluxo de pagamento da locacao"]
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(node:*), Bash(ls:*)
---

Gere ou atualize um diagrama deste projeto usando a skill **archify**.

Pedido: $ARGUMENTS

Se nada foi informado, assuma: diagrama de **architecture** do runtime do
projeto inteiro.

## 1. Consulte o indice antes de qualquer coisa

Leia `docs/diagramas/INDICE.md` (se existir). Compare o pedido com os
escopos ja registrados la.

- **Se algum diagrama ja cobre esse escopo**: voce vai ATUALIZAR o JSON
  dele, nao criar outro. Diga ao usuario qual diagrama voce identificou e
  siga.
- **Se nenhum cobre**: e um diagrama novo.

Nunca crie um segundo diagrama sobre o mesmo assunto so porque o pedido
veio com palavras diferentes.

## 2. Levante o conteudo

Fontes, nesta ordem:

1. As notas em `docs/referencia/` — elas ja descrevem o estado atual.
2. O codigo em si, para o que a nota nao cobrir.

Nao invente componente que voce nao viu em uma das duas.

## 3. Escolha o tipo

- **architecture** — componentes, servicos, storage, limites de confianca
- **workflow** — CI/CD, aprovacoes, runbooks
- **sequence** — chamadas de API, autenticacao, um fluxo no tempo
- **dataflow** — pipelines, origem e destino de dado, PII
- **lifecycle** — estados, retentativas, esperas, desfechos

Fluxo de negocio (pagamento, locacao, cobranca) normalmente e **sequence**.

## 4. Gere

- Escopo apertado: 8 a 12 componentes, um caminho primario, dependencias
  externas e limites de confianca. Detalhe extra vai em card, nao em mais
  seta.
- Ative evidencia de fonte nos nos quando possivel, para que cada no abra
  o arquivo e as linhas reais.
- Salve sempre os dois arquivos, lado a lado:
  - `docs/diagramas/<slug>.<tipo>.json` (a fonte tipada, e o que se revisa)
  - `docs/diagramas/<slug>.<tipo>.html` (o artefato gerado)
- Se for atualizacao, EDITE o JSON existente em vez de recriar, para que o
  diff mostre so o que mudou.
- Rode a validacao do archify e so entregue se passar. Se falhar, leia o
  `diagnostics[]` e corrija apenas o item apontado.

## 5. Atualize o indice

Escreva ou atualize `docs/diagramas/INDICE.md` neste formato exato:

```markdown
# Diagramas

| Arquivo | O que mostra | Tipo | Atualizado |
|---|---|---|---|
| `pagamento-locacao.sequence` | Da geracao da cobranca pelo franqueado ate a baixa do boleto | sequence | 2026-08-02 |
```

A coluna "O que mostra" deve descrever o escopo em uma frase completa —
e por ela que voce e o usuario vao reconhecer o diagrama daqui a meses.
Nunca repita so o nome do arquivo.

Mantenha a tabela em ordem alfabetica por arquivo.

## 6. Feche

Responda com:

- O caminho do `.html` para abrir no navegador.
- Se foi atualizacao: bullets do que mudou na arquitetura (componente
  adicionado, removido, rota alterada).
- Se o diagrama contradisser alguma nota em `docs/referencia/`: avise —
  uma das duas esta desatualizada.
- A linha que voce adicionou ou alterou no INDICE.md.

Nao commite.
