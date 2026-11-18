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

Analisa um repositório e grava um dossiê privado. O repositório alvo não é modificado. Tudo o que a skill precisa está neste arquivo. Execute só a fase pedida.

## Como usar

O usuário não preenche arquivo de configuração. Ele indica o projeto na frase. Se faltar o caminho, pergunte uma vez.

Exemplo: `Shadow, inicia o dossiê de ~/projetos/api-pedidos`

Disso saem:

- `REPO_ALVO` = `api-pedidos`, o nome da pasta
- `REPO_PATH` = o caminho que ele deu, somente leitura
- `DOSSIER_PATH` = `~/projetos/_analises-privadas/api-pedidos`, fora do Git do projeto

Mostre esses três valores na fase 00 e grave-os no plano. Se `_analises-privadas` cair dentro de um Git pai, proponha outro diretório privado e espere confirmação antes de criar. Se o usuário disser outro destino para o dossiê, use o destino dele.

| O usuário diz | Execute |
|---|---|
| `Shadow, inicia o dossiê de <repo>` | Fase 00. Grave o plano e pare. |
| `Shadow, fase 03` | Só a fase 03, se o pré-requisito existir. |
| `próxima fase` | A primeira fase da tabela cujo artefato ainda não existe. |
| `Shadow, sessão completa` | Fases 00 a 10, em ordem, neste arquivo. |

`próxima fase` não pula. Se o pré-requisito faltar, pare e diga qual fase falta.

Uma fase por conversa. Grave o artefato, mostre os caminhos criados no dossiê e pare. A sessão completa é a única exceção, e mesmo nela pare antes de qualquer ação que não seja leitura.

Ao terminar a fase 10, compare o estado do `REPO_PATH` com o registrado na fase 00. Liste só os arquivos criados no dossiê.

Resultado esperado:

```text
<PASTA_PAI>/
├── <REPO_ALVO>/                         # somente leitura
└── _analises-privadas/
    └── <REPO_ALVO>/
        ├── 00-plano-de-analise.md
        ├── 00-contexto-e-inventario.md
        ├── 01-project-spec.md
        ├── 02-arquitetura.md
        ├── 03-dependencias-e-integracoes.md
        ├── 04-runbook-local.md
        ├── 05-plano-docker.md
        ├── 06-openapi.yaml
        ├── 06-relatorio-openapi.md
        ├── 07-harness.md
        ├── 08-fluxos-criticos.md
        ├── 09-riscos-e-lacunas.md
        └── 10-relatorio-final.md
```

A fase 06 pode acrescentar `laboratorio-docker/`. A fase 08 pode acrescentar `harness/`.

## Variáveis

Não há caminho fixo. A fase 00 resolve e grava estes nomes em `00-plano-de-analise.md`. As fases seguintes leem o plano.

| Nome | Significado |
|---|---|
| `REPO_ALVO` | Nome da pasta do projeto |
| `REPO_PATH` | Caminho do código. Somente leitura |
| `DOSSIER_PATH` | Caminho do dossiê, fora do Git do projeto |

O dossiê é o estado. Não crie arquivo de progresso dentro do repositório alvo.

## Contrato

1. `REPO_PATH` é somente leitura. Não crie, altere, apague, mova ou formate nada nele.
2. Todo artefato vai para `DOSSIER_PATH`.
3. Sem commit, push, PR, checkout destrutivo ou limpeza do alvo.
4. Sem rede, build, teste, container, migração, seed, fila ou job sem autorização. Antes, descreva comando, impacto, destino e rollback, e pare.
5. Secrets: só nome, origem e finalidade. Valor mascarado.
6. Cada afirmação relevante é **Confirmado**, **Inferido** ou **Não confirmado**, com caminho e, quando possível, linha ou símbolo.
7. Não invente endpoint, variável, comando, porta, dependência ou comportamento. Use `NÃO IDENTIFICADO`.

## Modos

