# ADR-0007 — Revisão realizada exclusivamente pela interface administrativa

- Status: aceito
- Data: 2026-11-24
- Sprint: 1

## Contexto

Depois do envio, a inspeção é analisada pelo supervisor, que aprova ou reprova com motivo. O
Anexo B (decisão 14) define que a revisão é feita pela interface administrativa. Permitir a
revisão no aplicativo exigiria telas de análise de evidências e regras de autorização também
no mobile.

## Decisão

- Iniciar revisão, aprovar e reprovar são ações da interface administrativa
  (`POST /api/v1/inspections/{id}/begin-review`, `/approve` e `/reject`).
- No `backend`, essas rotas exigem o perfil `SUPERVISOR`
  (`AuthorizationPolicies.INSPECTIONS_REVIEW`). A restrição é garantida pela API, não só pela
  ausência de telas no aplicativo.
- O aplicativo apenas exibe o resultado da revisão ao técnico (motivo da reprovação e itens a
  corrigir). A reabertura para correção é do técnico, conforme o
  [ADR-002 do backend](https://github.com/FieldOps-Project/backend/blob/develop/docs/adr/ADR-002-explicit-reopen-after-rejection.md).
- Cada nova decisão depois de um novo envio cria um novo ciclo em `inspection_reviews`; os
  ciclos anteriores são preservados.

## Alternativas consideradas

- **Revisão também no aplicativo**: dobraria telas e testes e levaria decisões de supervisão
  para um ambiente sem tela adequada para analisar evidências.

## Consequências

- O supervisor depende do painel web para revisar.
- O aplicativo fica restrito à execução e à correção, o que simplifica autorização e telas.
