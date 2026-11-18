# my-skills

Skills pessoais para os agentes desta máquina. Cada skill é uma pasta na raiz, com um `SKILL.md`. Um symlink por skill liga essa pasta aos agentes. Editar aqui vale para todos.

## Layout

Uma pasta por skill, na raiz. Sem pasta `skills/` no meio.

| Pasta | Quando usar |
|---|---|
| `shadow-project/` | Dossiê privado de um repositório, uma fase por vez, sem alterar o alvo |

## Agentes

| Agente | Pasta de skills |
|---|---|
| Claude Code | `~/.claude/skills` |
| Cursor | `~/.cursor/skills` |
| Codex | `~/.codex/skills` |
| Compartilhado | `~/.agents/skills` |

O symlink é da pasta da skill, não uma cópia. Assim `references/` continua ao lado do `SKILL.md`.

## Ligar uma skill

Troque `<name>` pelo nome da pasta. Crie a pasta do agente se ela ainda não existir.

```sh
KIT="$HOME/Documents/Projects/my-skills"
name=shadow-project
for h in "$HOME/.claude/skills" "$HOME/.cursor/skills" "$HOME/.agents/skills" "$HOME/.codex/skills"; do
  mkdir -p "$h"
  ln -sfn "$KIT/$name" "$h/$name"
done
```

Reinicie o agente que já estiver aberto.

Para soltar só este symlink, apague o link em cada pasta. Não apague a pasta em `my-skills/`.

## Shadow Project

Abra o chat com acesso de leitura ao repositório alvo e diga uma fase.

| Você diz | O que acontece |
|---|---|
| `Shadow, inicia o dossiê de <repo>` | Fase 00: valida caminhos, grava o plano e para |
| `Shadow, fase 03` ou `próxima fase` | Só essa fase, se o artefato anterior existir |
| `Shadow, sessão completa` | As fases em ordem, ainda parando antes de ação mutável |

O repositório alvo é somente leitura. O dossiê sai em `/repos/_analises-privadas/<repo>/`. Convenções, modos e o formato dos documentos estão em `shadow-project/references/convencoes.md`.

O pacote antigo de prompts continua em `shadow-project-ai`. Esta skill é a forma de executar.