- **Inspeção estática.** Padrão. Só lê arquivos.
- **Validação controlada.** Comandos não destrutivos, testes e builds já existentes, depois de autorização.
- **Laboratório privado.** Dockerfile, Compose, scripts e specs só dentro do dossiê.

## Convenções do dossiê

Toda conclusão relevante usa um destes rótulos:

- **Confirmado:** demonstrado por código, configuração ou teste.
- **Inferido:** conclusão provável a partir de indícios citados.
- **Não confirmado:** falta evidência ou execução segura.

Mermaid:

- Todo documento temático contém ao menos um diagrama útil.
- Use `flowchart` para processos, `sequenceDiagram` para chamadas, `C4Context` ou `C4Container` somente se o renderizador suportar, `stateDiagram-v2` para estados e `erDiagram` para dados.
- Nomes curtos, sem secrets, URLs privadas, tokens ou dados pessoais.
- Inclua uma explicação textual curta abaixo do diagrama.
- Se o diagrama for inferido, escreva isso no título ou na legenda.

Formato de cada documento temático, da fase 01 à 10:

1. Objetivo e escopo
2. Resumo executivo
3. Evidências
4. Diagrama(s) Mermaid
5. Detalhes
6. Hipóteses e lacunas
7. Como validar
8. Próximos passos

Comandos permitidos por padrão são somente de inspeção. Antes de build, testes, rede, containers ou serviços, explique o impacto e peça autorização. Nunca execute comandos destrutivos, migrações ou chamadas a produção. Arquivos experimentais pertencem exclusivamente ao dossiê.

## Índice

| Fase | Produz | Pré-requisito no dossiê |
|---|---|---|
| 00 | `00-plano-de-analise.md` | `REPO_PATH` existe e o dossiê não está dentro dele |
| 01 | `00-contexto-e-inventario.md` | `00-plano-de-analise.md` |
| 02 | `01-project-spec.md` | `00-contexto-e-inventario.md` |
| 03 | `02-arquitetura.md` | `01-project-spec.md` |
| 04 | `03-dependencias-e-integracoes.md` | `02-arquitetura.md` |
| 05 | `04-runbook-local.md` | `03-dependencias-e-integracoes.md` |
| 06 | `05-plano-docker.md` e, se houver evidência, `laboratorio-docker/` | `04-runbook-local.md` |
| 07 | `06-openapi.yaml` e `06-relatorio-openapi.md` | `05-plano-docker.md` |
| 08 | `07-harness.md` e protótipos em `harness/` | `06-openapi.yaml` |
| 09 | `08-fluxos-criticos.md` e `09-riscos-e-lacunas.md` | `07-harness.md` |
| 10 | `10-relatorio-final.md` | os dois artefatos da fase 09 |

## Fase 00 — plano

Artefato: `00-plano-de-analise.md`. Pare ao terminar.

Objetivo: compreender o projeto, sua arquitetura, operação local, contrato HTTP e estratégia de validação isolada. O resultado é pessoal e não oficial.

Nesta fase:

- valide que `REPO_PATH` existe e que `DOSSIER_PATH` não está dentro do repositório alvo;
- registre o estado inicial do repo sem alterá-lo;
- faça somente um inventário superficial;
- crie `DOSSIER_PATH` e `00-plano-de-analise.md` com escopo, etapas, riscos, dúvidas e as variáveis resolvidas;
- pare e apresente o plano antes de análises profundas.

## Fase 01 — inventário e contexto

Artefato: `00-contexto-e-inventario.md`.

Faça uma inspeção estática do `REPO_PATH`. Identifique propósito provável, linguagens, frameworks, entrypoints, módulos, arquivos de build, testes, documentação, automação, configuração e sinais de infraestrutura.

O documento contém:

- resumo executivo e árvore estrutural relevante;
- tabela de tecnologias com evidência e confiança;
- entradas, saídas e responsabilidades aparentes;
- comandos documentados pelo próprio repo, sem executá-los;
- configurações sensíveis apenas por nome, nunca por valor;
- lacunas e perguntas;
- diagrama Mermaid mostrando repo, atores e sistemas percebidos.

