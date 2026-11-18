# Fase 08 — harness de validação

Artefatos: `07-harness.md` e protótipos somente em `DOSSIER_PATH/harness/`.

Projete um harness privado que valide uma instância isolada contra comportamento esperado e, quando aplicável, `06-openapi.yaml`.

Gere `07-harness.md` e protótipos somente em `DOSSIER_PATH/harness/`. Não execute testes nem faça chamadas sem autorização.

Cubra readiness, smoke tests, contrato, cenários felizes, erros, autenticação fictícia, isolamento de dados, fixtures, mocks e stubs, timeouts, retries, logs, relatório e cleanup. Prefira ferramentas já presentes no repo. Justifique qualquer nova ferramenta.

Inclua flowchart Mermaid do pipeline e sequenceDiagram de um teste. Defina critérios de sucesso, falha e inconclusivo. Garanta que o alvo padrão seja localhost ou rede Docker privada e bloqueie hosts não autorizados.
