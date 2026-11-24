# ADR-0004 — Modelo de inspeção versionado com snapshot por inspeção

- Status: aceito
- Data: 2026-11-24
- Sprint: 1

## Contexto

Modelos de inspeção mudam com o tempo, mas uma inspeção já agendada, executada ou aprovada
precisa continuar mostrando exatamente as perguntas que o técnico respondeu. O Anexo B
(decisões 7 e 8) e o documento 10.2 pedem modelo versionado e snapshot por inspeção.

## Decisão

- O modelo (`inspection_templates`) tem versões (`inspection_template_versions`) com seções
  e itens. Uma versão publicada é imutável; a única alteração permitida depois da publicação é
  retirá-la de novas inspeções (`active_for_new_inspections`).
- O rascunho é a versão `version_number = 0`, nas mesmas tabelas das versões publicadas. A
  publicação copia o rascunho para uma nova versão numerada. Detalhes no
  [ADR-001 do backend](https://github.com/FieldOps-Project/backend/blob/develop/docs/adr/ADR-001-draft-representation-strategy.md).
- Ao criar a inspeção, o servidor copia os itens da versão publicada para
  `inspection_item_snapshots` (título, tipo de resposta, obrigatoriedade, regras, opções e
  ordem). Respostas apontam para o item do snapshot, não para o item do modelo.
- Alterar o modelo depois da criação não altera nenhuma inspeção existente.

## Alternativas consideradas

- **Inspeção apontando direto para o item do modelo**: qualquer edição do modelo mudaria
  inspeções antigas.
- **Só versionar, sem snapshot**: protege contra edição, mas obriga toda leitura a
  reconstruir a versão e acopla a execução ao ciclo de vida do modelo.

## Consequências

- O snapshot duplica dados por inspeção; o volume é pequeno e aceitável.
- O aplicativo baixa o snapshot e pode executar offline sem conhecer o modelo.
- Tentativa de alterar versão publicada responde `409 TEMPLATE_VERSION_PUBLISHED`.