Não faça varredura de valores secretos. Não leia arquivos de credenciais além do necessário para reconhecer sua natureza. Ao final, liste exatamente quais arquivos do dossiê foram criados.

## Fase 02 — Project Spec

Artefato: `01-project-spec.md`.

Com base no inventário, e em nova leitura somente quando necessária, documente: problema resolvido, usuários e consumidores, escopo e não escopo, capacidades, entradas e saídas, regras de negócio observáveis, interfaces, requisitos operacionais, persistência, segurança, observabilidade, restrições e critérios de saúde. Para cada afirmação importante, cite evidência e marque Confirmado, Inferido ou Não confirmado.

Inclua:

- um flowchart Mermaid do ciclo principal;
- uma tabela de capacidades por evidências;
- cenários felizes, falhas esperadas e casos-limite;
- perguntas que exigem conhecimento humano.

Não transforme hipótese em requisito oficial.

## Fase 03 — arquitetura

Artefato: `02-arquitetura.md`.

Mapeie a arquitetura do `REPO_PATH` sem alterá-lo. Identifique fronteiras, camadas, módulos, entrypoints, dependências internas, chamadas síncronas e assíncronas, persistência, jobs e observabilidade. Explique inicialização e caminho de uma requisição ou evento representativo.

Inclua ao menos:

1. flowchart Mermaid de contexto;
2. flowchart Mermaid de containers e componentes;
3. sequenceDiagram de um fluxo principal.

Para cada nó e seta cite a evidência correspondente. Marque elementos inferidos. Registre acoplamentos, pontos únicos de falha e dúvidas.

## Fase 04 — dependências e integrações

Artefato: `03-dependencias-e-integracoes.md`.

Catalogue dependências e integrações observadas. Separe: bibliotecas de runtime, desenvolvimento, serviços internos, APIs externas, bancos, caches, filas, storage, identidade, telemetria e infraestrutura. Para cada item registre finalidade, direção, protocolo, configuração por nome, obrigatoriedade, comportamento de falha, alternativa local ou mock e evidência.

Inclua um Mermaid de dependências com legenda para obrigatório, opcional e inferido. Aponte versões fixas ou desatualizadas apenas quando comprovado localmente. Não consulte a internet. Nunca revele hosts, tokens ou credenciais sensíveis.

## Fase 05 — runbook local

Artefato: `04-runbook-local.md`.

Escreva o runbook para alguém preparar, iniciar, verificar, diagnosticar e encerrar a aplicação localmente.

Derive comandos apenas de evidências do repo. Não execute nada ainda. Cubra pré-requisitos, variáveis por nome e exemplo seguro, dependências locais, ordem de inicialização, health checks, smoke test, logs, erros comuns, limpeza segura e critérios de pronto.

Inclua:

- flowchart Mermaid de startup e diagnóstico;
- tabela comando, finalidade, evidência e efeito colateral;
- seção "não executar contra ambientes compartilhados";
- lacunas e um plano de validação controlada.

Qualquer comando potencialmente mutável fica apenas como proposta e claramente sinalizado.

## Fase 06 — Docker isolado

Artefatos: `05-plano-docker.md` e, se houver evidência suficiente, protótipos em `DOSSIER_PATH/laboratorio-docker/`.

Desenhe uma estratégia para executar a aplicação isoladamente em Docker. Se houver evidência suficiente, gere protótipos de `Dockerfile`, `compose.yaml`, `.env.example` e scripts somente dentro de `DOSSIER_PATH/laboratorio-docker/`.

Não altere o repo e não faça build, pull ou up sem autorização. Minimize serviços, use imagens não privilegiadas quando possível, healthchecks, rede privada, volumes descartáveis e configurações fictícias. Nunca copie secrets.

Documente contexto de build necessário, entrypoint, portas, dependências, mocks, persistência, readiness, shutdown, limitações e comandos propostos. Inclua flowchart Mermaid da topologia e sequenceDiagram do startup. Se arquivos do repo precisarem ser copiados para um build futuro, explique uma estratégia segura sem executá-la agora.

