# Contrato compartilhado

> Fonte única dos enums, identificadores, datas, formato de erro e paginação usados por
> `backend`, `frontend` e `app`. Os valores abaixo foram conferidos no código do `backend`
> (branch `develop`) e, quando o valor só existe no dispositivo, no `app` (branch `develop`).
>
> Referências: [10 - Modelo de Dados](notion/10-modelo-de-dados.md) (10.6 a 10.13),
> [12 - API REST](notion/12-api-rest.md) (12.1 a 12.3, 12.18) e RN-090.

Marcações usadas neste documento:

- **⚠ Divergência**: o código implementado difere do documento 10, do documento 12 ou do card.
  O valor que vale para integração é o do código; a divergência fica registrada para decisão.
- **❓ Dúvida**: ponto que o código não resolve e que precisa de decisão da equipe.

## 1. Regras gerais

- Prefixo de todas as rotas: `/api/v1`.
- Corpo em JSON. Nomes de campo em **camelCase** (`startedAtDevice`, `serverReceivedAt`).
  As colunas do PostgreSQL usam snake_case (`started_at_device`), mas esse nome nunca aparece
  no JSON.
- Enums trafegam como **string com o literal exato** da tabela da seção 2, em maiúsculas e com
  `_`. Não existe tradução, número ou minúscula no JSON. Rótulos em português
  (ex.: "Em andamento") são responsabilidade de cada interface.
- Campos desconhecidos são recusados nos corpos que declaram isso (respostas do checklist e
  lote de sincronização usam `@JsonIgnoreProperties(ignoreUnknown = false)`), com
  `400 INVALID_REQUEST`.

## 2. Enums

### 2.1 Enums trafegados pela API

| Enum | Literais exatos | Onde aparece | Classe no backend |
| --- | --- | --- | --- |
| `UserRole` | `ADMIN` \| `SUPERVISOR` \| `TECHNICIAN` \| `CLIENT_VIEWER` | `User.role` | `user.domain.model.UserRole` |
| `UserStatus` | `ACTIVE` \| `INACTIVE` \| `BLOCKED` | `User.status` | `user.domain.model.UserStatus` |
| `EntityStatus` | `ACTIVE` \| `INACTIVE` | `Client.status`, `InspectionSite.status` | `ClientStatus`, `InspectionSiteStatus` |
| `EquipmentStatus` | `ACTIVE` \| `INACTIVE` \| `DECOMMISSIONED` | `Equipment.status` | `equipment.domain.model.EquipmentStatus` |
| `TemplateStatus` | `DRAFT` \| `ACTIVE` \| `INACTIVE` | `InspectionTemplate.status` | `inspection.domain.model.TemplateStatus` |
| `ResponseType` | `TEXT_SHORT` \| `TEXT_LONG` \| `NUMBER` \| `BOOLEAN` \| `CONFORMITY` \| `SINGLE_CHOICE` \| `DATE` | `TemplateItem.responseType`, snapshot | `inspection.domain.model.ResponseType` |
| `Conformity` | `NOT_APPLICABLE` \| `CONFORMING` \| `NON_CONFORMING` | `InspectionResponse.conformity` | `InspectionResponseConformity` |
| `InspectionStatus` | `DRAFT` \| `ASSIGNED` \| `IN_PROGRESS` \| `SUBMITTED` \| `UNDER_REVIEW` \| `APPROVED` \| `REJECTED` \| `CANCELED` | `Inspection.status` | `inspection.domain.model.InspectionStatus` |
| `Priority` | `LOW` \| `MEDIUM` \| `HIGH` \| `CRITICAL` | `Inspection.priority` | `InspectionPriority` |
| `Severity` | `LOW` \| `MEDIUM` \| `HIGH` \| `CRITICAL` | `NonConformity.severity` | `NonConformitySeverity` |
| `NonConformityStatus` | `OPEN` | `NonConformity.status` | `inspection.domain.model.NonConformityStatus` |
| `ReviewDecision` | `APPROVED` \| `REJECTED` | `InspectionReview.decision` | `InspectionReviewDecision` |
| `EvidenceType` | `PHOTO` | `Evidence.type` | — (ver nota) |
| `SyncOpResult` | `APPLIED` \| `ALREADY_APPLIED` \| `REJECTED` \| `CONFLICT` \| `DEPENDENCY_FAILED` | `SyncOperationResult.status` | `sync.domain.model.SyncOperationResultStatus` |

Correspondência com os estados em português do documento 7.4: `DRAFT` = RASCUNHO,
`ASSIGNED` = ATRIBUÍDA, `IN_PROGRESS` = EM_ANDAMENTO, `SUBMITTED` = ENVIADA,
`UNDER_REVIEW` = EM_REVISÃO, `APPROVED` = APROVADA, `REJECTED` = REPROVADA,
`CANCELED` = CANCELADA. Os nomes em português nunca trafegam.

Notas:

