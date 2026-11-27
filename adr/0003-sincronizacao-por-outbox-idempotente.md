# ADR-0003 — Sincronização por outbox com operações idempotentes

- Status: aceito
- Data: 2026-11-24
- Sprint: 1

## Contexto

Com o aplicativo offline (ADR-0002), as alterações precisam chegar ao servidor quando houver
conexão. A rede de campo é instável: uma requisição pode ser aplicada no servidor e a resposta
se perder. O Anexo B (decisão 6) e o documento 11.9 pedem operações idempotentes.

## Decisão

- Cada alteração local gera uma operação na outbox do aplicativo (`SyncOperation`,
  documento 10.13.3) com `operation_id` UUID gerado no dispositivo, `entity_type`,
  `entity_id`, `base_version`, payload e dependências.
- O aplicativo envia as operações em ordem de dependência: um upload de foto só sai depois
  da resposta ou não conformidade a que pertence.
- O servidor registra cada operação processada em `sync_processed_operations`, com o
  `operation_id` como chave primária. Reenviar a mesma operação devolve o resultado já gravado
  (`ALREADY_APPLIED`) sem aplicar de novo. Um lock consultivo do PostgreSQL por
  `operation_id` serializa entregas concorrentes da mesma operação.
- Concorrência é controlada por versão: a operação carrega `baseVersion`; versão diferente da
  atual vira conflito (`409` na rota de recurso ou resultado `CONFLICT` no lote).
- O resultado de cada operação usa `APPLIED`, `ALREADY_APPLIED`, `REJECTED`, `CONFLICT` ou
  `DEPENDENCY_FAILED` (ver [contrato compartilhado](../contrato-compartilhado.md), seção 5.2).
- Alterações do servidor descem por `GET /api/v1/mobile/sync/pull` com cursor opaco; o
  aplicativo só avança o cursor depois de gravar as alterações no SQLite (documento 10.13.4).

## Alternativas consideradas

- **Enviar o estado completo da inspeção ao final**: simples, mas perde trabalho parcial e
  torna conflito uma operação de tudo ou nada.
- **Sincronização sem chave de idempotência**: um reenvio após timeout duplicaria respostas e
  fotos.
- **Última escrita vence, sem versão**: esconderia alterações concorrentes do supervisor.

## Consequências

- O servidor guarda um registro por operação processada, que também serve de rastro.
- O aplicativo precisa tratar cada resultado por operação, inclusive conflito e falha de
  dependência.
- ❓ Em `develop`, o aplicativo envia a outbox pelas rotas de recurso
  (`/inspections/{id}/responses/{responseId}`, `/inspections/{id}/evidence`, `start`,
  `submit`), e o lote `POST /api/v1/mobile/sync/push` existe no `backend` com um conjunto de
  `operationType` diferente (`UPSERT`, `CREATE`, `UPDATE`, `UPLOAD`). A escolha entre os dois
  caminhos, ou o mapeamento entre eles, ainda precisa ser registrada.
