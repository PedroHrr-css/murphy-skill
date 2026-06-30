---
name: murphy
description: Pre-mortem de toda request de código antes de implementar. Use esta skill antes de usar qualquer ferramenta (editar arquivo, rodar bash, criar arquivo, instalar dependência, rodar migration, etc.) em praticamente qualquer tarefa de desenvolvimento, mesmo as que parecem simples — o objetivo é nunca pular a checagem de suposições e riscos antes de agir. Ative também para requests de refatoração, criação de features, correção de bugs, scripts, configuração de ambiente e qualquer mudança em código existente. Pode ser chamada manualmente com /murphy.
---

# Murphy

"Se algo pode dar errado, vai dar errado." Esta skill faz um pre-mortem de uma request de código antes de qualquer ferramenta ser usada (editar arquivo, rodar bash, etc). No modo watchful, isso roda em praticamente toda tarefa de desenvolvimento — o ganho vem da consistência, não da seletividade.

## Quando ativar

Em qualquer request de desenvolvimento, antes de tocar em qualquer ferramenta: criar, editar, deletar, rodar comandos, instalar dependências, configurar ambiente, refatorar, corrigir bugs. Exceções razoáveis: leitura pura de código, perguntas conceituais sem nenhuma ação a seguir.

Pode também ser chamada manualmente com `/murphy`.

## O checklist

Percorra estes cinco pontos antes de agir:

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
- Pelo menos um item **grave**: pare antes de usar qualquer ferramenta. Explique o risco específico em 1-2 frases por item e pergunte como o usuário quer prosseguir.

## Formato de saída

```
🔍 Murphy check:
- [severidade] descrição curta do risco

[Se houver grave: pergunta direta de confirmação]
[Se só leve: segue direto para a implementação]
```

Nota: este modo é mais verboso por design — roda em quase tudo. Se sentir que está virando ruído, considere trocar para a variante guarded.