- `CANCELED` tem um único `L`.
- `EntityStatus` não é uma classe única no backend: cliente e local têm enums próprios com os
  mesmos literais. Para os clientes da API o contrato é o mesmo.
- **⚠ Divergência — `EvidenceType`:** o documento 10 define `PHOTO` como único valor do MVP,
  mas o backend guarda `type` como texto livre (`VARCHAR(32)`, só exige não vazio) e não
  valida o literal. O formato do arquivo é controlado pelo `mimeType` (`image/jpeg` ou
  `image/png`). Até o backend validar, mobile e painel devem enviar e esperar apenas `PHOTO`.

### 2.2 Ações de transição da inspeção

O backend nomeia cada transição com `InspectionAction`. O valor aparece nas mensagens de erro
e na auditoria; o cliente aciona a transição pela rota correspondente.

| Ação | Rota | Literal |
| --- | --- | --- |
| Atribuir técnico | `POST /api/v1/inspections/{id}/assign` | `ASSIGN` |
| Iniciar | `POST /api/v1/inspections/{id}/start` | `START` |
| Concluir e enviar | `POST /api/v1/inspections/{id}/submit` | `SUBMIT` |
| Cancelar | `POST /api/v1/inspections/{id}/cancel` | `CANCEL` |
| Iniciar revisão | `POST /api/v1/inspections/{id}/begin-review` | `START_REVIEW` |
| Aprovar | `POST /api/v1/inspections/{id}/approve` | `APPROVE` |
| Reprovar | `POST /api/v1/inspections/{id}/reject` | `REJECT` |
| Reabrir para correção | `POST /api/v1/inspections/{id}/reopen` | `REOPEN` |

As rotas foram conferidas em `InspectionController` e `InspectionReviewController`. O contrato
completo de cada rota (corpo, resposta) continua no OpenAPI do backend (`openapi/openapi.json`).

Transições permitidas (tabela 7.6, implementada em `InspectionStateMachine`):

| Estado atual | Ação | Próximo estado |
| --- | --- | --- |
| `DRAFT` | `ASSIGN` | `ASSIGNED` |
| `ASSIGNED` | `START` | `IN_PROGRESS` |
| `ASSIGNED` | `CANCEL` | `CANCELED` |
| `IN_PROGRESS` | `SUBMIT` | `SUBMITTED` |
| `IN_PROGRESS` | `CANCEL` | `CANCELED` |
| `SUBMITTED` | `START_REVIEW` | `UNDER_REVIEW` |
| `UNDER_REVIEW` | `APPROVE` | `APPROVED` |
| `UNDER_REVIEW` | `REJECT` | `REJECTED` |
| `REJECTED` | `REOPEN` | `IN_PROGRESS` |
| `REJECTED` | `CANCEL` | `CANCELED` |

Qualquer outra combinação responde `409 INSPECTION_INVALID_TRANSITION`.

### 2.3 Enums que existem só no aplicativo mobile

Não trafegam na API. Ficam no SQLite do `app` e servem à outbox e ao indicador de
sincronização.

| Enum | Literais exatos | Onde aparece |
| --- | --- | --- |
| `SyncOpType` | `CREATE` \| `UPDATE` \| `DELETE_LOGICAL` \| `TRANSITION` \| `UPLOAD` | `sync_operations.operation_type` (`app`) |
| `SyncOpStatus` | `PENDING` \| `PROCESSING` \| `FAILED` \| `CONFLICT` \| `COMPLETED` | `sync_operations.status` (`app`) |
| `LocalSyncStatus` | `SYNCED` \| `PENDING` \| `SYNCING` \| `ERROR` \| `CONFLICT` | `local_sync_status` das tabelas locais (`app`) |
| `LastSyncResult` | `SUCCESS` \| `PARTIAL` \| `FAILED` | `SyncMetadata.last_sync_result` (documento 10.13.4) |

Correspondência com o documento 7.5: `SYNCED` = SINCRONIZADO, `PENDING` = PENDENTE,
`SYNCING` = SINCRONIZANDO, `ERROR` = ERRO, `CONFLICT` = CONFLITO.

❓ **Dúvida — `LastSyncResult`:** está no documento 10.13.4, mas não foi encontrado no código do
`app` em `develop`. Fica listado para não divergir quando for implementado.

### 2.4 Lote de sincronização (`/api/v1/mobile/sync/push`)

O backend expõe `POST /api/v1/mobile/sync/push` e `GET /api/v1/mobile/sync/pull`. No lote,
`entityType` e `operationType` são strings validadas pelo servidor, não enums Java:

| `entityType` | `operationType` aceitos |
| --- | --- |
| `INSPECTION_RESPONSE` | `UPSERT` |
| `INSPECTION_EVIDENCE` | `CREATE` ou `UPSERT` (metadados), `UPLOAD` (arquivo em base64) |
| `NON_CONFORMITY` | `CREATE` ou `UPSERT`, `UPDATE` |

