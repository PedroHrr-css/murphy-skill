---
name: murphy
description: Pre-mortem manual de uma request de código. Analisa suposições implícitas, ambiguidades, reversibilidade e efeitos colaterais antes de implementar. Só ativa quando o usuário invocar /murphy explicitamente.
disable-model-invocation: true
argument-hint: [request opcional para analisar]
---

# Murphy

"Se algo pode dar errado, vai dar errado." Esta skill faz um pre-mortem de uma request de código antes de qualquer ferramenta ser usada (editar arquivo, rodar bash, etc).

## Quando usar

Só quando o usuário digitar `/murphy`. Se vier um argumento (`$ARGUMENTS`), analise essa request específica. Se não vier argumento, analise a última request relevante da conversa.

## O checklist

Antes de propor ou executar qualquer implementação, percorra estes cinco pontos:

1. **Suposições implícitas** — O que estou prestes a assumir que o usuário não disse explicitamente? (versão de linguagem, framework, formato de dados, comportamento em caso de erro, etc.)
2. **Ambiguidade** — Existe mais de uma leitura razoável do pedido? Qual interpretação eu escolheria e por quê?
3. **Reversibilidade** — A ação é fácil de desfazer (commit, branch, arquivo novo) ou é destrutiva/difícil de reverter (delete, drop, force push, sobrescrever arquivo sem backup, migration)?
4. **Efeitos colaterais** — Isso pode quebrar algo que não foi mencionado: outros arquivos, testes existentes, contratos de API, comportamento de outros usuários do código?
5. **Dependências frágeis** — Depende de algo que pode não existir, estar desatualizado, ou ter uma versão diferente da esperada (lib externa, schema de banco, variável de ambiente, arquivo de config)?

## Classificação de severidade

Para cada item identificado, classifique como:
- **Leve** — vale mencionar, mas não impede de seguir.
- **Grave** — irreversível, destrutivo, ou pode quebrar algo importante sem o usuário saber.

## Regra de saída

- Se só houver itens **leves**: liste rapidamente em 2-4 bullets no início da resposta e siga com a implementação normalmente.
- Se houver **pelo menos um item grave**: pare antes de usar qualquer ferramenta. Explique o risco específico em 1-2 frases por item (sem virar uma lista enorme — só o essencial) e pergunte como o usuário quer prosseguir. Não presuma a resposta mais segura por conta própria.

## Formato de saída

```
🔍 Murphy check:
- [severidade] descrição curta do risco

[Se houver grave: pergunta direta de confirmação]
[Se só leve: segue direto para a implementação]
```

Seja objetivo. Esta skill existe para evitar erros silenciosos, não para burocratizar cada interação.
