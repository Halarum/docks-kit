# Manual — docs-kit

Documentação viva para os projetos da Halarum. Tudo em markdown puro, dentro do
próprio repositório, versionado no git.

---

## Os comandos

| Comando | O que faz | Quando |
|---|---|---|
| `/docs-kit:init` | Cria `docs/referencia/` e `docs/.base`, e a seção no `CLAUDE.md` | 1x por repo |
| `/docs-kit:lista` | Monta `docs/_inventario.md` com os módulos a documentar | 1x por repo |
| `/docs-kit:nota` | Escreve a próxima nota da fila e marca como feita | repete até a fila acabar |
| `/docs-kit:doc` | Atualiza as notas do que mudou | todo dia, antes de commitar |
| `/docs-kit:mapa` | Gera ou atualiza um diagrama | quando o fluxo muda de verdade |

Nenhum comando commita nada. Quem commita é você.

---

## Instalação

### 1. O plugin (1x por máquina)

Dentro do Claude Code:

```
/plugin marketplace add halarumdigital/docs-kit
/plugin install docs-kit@halarum
/reload-plugins
```

Depois disso os comandos `/docs-kit:*` existem em qualquer projeto.

### 2. Archify (opcional, só para `/docs-kit:mapa`)

No terminal:

```
npx skills add tt-a1i/archify -g
```

Sem ele, os outros quatro comandos funcionam normalmente — só o `mapa` não.

---

## Preparar um projeto (1x por repo)

Dentro do repositório, no Claude Code:

```
/docs-kit:init
```

O que ele faz:

- cria `docs/referencia/` (onde as notas vão morar)
- pergunta qual é a branch base do repositório e grava em `docs/.base`
- adiciona uma seção `## Documentação` no `CLAUDE.md`

**A branch base** é aquela contra a qual o trabalho é comparado — `homolog`,
`dev`, `master`, o que for. Cada repositório tem a sua. Para mudar depois, edite
`docs/.base`; não precisa reinstalar nada.

---

## Carga inicial: documentar o que já existe

Isso acontece uma vez na vida de cada projeto.

### Passo 1 — montar a fila

```
/docs-kit:lista
```

Ele varre o repositório e escreve `docs/_inventario.md`:

```markdown
- [ ] `autenticacao` — `src/auth/` — login, JWT, refresh token
- [ ] `cobranca-asaas` — `src/integrations/asaas/` — boleto e webhook
- [ ] `relatorios` — `src/reports/` — consultas agregadas
```

Se tiver alguma linha que não deveria virar nota, apague. Se estiver tudo certo,
não mexa em nada.

### Passo 2 — escrever as notas

```
/docs-kit:nota
```

Ele pega o **primeiro item não marcado**, lê o código daquele caminho e os
testes relacionados, escreve `docs/referencia/<slug>.md` e marca `[x]` no
inventário.

Leia a nota. Onde ele não teve certeza, deixou `⚠️ confirmar` — resolva esses
pontos enquanto o assunto está fresco.

Rode `/docs-kit:nota` de novo para o próximo item. Repita até ele avisar que a
fila acabou.

**Dica:** revise a primeira nota com atenção antes de rodar as outras. Se o
formato não for o que você esperava, é melhor descobrir na nota 1 do que na 20.

### Passo 3 — subir

```
git add .
git commit -m "documentação inicial"
git push
```

---

## Dia a dia

Terminou a tarefa, **ainda na branch onde trabalhou**:

```
/docs-kit:doc
git add .
git commit -m "..."
git push
```

### O que o `/docs-kit:doc` faz, por dentro

1. Lê o que está sem commitar mais os commits da branch atual contra a base
   configurada em `docs/.base`.
2. Do diff extrai os nomes: arquivos, funções, pastas.
3. Faz grep desses nomes dentro de `docs/referencia/`.
4. **Achou a nota** → reescreve o trecho desatualizado. Substitui, não acumula
   histórico. É o caso da grande maioria dos dias.
5. **Não achou** → cria uma nota nova.
6. Verifica se o diff tem sinal de mudança estrutural e, se tiver, avisa.
7. Reporta o que criou e o que editou.

### Por que ele acha a nota certa

Porque cada nota lista os arquivos-fonte na seção "Onde fica no código". É esse
o índice. Sem ele, o grep não acharia nada.

### O aviso de arquitetura

No fim, se o diff tiver sinal de mudança estrutural, ele termina com:

```
⚠️ Possível mudança de arquitetura: <o que ele viu>
Se o fluxo mudou de fato, rode /docs-kit:mapa. Se não mudou, ignore.
```

Ele **não** mexe em diagrama nenhum — só levanta a bandeira. Ele prefere avisar
demais a deixar passar, então falso alarme vai acontecer. Responder "não mudou
nada" custa nada; um alarme perdido deixa o diagrama mentindo.

