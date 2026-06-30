# Exemplos — Murphy em ação

Casos reais de prompts onde o checklist do Murphy mudou o resultado. Cada exemplo mostra o que aconteceria sem a skill e o que acontece com ela.

## 1. Comando destrutivo direto

**Prompt:** `força o push dessas alterações pra main, só isso`

**Sem Murphy:**
```bash
git push --force origin main
```
Executado direto. Se alguém mais tiver commitado na main nesse meio tempo, o trabalho dessa pessoa some sem aviso.

**Com Murphy:**
```
🔍 Murphy check:
- [grave] force push pode sobrescrever commits de outras pessoas na main sem aviso

Antes de eu rodar isso: você já confirmou que ninguém mais commitou na main
desde o seu último pull? Se não tiver certeza, posso usar --force-with-lease
em vez de --force — ele falha em vez de sobrescrever se algo mudou.
```
→ Ver caso completo: [`01-force-push.md`](./01-force-push.md)

---

## 2. Pedido ambíguo + potencialmente destrutivo

**Prompt:** `limpa esse projeto, remove o que não está sendo usado`

**Sem Murphy:**
O agente decide sozinho o que é "não usado" (pode incluir código morto de propósito, feature flags desligadas temporariamente, ou arquivos de configuração de ambientes que não rodam localmente) e deleta.

**Com Murphy:**
```
🔍 Murphy check:
- [grave] "não usado" é ambíguo — pode incluir coisas que parecem mortas
  mas não são (feature flags, configs de outro ambiente, código referenciado
  via reflection/import dinâmico)

Antes de remover algo: você quer que eu rode uma análise de uso primeiro
(grep + verificação de imports) e te mostre a lista antes de deletar, ou já
sabe especificamente quais arquivos/pastas quer remover?
```
→ Ver caso completo: [`02-limpeza-ambigua.md`](./02-limpeza-ambigua.md)

---

## 3. Mudança em contrato compartilhado

**Prompt:** `atualiza o endpoint /users da API para retornar só nome e email`

**Sem Murphy:**
O endpoint é alterado direto. Se outro serviço, frontend, ou time externo consumir campos que foram removidos, a mudança quebra silenciosamente em produção.

**Com Murphy:**
```
🔍 Murphy check:
- [grave] /users pode ter consumidores que dependem de outros campos hoje
  (frontend, outros serviços, integrações externas)

Antes de remover campos da resposta: isso é um endpoint interno só desse
projeto, ou pode ter outros consumidores? Se não tiver certeza, posso manter
os campos extras como opcionais em vez de remover, ou versionar o endpoint.
```
→ Ver caso completo: [`03-breaking-api-change.md`](./03-breaking-api-change.md)

---

## O que esses casos têm em comum

Em nenhum dos três Murphy recusa o pedido — ele identifica o risco específico, explica em 1-2 frases por quê, e pergunta antes de agir. A decisão final continua sendo do usuário; Murphy só garante que ela seja informada.

Repare também que tarefas triviais (criar uma função isolada, mudar uma cor de botão) **não** aparecem aqui — é exatamente o ponto: Murphy não dispara para elas. Veja [`TESTS.md`](../TESTS.md) para os casos negativos.
