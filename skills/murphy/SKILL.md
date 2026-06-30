---
name: murphy
description: Pre-mortem de uma request de código antes de implementar. Use SEMPRE antes de usar qualquer ferramenta (editar arquivo, rodar bash, migration, etc.) quando a request envolver ações potencialmente destrutivas ou irreversíveis (deletar, sobrescrever, drop, force push, alterar schema, alterar API pública/contrato existente), ambiguidade real sobre o que o usuário quer, ou mudanças que podem afetar código/comportamento não mencionado na request. Não precisa ativar para requests simples, bem especificadas, reversíveis ou de baixo risco (ex: criar arquivo novo, adicionar função isolada, ler código, perguntas conceituais). Também pode ser chamada manualmente com /murphy.
---

# Murphy

"Se algo pode dar errado, vai dar errado." Esta skill faz um pre-mortem de uma request de código antes de qualquer ferramenta ser usada (editar arquivo, rodar bash, etc), mas só quando o risco justifica a pausa.

## Quando ativar (modo automático)

Ative SEMPRE que a request envolver pelo menos um destes sinais, antes de tocar em qualquer ferramenta:
- Ação destrutiva ou difícil de reverter (delete, drop, overwrite, force push, migration, rm)
- Mudança em algo compartilhado/público (API pública, schema de banco, contrato entre módulos, config de produção)
- Ambiguidade real: mais de uma leitura razoável do que o usuário quer
- Mudança que pode ter efeito colateral fora do escopo explícito da request

NÃO ative para: criar arquivo novo isolado, adicionar função sem mexer no resto, leitura/explicação de código, perguntas conceituais, requests já bem especificadas e de baixo risco. Nesses casos, siga direto.

Pode também ser chamada manualmente com `/murphy` para forçar a análise em qualquer request.

## O checklist

Quando ativada, percorra estes cinco pontos:

1. **Suposições implícitas** — O que estou prestes a assumir que o usuário não disse explicitamente? (versão de linguagem, framework, formato de dados, comportamento em caso de erro, etc.)
2. **Ambiguidade** — Existe mais de uma leitura razoável do pedido? Qual interpretação eu escolheria e por quê?
3. **Reversibilidade** — A ação é fácil de desfazer (commit, branch, arquivo novo) ou é destrutiva/difícil de reverter (delete, drop, force push, sobrescrever arquivo sem backup, migration)?
4. **Efeitos colaterais** — Isso pode quebrar algo que não foi mencionado: outros arquivos, testes existentes, contratos de API, comportamento de outros usuários do código?
5. **Dependências frágeis** — Depende de algo que pode não existir, estar desatualizado, ou ter uma versão diferente da esperada (lib externa, schema de banco, variável de ambiente, arquivo de config)?

## Classificação de severidade

- **Leve** — vale mencionar, mas não impede de seguir.
- **Grave** — irreversível, destrutivo, ou pode quebrar algo importante sem o usuário saber.

## Regra de saída

- Só itens **leves**: liste rapidamente em 2-4 bullets no início da resposta e siga com a implementação normalmente.
- Pelo menos um item **grave**: pare antes de usar qualquer ferramenta. Explique o risco específico em 1-2 frases por item (sem virar lista enorme) e pergunte como o usuário quer prosseguir. Não presuma a resposta mais segura por conta própria.

## Formato de saída

```
🔍 Murphy check:
- [severidade] descrição curta do risco

[Se houver grave: pergunta direta de confirmação]
[Se só leve: segue direto para a implementação]
```

Seja objetivo. Esta skill existe para evitar erros silenciosos, não para burocratizar cada interação. Erre para o lado de não interromper requests simples — a skill perde valor se vira ruído constante.