Outro `entityType` resulta em `SYNC_ENTITY_TYPE_UNSUPPORTED`; outro `operationType`, em
`SYNC_OPERATION_TYPE_UNSUPPORTED`.

**⚠ Divergência — `UPSERT`:** o lote do servidor usa `UPSERT`, que não existe no `SyncOpType`
do documento 10.13.3 nem no `app`. E o `SyncOpType` do documento (`DELETE_LOGICAL`,
`TRANSITION`) não é aceito pelo lote. Em `develop`, o `app` envia a outbox pelas rotas de
recurso (`/inspections/{id}/responses/{responseId}`, `/inspections/{id}/evidence`,
`/inspections/{id}/start`, `/inspections/{id}/submit`), e não pelo lote. Se o `app` passar a
usar o lote, é preciso decidir o mapeamento entre os dois conjuntos.

## 3. Identificadores

- Toda entidade é identificada por **UUID** no formato canônico textual, minúsculo, com
  hífens: `3f2b8c4e-9a1d-4e6b-8c2f-7d5a1b0e9c3f`.
- Entidades criadas em campo têm o UUID **gerado no dispositivo**, antes de qualquer contato
  com o servidor: resposta do checklist (`InspectionResponse.id`), evidência
  (`Evidence.id`), não conformidade (`NonConformity.id`) e cada operação da outbox
  (`operationId`). O `app` gera com `randomUUID()` do `expo-crypto` (UUID v4). O
  identificador da instalação (`deviceId`) também é um UUID do dispositivo.
- Entidades criadas no painel ou na API (usuário, cliente, local, equipamento, modelo,
  inspeção) têm o UUID gerado pelo servidor.
- O mesmo UUID é usado no SQLite e no PostgreSQL. Não existe id temporário nem remapeamento.
- Reenviar uma criação com o mesmo UUID é idempotente: o servidor reconhece a operação já
  aplicada (`ALREADY_APPLIED` no lote). Reusar um UUID existente para **outro** registro
  responde `409` (`INSPECTION_RESPONSE_ID_REUSED`, `EVIDENCE_ID_REUSED`,
  `NON_CONFORMITY_ID_REUSED`).
- UUID mal formado, na rota, em parâmetro ou no corpo, responde `400 INVALID_REQUEST`.

❓ **Dúvida — versão do UUID:** o card pede UUID v4. O backend aceita qualquer UUID válido e não
confere a versão. A regra do contrato é: **clientes geram v4**; o servidor não rejeita outras
versões.

## 4. Datas e horas

- Formato: **ISO 8601**. Instantes vão com fuso; o servidor sempre devolve em **UTC com `Z`**,
  por exemplo `2026-08-01T14:30:00Z` (frações de segundo podem aparecer:
  `2026-08-01T14:30:00.123456Z`).
- O servidor grava em UTC (`TIMESTAMPTZ` no PostgreSQL, `Instant` no Java).
- Clientes enviam instantes **em UTC com `Z`**.
  ❓ **Dúvida:** não foi verificado se o backend aceita instantes com offset
  (`2026-08-01T11:30:00-03:00`); até confirmar, não envie offset.
- Datas sem hora (resposta do tipo `DATE`, campo `valueDate`) usam `AAAA-MM-DD`, sem fuso.
- **Relógio do dispositivo versus relógio do servidor.** Todo fato registrado em campo
  carrega dois instantes, e o par **nunca** é colapsado em um campo único:

| Entidade | Relógio do aparelho (`*AtDevice`) | Relógio do servidor |
| --- | --- | --- |
| `Inspection` (início) | `startedAtDevice` | `startedAtServer` |
| `Inspection` (conclusão) | `completedAtDevice` | `submittedAtServer` |
| `InspectionResponse` | `answeredAtDevice` | `answeredAtServer` |
| `Evidence` | `capturedAtDevice` | `serverReceivedAt` |
| `NonConformity` | `createdAtDevice` | `serverReceivedAt` |
| `AuditEvent` | `deviceOccurredAt` | `occurredAt` |

- O campo `*AtDevice` é enviado pelo aplicativo e preservado como veio. O campo do servidor é
  sempre preenchido pelo servidor; o valor enviado pelo cliente, se houver, é ignorado.
- O relógio do aparelho pode estar errado. Ordenação, cursor de sincronização e prazos usam o
  relógio do servidor; o relógio do aparelho serve para registrar quando o fato aconteceu em
  campo.
- Campos com sufixo `At` sem qualificador (`createdAt`, `updatedAt`, `approvedAt`,
  `canceledAt`, `reviewedAt`, `publishedAt`) são do servidor.

**⚠ Divergência:** o documento 10 e o card falam em `*_at_server` / `server_received_at`. No
JSON os nomes são camelCase (`startedAtServer`, `serverReceivedAt`), e o par da auditoria é
`deviceOccurredAt` / `occurredAt`, que não segue o sufixo `*AtDevice`.

