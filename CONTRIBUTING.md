# Contribuindo com Murphy

Obrigado pelo interesse em melhorar o Murphy! Contribuições são bem-vindas, principalmente em duas frentes: ajuste de gatilho (quando a skill dispara) e expansão do checklist de riscos.

## Antes de abrir um PR

1. Teste sua mudança usando os casos em [`TESTS.md`](./TESTS.md) — confira que os testes negativos (casos triviais) continuam passando direto, sem interrupção. A skill perde valor se virar ruído.
2. Se você mudou a `description` de alguma variante, rode os 8 casos manualmente (ou via `evals/evals.json` com o skill-creator) e descreva nos comentários do PR o que mudou no comportamento.
3. Mantenha as três variantes (`manual`, `guarded`, `watchful`) consistentes no corpo das instruções — só a seção "Quando ativar" e a `description` do frontmatter devem diferir entre elas.

## Tipos de contribuição bem-vindos

- **Falsos positivos/negativos**: relatos de prompts onde Murphy travou à toa ou deixou passar algo arriscado. Abra uma issue com o prompt exato, a variante usada e o comportamento esperado.
- **Novos itens de checklist**: se você identificar uma categoria de risco recorrente que não está nos 5 pontos atuais (suposições, ambiguidade, reversibilidade, efeitos colaterais, dependências frágeis), proponha via issue antes de implementar — queremos manter o checklist enxuto.
- **Traduções**: variantes do `SKILL.md` em outros idiomas são bem-vindas em `variants/i18n/<idioma>/`.
- **Melhorias de description**: a `description` da variante guarded é a parte mais sensível (decide quando o Claude ativa a skill automaticamente). PRs que ajustam isso devem incluir evidência de teste (antes/depois).

## Estrutura do projeto

```
murphy-skill/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── TESTS.md
├── CHANGELOG.md
├── variants/
│   ├── SKILL-manual.md
│   ├── SKILL-guarded.md
│   └── SKILL-watchful.md
├── evals/
│   └── evals.json
└── assets/
    └── banner.svg
```

## Reportando problemas

Abra uma issue com:
- Variante instalada
- Prompt usado
- Comportamento esperado vs. observado
- Versão do Claude Code, se relevante

## Código de conduta

Seja respeitoso. Issues e PRs com tom hostil serão fechados sem aviso.
