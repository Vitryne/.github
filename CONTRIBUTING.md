# Como contribuir - Vitryne

Documento único da equipe. Vale para todos os repositórios.

> Este arquivo mora em `vitryne/.github` e é propagado automaticamente pelo
> GitHub para todos os repositórios da organização.

---

## Estrutura de branches

```txt
main ────●─────────────────────●──────────  releases
          \                   /
dev ──●──●──●──●──●──●───●─────────────  integração
              \     \     \
               ●     ●     ●                feat/ETI-183-...
```

| Branch | Papel |
| --- | --- |
| `main` | Somente releases. Ninguém trabalha direto aqui |
| `dev` | Integração do time. **Toda tarefa nasce e volta para cá** |
| `tipo/ETI-XXX-desc` | Uma tarefa do Jira. Nasce de `dev`, morre em `dev` |

**Toda tarefa é executada em uma branch separada, derivada de `dev`.** A `dev` é a branch padrão dos repositórios `frontend`, `backend` e `mobile`: um clone novo já começa nela, e todo PR novo já sugere a `dev` como destino.

> **Exceção:** os repositórios `docs` e `.github` não têm `dev`, só `main`.
> Neles a branch da tarefa nasce da `main` e o PR volta para a `main`.
> O resto do fluxo (branch por card, review, merge commit) é igual.

## O fluxo

### 1. Pegue o card no Jira

A tarefa precisa já existir no Jira antes de você começar, criada no planejamento da sprint.

Mova o card para "Em andamento" e mantenha o status atualizado conforme o trabalho avança. Uma branch corresponde a um card.

### 2. Atualize a `dev` - não pule este passo

```bash
git checkout dev
git pull origin dev
```

Nos repositórios `docs` e `.github`, utilize `main` no lugar de `dev`.

### 3. Crie a branch

```bash
git checkout -b feat/ETI-183-cadastro-lojista
```

Formato: `tipo/ETI-XXX-descricao-curta`

Minúsculas, sem acento, hífen como separador, até ~50 caracteres.

```txt
feat/ETI-183-cadastro-lojista
fix/ETI-192-calculo-frete
docs/ETI-203-atualizar-readme
refactor/ETI-210-extrair-service-pedido
chore/ETI-215-configurar-eslint
```

### 4. Trabalhe e commite

**Faça pelo menos um commit e um push a cada dia trabalhado.** Trabalho que fica só na sua máquina não aparece para o time e se perde se algo der errado.

```bash
git status
git add src/caminho/do/arquivo.tsx
git commit -m "ETI-183 feat: adiciona formulário de cadastro de lojista"
```

Prefira `git add` com o caminho de cada arquivo em vez de `git add .`, para que só entre no commit o que você pretende.

No **primeiro push** da branch, avise o Git para onde ela vai:

```bash
git push -u origin feat/ETI-183-cadastro-lojista
```

Daí em diante, basta `git push`.