## Fase 07 — reverse engineering OpenAPI

Artefatos: `06-openapi.yaml` e `06-relatorio-openapi.md`.

Faça reverse engineering das interfaces HTTP por inspeção estática de rotas, handlers, DTOs e schemas, validações, middlewares, autenticação, testes e exemplos.

Gere:

- `06-openapi.yaml`, OpenAPI 3.1 quando compatível;
- `06-relatorio-openapi.md`, com cobertura, evidências, hipóteses e lacunas.

Para cada operação, documente método, path, parâmetros, corpo, respostas, erros, segurança e exemplos seguros somente quando sustentados. Marque extensões inferidas com `x-analysis-confidence` e `x-analysis-evidence`. Não invente endpoints. Não inclua servidores reais, credenciais ou dados pessoais.

Se não houver superfície HTTP, grave mesmo assim um `06-openapi.yaml` com `paths` vazio e registre a ausência como **Não confirmado** no relatório. A fase 08 depende desse arquivo.

Inclua no relatório um Mermaid request-to-handler e uma matriz rota, handler, schema, teste e confiança. Faça validação estrutural offline somente se uma ferramenta já estiver disponível. Caso contrário, proponha o comando e registre como não validado.

## Fase 08 — harness de validação

Artefatos: `07-harness.md` e protótipos somente em `DOSSIER_PATH/harness/`.

Projete um harness privado que valide uma instância isolada contra comportamento esperado e, quando aplicável, `06-openapi.yaml`.

Não execute testes nem faça chamadas sem autorização.

Cubra readiness, smoke tests, contrato, cenários felizes, erros, autenticação fictícia, isolamento de dados, fixtures, mocks e stubs, timeouts, retries, logs, relatório e cleanup. Prefira ferramentas já presentes no repo. Justifique qualquer nova ferramenta.

Inclua flowchart Mermaid do pipeline e sequenceDiagram de um teste. Defina critérios de sucesso, falha e inconclusivo. Garanta que o alvo padrão seja localhost ou rede Docker privada e bloqueie hosts não autorizados.

## Fase 09 — fluxos críticos, riscos e lacunas

Artefatos: `08-fluxos-criticos.md` e `09-riscos-e-lacunas.md`.

Selecione os fluxos mais importantes com base em evidências. Para cada fluxo, descreva gatilho, pré-condições, passos, dados, dependências, resultado, falhas, observabilidade e recuperação. Use sequenceDiagram ou stateDiagram-v2 Mermaid.

No registro de riscos, avalie impacto, probabilidade qualitativa, evidência, detectabilidade, mitigação sugerida e método de validação. Separe riscos confirmados de suspeitas. Inclua segurança, confiabilidade, dados, operação, contrato, testabilidade e conhecimento ausente. Não classifique vulnerabilidades sem evidência suficiente.

## Fase 10 — consolidação final

Artefato: `10-relatorio-final.md`.

Revise todos os artefatos do `DOSSIER_PATH`. Inclua visão executiva, mapa do sistema, como ele inicia e opera, contrato, estratégia Docker, harness, fluxos críticos, riscos, lacunas, nível de confiança e próximos passos priorizados. Adicione um flowchart Mermaid ponta a ponta e links relativos para todos os artefatos.

Faça uma checagem de consistência: nomes, portas, comandos, componentes e endpoints devem concordar entre documentos. Liste divergências corrigidas e pendências. Confirme por comparação do estado inicial e final que `REPO_PATH` permaneceu inalterado. Se não puder confirmar, diga explicitamente. Finalize com um índice do dossiê e uma lista de validações que ainda exigem autorização.

## Sessão completa

Percorra as fases 00 a 10, nesta ordem, usando as seções deste arquivo. Mantenha um log de progresso no `00-plano-de-analise.md`.

Pare antes de qualquer ação mutável e peça autorização informando comando, impacto, destino e rollback. Ao final, apresente o índice do dossiê, o nível de confiança por área, os arquivos criados e as validações pendentes.
