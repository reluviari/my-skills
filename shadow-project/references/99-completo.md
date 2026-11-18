# Sessão completa

Percorra as fases 00 a 10, nesta ordem, lendo cada arquivo em `references/` só quando a fase começar. Mantenha um log de progresso no `00-plano-de-analise.md`.

`REPO_PATH` é estritamente somente leitura. Todo arquivo gerado fica em `DOSSIER_PATH`. Não faça commits, instalações, rede, builds, testes, containers, migrações ou chamadas externas sem autorização. Não revele secrets. Registre fatos como Confirmado, Inferido ou Não confirmado e cite caminhos, símbolos e linhas. Não invente dados.

Ordem:

1. validar caminhos, registrar estado inicial e criar o plano;
2. inventário e contexto;
3. Project Spec;
4. arquitetura;
5. dependências e integrações;
6. runbook local;
7. plano e protótipos Docker privados;
8. reverse engineering OpenAPI;
9. harness privado;
10. fluxos críticos, riscos e lacunas;
11. relatório final e verificação de que o repo não mudou.

Cada documento temático segue `references/convencoes.md`: ao menos um diagrama Mermaid útil, evidências, hipóteses, lacunas, como validar e próximos passos. OpenAPI e protótipos devem ser conservadores e rastreáveis à evidência.

Pare antes de qualquer ação mutável e peça autorização informando comando, impacto, destino e rollback. Ao final, apresente o índice do dossiê, o nível de confiança por área, os arquivos criados e as validações pendentes.
