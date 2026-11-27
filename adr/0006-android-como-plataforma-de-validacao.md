# ADR-0006 — Android como plataforma de validação do mobile

- Status: aceito
- Data: 2026-11-24
- Sprint: 1

## Contexto

O aplicativo usa Expo e React Native, que geram Android e iOS a partir do mesmo código. Com
16 semanas de MVP, validar câmera, QR Code, localização e armazenamento local nas duas
plataformas dobraria o esforço de teste. O Anexo B (decisão 3) define o mobile como
prioritariamente Android.

## Decisão

- Android é a plataforma obrigatória de validação do aplicativo: critérios de aceitação,
  testes manuais e evidências de PR são feitos em Android (emulador Android 13, API 33, ou
  superior, ou aparelho físico).
- iOS não é bloqueado no código, mas não é critério de aceite do MVP.

## Alternativas consideradas

- **Validar Android e iOS**: aumentaria o custo de teste e exigiria equipamento Apple para toda
  a equipe.
- **Só iOS**: contraria o Anexo B, decisão 3.

## Consequências

- Recursos nativos (câmera, QR Code, localização, permissões) são verificados apenas em
  Android.
- Diferenças de comportamento no iOS podem aparecer depois do MVP e não são consideradas
  defeito do MVP.