❓ **Dúvida:** no lote de sincronização, quando `answeredAtDevice`, `capturedAtDevice` ou
`createdAtDevice` vem nulo, o servidor preenche com o próprio relógio. Isso perde a distinção
entre os dois relógios; o aplicativo deve sempre enviar o instante do aparelho.

## 5. Formato de erro

Todo erro da API, inclusive os de autenticação gerados antes do controller, usa o mesmo
objeto (`shared.presentation.dto.ApiError`):

| Campo | Tipo | Regra |
| --- | --- | --- |
| `timestamp` | string ISO 8601 UTC | Momento do erro no servidor. |
| `status` | número | Mesmo valor do status HTTP. |
| `code` | string | Código estável, legível por máquina (seção 6). **É o campo que o cliente usa para decidir.** |
| `message` | string | Texto para diagnóstico, em inglês. Não é para exibir diretamente ao usuário final. |
| `path` | string | URI da requisição, sem query string. |
| `requestId` | string | Identificador de correlação. Vem do cabeçalho `X-Request-Id` enviado pelo cliente ou é um UUID gerado pelo servidor; também volta no cabeçalho `X-Request-Id` da resposta. |
| `fieldErrors` | lista | Sempre presente; vazia quando não há erro por campo. Cada item tem `field` e `message`. |

Regras:

- O cliente decide pelo `code`, nunca pelo texto de `message`. A tradução para português é
  feita em cada interface.
- Nenhum erro expõe stack trace, SQL ou nome de classe interna. O detalhe técnico fica no log
  do servidor, ligado ao `requestId`.
- `500 INTERNAL_ERROR` é o único código genérico de falha não prevista.

**⚠ Divergência:** o exemplo do documento 12.2 tem `message` em português. O backend responde
em inglês, e a exibição em português fica a cargo do painel e do aplicativo.

### 5.1 Exemplos por status

Os exemplos usam códigos e mensagens reais do backend. Ids e `requestId` são ilustrativos.

**400 — requisição malformada ou inválida**

```json
{
  "timestamp": "2026-08-01T14:30:00Z",
  "status": 400,
  "code": "VALIDATION_ERROR",
  "message": "One or more fields failed validation",
  "path": "/api/v1/users",
  "requestId": "7d0c1d1e-2a7b-4d55-9b2a-1b7c3f0e9a11",
  "fieldErrors": [
    { "field": "email", "message": "must be a well-formed email address" }
  ]
}
```

**401 — sessão ausente, inválida ou expirada**

```json
{
  "timestamp": "2026-08-01T14:30:00Z",
  "status": 401,
  "code": "AUTH_INVALID_TOKEN",
  "message": "Authentication token is invalid or expired",
  "path": "/api/v1/inspections",
  "requestId": "0b8f6a52-6f1e-4c2e-9d0a-5e3c2f1a7b44",
  "fieldErrors": []
}
```

**403 — autenticado sem permissão**

```json
{
  "timestamp": "2026-08-01T14:30:00Z",
  "status": 403,
  "code": "AUTH_FORBIDDEN",
  "message": "You do not have permission to access this resource",
  "path": "/api/v1/users",
  "requestId": "c4e2b7a9-1f3d-4b6a-8e5c-2d9f0a7b3c18",
  "fieldErrors": []
}
```

**404 — recurso inexistente ou não visível ao usuário**

```json
{
  "timestamp": "2026-08-01T14:30:00Z",
  "status": 404,
  "code": "INSPECTION_NOT_FOUND",
  "message": "Inspection with id 5a1f0c3e-8b2d-4e7a-9c6f-0d4b2a8e1f37 not found",
  "path": "/api/v1/inspections/5a1f0c3e-8b2d-4e7a-9c6f-0d4b2a8e1f37/start",
  "requestId": "e91a4c2f-7b3d-4f0e-a6c8-3b5d1e9f2a60",
  "fieldErrors": []
}
```

**409 — conflito de estado, versão ou unicidade**

```json
{
  "timestamp": "2026-08-01T14:30:00Z",
  "status": 409,
  "code": "INSPECTION_INVALID_TRANSITION",
  "message": "Cannot approve an inspection in IN_PROGRESS.",
  "path": "/api/v1/inspections/5a1f0c3e-8b2d-4e7a-9c6f-0d4b2a8e1f37/approve",
  "requestId": "3c7e9a1b-5d2f-4a8c-b0e6-9f1d3a5c7e22",
  "fieldErrors": []
}
```

**422 — regra de negócio não atendida**

```json
{
  "timestamp": "2026-08-01T14:30:00Z",
  "status": 422,
  "code": "INSPECTION_REQUIRED_ITEMS_MISSING",
  "message": "The inspection has incomplete required responses",
  "path": "/api/v1/inspections/5a1f0c3e-8b2d-4e7a-9c6f-0d4b2a8e1f37/submit",
  "requestId": "a2d4f6b8-0c1e-4a3b-9d5f-7e9a1c3b5d70",
  "fieldErrors": [
    { "field": "items[9b1e7c3a-2f4d-4a6b-8c0e-1d3f5a7b9c21]", "message": "A response is required" },
    { "field": "items[1c3e5a7b-9d0f-4b2a-8c4e-6f8a0b2d4e63]", "message": "A response is required" }
  ]
}
```

