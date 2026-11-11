# Skills Claude

Skills pessoais para o Claude Code, uma pasta por skill (`<nome>/SKILL.md`).

| Skill | O que faz |
|---|---|
| `analisador-codigo` | Revisão de código (arquitetura, segurança, legibilidade) |
| `precificar-sass` | Custo de infra e proposta de preço em BRL |
| `sugerir-novas-features` | Features priorizadas a partir de dores e dados de uso |
| `avaliar-aplicativo-escala` | Comparativo com concorrentes e plano de crescimento |
| `analise-seguranca` | Análise defensiva de segurança (estática) |
| `fazer-pentest` | Pentest white-box autorizado (somente leitura) |
| `testar-com-personas` | Teste pré-lançamento com usuários simulados (personas diversas navegando no app) |

## Instalar (linkar na pasta global)

Rode **de dentro da pasta `claude`**. O link precisa ser **absoluto** (`$PWD`); com caminho relativo (`./skill`) ele nasce quebrado.

```bash
mkdir -p ~/.claude/skills
for d in "$PWD"/*/; do
  ln -sfn "${d%/}" ~/.claude/skills/"$(basename "$d")"
done
```

Linke **uma skill por vez**, não a pasta inteira. `~/.claude/skills` já pode ter outros itens (ex.: `synced`), e se ela existir, `ln -s` criaria o link dentro dela em vez de substituí-la.

## Verificar

```bash
ls -l ~/.claude/skills          # cada skill deve aparecer com "->" para este repositório
ls ~/.claude/skills/*/SKILL.md  # deve listar um SKILL.md por skill
find ~/.claude/skills -xtype l  # não deve imprimir nada (imprime links quebrados)
```

Depois, **reinicie a sessão** do Claude Code e digite `/` para ver as skills na lista.

## Remover

```bash
rm ~/.claude/skills/<nome-da-skill>   # remove só o link, não apaga a skill do repositório
```
