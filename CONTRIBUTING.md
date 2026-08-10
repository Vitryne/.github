# Como contribuir - Vitryne

O que é descrito aqui, valm para todos os repositórios.

> Este arquivo mora em `vitryne/.github` e é propagado automaticamente

---

## O fluxo

### 1. Pegue o card no Jira

Mova para "Em andamento". Uma branch = um card.

### 2. Atualize a `main` — **não pule este passo**

```bash
git checkout main
git pull origin main
```

### 3. Crie a branch

```bash
git checkout -b feat/cadastro-lojista
```

Formato: `tipo/descricao-curta`
Minúsculas, sem acento, hífen como separador.

### 4. Trabalhe e commite

```bash
git commit -m "feat(ETI-183): criar formulário de cadastro de lojista"
```

Os commits da branch **somem no squash**. Não tem problema os commits com `wip` pois eles não chegam na `main`. O que sempre irá ficar é o titulo do PR.

### 5. Review

- Requer sempre **1 aprovação** do responsável
- Responda em até **24h úteis** — "vou olhar hoje à noite" já conta
- Ninguém revisou em 24h? Cobre no canal. Revisar é trabalho, não favor

### 6. Merge

**Squash and merge** - é a única opção habilitada. O autor clica no
botão, ou use **Enable auto-merge**.

## Título do PR

**O título do PR vira o commit da `main`.** É o único ponto validado
automaticamente, e o que fica no histórico para sempre.

```bash
tipo(ETI-XXX): descrição em minúscula, no imperativo, sem ponto final
```

⚠️ **Sem espaço antes do parêntese.** `feat(ETI-183)`, não `feat (ETI-183)`.

```bash
feat(ETI-183): permitir cadastro de lojista com CNPJ
fix(ETI-192): corrigir cálculo de frete em rotas com contorno
docs(ETI-203): documentar endpoints de pedido
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

Em dúvida entre dois tipos? Escolha um e siga.

---

## Trabalho que atravessa repositórios

1. **Mesma chave Jira** nos dois repositórios
2. **Backend mergeia primeiro, sempre.**
3. Marque a dependência na descrição (Caso se aplicar):
   `🔗 Depende de vitryne/backend#42 - não mergear antes`
