# ADR-0002 — SQLite como persistência operacional, não cache descartável

- Status: aceito
- Data: 2026-11-24
- Sprint: 1

## Contexto

O técnico trabalha em locais sem internet. O documento 11.9 e o Anexo B (decisões 4 e 5)
exigem que o aplicativo opere offline depois de baixar a inspeção e que o SQLite seja
persistência operacional. Se o banco local fosse um cache, apagar o cache ou reinstalar a
consulta apagaria respostas, fotos e não conformidades ainda não enviadas.

## Decisão

- O SQLite do aplicativo (`expo-sqlite`) é a fonte de verdade do dispositivo para o que o
  técnico registra em campo até a confirmação do servidor.
- Toda escrita do técnico vai primeiro para o SQLite, na mesma transação que gera a operação
  da outbox (ADR-0003).
- Cada entidade sincronizável guarda metadados locais (`local_sync_status`, `is_dirty`,
  `base_version`, `sync_error`, `deleted_locally`), conforme o documento 10.13.2.
- O esquema local é versionado por migrações próprias do aplicativo
  (`src/infrastructure/database/migrations.ts`).
- Dados locais com operação pendente nunca são descartados por limpeza de cache ou por novo
  download.

## Alternativas consideradas

- **Cache em memória ou AsyncStorage**: sem transações nem consultas, e sujeito a limpeza.
- **Banco local como cache descartável, recarregado a cada abertura**: perderia o trabalho
  offline, contrariando o Anexo B.

## Consequências

- O aplicativo precisa de migrações locais e de cuidado com atualização de esquema.
- O estado local de sincronização (`SYNCED`, `PENDING`, `SYNCING`, `ERROR`, `CONFLICT`) fica
  separado do estado de negócio da inspeção (documento 10.2).
- Logout com operações pendentes precisa de tratamento explícito para não perder dados.