Sinais que disparam o aviso: dependência nova, variável de ambiente nova, pasta
de topo nova ou removida, arquivo de rota novo, endpoint de webhook, migration,
tabela ou coluna nova, cliente de API externa, fila ou cron, cache, middleware,
mudança de autenticação, alteração em docker-compose, PM2, vhost ou CI.

---

## Diagramas

### Gerar

Você descreve o que quer, em português:

```
/docs-kit:mapa fluxo de pagamento da locação
/docs-kit:mapa fluxo de pagamento da oficina
```

Cada execução gera um diagrama separado. Sem argumento, ele assume o diagrama de
arquitetura do projeto inteiro.

**Quanto mais específico, melhor sai.** Compare:

- `/docs-kit:mapa pagamento` → vago, ele chuta o escopo
- `/docs-kit:mapa fluxo de pagamento da locação, da geração da cobrança pelo
  franqueado até a baixa do boleto` → ele sabe onde começa e onde termina

### O que ele produz

Dois arquivos por diagrama, em `docs/diagramas/`:

```
pagamento-locacao.sequence.json    ← a fonte tipada (é isso que se revisa)
pagamento-locacao.sequence.html    ← o diagrama, abre no navegador
```

O JSON descreve os componentes e as setas. O HTML é gerado a partir dele e
validado antes de ser entregue. Como o JSON é versionado, o diff no PR mostra
exatamente o que mudou na arquitetura.

### O índice — como você lembra o que existe

O `/docs-kit:mapa` mantém `docs/diagramas/INDICE.md`:

| Arquivo | O que mostra | Tipo | Atualizado |
|---|---|---|---|
| `pagamento-locacao.sequence` | Da geração da cobrança pelo franqueado até a baixa do boleto | sequence | 2026-08-02 |
| `pagamento-oficina.sequence` | Ordem de serviço da oficina até o recebimento | sequence | 2026-08-02 |

A coluna "O que mostra" descreve o escopo em uma frase — é por ela que você
reconhece o diagrama meses depois, não pelo nome do arquivo.

**Antes de gerar qualquer coisa, o comando lê esse índice.** Se já existe um
diagrama daquele escopo, ele atualiza o JSON existente em vez de criar um
terceiro arquivo — mesmo que você tenha pedido com palavras diferentes.

### Quando atualizar

Só quando a **estrutura** muda: serviço novo, integração externa nova, rota
nova, dependência nova entre módulos, mudança de banco ou fila. Correção de bug,
refatoração interna e ajuste de regra de negócio não mexem no diagrama.

Na prática, poucas vezes por mês.

### Quantos manter

Poucos e vivos. Um de arquitetura por projeto, mais dois ou três dos fluxos que
realmente importam. Cada diagrama é uma coisa a manter — vinte diagramas são
vinte coisas apodrecendo. Fluxo secundário fica melhor descrito em texto dentro
da nota, porque texto o `/docs-kit:doc` mantém sozinho e diagrama não.

---

## Regras das notas

- Uma nota por **módulo ou comportamento**, nunca por função. Função muda de
  nome e a nota apodrece.
- A nota descreve como funciona **hoje**. Não acumula histórico — nada de
  "antes era X, agora é Y".
- Sempre cita os arquivos-fonte reais. É o que permite encontrá-la depois.
- A nota se sustenta sozinha: quem lê só ela precisa entender.
- Onde há dúvida, fica `⚠️ confirmar`. Nunca chute.

---

## Modelo a usar

- **`lista` e `nota`** (carga inicial): o modelo mais forte disponível. É aqui
  que a documentação nasce do zero, e modelo fraco inventa comportamento que o
  código não tem. Acontece uma vez por projeto — não vale economizar.
- **`doc`** (dia a dia): Sonnet dá conta. Ele já tem a nota pronta como
  referência e só ajusta o que mudou.

Troque com `/model` antes de rodar.

---

## Perguntas frequentes

**Rodei o `doc` e ele não mexeu em nada.**
Se a mudança não alterou nenhum comportamento documentado, está certo. Só
desconfie se algo grande mudou e nenhuma nota foi tocada.

**Ele criou uma nota nova de um assunto que já tinha nota.**
O grep não achou a existente. Apague a duplicada; o próximo `/docs-kit:doc` acha
a certa. Vale olhar com atenção todo arquivo novo em `docs/referencia/` que
aparecer no PR.

**Troquei de branch antes de rodar o `doc`.**
`git switch <branch-anterior>`, roda o comando, commita, volta.

**Trabalhei em várias branches.**
O comando só enxerga a branch onde você está agora. Rode uma vez dentro de cada
uma.

**Mudou a branch base do projeto.**
Edite `docs/.base`. Não precisa reinstalar o plugin.

**Dois devs editaram a mesma nota.**
Conflito de git normal em markdown, resolve na mão. É raro, porque cada nota é
de um módulo e dois devs raramente mexem no mesmo módulo no mesmo dia.

**O `/docs-kit:mapa` não funciona.**
Ele depende do Archify, que se instala à parte (veja Instalação). Os outros
comandos não dependem de nada além do Claude Code.
