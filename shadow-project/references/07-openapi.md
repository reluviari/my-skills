# Fase 07 — reverse engineering OpenAPI

Artefatos: `06-openapi.yaml` e `06-relatorio-openapi.md`.

Faça reverse engineering das interfaces HTTP por inspeção estática de rotas, handlers, DTOs e schemas, validações, middlewares, autenticação, testes e exemplos.

Gere no `DOSSIER_PATH`:

- `06-openapi.yaml`, OpenAPI 3.1 quando compatível;
- `06-relatorio-openapi.md`, com cobertura, evidências, hipóteses e lacunas.

Para cada operação, documente método, path, parâmetros, corpo, respostas, erros, segurança e exemplos seguros somente quando sustentados. Marque extensões inferidas com `x-analysis-confidence` e `x-analysis-evidence`. Não invente endpoints. Não inclua servidores reais, credenciais ou dados pessoais.

Inclua no relatório um Mermaid request-to-handler e uma matriz rota, handler, schema, teste e confiança. Faça validação estrutural offline somente se uma ferramenta já estiver disponível. Caso contrário, proponha o comando e registre como não validado.