Os mesmos campos aparecem em `405 METHOD_NOT_ALLOWED`, `413` (`EVIDENCE_TOO_LARGE`,
`SYNC_BATCH_TOO_LARGE`), `415 EVIDENCE_UNSUPPORTED_FORMAT` e `500 INTERNAL_ERROR`.

### 5.2 Erros dentro do lote de sincronização

O `POST /api/v1/mobile/sync/push` responde `200` mesmo quando operações falham. Cada operação
tem um resultado próprio, sem o objeto de erro:

```json
{
  "operationId": "6e2a8c4f-1b3d-4f5a-9c7e-0d2b4f6a8c19",
  "status": "CONFLICT",
  "entityVersion": null,
  "errorCode": "INSPECTION_RESPONSE_VERSION_CONFLICT",
  "errorMessage": "..."
}
```

- `status` usa o `SyncOpResult` da seção 2.1.
- `errorCode` usa os mesmos códigos da seção 6. Um `409` vira `CONFLICT`; os demais erros de
  negócio ou de validação viram `REJECTED`.
- Só o lote inteiro gera objeto de erro: `400 VALIDATION_ERROR`, `401`, `403` ou
  `413 SYNC_BATCH_TOO_LARGE` (limite padrão de 100 operações, `app.sync.max-operations`).

## 6. Catálogo de códigos de erro

Status HTTP por tipo de exceção no backend: `InvalidRequestException` → 400,
`AuthException` → 401, `ResourceNotFoundException` → 404, `ConflictException` → 409,
`PayloadTooLargeException` → 413, `UnsupportedFileTypeException` → 415,
`BusinessRuleException` → 422. Um código novo entra neste catálogo no mesmo PR do backend que
o cria (RN-090).

### 6.1 Gerais

| Código | HTTP | Significado |
| --- | --- | --- |
| `VALIDATION_ERROR` | 400 | Um ou mais campos do corpo falharam na validação; detalhes em `fieldErrors`. |
| `INVALID_REQUEST` | 400 | JSON malformado, campo desconhecido ou valor com tipo errado (enum, UUID, data). |
| `INVALID_SORT` | 400 | Campo ou direção de `sort` não permitidos para a listagem. |
| `METHOD_NOT_ALLOWED` | 405 | Método HTTP não suportado na rota. |
| `DATA_CONFLICT` | 409 | Restrição do banco violada por concorrência; recarregar e tentar de novo. |
| `INTERNAL_ERROR` | 500 | Falha não prevista; usar o `requestId` para suporte. |

### 6.2 Autenticação e autorização

| Código | HTTP | Significado |
| --- | --- | --- |
| `AUTH_INVALID_CREDENTIALS` | 401 | E-mail ou senha inválidos no login. |
| `AUTH_USER_INACTIVE` | 401 | Usuário `INACTIVE` ou `BLOCKED` tentou entrar ou usar um token. |
| `AUTH_INVALID_TOKEN` | 401 | Access token inválido ou expirado. |
| `AUTH_INVALID_REFRESH_TOKEN` | 401 | Refresh token inválido, expirado ou revogado. |
| `AUTH_AUTHENTICATION_REQUIRED` | 401 | Rota protegida chamada sem token. |
| `AUTH_FORBIDDEN` | 403 | Perfil sem permissão para a operação. |

### 6.3 Recurso não encontrado (404)

O padrão é `<ENTIDADE>_NOT_FOUND`. Recurso que existe mas não pertence ao usuário (por exemplo,
inspeção de outro técnico) responde o mesmo `404`, para não revelar a existência.

| Código | Significado |
| --- | --- |
| `USER_NOT_FOUND` | Usuário inexistente. |
| `CLIENT_NOT_FOUND` | Cliente inexistente. |
| `EQUIPMENT_NOT_FOUND` | Equipamento inexistente (inclusive por QR Code). |
| `INSPECTION_NOT_FOUND` | Inspeção inexistente ou não atribuída ao técnico autenticado. |
| `EVIDENCE_NOT_FOUND` | Evidência inexistente. |
| `INSPECTION_TEMPLATE_NOT_FOUND` | Modelo de inspeção inexistente. |
| `TEMPLATE_DRAFT_NOT_FOUND` | Modelo sem rascunho editável. |
| `TEMPLATE_SECTION_NOT_FOUND` | Seção inexistente no rascunho. |
| `TEMPLATE_ITEM_NOT_FOUND` | Item inexistente no rascunho. |
| `TEMPLATE_VERSION_NOT_FOUND` | Versão publicada inexistente. |

**⚠ Divergência — códigos 404 montados a partir do nome da entidade.** O backend gera o código
com `entity.toUpperCase() + "_NOT_FOUND"`, e alguns pontos passam nomes com espaço ou sem
`_`. Os códigos resultantes hoje são:

