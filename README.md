<p align="center">
  <img src="assets/mascot.svg" alt="Mascote do Murphy" width="160">
</p>

<p align="center">
  <img src="assets/banner.svg" alt="Murphy — Pre-mortem de código para Claude Code" width="100%">
</p>

# 🔍 Murphy

> "Se algo pode dar errado, vai dar errado."

<p align="center">
  <a href="https://github.com/PedroHrr-css/murphy-skill/stargazers"><img src="https://img.shields.io/github/stars/PedroHrr-css/murphy-skill?style=flat-square&color=111111&label=stars" alt="Stars"></a>
  <a href="https://github.com/PedroHrr-css/murphy-skill/releases"><img src="https://img.shields.io/github/v/release/PedroHrr-css/murphy-skill?style=flat-square&color=111111&label=release" alt="Release"></a>
  <a href="./LICENSE"><img src="https://img.shields.io/badge/license-MIT-111111?style=flat-square" alt="MIT license"></a>
  <img src="https://img.shields.io/badge/claude--code-skill-111111?style=flat-square" alt="Claude Code skill">
</p>

Uma skill para [Claude Code](https://claude.com/claude-code) que faz um **pre-mortem** da sua request antes de qualquer alteração no código. Murphy analisa suposições implícitas, ambiguidades, reversibilidade e efeitos colaterais — e só te interrompe quando o risco é real.

## O que ela faz

Antes de usar qualquer ferramenta (editar arquivo, rodar bash, migration, etc.), Murphy passa a request por um checklist:

1. **Suposições implícitas** — o que está sendo assumido sem o usuário ter dito?
2. **Ambiguidade** — existe mais de uma leitura razoável do pedido?
3. **Reversibilidade** — dá pra desfazer fácil ou é destrutivo (delete, drop, force push)?
4. **Efeitos colaterais** — pode quebrar algo fora do escopo mencionado?
5. **Dependências frágeis** — depende de algo que pode não existir ou estar desatualizado?

Riscos **leves** são listados e o trabalho segue normalmente. Riscos **graves** (irreversíveis ou destrutivos) fazem Murphy parar e perguntar antes de agir.

## Instalação

### Via comando (recomendado)

Dentro de uma sessão do Claude Code:

```
/plugin marketplace add PedroHrr-css/murphy-skill
/plugin install murphy@murphy
```

(são dois comandos separados — o primeiro registra o repositório como fonte, o segundo instala o plugin)

Isso instala a variante **guarded** por padrão (dispara automaticamente só em risco real). Para trocar de nível depois de instalado, veja [Trocando de nível](#trocando-de-nível-depois) abaixo.

Para atualizar depois de uma nova versão:
```
/plugin marketplace update
```

Para desinstalar:
```
/plugin remove murphy
```

### Manual (cópia direta de arquivo)

Se preferir não usar o sistema de plugins, ou quiser escolher um nível diferente de `guarded` já na instalação, escolha um dos três níveis abaixo e copie o arquivo correspondente.

| Nível | Quando dispara | Arquivo |
|---|---|---|
| **Manual** | Só quando você digitar `/murphy` | `variants/SKILL-manual.md` |
| **Guarded** (recomendado, padrão do plugin) | Automático, só em risco real (ações destrutivas, ambiguidade, mudanças em algo compartilhado) | `variants/SKILL-guarded.md` |
| **Watchful** | Automático, em praticamente toda request de código | `variants/SKILL-watchful.md` |

```bash
# Escolha pessoal (qualquer projeto)
mkdir -p ~/.claude/skills/murphy
cp variants/SKILL-guarded.md ~/.claude/skills/murphy/SKILL.md

# OU escopo de projeto (versionado com o time)
mkdir -p .claude/skills/murphy
cp variants/SKILL-guarded.md .claude/skills/murphy/SKILL.md
```

Troque `SKILL-guarded.md` por `SKILL-manual.md` ou `SKILL-watchful.md` conforme o nível desejado.

Se você criou um diretório de skills que não existia antes (primeira skill pessoal ou de projeto), reinicie a sessão do Claude Code para que ele seja observado. Edições em uma skill já existente são detectadas ao vivo, sem precisar reiniciar.

## Exemplos

Veja [`examples/`](./examples) para casos reais de antes/depois — o que aconteceria sem Murphy vs. com Murphy, em situações como force push, limpeza ambígua de código e mudanças em endpoints já em uso.

## Testando

```
/murphy
```

ou simplesmente peça algo arriscado, por exemplo:

```
delete a tabela de usuários antigos e recria do zero
```

No modo guarded ou watchful, Murphy deve parar e perguntar antes de executar.

Para um conjunto completo de casos de teste (positivos e negativos), veja [`TESTS.md`](./TESTS.md) e [`evals/evals.json`](./evals/evals.json).

## Estrutura do repositório

```
murphy-skill/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── CHANGELOG.md
├── TESTS.md
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── skills/
│   └── murphy/
│       └── SKILL.md        ← versão guarded, usada pelo /plugin install
├── examples/
│   ├── README.md
│   ├── 01-force-push.md
│   ├── 02-limpeza-ambigua.md
│   └── 03-breaking-api-change.md
├── variants/
│   ├── SKILL-manual.md
│   ├── SKILL-guarded.md
│   └── SKILL-watchful.md
├── evals/
│   └── evals.json
└── assets/
    ├── banner.svg
    └── mascot.svg
```

## Trocando de nível depois

**Se instalou via `/plugin install`:** o plugin instalado fica em cache local gerenciado pelo Claude Code (não em `~/.claude/skills/`). Pra trocar de nível, edite `skills/murphy/SKILL.md` no seu fork/clone do repositório com o conteúdo da variante desejada (`variants/SKILL-manual.md` ou `variants/SKILL-watchful.md`), faça commit, e rode `/plugin marketplace update` seguido de reinstalar o plugin. Alternativamente, instale manualmente (veja abaixo) para trocar de nível sem precisar mexer no repositório.

**Se instalou manualmente:** basta sobrescrever o `SKILL.md` instalado com o conteúdo de outra variante em `variants/`. Não precisa reiniciar — é o mesmo arquivo sendo editado.

## Contribuindo

Veja [`CONTRIBUTING.md`](./CONTRIBUTING.md) para reportar falsos positivos/negativos ou propor mudanças no checklist.

## Licença

MIT — veja [`LICENSE`](./LICENSE).
