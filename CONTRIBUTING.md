# Como contribuir - Vitryne

Documento único da equipe. Vale para todos os repositórios.

> Este arquivo mora em `vitryne/.github` e é propagado automaticamente pelo
> GitHub para todos os repositórios da organização.

As regras deste documento derivam do [**Regulamento da Escola de TI 2026**](https://github.com/Vitryne/docs/blob/main/Regulamenta%C3%A7%C3%A3o/Regulamento%20-%20ETI.pdf).
Descumprimentos são registrados como não-conformidades (NC) e descontam nota.
As regras que correspondem diretamente a uma NC estão marcadas com 🔻.

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

🔻 **Toda tarefa é executada em uma branch separada, derivada de `dev`.**
Trabalhar em uma task sem a branch correspondente é NC **Grave**.

> **Exceção:** os repositórios `docs` e `.github` não têm `dev`, só `main`.
> Neles a branch da tarefa nasce da `main` e o PR volta para a `main`.
> O resto do fluxo (branch por card, review, merge commit) é igual.

## O fluxo

### 1. Pegue o card no Jira

🔻 A tarefa precisa já existir no Jira antes de você começar. Issues criadas fora do planejamento ou sem a autorização do responsável definido pelo time são NC graves, conforme o regulamento.

Mova o card para "Em andamento". Uma branch corresponde a um card.

🔻 Mantenha o status do card atualizado conforme o trabalho avança. Não fazer
isso é NC.

### 2. Atualize a `dev` - não pule este passo

```bash
git checkout dev
git pull origin dev
```

Nos repositórios docs e .github, utilize main no lugar de dev.

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

🔻 **Faça pelo menos um commit e um push a cada dia trabalhado.** Deixar o trabalho exclusivamente na máquina local por dias é NC **grave**, com desconto por ocorrência, conforme o regulamento.

```bash
git status
git add src/caminho/do/arquivo.tsx
git commit -m "ETI-183 feat: adiciona formulário de cadastro de lojista"
```

No **primeiro push** da branch, avise o Git para onde ela vai:

```bash
git push -u origin feat/ETI-183-cadastro-lojista
```

Daí em diante, basta `git push`.

### 5. Review

- **Uma aprovação** obrigatória, de quem não é o autor
- **Checks do CI verdes.** PR com check falhando não pode ser mergeado
- O revisor deve responder em até 24 horas úteis.
- Se não houver retorno nesse prazo, solicite apoio no canal do projeto.

### 6. Merge

Utilize a opção **Merge pull request**, que gera um merge commit conforme a configuração do repositório.

🔻 O merge deve preservar o commit de merge (--no-ff). Fazer merge sem gerar esse commit é NC. Por isso, squash e rebase estão desabilitados no GitHub.

O commit de merge leva só o título do PR. A descrição fica no PR, no GitHub.

**Consequência prática:** o merge commit normalmente preserva os commits individuais da branch no histórico do projeto. Por isso, a mensagem de cada commit precisa descrever adequadamente o que foi alterado.

---

## Mensagem de commit

🔻 Commits que não descrevem adequadamente as alterações realizadas podem configurar NC, conforme o regulamento.

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
| `chore` | Tarefa sem impacto no código de produção |
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

Nenhum deles descreve o que foi feito. Todos são NC.

### Regra prática

Faça commits por unidades de trabalho que façam sentido individualmente. Se uma mensagem precisar descrever várias alterações independentes, considere separá-las em commits distintos.

Em caso de dúvida entre dois tipos, escolha o mais adequado e mantenha a consistência do padrão.

### Errou a mensagem?

Se **ainda não deu push**:

```bash
git commit --amend -m "ETI-183 feat: mensagem corrigida"
```

Seguro - o commit só existe na sua máquina.

Se **já deu push**: deixe como está e avise no canal. Corrigir exigiria
reescrever o histórico e `push --force`, que está bloqueado justamente para
ninguém perder trabalho.

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

Todo PR passa pelo check **`governanca / Governança`**, que confere:

| O quê | Fora do padrão |
| --- | --- |
| Título do PR | Bloqueia o merge |
| Nome da branch | Bloqueia o merge |
| Mensagens de commit | Só avisa, com uma anotação amarela no PR |

**Título errado:** edite o título do PR. O check roda de novo sozinho.

**Branch com nome errado:** o nome não pode ser trocado. Crie uma branch nova
a partir da atual, que já leva todos os commits, e abra outro PR:

```bash
git checkout -b feat/ETI-183-nome-certo
git push -u origin feat/ETI-183-nome-certo
```

Feche o PR antigo com um comentário apontando para o novo.

**Commit fora do padrão:** não bloqueia, porque um commit já enviado só se
corrige com `push --force`, que é proibido. O revisor cobra, e os próximos
commits saem no padrão.

---

## Como revisar bem

Nesta ordem:

1. Faz o que o card do Jira pediu?
2. Quebra algo que já existia? (assinatura de método, contrato de API)
3. A regra de negócio está correta?
4. Tem credencial, URL ou senha hardcoded?
5. As mensagens de commit descrevem o que foi feito?
6. Dá para entender daqui a seis meses?

O item 5 importa porque os commits vão para o histórico, e commit ruim
aprovado no review vira NC do time inteiro.

**Não revise** formatação, aspas ou ponto e vírgula. Isso é trabalho do
linter, e discussão de estilo em review só desgasta a equipe.

---

## Release: `dev` → `main`

A `main` recebe apenas releases, ao fim de cada sprint ou quando o time
decidir que há um conjunto estável.

1. Abre-se um PR de `dev` para `main`
2. Título: `chore/ETI-XXX-release-sprint-n` (ex.: `chore/ETI-310-release-sprint-12`)
3. Mesma exigência de aprovação
4. Merge commit, como qualquer outro

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

---

## Segurança — regras inegociáveis

| Nunca vai para o Git | Onde fica |
| --- | --- |
| `.env` | Só na sua máquina |
| Chaves do gateway de pagamento | `.env` |
| Senha do banco, `JWT_SECRET` | `.env` |
| `google-services.json`, keystore Android | Local |
| `*.pem`, `*.key`, `id_rsa` | Lugar nenhum |
| **Dump de banco com CPF/CNPJ real** | Lugar nenhum |

Adicionou variável de ambiente nova? **Adicione também no `.env.example`**,
sem o valor. Senão quebra o setup do resto do time.

### Se uma credencial vazar

1. **Invalide a credencial agora.** Rotacione a chave, troque a senha. Este é
   o passo 1, não o 3
2. Avise a equipe
3. Só então se preocupe com o histórico

Deletar o arquivo e commitar de novo **não resolve** - o conteúdo continua
acessível em qualquer commit anterior.

### Dados pessoais

O projeto lida com CPF, CNPJ e dados bancários. Em repositório público, um
`seed.sql` com dado real de colega é vazamento de verdade, não exercício
acadêmico. Use gerador de dados fake.

---

## Comandos que exigem cuidado

| Comando | O que faz | Quando usar |
| --- | --- | --- |
| `git reset --soft HEAD~1` | Desfaz o commit, **mantém as alterações** | Seguro. Errou a mensagem antes do push |
| `git reset --hard` | Desfaz o commit **e apaga as alterações** | Só na sua branch local. **Nunca em `main` ou `dev`** |
| `git push --force` | Sobrescreve o histórico remoto | **Bloqueado** em `main` e `dev`, **proibido** nas demais. Único jeito de perder trabalho no Git |
| `git branch -d` | Deleta **se já foi mergeada** | Seguro. Use sempre este |
| `git branch -D` | Deleta **mesmo sem merge** | Perde o que não foi mergeado |
| `git commit --amend` | Substitui o último commit | Só antes do push |
| `git revert <hash>` | Cria um commit que **desfaz** outro | Forma correta de reverter algo já mergeado |
| `docker compose down -v` | Para os containers **e apaga o banco** | Só se quiser zerar os dados |

**A regra que resume tudo:** se o comando reescreve algo que você já enviou
para o GitHub, pare e pergunte no canal antes.

---

## Ritmo de sprint

Sprints de duas semanas, com review e retrospectiva ao final.

- Branch dura **2–3 dias**, nunca atravessa a sprint
- PR não fechou até o fim da sprint? Ou fecha, ou volta ao backlog
- Não acumule PRs para o último dia, cinco PRs na véspera significa cinco
  aprovações automáticas
- **Nada de merge sem review em véspera de entrega**

🔻 **Aponte as horas no Jira no dia em que trabalhou**, nunca consolidado no
fim da semana. Apontamento inconsistente é NC.

🔻 **Compareça às reuniões do time.** Máximo de 1 falta a cada 4. Faltar às
reuniões bimestrais de apresentação do projeto faz a equipe inteira perder a
nota do bimestre.