| Chamada no backend | Código gerado | Código esperado |
| --- | --- | --- |
| `of("Inspection site", ...)` (`EquipmentService`, `InspectionSchedulingService`) | `INSPECTION SITE_NOT_FOUND` | `INSPECTION_SITE_NOT_FOUND` |
| `of("InspectionSite", ...)` (`InspectionSiteService`) | `INSPECTIONSITE_NOT_FOUND` | `INSPECTION_SITE_NOT_FOUND` |
| `of("NonConformity", ...)` (`NonConformityService`) | `NONCONFORMITY_NOT_FOUND` | `NON_CONFORMITY_NOT_FOUND` |
| `of("Inspection template version", ...)` (`InspectionSchedulingService`) | `INSPECTION TEMPLATE VERSION_NOT_FOUND` | `TEMPLATE_VERSION_NOT_FOUND` |

Até a correção no backend, clientes devem tratar qualquer `404` pelo status, não pelo código
dessas entidades.

### 6.4 Cadastros

| Código | HTTP | Significado |
| --- | --- | --- |
| `EMAIL_ALREADY_EXISTS` | 409 | Já existe usuário com o e-mail. |
| `EQUIPMENT_QR_CODE_ALREADY_EXISTS` | 409 | Já existe equipamento com o QR Code. |
| `INVALID_QR_CODE` | 400 | QR Code vazio. |
| `INVALID_CLIENT_DOCUMENT` | 400 | Documento do cliente fora do formato de CPF ou CNPJ. |
| `INVALID_COORDINATES` | 400 | Latitude ou longitude do local fora do intervalo. |
| `CLIENT_INACTIVE` | 422 | Cliente inativo não pode ser usado em nova inspeção. |
| `INSPECTION_SITE_INACTIVE` | 422 | Local inativo não pode ser usado em nova inspeção. |
| `INACTIVE_SITE` | 422 | Não é possível cadastrar equipamento em local inativo. |

### 6.5 Modelos de inspeção

| Código | HTTP | Significado |
| --- | --- | --- |
| `INVALID_TEMPLATE_STATUS` | 400 | O status do modelo só pode ser alterado para `ACTIVE` ou `INACTIVE`. |
| `TEMPLATE_ITEM_RESPONSE_TYPE_REQUIRED` | 400 | Item sem `responseType`. |
| `TEMPLATE_ITEM_OPTIONS_REQUIRED` | 400 | `SINGLE_CHOICE` sem opções. |
| `TEMPLATE_ITEM_OPTIONS_NOT_ALLOWED` | 400 | Opções em tipo que não é `SINGLE_CHOICE`. |
| `TEMPLATE_ITEM_OPTIONS_DUPLICATED` | 400 | Opções repetidas. |
| `TEMPLATE_ITEM_FAILURE_RULES_NOT_ALLOWED` | 400 | Regra de observação ou evidência na falha em item que não é `CONFORMITY` nem `BOOLEAN`. |
| `TEMPLATE_TITLE_ALREADY_EXISTS` | 409 | Já existe modelo com o título. |
| `TEMPLATE_VERSION_PUBLISHED` | 409 | Tentativa de alterar versão publicada (imutável). |
| `TEMPLATE_PUBLISH_INVALID` | 422 | Rascunho com pendências; todas em `fieldErrors`. |
| `TEMPLATE_REORDER_MISMATCH` | 422 | Lista de reordenação não corresponde às seções ou itens existentes. |
| `TEMPLATE_NOT_PUBLISHED` | 422 | Modelo sem versão publicada não pode ser ativado nem retirado. |
| `TEMPLATE_VERSION_NOT_PUBLISHED` | 422 | Só versões publicadas podem ser disponibilizadas para novas inspeções. |
| `TEMPLATE_VERSION_UNAVAILABLE` | 422 | Versão retirada de novas inspeções. |

### 6.6 Agendamento e transições da inspeção

