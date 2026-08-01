---
description: Sincroniza a documentacao com o que mudou (o comando do dia a dia)
allowed-tools: Read, Write, Edit, Grep, Glob, Bash(git diff:*), Bash(git status:*), Bash(git branch:*), Bash(git merge-base:*), Bash(git log:*), Bash(cat:*)
---

Sincronize as notas de referencia deste projeto com as mudancas recentes.

Branch atual: !`git branch --show-current`
Branch base configurada: !`cat docs/.base 2>/dev/null || echo "(nenhuma — usando master/main)"`
Nao commitado: !`git status --short`
Commits desta branch: !`B=$(cat docs/.base 2>/dev/null | tr -d '[:space:]'); B=${B:-master}; git log --oneline $(git merge-base HEAD origin/$B 2>/dev/null || git merge-base HEAD origin/main 2>/dev/null)..HEAD 2>/dev/null | head -30`

Mudancas ja commitadas nesta branch (contra a base):
!`B=$(cat docs/.base 2>/dev/null | tr -d '[:space:]'); B=${B:-master}; git diff $(git merge-base HEAD origin/$B 2>/dev/null || git merge-base HEAD origin/main 2>/dev/null)...HEAD 2>/dev/null`

Mudancas ainda nao commitadas:
!`git diff HEAD`

A branch base vem de `docs/.base` (um arquivo com o nome da branch numa
linha). Se esse arquivo nao existir, cai em `master` e depois `main`.

Considere **as duas** listas juntas como "o que mudou". O fluxo aqui abre PR
automaticamente no push, entao boa parte do trabalho ja vai estar commitada
quando este comando rodar — isso e normal, nao e motivo para ignorar.

Se as duas estiverem vazias, diga isso em uma linha e pare.

## 1. Notas de referencia

Para cada comportamento, modulo ou funcionalidade tocado no diff acima:

1. Procure em `docs/referencia/` (Grep/Glob) a nota que descreve esse
   comportamento. Considere nomes parecidos e sinonimos antes de concluir
   que nao existe.
2. **Se existir**: atualize-a para refletir o estado ATUAL. Substitua o
   texto desatualizado. A nota descreve como funciona HOJE — ela nunca
   acumula historico ("antes era X, agora e Y" esta errado).
3. **Se nao existir**: crie `docs/referencia/<slug>.md` usando exatamente
   esta estrutura:

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

Regras para as notas:

- Ancore a nota em um **comportamento ou modulo**, nao em uma funcao
  isolada. Funcao muda de nome e a nota apodrece.
- Cite os arquivos-fonte reais (ex: `src/pricing/calc.ts`) para dar
  rastreabilidade.
- A nota tem que se sustentar sozinha: quem ler so ela precisa entender.
- Conclusao no topo, nao no fim.
- Nao invente. Documente apenas o que esta no diff e no codigo que voce
  leu. Onde tiver duvida, escreva `⚠️ confirmar` em vez de chutar.

## 2. Alerta de arquitetura

Verifique se o diff contem algum sinal de mudanca ESTRUTURAL. Prefira
avisar demais a deixar passar: um falso alarme custa o dev responder
"nao mudou nada", enquanto um alarme perdido deixa o diagrama mentindo.

Sinais a procurar:

**Dependencias e configuracao**
- `package.json`, `package-lock.json`, `bun.lock`, `requirements.txt`,
  `go.mod`, `pom.xml`: dependencia adicionada ou removida
- workspace/pacote novo em monorepo (`packages/`, `apps/`)
- `.env`, `.env.example`: variavel de ambiente nova ou removida
- `docker-compose.yml`, `Dockerfile`, manifests de k8s
- `ecosystem.config.js` / configuracao do PM2, systemd unit
- vhost de Apache/nginx, configuracao de proxy
- workflow de CI (`.github/workflows/`)

**Estrutura do codigo**
- diretorio de topo novo ou removido
- diretorio renomeado ou movido
- modulo inteiro deletado
- arquivo de entrada novo (`main`, `index`, `server`, `app`)

**Rotas e interfaces**
- arquivo de rota/controller novo ou removido
- registro de rota novo (`app.register`, `router.use`, `@Controller`)
- endpoint de webhook novo
- rota publica que virou autenticada, ou o contrario

**Dados**
- migration nova
- tabela, coluna, indice ou view criados ou removidos
- schema do Prisma/ORM alterado
- banco novo, conexao nova, mudanca de host de banco

**Integracoes e servicos externos**
- cliente de API externa novo (SDK, `fetch` para dominio novo)
- credencial/token de servico novo
- fila, worker, cron ou job agendado novo ou removido
- cache (Redis, memoria) introduzido ou removido
- storage de arquivo, envio de e-mail, SMS, push, WhatsApp

**Seguranca e limites**
- middleware novo (auth, rate limit, CORS, validacao)
- mecanismo de autenticacao alterado
- limite de confianca mudou (servico interno exposto, ou fechado)

Se encontrar QUALQUER um desses sinais, termine a resposta com:

```
⚠️ Possivel mudanca de arquitetura: <o que voce viu, em uma linha>
Se o fluxo mudou de fato, rode /docs-kit:mapa. Se nao mudou, ignore.
```

Voce NAO atualiza diagrama nenhum aqui — apenas avisa.

## 3. Feche

Liste em poucas linhas: notas criadas e notas atualizadas.
Nao commite — quem commita e o dev.

O PR deste repositorio abre automaticamente no push, entao nao ha nada a
colar em lugar nenhum: basta que as notas estejam no mesmo commit.
