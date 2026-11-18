---
name: shadow-project
description: >-
  Produz um dossiê privado de um repositório, uma fase por vez, sem alterar
  o alvo. Use quando o usuário disser shadow, dossiê, análise privada,
  inicia o dossiê, próxima fase, fase 00 a 10, sessão completa, ou pedir
  inventário, project spec, arquitetura, dependências, runbook, Docker
  isolado, OpenAPI, harness, fluxos, riscos ou relatório final de um
  repositório somente leitura.
---

# Shadow Project

Analisa um repositório e grava um dossiê privado. Uma fase por conversa. O repositório alvo não é modificado.

Antes de qualquer fase, leia `references/convencoes.md` nesta skill. Leia só o arquivo da fase pedida.

## Variáveis

```text
REPOS_BASE=/repos
REPO_ALVO=<NOME_DO_REPO>
REPO_PATH=/repos/<NOME_DO_REPO>
DOSSIER_PATH=/repos/_analises-privadas/<NOME_DO_REPO>
```

Se o usuário informar outros caminhos, use os dele. Se `_analises-privadas` cair dentro de um Git pai, escolha outro diretório privado fora desse Git. Grave `REPO_ALVO`, `REPO_PATH` e `DOSSIER_PATH` em `00-plano-de-analise.md`. As fases seguintes leem esse plano.

## Contrato

1. `REPO_PATH` é somente leitura. Não crie, altere, apague, mova ou formate nada nele.
2. Todo artefato vai para `DOSSIER_PATH`.
3. Sem commit, push, PR, checkout destrutivo ou limpeza do alvo.
4. Sem rede, build, teste, container, migração, seed, fila ou job sem autorização. Antes, descreva comando, impacto, destino e rollback, e pare.
5. Secrets: só nome, origem e finalidade. Valor mascarado.
6. Cada afirmação relevante é **Confirmado**, **Inferido** ou **Não confirmado**, com caminho e, quando possível, linha ou símbolo.
7. Não invente endpoint, variável, comando, porta, dependência ou comportamento. Use `NÃO IDENTIFICADO`.
8. O dossiê é o estado. Não crie arquivo de progresso dentro do repositório alvo.

## Modos

- **Inspeção estática.** Padrão. Só lê arquivos.
- **Validação controlada.** Comandos não destrutivos, testes e builds já existentes, depois de autorização.
- **Laboratório privado.** Dockerfile, Compose, scripts e specs só dentro do dossiê.

## Fases

| Fase | Leia | Produz | Pré-requisito no dossiê |
|---|---|---|---|
| 00 | `references/00-mestre.md` | `00-plano-de-analise.md` | `REPO_PATH` existe e o dossiê não está dentro dele |
| 01 | `references/01-inventario.md` | `00-contexto-e-inventario.md` | `00-plano-de-analise.md` |
| 02 | `references/02-project-spec.md` | `01-project-spec.md` | `00-contexto-e-inventario.md` |
| 03 | `references/03-arquitetura.md` | `02-arquitetura.md` | `01-project-spec.md` |
| 04 | `references/04-dependencias.md` | `03-dependencias-e-integracoes.md` | `02-arquitetura.md` |
| 05 | `references/05-runbook.md` | `04-runbook-local.md` | `03-dependencias-e-integracoes.md` |
| 06 | `references/06-docker.md` | `05-plano-docker.md` e, se houver evidência, `laboratorio-docker/` | `04-runbook-local.md` |
| 07 | `references/07-openapi.md` | `06-openapi.yaml` e `06-relatorio-openapi.md` | `05-plano-docker.md` |
| 08 | `references/08-harness.md` | `07-harness.md` e protótipos em `harness/` | `06-openapi.yaml` |
| 09 | `references/09-fluxos-riscos.md` | `08-fluxos-criticos.md` e `09-riscos-e-lacunas.md` | `07-harness.md` |
| 10 | `references/10-relatorio.md` | `10-relatorio-final.md` | os dois artefatos da fase 09 |

`próxima fase` escolhe a primeira linha da tabela cujo artefato ainda não existe. Se o pré-requisito faltar, pare e diga qual fase falta. Não pule.

## Portão

Execute uma fase. Grave o artefato. Mostre os caminhos criados no dossiê e pare. Não comece a fase seguinte na mesma conversa, a menos que o usuário tenha pedido `sessão completa`.

`sessão completa` lê `references/99-completo.md` e percorre as fases em ordem. Continua parando antes de qualquer ação que não seja leitura.

Ao terminar a fase 10, compare o estado do `REPO_PATH` com o registrado na fase 00. Liste só os arquivos criados no dossiê.