| Código | HTTP | Significado |
| --- | --- | --- |
| `INSPECTION_CANCEL_REASON_REQUIRED` | 400 | Cancelamento sem motivo. |
| `INSPECTION_LOCATION_INVALID` | 400 | Localização de início ou conclusão inválida. |
| `INSPECTION_INVALID_TRANSITION` | 409 | Ação não permitida no estado atual (tabela 2.2). |
| `SCHEDULED_FOR_REQUIRED` | 422 | Inspeção sem data agendada. |
| `SITE_CLIENT_MISMATCH` | 422 | Local não pertence ao cliente informado. |
| `EQUIPMENT_REQUIRED` | 422 | Categoria do modelo exige equipamento. |
| `EQUIPMENT_INACTIVE` | 422 | Equipamento inativo não pode ser usado em nova inspeção. |
| `EQUIPMENT_SITE_MISMATCH` | 422 | Equipamento não pertence ao local. |
| `TECHNICIAN_NOT_ELIGIBLE` | 422 | Só usuário ativo com perfil `TECHNICIAN` pode ser atribuído. |
| `INVALID_TECHNICIAN`, `INVALID_SUPERVISOR` | 422 | Usuário informado não está ativo com o perfil exigido. ❓ código montado dinamicamente a partir do nome do campo (`"INVALID_" + campo`); os dois valores acima são os esperados. |
| `INSPECTION_NOT_EDITABLE` | 422 | Só inspeções `DRAFT` ou `ASSIGNED` podem ser editadas. |
| `INSPECTION_SNAPSHOT_INVALID` | 422 | Snapshot dos itens inconsistente. |
| `INSPECTION_REQUIRED_ITEMS_MISSING` | 422 | Envio com itens obrigatórios sem resposta; um item por entrada em `fieldErrors` (`items[<id>]`). |
| `INSPECTION_CRITICAL_NON_CONFORMITY_EVIDENCE_MISSING` | 422 | Não conformidade crítica sem evidência no envio. |
| `INSPECTION_REJECTION_REASON_REQUIRED` | 422 | Reprovação sem motivo. |

### 6.7 Respostas do checklist

| Código | HTTP | Significado |
| --- | --- | --- |
| `RESPONSE_ID_MISMATCH` | 400 | Id da rota difere do id do corpo. |
| `INSPECTION_RESPONSE_ID_REUSED` | 409 | UUID já usado em outra resposta. |
| `INSPECTION_RESPONSE_ALREADY_EXISTS` | 409 | Item já respondido com outro id. |
| `INSPECTION_RESPONSE_VERSION_CONFLICT` | 409 | `baseVersion` diferente da versão atual no servidor. |
| `INSPECTION_RESPONSE_INVALID` | 422 | Valor incompatível com o `responseType` do item. |
| `INSPECTION_ITEM_NOT_IN_SNAPSHOT` | 422 | Item não pertence ao snapshot da inspeção. |
| `INSPECTION_RESPONSES_LOCKED` | 422 | Respostas não podem ser alteradas depois do envio da inspeção. |
| `INSPECTION_REOPEN_REQUIRED` | 422 | Inspeção `REJECTED`; é preciso reabrir antes de corrigir. |
| `INSPECTION_ITEM_NOT_IN_CORRECTION` | 422 | Item fora da lista `itemsToCorrect` da última reprovação. |
| `INSPECTION_APPROVED_READ_ONLY` | 422 | Inspeção aprovada não aceita alteração. |

### 6.8 Evidências

| Código | HTTP | Significado |
| --- | --- | --- |
| `EVIDENCE_TOO_LARGE` | 413 | Arquivo acima do limite (padrão 10 MiB, `app.evidence.max-size-bytes`). |
| `EVIDENCE_UNSUPPORTED_FORMAT` | 415 | Formato diferente de `image/jpeg` ou `image/png`. |
| `EVIDENCE_ID_REUSED` | 409 | UUID já usado em outra evidência. |
| `EVIDENCE_READ_ONLY` | 422 | Evidência de inspeção aprovada é somente leitura. |
| `EVIDENCE_RESPONSE_NOT_IN_INSPECTION` | 422 | Resposta vinculada não pertence à inspeção. |
| `EVIDENCE_NON_CONFORMITY_NOT_IN_INSPECTION` | 422 | Não conformidade vinculada não pertence à inspeção. |
| `EVIDENCE_LOCATION_INVALID` | 422 | Localização da captura inválida. |

### 6.9 Não conformidades

| Código | HTTP | Significado |
| --- | --- | --- |
| `NON_CONFORMITY_ID_REUSED` | 409 | UUID já usado em outra não conformidade. |
| `NON_CONFORMITY_ITEM_NOT_IN_INSPECTION` | 422 | Item não pertence à inspeção. |
| `NON_CONFORMITY_RESPONSE_NOT_IN_INSPECTION` | 422 | Resposta não pertence à inspeção. |
| `NON_CONFORMITY_STATUS_UNSUPPORTED` | 422 | Status diferente de `OPEN` (único do MVP). |

### 6.10 Sincronização

