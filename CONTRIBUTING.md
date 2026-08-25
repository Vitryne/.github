# Como contribuir - Vitryne

Documento único da equipe. Vale para todos os repositórios.

> Este arquivo mora em `vitryne/.github` e é propagado automaticamente pelo
> GitHub para todos os repositórios da organização.

As regras deste documento derivam do **Regulamento da Escola de TI 2026**.
Descumprimentos são registrados como não-conformidades (NC) e descontam nota.
As regras que correspondem diretamente a uma NC estão marcadas com 🔻.

---

## Estrutura de branches

```
main ────●─────────────────────●──────────  releases
          \                   /
dev ──●──●──●──●──●──●───●─────────────  integração
              \     \     \
               ●     ●     ●                feat/ETI-183-...
```

| Branch | Papel |
|---|---|
| `main` | Somente releases. Ninguém trabalha direto aqui |
| `dev` | Integração do time. **Toda tarefa nasce e volta para cá** |
| `tipo/ETI-XXX-desc` | Uma tarefa do Jira. Nasce de `dev`, morre em `dev` |

🔻 **Toda tarefa é executada em uma branch separada, derivada de `dev`.**
Trabalhar em uma task sem a branch correspondente é NC **Grave**.

## O fluxo

### 1. Pegue o card no Jira

🔻 A tarefa precisa **já existir no Jira** antes de você começar. Issues criadas
à revelia (fora do planning ou sem o scrum master) são NC **Grave**.

Mova o card para "Em andamento". Uma branch = um card.

🔻 Mantenha o status do card atualizado conforme o trabalho avança. Não fazer
isso é NC.

### 2. Atualize a `dev` - não pule este passo

```bash
git checkout dev
git pull origin dev
```

### 3. Crie a branch

```bash
git checkout -b feat/ETI-183-cadastro-lojista
```

Formato: `tipo/ETI-XXX-descricao-curta`

Minúsculas, sem acento, hífen como separador, até ~50 caracteres.

```
feat/ETI-183-cadastro-lojista
fix/ETI-192-calculo-frete
docs/ETI-203-atualizar-readme
refactor/ETI-210-extrair-service-pedido
chore/ETI-215-configurar-eslint
```

### 4. Trabalhe e commite

🔻 **Faça pelo menos um commit e um push a cada dia trabalhado.** Ficar dias
com trabalho só na sua máquina é NC **Grave** - e o desconto é por ocorrência.

```bash
git add .
git commit -m "ETI-183 feat: adiciona formulário de cadastro de lojista"
git push
```

### 5. Review

- **1 aprovação** obrigatória, de quem não é o autor
- Responda em até **24h úteis** — "vou olhar hoje à noite" já conta
- Ninguém revisou em 24h? Cobre no canal. Revisar é trabalho, não favor

### 6. Merge

**Merge pull request** - é a única opção habilitada.

🔻 O merge **precisa gerar um commit de merge** (`--no-ff`). Merge sem
`--no-ff` é NC. Por isso squash e rebase estão desabilitados no GitHub: a
única opção disponível já é a correta.

**Consequência prática:** todos os commits da sua branch vão para a
`dev` e ficam no histórico permanentemente. Eles não são fundidos nem
apagados. Por isso a mensagem de cada commit importa - veja a seção abaixo.

---

## Mensagem de commit

🔻 *"Se os commits não descreverem uma prévia do que está sendo commitado"* é NC.

Como usamos merge commit, **cada commit seu vai para o histórico do projeto**.

### Formato

```
ETI-XXX tipo: descrição em minúscula, no imperativo, sem ponto final
```

```
ETI-161 feat: adiciona validação de CPF no cadastro
ETI-192 fix: corrige cálculo de frete em rotas com contorno
ETI-203 docs: atualiza documentação dos endpoints de pedido
```

| Tipo | Quando usar |
|---|---|
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

```
wip
teste
agora vai
arrumando
ajustes
```

Nenhum deles descreve o que foi feito. Todos são NC.

### Regra prática

Um commit por unidade de trabalho que faça sentido sozinha. Se a mensagem
precisa de "e", provavelmente são dois commits.

Em dúvida entre dois tipos? Escolha um e siga. O valor está na consistência
do formato, não na taxonomia perfeita.

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

Mesmo formato da Branch:

```
feat/ETI-183-cadastro-lojista
```

---

## Como revisar bem

Nesta ordem:

1. Faz o que o card do Jira pediu?
2. Quebra algo que já existia? (assinatura de método, contrato de API)
3. A regra de negócio está correta?
4. Tem credencial, URL ou senha hardcoded?
5. As mensagens de commit descrevem o que foi feito?
6. Dá para entender daqui a seis meses?

O item 5 é novo e importa: os commits vão para o histórico, e commit ruim
aprovado no review vira NC do time inteiro.

**Não revise** formatação, aspas ou ponto e vírgula. Isso é trabalho do
linter, e discussão de estilo em review só desgasta a equipe.

---

## Release: `dev` → `main`

A `main` recebe apenas releases, ao fim de cada sprint ou quando o time
decidir que há um conjunto estável.

1. Abre-se um PR de `dev` para `main`
2. Título: `ETI-XXX chore: release sprint N`
3. Mesma exigência de aprovação
4. Merge commit, como qualquer outro

Nada é desenvolvido diretamente na `main`.

### Hotfix

Correção urgente de algo que está na `main`:

1. Branch a partir da `main`: `fix/ETI-XXX-descricao`
2. PR para `main`
3. Depois do merge, **abra um segundo PR levando a correção para `dev`**

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
|---|---|
| `.env` | Só na sua máquina |
| Chaves do gateway de pagamento | `.env` |
| Senha do banco, `JWT_SECRET` | `.env` |
| `google-services.json`, keystore Android | Local |
| `*.pem`, `*.key`, `id_rsa` | Lugar nenhum |
| **Dump de banco com CPF/CNPJ real** | Lugar nenhum |

Adicionou variável de ambiente nova? **Adicione também no `.env.example`**,
sem o valor. Senão quebra o setup dos outros seis.

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
|---|---|---|
| `git reset --soft HEAD~1` | Desfaz o commit, **mantém as alterações** | Seguro. Errou a mensagem antes do push |
| `git reset --hard` | Desfaz o commit **e apaga as alterações** | Só na sua branch local. **Nunca em `main` ou `dev`** |
| `git push --force` | Sobrescreve o histórico remoto | **Bloqueado.** Único jeito de perder trabalho no Git |
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
