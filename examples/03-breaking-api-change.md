# Caso 3 — Alterar resposta de um endpoint existente

**Variante:** guarded (dispara automaticamente)
**Categoria de risco:** Efeitos colaterais

## Prompt do usuário

```
atualiza o endpoint /users da API para retornar só os campos nome e email
```

## Por que isso é arriscado

`/users` já existe e provavelmente já tem consumidores: o frontend do próprio
projeto, outros serviços internos, ou até integrações de terceiros. Remover
campos da resposta é uma mudança *breaking* — quem depende dos campos
removidos (`id`, `created_at`, `role`, o que for) quebra sem aviso, muitas
vezes só percebido em produção.

A request em si não é ambígua (o pedido é claro), mas o efeito colateral fora
do escopo mencionado é real: o usuário só falou do endpoint, não de quem o
consome.

## Comportamento sem Murphy

O endpoint é alterado direto, response simplificado para os dois campos
pedidos. Funciona perfeitamente nos testes do próprio projeto — porque os
testes também só checam o que foi pedido.

## Comportamento com Murphy

```
🔍 Murphy check:
- [grave] /users pode ter consumidores que dependem de outros campos hoje
  (frontend, outros serviços, integrações externas)

Antes de remover campos da resposta: isso é um endpoint interno só desse
projeto, ou pode ter outros consumidores? Se não tiver certeza, posso manter
os campos extras como opcionais em vez de remover, ou criar uma versão nova
do endpoint (ex: /v2/users) em vez de alterar a existente.
```

## O que o usuário pode responder

- "É só esse projeto, pode remover" → Murphy aplica a mudança direto
- "Não tenho certeza" → Murphy sugere manter compatibilidade (campos extras
  ignoráveis) em vez de remover, ou propõe versionar o endpoint
- "Cria uma v2 mesmo" → Murphy implementa a nova versão sem tocar na atual

## Por que isso vale o checklist

Ponto 4 do checklist (efeitos colaterais): a pergunta não é "o que o usuário
pediu está certo", é "o que mais essa mudança pode afetar que não foi
mencionado". Mudanças em contratos compartilhados (APIs, schemas, interfaces
públicas) são o caso clássico onde isso importa.