| Código | HTTP | Significado |
| --- | --- | --- |
| `SYNC_BATCH_TOO_LARGE` | 413 | Lote acima do limite de operações. |
| `SYNC_CURSOR_INVALID` | 400 | Cursor do `pull` inválido. |
| `SYNC_LIMIT_INVALID` | 400 | `limit` do `pull` fora do intervalo permitido (padrão 200). |
| `SYNC_ENTITY_TYPE_UNSUPPORTED` | resultado `REJECTED` | `entityType` não suportado. |
| `SYNC_OPERATION_TYPE_UNSUPPORTED` | resultado `REJECTED` | `operationType` não suportado para a entidade. |
| `SYNC_PAYLOAD_INVALID` | resultado `REJECTED` | Payload ausente ou inválido. |
| `SYNC_INSPECTION_ID_REQUIRED` | resultado `REJECTED` | Payload sem `inspectionId`. |
| `EVIDENCE_FILE_DESCRIPTION_REQUIRED` | resultado `REJECTED` | Metadados de evidência sem tamanho ou SHA-256. |
| `EVIDENCE_CONTENT_REQUIRED` | resultado `REJECTED` | `UPLOAD` sem conteúdo. |
| `EVIDENCE_CONTENT_INVALID` | resultado `REJECTED` | Conteúdo que não é base64 válido. |
| `SYNC_DUPLICATE_OPERATION_ID` | resultado `REJECTED` | O mesmo `operationId` aparece mais de uma vez no lote. |
| `SYNC_DEPENDENCY_NOT_FOUND` | resultado `REJECTED` | `dependsOn` aponta para operação fora do lote. |
| `SYNC_DEPENDENCY_CYCLE` | resultado `REJECTED` | Dependências em ciclo. |
| `SYNC_DEPENDENCY_FAILED` | resultado `DEPENDENCY_FAILED` | Uma dependência não foi aplicada. |
| `SYNC_OPERATION_FAILED` | resultado `REJECTED` | Falha não prevista ao aplicar a operação. |

"Resultado" indica que o código aparece em `errorCode` do resultado por operação (seção 5.2), e
não como status HTTP.

### 6.11 Códigos pedidos pelo card e o código real

O card da issue #2 lista códigos que o backend implementou com outro nome ou de outra forma:

| Pedido no card | Implementado | Observação |
| --- | --- | --- |
| `AUTH_TOKEN_EXPIRED` | `AUTH_INVALID_TOKEN` (401) | Token expirado e token inválido usam o mesmo código. Para o refresh, `AUTH_INVALID_REFRESH_TOKEN`. |
| `RESOURCE_NOT_FOUND` | `<ENTIDADE>_NOT_FOUND` (404) | Ver seção 6.3. |
| `QR_CODE_ALREADY_EXISTS` | `EQUIPMENT_QR_CODE_ALREADY_EXISTS` (409) | |
| `INSPECTION_NOT_ASSIGNED_TO_USER` | `INSPECTION_NOT_FOUND` (404) | Inspeção de outro técnico é tratada como inexistente. |
| `EVIDENCE_UNSUPPORTED_TYPE` | `EVIDENCE_UNSUPPORTED_FORMAT` (415) | Status 415, não 422. |
| `EVIDENCE_TOO_LARGE` | `EVIDENCE_TOO_LARGE` (413) | Igual. |
| `SYNC_VERSION_CONFLICT` | resultado `CONFLICT` com `errorCode` da entidade, por exemplo `INSPECTION_RESPONSE_VERSION_CONFLICT` | Na rota de recurso, `409 INSPECTION_RESPONSE_VERSION_CONFLICT`. |
| `SYNC_DEPENDENCY_FAILED` | `SYNC_DEPENDENCY_FAILED` | Só como resultado por operação. |

## 7. Paginação, ordenação e filtros

- Parâmetros: `?page=0&size=20&sort=campo,asc`.
  - `page`: índice começando em `0`. Valor negativo é tratado como `0`.
  - `size`: padrão `20`, máximo `100`. Valor acima do máximo é reduzido para `100` (não gera
    erro); valor menor que `1` vira `1`.
  - `sort`: `campo` ou `campo,direção`, com direção `asc` ou `desc`. Um único campo por
    requisição. Campo fora da lista permitida da listagem responde `400 INVALID_SORT`.
- Ordem padrão de cada listagem é definida pela rota (por exemplo, `createdAt,desc` em
  usuários e `scheduledFor,asc` em inspeções).
- Filtros são parâmetros de consulta explícitos e nomeados, combinados por "e"
  (ex.: `GET /api/v1/inspections?status=IN_PROGRESS&technicianId=<uuid>&priority=HIGH`).
  Filtros de enum usam o literal exato da seção 2; filtros de período usam instantes ISO 8601
  (`scheduledFrom`, `scheduledTo`).
- Envelope de resposta de toda listagem paginada (`shared.presentation.dto.PageResponse`):

```json
{
  "content": [],
  "page": 0,
  "size": 20,
  "totalElements": 57,
  "totalPages": 3,
  "first": true,
  "last": false
}
```

- A sincronização (`GET /api/v1/mobile/sync/pull`) não usa `page`/`size`: usa cursor opaco
  (`cursor`, `nextCursor`, `hasMore`). O cliente guarda o cursor sem interpretar o conteúdo.

## 8. Evolução do contrato

- Mudança compatível (novo campo opcional, novo código de erro, novo valor de enum
  **apenas** quando os três clientes já tratam valor desconhecido) pode entrar em `/api/v1`.
- Remover ou renomear campo, literal de enum ou código de erro é mudança incompatível e exige
  decisão registrada em ADR.
- Toda alteração de enum, código de erro ou regra deste documento entra no mesmo PR, ou em PR
  vinculado, da mudança no `backend`.
