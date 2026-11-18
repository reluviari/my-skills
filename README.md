# my-skills

Repositório único das skills pessoais desta máquina. Cada skill é uma pasta na raiz, com um `SKILL.md`. Um symlink por skill liga essa pasta aos agentes. Editar aqui vale para todos.

## Layout

Uma pasta por skill, na raiz. Sem pasta `skills/` no meio. O procedimento de cada skill fica no `SKILL.md` dela.

## Agentes

| Agente | Pasta de skills |
|---|---|
| Claude Code | `~/.claude/skills` |
| Cursor | `~/.cursor/skills` |
| Codex | `~/.codex/skills` |
| Compartilhado | `~/.agents/skills` |

O symlink é da pasta da skill, não uma cópia.

## Ligar uma skill

Troque `nome-da-skill` pelo nome da pasta. Crie a pasta do agente se ela ainda não existir.

```sh
# rode dentro da pasta deste clone
KIT="$(pwd)"
name="nome-da-skill"
for h in "$HOME/.claude/skills" "$HOME/.cursor/skills" "$HOME/.agents/skills" "$HOME/.codex/skills"; do
  mkdir -p "$h"
  ln -sfn "$KIT/$name" "$h/$name"
done
```

Reinicie o agente que já estiver aberto.

Para soltar só este symlink, apague o link em cada pasta. Não apague a pasta em `my-skills/`.
