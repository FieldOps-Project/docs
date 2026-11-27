# ADR-0005 — UUID gerado também no dispositivo

- Status: aceito
- Data: 2026-11-24
- Sprint: 1

## Contexto

O técnico cria respostas, evidências e não conformidades sem conexão. Com id sequencial do
banco, o aplicativo precisaria de ids temporários e de remapeamento depois de sincronizar, e um
reenvio poderia criar registros duplicados (documento 10.2, "Identificadores globais").

## Decisão

- Todas as entidades usam UUID como identificador, no SQLite e no PostgreSQL.
- Entidades criadas em campo (resposta, evidência, não conformidade e cada operação da outbox)
  têm o UUID gerado no dispositivo, com UUID v4 (`randomUUID()` do `expo-crypto`). O
  servidor aceita o id enviado e não gera outro.
- Entidades criadas pelo painel ou pela API têm o UUID gerado pelo servidor.
- Reusar um UUID existente para outro registro responde `409` (`*_ID_REUSED`).
- As regras completas estão no [contrato compartilhado](../contrato-compartilhado.md),
  seção 3.

## Alternativas consideradas

- **Id sequencial com id temporário no dispositivo**: exige tabela de mapeamento e reescrita
  de chaves estrangeiras locais depois de cada sincronização.
- **Id gerado só pelo servidor, com reserva prévia de faixa**: depende de conexão antes do
  trabalho em campo.

## Consequências

- Criação offline sem remapeamento, e reenvio idempotente com o mesmo id.
- Índices UUID são maiores que inteiros; o volume do MVP torna isso irrelevante.
- O servidor não confere a versão do UUID; a exigência de v4 vale para os clientes.