No `frontend` e no `mobile`, um hook de pre-commit formata e corrige o lint dos arquivos do commit automaticamente. Veja [Hook de pre-commit](#hook-de-pre-commit).

### 5. Review

- **Uma aprovação** obrigatória, de quem não é o autor, entre os code owners do repositório
- **Checks do CI verdes.** PR com check falhando não pode ser mergeado
- O revisor deve responder em até 24 horas úteis
- Se não houver retorno nesse prazo, solicite apoio no canal do projeto

No repositório `.github`, mantido por uma única pessoa, o PR é obrigatório, mas não exige aprovação.

### 6. Merge

Utilize a opção **Merge pull request**, que gera um merge commit. O merge sempre preserva o commit de merge (`--no-ff`): squash e rebase estão desabilitados por configuração.

O commit de merge leva só o título do PR. A descrição fica no PR, no GitHub.

**Consequência prática:** o merge commit preserva os commits individuais da branch no histórico do projeto. Por isso, a mensagem de cada commit precisa descrever adequadamente o que foi alterado.

---

## Mensagem de commit

Como usamos merge commit, **cada commit seu vai para o histórico do projeto**.

### Formato

```txt
ETI-XXX tipo: descrição em minúscula, no imperativo, sem ponto final
```

```txt
ETI-161 feat: adiciona validação de CPF no cadastro
ETI-192 fix: corrige cálculo de frete em rotas com contorno
ETI-203 docs: atualiza documentação dos endpoints de pedido
```

| Tipo | Quando usar |
| --- | --- |
| `feat` | Funcionalidade nova para o usuário |
| `fix` | Correção de bug |
| `docs` | Só documentação |
| `refactor` | Muda o código sem mudar o comportamento |
| `test` | Adiciona ou corrige teste |
| `chore` | Tarefa sem impacto no código de produção, inclusive formatação |
| `build` | Dependências, Maven, package.json |
| `ci` | Workflows e pipeline |
| `perf` | Melhoria de performance |

### O que não pode

```txt
wip
teste
agora vai
arrumando
ajustes
```

Nenhum deles descreve o que foi feito.

### Regra prática

Faça commits por unidades de trabalho que façam sentido individualmente. Se uma mensagem precisar descrever várias alterações independentes, considere separá-las em commits distintos.

Em caso de dúvida entre dois tipos, escolha o mais adequado e mantenha a consistência do padrão.

### Errou a mensagem ou esqueceu um arquivo?

Se **ainda não deu push**:

```bash
git add <arquivo esquecido>
git commit --amend -m "ETI-183 feat: mensagem corrigida"
```

Seguro: o commit só existe na sua máquina.

Se **já deu push, mas o PR ainda não recebeu review**, você pode corrigir na sua própria branch e atualizar com:

```bash
git push --force-with-lease
```

O `--force-with-lease` recusa o push se alguém tiver enviado algo para a branch depois de você, então não sobrescreve trabalho de outra pessoa.

Se **o PR já recebeu review**, não reescreva o histórico: faça um commit novo com a correção. Em `main` e `dev`, o force push é bloqueado pelo GitHub.

---

## Título do PR

O título do PR deve seguir o mesmo padrão de nomenclatura da branch. O CI reprova PRs com títulos fora do padrão.

```txt
feat/ETI-183-cadastro-lojista
```

Vale também para os PRs que não saem de uma branch de tarefa, como o de
release: o título segue o mesmo formato, com o card do Jira correspondente.

---

## Verificações automáticas

Todo PR passa por checks que rodam no GitHub Actions. Os obrigatórios precisam estar verdes para o merge ser liberado.

| Check | O quê | Repositórios | Fora do padrão |
| --- | --- | --- | --- |
| `governanca / Governança` | Título do PR e nome da branch | todos | Bloqueia o merge |
| | Mensagens de commit | todos | Só avisa, com uma anotação amarela no PR |
| `qualidade / Qualidade` | Lint, tipos e build | `frontend` | Bloqueia o merge |
| | Lint e tipos | `mobile` | Bloqueia o merge |
| | Build e testes (`mvn verify`, com PostgreSQL) | `backend` | Bloqueia o merge |
| `qualidade / Formatação` | Prettier | `frontend`, `mobile` | Bloqueia o merge |

Os workflows ficam centralizados no repositório `.github` e são reaproveitados por todos os repositórios.

**Título errado:** edite o título do PR. O check roda de novo sozinho.

**Branch com nome errado:** o nome não pode ser trocado. Crie uma branch nova
a partir da atual, que já leva todos os commits, e abra outro PR:

```bash
git checkout -b feat/ETI-183-nome-certo
git push -u origin feat/ETI-183-nome-certo
```

Feche o PR antigo com um comentário apontando para o novo.

**Commit fora do padrão:** não bloqueia. O revisor cobra, e os próximos
commits saem no padrão.

**Qualidade falhou:** abra o check, veja qual passo ficou vermelho e rode o mesmo comando na sua máquina para ver o erro completo:

```bash
# frontend e mobile
npm run lint
npm run typecheck
npm run build        # só no frontend

# backend
mvn verify
```

**Formatação falhou:** rode o Prettier e commite o resultado:

```bash
npm run format
git add <arquivos formatados>
git commit -m "ETI-XXX chore: aplica formatação do prettier"
```

### Hook de pre-commit

No `frontend` e no `mobile`, cada `git commit` roda automaticamente `eslint --fix` e `prettier --write` nos arquivos que estão entrando no commit (`husky` + `lint-staged`). Os arquivos já entram formatados, e o commit é bloqueado se sobrar algum erro de lint.

O hook é instalado pelo `npm install`. Depois de clonar o projeto, ou se o hook não estiver rodando, execute `npm install` uma vez.

O check de formatação do CI continua como rede de segurança, para commits feitos com `--no-verify` ou pela interface do GitHub.

### Bypass das regras

Os administradores da organização (Gabriel Rodrigues e Hugo Zuin) podem ignorar as regras de proteção de um PR. O uso é restrito a **PRs de CI e governança**, quando a própria mudança precisa ser validada antes de existir uma aprovação possível. Quem usar o bypass deixa um comentário no PR explicando o motivo.

---

## Como revisar bem

Nesta ordem:

1. Faz o que o card do Jira pediu?
2. Quebra algo que já existia? (assinatura de método, contrato de API)
3. A regra de negócio está correta?
4. Tem credencial, URL ou senha hardcoded?
5. As mensagens de commit descrevem o que foi feito?
6. Dá para entender daqui a seis meses?

O item 5 importa porque os commits vão para o histórico, e um commit ruim
aprovado no review fica lá para sempre.

**Não revise** formatação, aspas ou ponto e vírgula. Isso é trabalho do
linter e do Prettier, e discussão de estilo em review só desgasta a equipe.

---

## Release: `dev` → `main`

A `main` recebe apenas releases, ao fim de cada sprint ou quando o time
decidir que há um conjunto estável.

1. Abre-se um PR de `dev` para `main`
2. Título: `chore/ETI-XXX-release-sprint-n` (ex.: `chore/ETI-310-release-sprint-12`)
3. Mesma exigência de aprovação e de checks
4. Merge commit, como qualquer outro
5. Depois do merge, crie uma tag anotada na `main` com a versão:

```bash
git checkout main
git pull origin main
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
```

Nada é desenvolvido diretamente na `main`.

### Hotfix

Correção urgente de algo que está na `main`:

1. Branch a partir da `main`: `fix/ETI-XXX-descricao`
2. PR para `main`
3. Depois do merge, **abra um segundo PR da mesma branch `fix/...` para a
   `dev`**, com o mesmo título

Se pular o passo 3, a correção some na próxima release porque `dev`
não a conhece.

---

## Trabalho que atravessa repositórios

1. **Mesma chave Jira** nos dois repositórios
2. **Backend mergeia primeiro, sempre.** Cliente nunca vai para a `dev`
   chamando endpoint que ainda não existe
3. Na descrição do PR, aponte o PR do outro repositório em Observações

---

## Segurança — regras inegociáveis

| Nunca vai para o Git | Onde fica |
| --- | --- |
| `.env` | Só na sua máquina |
| Chaves do gateway de pagamento | `.env` |
| Senha do banco | `.env` |
| `google-services.json`, keystore Android | Local |
| `*.pem`, `*.key`, `id_rsa` | Lugar nenhum |
| **Dump de banco com CPF/CNPJ real** | Lugar nenhum |

Adicionou variável de ambiente nova? **Adicione também no `.env.example`**,
sem o valor. Senão quebra o setup do resto do time.

Variáveis com prefixo `NEXT_PUBLIC_` (frontend) e `EXPO_PUBLIC_` (mobile) são
incorporadas ao código que vai para o usuário: nunca coloque segredos nelas.

Deletar o arquivo e commitar de novo **não resolve** - o conteúdo continua
acessível em qualquer commit anterior.

---

## Ritmo de sprint

Sprints de duas semanas, com review e retrospectiva ao final.

- PR não fechou até o fim da sprint? Ou fecha, ou volta ao backlog
- Não acumule PRs para o último dia: cinco PRs na véspera significam cinco
  revisões feitas às pressas
- **Nada de merge sem review**
- **Aponte as horas no Jira no dia em que trabalhou**, nunca consolidado no
  fim da semana
- **Compareça às reuniões do time**
